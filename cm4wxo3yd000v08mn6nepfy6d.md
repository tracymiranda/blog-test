---
title: "PostgreSQL Support for Certificate Transparency Logs Released"
seoTitle: "PostgreSQL Support for Certificate Transparency Logs Released"
seoDescription: "Trillian Now Supports PostgreSQL Storage Backend Contributed by Sectigo"
datePublished: Fri Dec 20 2024 15:56:49 GMT+0000 (Coordinated Universal Time)
cuid: cm4wxo3yd000v08mn6nepfy6d
slug: postgresql-support-for-certificate-transparency-logs-released
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/P4vkOwEav7I/upload/110318ad7d043dbb56339c5df34227d6.jpeg
tags: postgresql, certificate-transparency

---

Certificate Transparency logs can now leverage PostgreSQL’s reliability and performance, giving log operators additional storage choice and flexibility. Trillian, the open source project that powers the certificate transparency (CT) ecosystem, now supports PostgreSQL as a storage and quota manager backend, thanks to a major contribution by Sectigo. Sectigo, as a public CA and an established part of the CT ecosystem, operates its own logs and runs the popular [crt.sh](http://crt.sh) Certificate Search tool. Trillian originally supports Google’s Cloud Spanner as well as MySQL as supported storage backends, though was always designed to be extensible to other storage implementations. And it is not just the CT ecosystem that benefits but any other Trillian-based transparency logs can take advantage of the new storage backend available in the recent release [Trillian v1.7.0](https://github.com/google/trillian/releases/tag/v1.7.0).

### Why PostgreSQL?

PostgreSQL is a powerful, open source relational database known for its robustness, scalability, and extensibility. Sectigo originally used MariaDB as the transparency log storage backend. However, a [CT log failure earlier this year](https://groups.google.com/a/chromium.org/g/ct-policy/c/038B7F4g8cU/m/KsOJaEhnBgAJ) due to MariaDB corruption after disk space exhaustion provided the motivation for a change. PostgreSQL, with its robust Write-ahead Logging (WAL) and strict adherence to ACID (Atomicity, Consistency, Isolation, Durability) principles, made it a better option for avoiding corruption and improving data integrity of the log.

While reliability was the catalyst for this transition, other factors also motivated the shift. The Sectigo team has deep expertise with PostgreSQL, including extensive experience operating [crt.sh](http://crt.sh) for years. Their experience, which included architecting several iterations of crt.sh’s CT log ingestion application to accommodate the rapid growth of WebPKI issuance rates, proved invaluable when optimising write performance in this new Trillian feature.

### Trillian + PostgreSQL

Open source thrives on collaboration and contributions and the [transparency.dev](http://transparency.dev) community embodies this. Trillian [welcomes contributions](https://github.com/google/trillian/blob/master/CONTRIBUTING.md) and using this process Sectigo [opened a PR](https://github.com/google/trillian/pull/3644) and worked with Google’s TrustFabric team to incorporate the changes, including tests and documentation, into Trillian. The [Feature Implementation Matrix](https://google.github.io/trillian/docs/Feature_Implementation_Matrix.html) highlights PostgreSQL as one of the supported options now, currently in Alpha stage. Sectigo is working on spinning up development logs and will spin up new production logs (code name Elephant) in due course and looks forward to keeping the community up to date.

### Getting Started with Trillian and PostgreSQL for CT

To take advantage of using PostgreSQL for CT:

1. Install the latest versions which support PostgreSQL: users will require upgrading to
    
    * [Trillian v1.7.0](https://github.com/google/trillian/releases/tag/v1.7.0)
        
    * [certificate-transparency-go v1.3.0](https://github.com/google/certificate-transparency-go/releases/tag/v1.3.0)
        
2. Set up PostgreSQL.
    
3. Configure a PostgreSQL instance with the appropriate schemas and permissions as needed
    
4. See [Trillian deployment examples](https://github.com/google/trillian/tree/master/examples/deployment) which now include a [PostgreSQL docker-compose example](https://github.com/google/trillian/tree/master/examples/deployment/postgresql).
    

Whether you’re setting up your first CT log or scaling an existing one, you now have more choices than ever and can now leverage PostgreSQL’s reliability and performance.