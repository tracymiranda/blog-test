---
title: "Merklemap: Scaling Certificate Transparency with 100B+ Rows of Data"
datePublished: Wed Apr 23 2025 11:41:02 GMT+0000 (Coordinated Universal Time)
cuid: cm9tv4snz000i08i650yzd92r
slug: merklemap-scaling-certificate-transparency-with-100b-rows-of-data
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ZiQkhI7417A/upload/720a8500f3175f8a2a3a792ffee66205.jpeg
tags: postgresql, zfs, certificate-transparency, merklemap

---

[Merklemap](https://www.merklemap.com/) is a certificate transparency monitor that not only continuously fetches the latest data from certificate transparency logs in real-time, but also stores that data for search, archival, and statistical purposes. While certificate transparency (CT) logs are heavily sharded across different logs, we wanted to gather all this data into a single source of truth that can be indexed, queried and searched. In this post, we dive into the architecture and technical choices that enable Merklemap to operate at CT scale.

## Postgres and ZFS

While many distributed databases boast large-scale capabilities, we chose PostgreSQL for its proven performance, our operational experience, and because alternatives offered no clear benefit for our needs. With today's hardware capabilities, we were confident that we could fit all certificate transparency data in a single shard for the foreseeable future, and possibly indefinitely. The database we currently operate contains approximately **20TB of storage** and over **100 billion unique rows**.

We run two replicas for the core database with AMD EPYC 9454Ps, 6NVMe drives and 1TB of memory in each replica.

We chose ZFS as our storage solution for PostgreSQL to leverage its compression, snapshots, performance characteristics, and send/receive capabilities. ZFS's default recordsize of 128K serves as an excellent baseline for maintaining optimal performance in terms of compression ratio, sequential scans, and system overhead.

However, PostgreSQL's default page size of 8K presented a challenge: we would experience significant read/write amplification. For each page operation, we'd need to read or write an entire 128K record, resulting in a 16x overhead for many operations.

Traditionally, database administrators are advised to reduce the ZFS recordsize to match or approximate PostgreSQL's page size. This approach, however, would result in very small ZFS records (8K or 16K), substantially increasing global overhead and nearly eliminating the compression benefits, one of the primary advantages that make ZFS attractive in the first place.

Instead, we took the opposite approach: increasing PostgreSQL's block size to get closer to ZFS's default recordsize. Postgres maximum blocksize is 32L, so we will use that.

This setup is not difficult to achieve and can be done using the "configure" script:

`./configure --with-blocksize=32 [other-options]`

## Optimizing for Append-Only Operations

PostgreSQL's VACUUM process handles multiple critical "cleanup" operations, including recycling dead tuples, updating statistics, and preventing transaction ID wraparound.

We designed our schema to be as "append-only" as possible. This design choice significantly reduces the workload on VACUUM operations since fewer updates and deletes mean fewer dead tuples to clean up. We configured autovacuum to be very aggressive with the following settings:

`- autovacuum_max_workers to the number of merklemap tables`

`- autovacuum_naptime 10s`

`- autovacuum_vacuum_cost_delay 1ms`

`- autovacuum_vacuum_cost_limit 2000`

## Handling Transaction ID Wraparound

Despite our aggressive autovacuum configuration, sometimes the system would still become I/O constrained due to the high rate of data ingestion. This constraint particularly affects transaction ID wraparound issues.

According to the PostgreSQL [VACUUM-FOR-WRAPAROUND documentation](https://www.postgresql.org/docs/current/routine-vacuuming.html#VACUUM-FOR-WRAPAROUND): “*…since transaction IDs have limited size (32 bits) a cluster that runs for a long time (more than 4 billion transactions) would suffer transaction ID wraparound: the XID counter wraps around to zero, and all of a sudden transactions that were in the past appear to be in the future — which means their output become invisible. In short, catastrophic data loss. (Actually the data is still there, but that's cold comfort if you cannot get at it.) To avoid this, it is necessary to vacuum every table in every database at least once every two billion transactions.*”

We proactively monitor database age metrics continuously. When predetermined thresholds are crossed, we implement a dynamic throttling mechanism: reducing ingestion rate, prioritizing newer logs, and once autovacuum catches up we ramp up ingestion again. . Once stable, ingestion ramps up again. Our append-only schema design significantly helps this process by minimizing the workload for autovacuum operations.

## Relation-less?

A frequent recommendation for scaling write operations in PostgreSQL is to use the COPY command, minimize relations and constraints, and essentially "just pipe the data in." While this advice is common, it doesn't represent the optimal approach for every use case.

## Prioritizing Data Integrity

When developing Merklemap, one of our primary motivations for choosing PostgreSQL was to leverage its robust constraint enforcement and ACID properties. Consequently, we designed our schema as a well-normalized relational model rather than optimizing solely for insertion efficiency.

This design decision does necessitate significantly more read operations and database commits, but we consider this performance trade-off worthwhile. The benefits of data integrity, consistency, and the ability to enforce complex relationships outweigh the potential write performance gains of a denormalized approach.

## Optimizing Commit Performance

To mitigate the impact of our high commit frequency, we utilize PostgreSQL's commit grouping capabilities through the combination of commit\_delay and commit\_siblings parameters. This configuration allows the database to batch multiple commits together, reducing overhead.

At Merklemap, we maintain commit\_delay at 100ms and keep commit\_siblings at its default value of 5. This setup effectively balances our need for timely transaction completion with the efficiency benefits of grouped commits.Handling Transaction ID Wraparound

Despite our aggressive autovacuum configuration, sometimes the system would still become I/O constrained due to the high rate of data ingestion. This constraint particularly affects transaction ID wraparound issues.

### **Avoiding Partitioning**

While PostgreSQL supports table partitioning, we found its benefits too narrow to justify the complexity. Partitioning can ease deletions, but brings challenges in constraint handling, indexing, foreign key management, and query planning. For us, well-indexed regular tables with thoughtful query discipline proved more efficient and simpler to maintain.

### **Query Discipline at Scale**

Our general queries execute extremely fast, typically returning results in just a few microseconds. This efficiency stems from our strict adherence to querying only indexed data. We've carefully designed our indexing strategy to support our most common access patterns while balancing the overhead of index size and insert performance impact.

With over 100+ billion rows, casual querying is a non-starter. We strictly query indexed data, use partial, covering and expression indexes, and maintain materialized views for common aggregations. We also enforce query governor policies to prevent expensive scans during peak usage periods.

## Compression algorithm

We use lz4 for ZFS compression, as it proved to be computationally negligible for our workload while providing a compression ratio of approximately 2x. We evaluated zstd, but lz4 offered the optimal balance between performance impact and storage savings.

## Resilient Ingestion Strategy

We follow a "fail-fast, retry-often" approach. While certificate transparency logs are generally reliable, like any remotely accessible service, they can experience downtime or performance issues. Fortunately, since certificates and pre-certificates are replicated across multiple logs, monitoring interruptions don't pose a significant problem.

Our operation resembles that of a traditional search engine, dynamically throttling our queries based on "perceived" load. When we detect that a log is slower than usual or returning errors, we adjust accordingly.

We implement intelligent retry mechanisms that activate when logs return to a healthy state, ensuring we fill any gaps in our data where we previously encountered errors. This approach handles scenarios where specific blocks in the logs consistently fail for a period while other blocks remain accessible.

## Efficient Ingestion

We implement our ingest workers using Rust’s Tokio library. We carefully pool a minimal number of PostgreSQL connections to maintain optimal performance, following best practices outlined in [EDB's connection pooling recommendations](https://www.enterprisedb.com/postgres-tutorials/why-you-should-use-connection-pooling-when-setting-maxconnections-postgres).

With this optimized approach, we only require 5-6 CPU cores to ingest 5-8 million entries per hour.

## Historical Data Volume Explanation?

You might wonder why our database has accumulated such a large volume of data. While the current certificate transparency traffic averages around 1 million certificates per hour, we've been ingesting data available since the inception of certificate transparency.

# Conclusion

At Merklemap, we believe that traditional relational databases like PostgreSQL can still be the right choice for specialized large-scale applications when properly configured and optimized. By housing over 100 billion rows of certificate transparency data in a single PostgreSQL shard, we think we’ve shown that thoughtful architecture decisions can outweigh the immediate appeal of distributed systems in specific use cases.

While our current infrastructure comfortably handles the present certificate transparency ecosystem, we continue to monitor growth trends and optimize our processes. The techniques outlined in this article aren't just applicable to certificate transparency, they represent valuable approaches for any PostgreSQL deployment dealing with high-volume workloads.