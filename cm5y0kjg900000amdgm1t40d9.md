---
title: "What 2025 Holds for Certificate Transparency and the Transparency.dev Ecosystem"
datePublished: Wed Jan 15 2025 14:45:30 GMT+0000 (Coordinated Universal Time)
cuid: cm5y0kjg900000amdgm1t40d9
slug: what-2025-holds-for-certificate-transparency-and-the-transparencydev-ecosystem
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/v8aLw1zBGdM/upload/816e0473f4acee207427c232737847f0.jpeg
tags: sigstore, certificate-transparency, transparencydev, sigsum

---

Certificate Transparency (CT) has been a cornerstone of internet security for over 11 years. The CT ecosystem has caught tens of thousands of certificate misissuances, numerous backdated certificates, and countless baseline requirement violations. By enabling timely corrective action where necessary, CT has played a critical role in safeguarding the integrity and reliability of the TLS ecosystem. It’s no wonder, then, that in 2024 [CT won the prestigious Levchin Prize for Real-World Cryptography](https://blog.transparency.dev/certificate-transparency-wins-the-levchin-prize), underlining the profound impact CT is making on securing the digital world.

As we step into 2025, the rapid evolution of the digital landscape means security concerns continue to be of paramount importance. The [Transparency.dev](https://transparency.dev/) ecosystem has emerged as a pivotal community for fostering innovation in CT and beyond. This post takes a look at some key developments you can expect to see take place in 2025.

# Tile-based Logs are the Future

After years of working with transparency logs at scale, the transparency.dev community has found there are better ways to make logs more scalable and cheaper to operate. A new approach ‘[Tile-based Transparency Logs](https://transparency.dev/articles/tile-based-logs/)’, built on the concept of exposing storage as “tiles”, has already been tested and proven in several ecosystems since 2021. 2024 was a transformative year for tile-based logs, beginning with the [release of the Sunlight log by Let’s Encrypt](https://letsencrypt.org/2024/03/14/introducing-sunlight/). This was followed by a community developed standardized API for CT tile-based logs known as the [Static-CT API](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md), which rapidly became the foundation for new and experimental log implementations (e.g. [itko](https://groups.google.com/a/chromium.org/g/ct-policy/c/-P0rGATLQFA/m/w6hMxgn5AAAJ)). The year concluded with the [alpha release of Tessera](https://blog.transparency.dev/announcing-the-alpha-release-of-trillian-tessera), a general-purpose tile-based log developed by the creators of Trillian, further cementing the viability of this innovative approach.

The Chrome CT team have already [shared plans](https://groups.google.com/a/chromium.org/g/ct-policy/c/W7OSO3SbrFo/m/S2XyhXx_AAAJ) to start accepting tile-based Static-CT API logs into the ecosystem, signalling a significant evolution in Certificate Transparency. Introducing major changes to a mature ecosystem is a huge undertaking and can often be highly disruptive. For context, [RFC 9162](https://datatracker.ietf.org/doc/html/rfc9162) was developed as an improved version of [RFC 6962](https://www.rfc-editor.org/rfc/rfc6962) but was never widely adopted as the incremental benefits did not justify the cost and complexity of implementation. This highlights the importance of tile-based logs, as their adoption demonstrates the CT ecosystem’s willingness to evolve when the benefits are compelling and transformative.

# Cloud Native CT Logs

While next-generation transparency logs emphasize their tile-based architecture and API, it is equally important to highlight their cloud native approach to storage management. By natively supporting cloud object storage, these logs unlock significant advantages such as operational simplicity, high throughput and the ability to dynamically scale based on usage. Sunlight, for instance, supports S3-compatible object stores, which [Let’s Encrypt leverages to reduce costs and scale up easily](https://youtu.be/QZnz8pvsN_w?si=guO0ME8yyTI9IEkw). In the case of Trillian Tessera, each storage backend is [independently implemented](https://github.com/transparency-dev/trillian-tessera/blob/main/docs/design/philosophy.md#multi-implementation-storage) and takes the most native approach for the infrastructure it targets. The Tessera alpha includes support for object storage on both GCP and AWS, showcasing its commitment to leveraging cloud native principles. By embracing cloud object storage, next-generation transparency logs reduce the complexities associated with traditional storage solutions. This shift enables organizations to benefit from high availability, dynamic scaling, and simplified maintenance, paving the way for more efficient and resilient transparency logging systems.

# The Growth of Transparency Ecosystems

The CT ecosystem has inspired several other transparency-based ecosystems such as [Sigsum for transparent key-usage](https://youtu.be/Mp23yQxYm2c?si=-FGc4YJ-6f-y8FWD) and [Key Transparency in Proton Mail](https://youtu.be/shUwnSFtP8g?si=zlnmOvozXpdkFBbR),

In software supply chain security, [Sigstore](https://www.sigstore.dev/) uses transparency logs to make it easer for developers to sign software and for users to verify its authenticity. In 2024, Sigstore saw significant adoption into popular OSS packaging ecosystems such as [Python](https://youtu.be/ZVC9dwmqX2s?si=ygAh_eA1SVpT-zmp) and [Homebrew](https://blog.sigstore.dev/homebrew-build-provenance/), demonstrating its practical application and broad impact. Python is moving towards [using Sigstore exclusively](https://peps.python.org/pep-0761/) for signing release artifacts, while Homebrew leverages Sigstore to provide verifiable provenance for its binary packages. Sigstore has evolved into being *critical internet infrastructure* that is relied upon by GitHub, GitLab, and other major software organizations across the planet. Looking ahead to 2025, [Sigstore plan to transition to tile-based logs](https://youtu.be/sv-1ue-nAfo?si=uWEnsmIkrqTQqTy4) to take advantage of their lower operational costs and improved scalability.

In general as logs become easier to set up, maintain and operate, expect more industries to take advantage of transparency technologies. This trend could extend beyond traditional applications to emerging areas such as [AI transparency (transparency in machine level models)](https://youtu.be/QHOzEkw_9d4?si=npi4uw5cOwbXg69b) and [enhancing trust in digital identity ecosystems](https://youtu.be/6cNQTMQ5Qgg?si=KecVAmRp5dUhZz58).

# The Rise of Standards and Interoperability

With the rise of transparency adoption, there has been a parallel evolution of protocols and formats aimed at standardizing transparency systems. This development has led to the creation of more general-purpose and reusable building blocks. These evolving specifications, shaped through implementation, experimentation, and collaboration, have been consolidated in the [The Community Cryptography Specification Project (C2SP) repository](https://github.com/C2SP/C2SP). Key specifications include:

* **Generic Checkpoint** - a spec for a [signed note](https://c2sp.org/signed-note) where the body is precisely formatted for use in transparency log applications
    
* **Generic Tiles** - a [spec](https://github.com/C2SP/C2SP/blob/main/tlog-tiles.md) for an efficient HTTP API to fetch the signed checkpoint, Merkle Tree hashes, and entries of a transparency log containing arbitrary entries.
    
* **Generic Witnessing** - a spec for a synchronous HTTP-based protocol to obtain [cosignatures](https://c2sp.org/tlog-cosignature) from transparency log witnesses.
    

These specifications significantly improve interoperability not only within Certificate Transparency (CT) but also across different transparency ecosystems. This growing standardization enables the creation of shared horizontal services that can be utilized across various domains. A prime example of such a service is witnessing. Witnessing plays a crucial role in maintaining the append-only, globally consistent, and verifiable properties of transparency logs. The [ArmoredWitness network](https://youtu.be/v9cgvZXRRZU) made a significant advancement in this area by launching the first sustainable, operational network capable of supporting multiple logs, regardless of their payloads and will continue to grow and evolve in 2025.

# The Road Ahead

The future of Certificate Transparency and the Transparency.dev ecosystem is bright. As more organizations recognize the importance of transparency in securing digital communications, the demand for robust CT solutions will only grow. While there is much to look forward to in 2025, several challenges remain, not least the looming [challenge of adoption post quantum cryptography](https://youtu.be/s0HQCx4wy44?si=PD5PSUDQt3SjRTOO).

The Transparency.dev community will continue to play a pivotal role in shaping this future. By fostering collaboration, sharing knowledge, and building innovative tools, the ecosystem will empower security professionals and developers to make the digital worlds a safer place for everyone. Join us as we build a more transparent, accountable, and secure digital world!

* Join the transparency-dev [Slack](https://transparency.dev/slack/)
    
* Follow the [**Transparency-dev Community Blog**](https://blog.transparency.dev/)
    
* Learn more about the [Transparency.dev Roadmap](https://github.com/transparency-dev)