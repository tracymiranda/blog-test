---
title: "I Built a New Certificate Transparency Log in 2024 - Here’s What I Learned"
datePublished: Thu Jan 23 2025 15:54:11 GMT+0000 (Coordinated Universal Time)
cuid: cm69ijox6000b09js9rb2dcoi
slug: i-built-a-new-certificate-transparency-log-in-2024-heres-what-i-learned
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/lgbbUZ7VHJI/upload/8a97fd16c87082eda1bf29ef0b052ff3.jpeg
tags: sunlight, certificate-transparency, tessera, trillian

---

Certificate transparency (CT) is such a useful research tool and I’d been wanting to learn more about it for a while. After [Sunlight](https://sunlight.dev) was announced last year, I decided the best way to learn was to write a CT log, and set off on quite the adventure.

The CT ecosystem is evolving. The original specification, [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962), was published in June 2013. Subsequent projects such as Go’s SumDB extended the application of transparency logs to other places. The [Static CT API](https://c2sp.org/static-ct-api) takes the lessons learned from these projects and applies them back to CT, moving some expensive operations from server-side to client-side. This change drastically simplifies how CT logs function and makes them much cheaper to operate.

To better appreciate the impact of these changes, my CT log implementation, [Itko](https://github.com/aditsachde/itko), supports both RFC6962 and the Static CT API. Itko also has an [operational instance](https://github.com/aditsachde/itko?tab=readme-ov-file#public-instance) which is available for querying and testing against.

# **Lessons learned**

The Static CT API is a variant of the more generic [tiled transparency log specification](https://c2sp.org/tlog-tiles), a specification you can use to bring transparency to your own ecosystem – here are some of my takeaways which may be useful for you.

## *Community*

First, the most important takeaway for me from this project is how welcoming and helpful the transparency community is. I would not have been able to build Itko without them. The [Transparency.dev Slack](https://join.slack.com/t/transparency-dev/shared_invite/zt-27pkqo21d-okUFhur7YZ0rFoJVIOPznQ) and the [CT policy mailing list](https://groups.google.com/a/chromium.org/g/ct-policy) are great places to learn, ask questions, and follow along on development.

## *Tooling*

In 2024, the ecosystem was in a bit of a scattered state. The library and tools exist, but they are spread over a number of packages and projects. Itko uses components from Go’s SumDB, Trillian, and certificate-transparency-go.

Changing this is a focus for the ecosystem going into 2025, and large strides have already been made. [Trillian Tessera](https://github.com/transparency-dev/trillian-tessera) is the focal point of this effort to make tiled transparency logs usable by all. Currently out in [alpha](https://blog.transparency.dev/announcing-the-alpha-release-of-trillian-tessera), Trillian Tessera bundles all the common functionality needed by tiled transparency logs, allowing implementers to focus on the bits unique to their ecosystem.

## *Operational Simplicity*

Tiled logs are operationally awesome. The only component in the read path is a filesystem, no database needed! And, static files means most requests hit the page cache, not disk. Itko ingests two million certificates and serves millions of requests per day on a quarter of a cpu core, because there is no database involved.

However, write path uptime is at odds with this simplicity. A transparency log needs all its entries to have a unique and ordered sequence number. This introduces a global counter that must be coordinated between nodes in a distributed system. There are a variety of approaches to tackle this problem, but with Itko, I instead chose to not tackle it.

The CT ecosystem baselines 99% uptime due to the presence of multiple redundant logs. Itko takes advantage of this by handling all write operations in a single process, with additional processes in standby mode ready to take over if a short period of unavailability is detected. Instead of targeting the highest uptime possible,  a careful consideration of ecosystem requirements can allow logs to remove a lot of complexity and potential failure modes.

## *Unexpected visitors*

Transparency logs are a great investigative research tool, which means quite a few folks monitor them. The Itko 2025 shard was meant to be a demo and is not considered to be a trusted log by Chrome or Safari. But surprisingly, the log was quickly serving more than 300gb of traffic a day as multiple people were monitoring it!

The number of requests your log receives will have an impact on the most economical choice of storage. Tiled logs fit well with object storage, with the benefit of never having to worry about downtime. However, object storage has a cost on every read and write request, which can be cost prohibitive for public good services like transparency logs. For Itko, I found a shared persistent disk to be a more economical choice, one that also resulted in better performance due to lower write latencies.

## *Testing*

Having good test coverage is always useful, but having a comprehensive test suite is especially important when the servers and clients are written by different people.

When writing Itko, my first step was to write integration tests, adapting the test cases and the [CT hammer tool](https://github.com/google/certificate-transparency-go/blob/master/trillian/integration/ct_hammer/main.go) from certificate-transparency-go. Then, I wrote the actual implementation, which allowed for a high degree of confidence in spec compliance. And it totally paid off. After the log passed the test cases, it worked with other OSS tools on the first try.

## *Just do it!*

I had zero understanding of how transparency logs worked when I first started. Over the course of about 40 extremely scattered hours of work, I was able to get Itko to pass its spec compliance test cases. The trust guarantees that transparency logs provide make them seem a bit magical. But today, they are not very complicated to implement. Join the Slack, take a look at Trillian Tessera, and just dive in!

### **Resources:**

* [Itko Repository and Development Instance](https://github.com/aditsachde/itko)
    
* [Join the Slack!](https://transparency-dev.slack.com/join/shared_invite/zt-2hw3qg8w6-FTaCast0CflW~EuaDTpLwA#/shared-invite/email)
    
* [Transparency.dev Roadmap](https://github.com/transparency-dev)