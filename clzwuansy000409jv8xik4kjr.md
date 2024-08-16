---
title: "Introducing Trillian Tessera"
seoTitle: "Trillian Tessera: Next Generation Tile-based Transparency Logs"
seoDescription: "Trillian Tessera: Next Generation Tile-based Transparency Logs"
datePublished: Fri Aug 16 2024 15:03:51 GMT+0000 (Coordinated Universal Time)
cuid: clzwuansy000409jv8xik4kjr
slug: introducing-trillian-tessera
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/0hUDzambiwA/upload/982f9d429fc3d63f0f983234e6a9ee99.jpeg
tags: transparency, certificate-transparency, tlogs

---

Today, we’re introducing Trillian Tessera, a Go library for building tile-based transparency logs (tlogs). As the next evolution of Trillian, Trillian Tessera implements [tile-based logs](https://research.swtch.com/tlog#tiling_a_log) to simplify and reduce the cost associated with building and operating transparency logs. Tile-based logs strengthen transparency systems by being more efficient and cost-effective, providing clients with higher performance.

### Why Tiles?

In this article titled [Tile-Based Transparency Logs](https://transparency.dev/articles/tile-based-logs/), Jay Hou outlines how the tiles approach works and has been tested and proven in ecosystems such as the Go module checksum database and Pixel binary transparency. The article also outlines the many benefits of a tiles-based architecture:

* Improved cacheability
    
* Logs are easier to spin up
    
* Logs are easier to maintain
    
* Static responses
    
* Uniform interface across logs and ecosystems e.g. witness network can be shared across all logs
    

### Why Tessera?

The name “Tessera” was chosen to reflect the core concept of the new design. A *tessera* is a small square **tile** of stone, glass, marble etc. that was used in mosaics. This is an apt metaphor for our project, as it emphasizes the tile-based structure of transparency logs.

### Trillian Tessera Design Philosophy

While a tiles-native API is central to the design, we also incorporated several insights gained over the years. These insights have informed our key design philosophy.

* **Simplicity** - Tessera is designed to be user-friendly, easy to adopt, and straightforward to maintain. It offers a more cost-effective and simpler operation compared to Trillian v1. As a  library rather than a microservice architecture, Tessera aims to facilitate the easy creation and deployment of new transparency logs on supported infrastructure.
    
* **Multi-implementation Storage** - Each supported storage infrastructure for Trillian Tessera is independently implemented, and takes the most "native" approach for the targeted infrastructure. This includes support for both cloud and on-premises environments. Initially we will provide support for GCP and AWS. Additionally, Tessera will offer cloud-agnostic support for MySQL and POSIX file systems, making it cloud-agnostic and scalable. This allows Tessera to accommodate a range of transparency log use-cases, from high-performance, high-uptime logs like Certificate Transparency (CT) and Sigstore, to those with more modest requirements, such as Firmware Transparency.
    
* **Fast Sequencing and integration of Entries** - In Trillian, entries were added to a log via a fully decoupled queue, providing a timestamp and a future promise of integration. With Trillian Tessera, we have refined the storage contract so that calls to add entries to the log will return with a durably assigned sequence number or an error, with integration occurring within seconds. This separation of sequencing and integration allows for higher write throughput and enables applications to be built with better practices, such as offering offline-proof bundles.
    

### Trillian Tessera Status

Trillian Tessera is currently under active development, and is not yet ready for general use. However, early feedback is very welcome. We expect an alpha release by Q4 2024, and production ready anticipated in the first half of 2025. We are also developing a [CT static API](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md) compatible server built with Tessera, which is expected to be released on a similar timeline. Trillian v1 is still in use in production environments by multiple organisations in multiple ecosystems, and is likely to remain so for the mid-term. New ecosystems, or existing ecosystems looking to evolve, should strongly consider planning a migration to Tessera and adopting the patterns it encourages.

### Get Started

You can learn more about Trillian Tessera and try it out here: [https://github.com/transparency-dev/trillian-tessera](https://github.com/transparency-dev/trillian-tessera)