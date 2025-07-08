---
title: "Announcing the Alpha release of Trillian Tessera"
seoTitle: "Announcing the Alpha release of Trillian Tessera"
datePublished: Thu Dec 05 2024 18:23:57 GMT+0000 (Coordinated Universal Time)
cuid: cm4bnbjp7000709me5uymhjb0
slug: announcing-the-alpha-release-of-trillian-tessera
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/jR4Zf-riEjI/upload/4a1b22c3886bbf4bce6da5cca040eb5d.jpeg
tags: tessera, trillian

---

We are thrilled to announce the alpha release of Trillian Tessera, the tile-based transparency log designed to improve the lives of log operators. Whether you’re working with Certificate Transparency, Binary Transparency, Firmware Transparency or any transparency log ecosystem, Tessera brings a new level of performance and flexibility to verifiable log ecosystems. A few weeks ago we first [introduced Trillian Tessera](https://blog.transparency.dev/introducing-trillian-tessera) and shared its design philosophy. Since then, we’ve been hard at work, and today we’re excited to share the alpha version.

### Key Features

1. **Tiles API -** Trillian v1 uses tiles in [its internal storage](https://github.com/google/trillian/blob/master/docs/storage/storage.md), but does not expose them via its API and instead serves pre-built proofs. Trillian Tessera fully embraces the tiles concept by introducing a [Tiles API](https://github.com/C2SP/C2SP/blob/main/tlog-tiles.md), allowing clients direct access to subtree tiles from the log. This API unlocks the full potential of tile-based architecture and has [many benefits](https://transparency.dev/articles/tile-based-logs/) including improved cacheability, and making logs easier to spin up and maintain. Ultimately, the tiled log architecture and API significantly reduce the total cost of operation compared with Trillian v1.
    
2. **Multi-environment Support**: Trillian Tessera has native implementations for on-premises and cloud environments. Initial support is provided for running with AWS, Google Cloud Platform (GCP), POSIX-compliant file systems, or MySQL databases. Tessera takes the most native approach for infrastructure it targets, with each [storage implementation implemented independently](https://github.com/transparency-dev/trillian-tessera/blob/main/docs/design/philosophy.md#multi-implementation-storage). This enables a new level of performance and flexibility that ensures that no matter your infrastructure, Tessera can be deployed and adapted to a wide range of use cases.
    
3. **High Write Throughput:** Tessera was built with speed in mind. The [refined design](https://github.com/transparency-dev/trillian-tessera/blob/main/docs/design/philosophy.md#asynchronous-integration-in-storage-implementation) separates sequencing and integration of entries to allow for higher write throughput and enables applications to be built with better practices, such as offering offline-proof bundles.
    
4. **Bring-Your-Own Log Personality**: Trillian Tessera enables building of customizable log personalities, including full support for the unique requirements of [static CT API](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md)\-compliant logs.
    
5. **Resilience and Availability**: Tessera’s design allows for multiple write frontends to run concurrently on the same storage, ensuring [high availability and resilience](https://github.com/transparency-dev/trillian-tessera/blob/main/docs/design/philosophy.md#resilience-and-availability), and reads to be served directly by storage infrastructure. This setup provides safety measures against accidental misconfigurations, such as launching multiple instances with the same storage settings, offering greater reliability compared to Trillian v1 logs on similar infrastructure.
    

### Tessera Performance

Tessera is designed to scale to meet the needs of most currently envisioned workloads in a cost-effective manner. All storage backends have been tested to meet the write-throughput of CT-scale loads without issue. The read API of Tessera based logs scales extremely well due to the immutable resource based approach used, which allows for:

* Aggressive caching to be applied, e.g. via CDN
    
* Horizontal scaling of read infrastructure (e.g. object storage)
    

Exact performance numbers are highly dependent on the exact infrastructure being used (e.g. storage type). During our alpha testing phase, we ran extensive benchmarks using [Hammer](https://github.com/transparency-dev/trillian-tessera/tree/833a33cb4f7ce7144adf43c72b66af2b27597b7e/internal/hammer), our load testing tool for Tessera logs. This was done to ensure Tessera could handle the demands of high-throughput environments, while ensuring the logs were behaving correctly.

We have captured a snapshot of performance metrics for Trillian Tessera storage implementations under different conditions. Additionally, we documented the methodology for users to recreate these experiments independently. For further details, refer to [Tessera Storage Performance](https://github.com/transparency-dev/trillian-tessera/blob/main/docs/performance.md).

### Get Started

Trillian Tessera is a Go library designed to make it easy to build and deploy new transparency logs on supported infrastructure. To get started, check out the accompanying [Conformance Codelab](https://github.com/transparency-dev/trillian-tessera/tree/main/cmd/conformance#conformance-personalities), which provides a step-by-step guide for setting up your first log, writing some entries to it using HTTP, and inspecting its contents with ease.

We’ve made it easy to start experimenting with Tessera by providing these example personalities:

1. [POSIX](https://github.com/transparency-dev/trillian-tessera/blob/main/cmd/conformance/posix): example of operating a log backed by a local filesystem. This example runs an HTTP web server that takes arbitrary data and adds it to a file-based log.
    
2. [MySQL](https://github.com/transparency-dev/trillian-tessera/blob/main/cmd/conformance/mysql): example of operating a log that uses MySQL. This example is easiest deployed via docker compose, which allows for easy setup and teardown
    
3. [AWS:](https://github.com/transparency-dev/trillian-tessera/blob/main/cmd/conformance/aws) example of operating a log running on AWS. This example can be deployed via terraform, see the [deployment instructions](https://github.com/transparency-dev/trillian-tessera/blob/main/deployment/live/aws/codelab#aws-codelab-deployment).
    
4. [GCP:](https://github.com/transparency-dev/trillian-tessera/blob/main/deployment/live/aws/codelab#aws-codelab-deployment) example of operating a log running in GCP. This example can be deployed via terraform, see the [deployment instructions](https://github.com/transparency-dev/trillian-tessera/tree/main/deployment/live/gcp/conformance#manual-deployment).
    

For more information on getting started and to stay updated with future examples, visit the  [Getting Started](https://github.com/transparency-dev/trillian-tessera/tree/main?tab=readme-ov-file#getting-started) section in the Tessera README

### Get Involved in the Alpha

As we continue to refine and enhance Trillian Tessera, we invite developers and organizations to start using the alpha. Your feedback on Tessera’s performance and usability is crucial as we work towards a full release. By trying out the alpha, you’ll have the opportunity to influence the direction of development and ensure it meets the needs of the community. We also welcome contributors - check out our [contributing guide](https://github.com/transparency-dev/trillian-tessera/blob/main/CONTRIBUTING.md) for more details.

### What’s Next?

Looking forward, we have ambitious plans for Trillian Tessera. Future plans will focus on adding features that enhance usability and integration with other tools, especially based on feedback we hear from our early adopters. We are developing a [static CT API](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md) compatible personality built with Tessera, anticipated in early 2025. Additionally, we are committed to ensuring that existing Trillian users have a smooth migration path to Trillian Tessera logs.

Stay tuned for updates, and if you’re interested in getting involved check out the [Tessera Github repository](https://github.com/transparency-dev/trillian-tessera) and sign up to the [Transparency.dev Slack](https://transparency.dev/slack/).

This release marks an important milestone for the Transparency.dev community as tiled-logs become available for any transparency ecosystem. Thank you for your support, and we can’t wait to see what you build with Trillian Tessera!