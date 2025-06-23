---
title: "Tessera: Bigger and Beta"
datePublished: Thu Jun 12 2025 10:12:33 GMT+0000 (Coordinated Universal Time)
cuid: cmbt7zlp0000702l44gu54dzz
slug: tessera-bigger-and-beta
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/gREi-9tI5Mg/upload/de9cd6639bc0bff4e90f1956faa2beee.jpeg
tags: tessera, trillian

---

Tessera Beta is here!

[Tessera](https://github.com/transparency-dev/tessera) is a [tile-based transparency log](https://transparency.dev/articles/tile-based-logs/) library designed to improve the experience of log-operators running logs in any transparency space. After [announcing](https://blog.transparency.dev/announcing-the-alpha-release-of-trillian-tessera) the alpha release of Trillian Tessera late last year, we are excited to present the latest development milestone - the beta release of Tessera - which takes us one step closer to that goal!

## **From Alpha to Beta: What’s Changed?**

As with any beta release, the objective was to produce a version of Tessera ready for external users to test and provide feedback on, so that we can go on to produce a production-ready version of Tessera.  With that in mind, the [changes](https://github.com/transparency-dev/tessera/releases) between the alpha and beta releases improve the usability of the API, the stability of the codebase, alongside adding a few new features and tools. Highlights include:

* **Snappier name:** We’ve dropped the ‘Trillian’ prefix, it’s now simply ‘Tessera’. Although inspired by lessons learned from running Trillian, Tessera is a project in its own right, now quite different from Trillian.  It’s time for it to leave the Trillian nest and independently find its own way in the world. Plus, it’s shorter, and doesn't contain a hyphen, so less typing, and no aliasing required when importing the Go package.
    
* **API Improvements:** The Tessera API is more intuitive and streamlined, with types and functions that are unnecessary to expose to users of the codebase hidden away.
    
* **Expanded anti-spam support:** Anti-spam support for AWS and POSIX has been added alongside the previously existing GCP support.
    
* **New tools:** We’ve added an `fsck` tool for checking the self-consistency and integrity of tlog-tiles logs via HTTP. Additionally, an experimental mirroring tool has been introduced, initially supporting local filesystem destinations.
    
* **Cleaner docs:** Documentation improvements and updates ensure clarity and accuracy for users.
    
* **Bug fixes:** We’ve addressed a number of bugs, including fixes for missing errors, inconsistencies in function names, and issues with checkpoint publishing in GCP and AWS.
    

## **It’s Testing Time**

The latest improvements to Tessera refine the developer experience to the point that it is now ready for beta-level testing.  So far, we’ve validated the functionality of Tessera by successfully running a log on GCP, scaling to over 2 billion entries.  In parallel, we’ve also run a log on AWS, growing it to 800 million entries, demonstrating reliable performance across both platforms.

We invite all existing log operators, along with anyone else with an interest in improving and supporting transparency ecosystems, to try spinning up a log using the beta version of Tessera, and let us know how it goes!  Feedback on your experience is invaluable, as hearing from external users about how Tessera fares when used for real applications helps us ensure that the upcoming production release is stable and meets your needs and expectations.

## **How to Get Involved**

Tessera is designed to make it easy to build and deploy new transparency logs on supported infrastructure. To get started, check out the [Codelab](https://github.com/transparency-dev/tessera/tree/main/cmd/conformance#codelab), which provides a simple step-by-step guide to set up your first log, write a few entries to it, and then inspect its contents.

To start experimenting with Tessera you can choose one of these example personalities:

* [POSIX](https://github.com/transparency-dev/tessera/tree/main/cmd/conformance/posix#bring-up-a-log): example of operating a log backed by a local filesystem. This example runs an HTTP web server that takes arbitrary data and adds it to a file-based log.
    
* [MySQL](https://github.com/transparency-dev/tessera/tree/main/cmd/conformance/mysql#bring-up-a-log): example of operating a log that uses MySQL. This example is most easily deployed via docker compose, allowing for easy setup and teardown
    
* [GCP](https://github.com/transparency-dev/tessera/tree/main/deployment/live/gcp/conformance#manual-deployment): example of operating a log running on GCP. This example can be deployed via terraform.
    
* [AWS](https://github.com/transparency-dev/tessera/tree/main/deployment/live/aws/codelab): example of operating a log running on AWS. This example can be deployed via terraform.
    

In addition, keep an eye on the [Getting Started](https://github.com/transparency-dev/tessera/tree/main?tab=readme-ov-file#getting-started) section in the [Tessera README](https://github.com/transparency-dev/tessera/tree/main?tab=readme-ov-file#tessera) for future example personalities.

For any questions or guidance on working with Tessera please reach out to the team by posting questions on the [transparency.dev community slack](https://join.slack.com/t/transparency-dev/shared_invite/zt-2jt6643n4-I5wLUo90_tvTVd4nfmfDug). We can’t wait to hear what you think!