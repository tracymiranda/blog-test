---
title: "Introducing TesseraCT"
datePublished: Thu Aug 14 2025 17:01:51 GMT+0000 (Coordinated Universal Time)
cuid: cmebncmxr000102l56n9l3vkf
slug: introducing-tesseract
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cZx4tDpIwPA/upload/00a72c437c168770e239693a08a729c8.jpeg
tags: tesseract, certificate-transparency, tessera

---

We’re excited to introduce [TesseraCT](https://github.com/transparency-dev/tesseract), a new tile-based Certificate Transparency (CT) log implementation built on [Tessera](https://blog.transparency.dev/tessera-bigger-and-beta). TesseraCT is designed to make it easier and more-cost effective to operate CT logs at scale, while maintaining strong performance and reliability.

At this stage we are releasing the alpha of TesseraCT, ready for early testing and use by others.

## What is TesseraCT?

TesseraCT is a CT server that implements the [Static CT API](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md). It is built on top of [Tessera](https://github.com/transparency-dev/tessera), our Go library for tile-based transparency logs.

Historically, operating CT logs has been complex and costly, but TesseraCT changes that by providing a modern CT implementation that incorporates lessons learned from operating real-world logs at scale.

### Key features

* **Tile-based architecture and API**: Enables efficient reads and writes, following the [Static CT API C2SP specification](https://c2sp.org/static-ct-api).
    
* **Cloud-native & on-prem**: Supports AWS, Google Cloud, POSIX filesystems, or vanilla S3/MySQL storage systems.
    
* **High uptime**: Engineered to offer high availability, as required by user agents. While TesseraCT can run as a single instance, it will also safely run with multiple concurrent instances for higher availability.
    
* **Low operational overhead**: Fewer moving parts and easier setup than [Trillian](https://github.com/google/trillian)+[CTFE](https://github.com/google/certificate-transparency-go). At its simplest, TesseraCT requires only a single server rather than three, and is simpler to configure.
    
* **Future-proof**: Designed for the highest level of availability, and to scale easily as certificate lifetimes get shorter and the volume of certificates issued increases.
    
* **Fast merging:** Entries are integrated into the log within seconds, with the option to wait for submissions to be fully integrated before client requests return.
    

### Where can it run?

TesseraCT can be run on Google Cloud Platform (GCP), Amazon Web Services (AWS), POSIX filesystems, or using vanilla S3+MySQL storage systems. We have tested each of these implementations using various deployment setups, and have confirmed that they are able to handle the current certificate submission rate seen by existing logs, of roughly [300](https://ct.cloudflare.com/) certificates per second. At the higher end, our staging GCP logs happily accept up to 1.7k QPS, and a local POSIX deployment was tested up to 10k QPS! More details and reproducible [performance tests](https://github.com/transparency-dev/tesseract/blob/main/docs/performance.md) are available in the TesseraCT repo.

We recommend you choose which platform to use with TesseraCT based on your existing production environment:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755189703774/40f3fb48-5900-4275-975f-3f32a56e1574.png align="center")

## Tile-Based Logs

[Tile-based logs](https://transparency.dev/articles/tile-based-logs/) became popular in the CT world due to the caching benefits they provide, making read requests to the log simpler and cheaper to respond to. Even [Trillian](https://github.com/google/trillian), used by our [existing RFC 6962 CT log implementation](https://github.com/google/certificate-transparency-go), relies on a tiled architecture under the hood. However, TesseraCT directly exposes [C2SP](https://c2sp.org/) compliant tiles through the [static CT API](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md).

In contrast to the microservices-based approach of Trillian and the CTFE, TesseraCT offers a fresh architecture with a flexible, and simplified deployment model. This is a transformative step for CT log infrastructure, for existing and new log operators.

TesseraCT builds on the heritage of Trillian+CTFE, and was designed after much discussion with current CT log operators. It is just one of a growing number of CT log implementations that conform to the static CT API specification, including the trailblazing [Sunlight](https://sunlight.dev/), [Azul](https://github.com/cloudflare/azul), [Itko](https://github.com/aditsachde/itko) and [CompactLog](https://github.com/Barre/compact_log). Having multiple log implementations provides diversity and resilience, and makes it possible to run static CT API logs on a variety of platforms, resulting in a healthier Certificate Transparency ecosystem overall.

## Try out TesseraCT!

We’ve launched the alpha release of TesseraCT so that developers, log operators, and the CT ecosystem can test it as early as possible and provide feedback to be incorporated into future releases.

There are three TesseraCT [staging logs](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#test_tube-public-test-instances) available for testing:

* **arche2025h1** at https://arche2025h1.staging.ct.transparency.dev
    
* **arche2025h2** at https://arche2025h2.staging.ct.transparency.dev
    
* **arche2026h1** at https://arche2026h1.staging.ct.transparency.dev
    

**CT log operators**: Try out the [Arche logs](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#test_tube-public-test-instances), or run your own! See our [Getting Started](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#getting-started) guide and [architecture overview](https://github.com/transparency-dev/tesseract/blob/main/docs/architecture.md) for guidance on how. Give TesseraCT a try and help us answer the most important question: does this reduce your pain?

**CT monitors**: The [Arche logs](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#test_tube-public-test-instances) are now available for monitoring. Let us know if you run into any problems querying the logs.

**Certificate Authorities**: Submit certificates to our [Arche logs](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#test_tube-public-test-instances).

We’re excited to share this milestone and we welcome your [feedback](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#wave-contact). TesseraCT is about making it less painful to operate transparency infrastructure, so try the alpha and tell us what hurts. Let’s make the next generation of transparency logs better, together.

Keep an eye on the [TesseraCT repo](https://github.com/transparency-dev/tesseract) to follow along, and reach out with any questions or feedback on the [transparency.dev slack](https://transparency.dev/slack/). We look forward to hearing from you!

## What’s Next?

We are continuing to push TesseraCT's development towards the goal of production tile-based CT logs. Future versions of TesseraCT will include additional features such as [witnessing](https://github.com/C2SP/C2SP/blob/main/tlog-witness.md) and a root update process integrated with [CCADB](https://www.ccadb.org/). Check out our [roadmap](https://github.com/transparency-dev/tesseract?tab=readme-ov-file#motorway-roadmap) for more information.