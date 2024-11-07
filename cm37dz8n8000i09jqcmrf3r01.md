---
title: "Can I Get A Witness (Network)?"
seoTitle: "Can I Get A Witness (Network)?"
seoDescription: "Bootstrapping a Global Witness Network with the ArmoredWitness"
datePublished: Thu Nov 07 2024 14:11:39 GMT+0000 (Coordinated Universal Time)
cuid: cm37dz8n8000i09jqcmrf3r01
slug: can-i-get-a-witness-network
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730905469011/25ee1237-a090-412e-9706-6cbadeed99a9.jpeg
tags: sigstore, transparencydev, armoredwitness

---

While transparency logs are recognized as providing the [highest standards for trust in the industry](https://blog.transparency.dev/transparencydev-summit-recap), this only remains true if transparency systems deliver on their promise of providing a verifiable, globally consistent, append-only list of data.

Breaking down these properties:

* **Verifiable**: you can cryptographically prove that the data hasn’t been unexpectedly changed
    
* **Globally consistent**: whoever comes to the log, wherever they are in the world, and whoever they happen to be, sees the same list of the data in the log
    
* **Append-only**: once data is added to the log it can neither be deleted nor modified; new data can only be added to the end of the log
    

So how do we ensure that these properties are being met in a transparency system? How do we make sure our verifiable systems are actually *verified?*

## Why Witnessing Matters

This is the exact problem that witnesses were invented to solve. A witness is an entity that implements a [gossip protocol](https://arxiv.org/pdf/2011.04551) to effectively verify that logs are **append-only**, ensuring their integrity and preventing unauthorized modifications. When operating in coordination with other such entities, witnesses help safeguard against split view attacks by maintaining **global consistency** for all logs. This collaborative approach ensures that all log users have a unified, tamper-resistant view of the log’s history.

## How do Witnesses Work?

A witness cosigns checkpoints consistent with those it previously signed. A witness gets a [checkpoint](https://github.com/C2SP/C2SP/blob/main/tlog-checkpoint.md) from a transparency log, and signs it only after verifying the consistency proof, ensuring the checkpoint is consistent with any previous checkpoints it has signed. By doing so it asserts the append-only security property of the log. Only a single witness is required to prove a log is append-only.

A set of witnesses (ideally an odd number) can be used to detect split view attacks and help reliably prove that a log is globally consistent. The more witnesses you can trust who have seen the same thing, the more you can trust that the view of the log. Strengthen that further with a finite pool of witnesses and then using a quorum technique N of M to guarantee that the log is globally consistent.

It is important to note that witnesses are not concerned with the contents of the log's leaves. For example for a firmware transparency log, a witness would not be able to validate legit firmware was logged. Verifiers such as auditors or monitors are used to ensure log contents are legitimate. Such verifiers are bespoke to a specific ecosystem, unlike witnesses which are designed to witness almost all logs. Witnesses underpin these verifiers to ensure that the same view of the contents is presented to every log user.

## Evolution of the Witness Network

### OmniWitness

The first significant implementation of a witness network was the [OmniWitness](https://github.com/transparency-dev/witness/tree/main/cmd/omniwitness) network. Developed by the TrustFabric team, the OmniWitness network monitored several logs that use the [generic checkpoint format](https://github.com/transparency-dev/formats/tree/main/log). These logs include:

* Go SumDB
    
* Sigsum
    
* Sigstore
    
* Android Binary Transparency
    
* LVFS
    
* Armored Witness
    

A number of participants helped run the OmniWitness network and get it working. However one significant problem that arose was effectively handling policy updates (e.g. add a new log to the monitor list) and software updates, to ensure they were more deterministic. Nevertheless it was an invaluable way to kick start the concept.

### ArmoredWitness

To create a more scalable and viable witness network, [ArmoredWitness](https://github.com/transparency-dev/armored-witness) introduces a specialized, lightweight, plug and play solution that custodians around the world can easily install, verify and maintain. This iteration builds on the strengths of the OmniWitness software, integrating it with a custom, open source hardware chip and tailored packaging to streamline deployment.

The ArmoredWitness was engineered to be fully transparent, open to inspection and verification. Key features include:

* Open source hardware and software
    
* Reproducible builds with a firmware transparency log: updates are easily managed and traceable, establishing clear, auditable provenance.
    
* Full boot-chain verification from mask ROM to running software
    

Currently, 15 ArmoredWitness devices are deployed with trusted custodians, collaboratively bootstrapping a diverse, tamper-resistant witness network. This network not only offers greater resiliency and minimizes risk of team coercion, but also serves as a living example of fully-end-to-end firmware transparency in action.

## Looking Forward: The Future of Witness Networks

As the adoption of transparency logs grows, implementing a comprehensive set of transparency roles to uphold core transparency principles is more important than ever. Witnessing plays a key role in this ecosystem, underpinning the append-only, globally consistent and verifiable properties. The ArmoredWitness network advances the field by offering the first sustainable, operational network capable of supporting multiple logs, regardless of payload.

Looking forward, recent community discussions focussed on a number of areas including:

* Driving adoption of the ArmoredWitness network
    
* Addressing scalability challenges such as configuration and format standardization
    
* Establishing robust policy frameworks 
    
* Increasing number of witnesses as well as reputational units in the ecosystem. (TrustFabric, as the operators of the ArmoredWitness, are the reputational unit behind the current network.) The more independent reputational units running witness networks, the more the ecosystem is strengthened.
    

By tackling these challenges, we can build towards the goal of ensuring resilient, scalable witness networks that reinforce transparency and trust across log ecosystems.

*This blog post is based on the talk ‘*[*ArmoredWitness, putting verifiable transparency in your verifiable transparency*](https://youtu.be/v9cgvZXRRZU?si=ACAVHUffrKNyLPVQ)*’ by Martin Hutchinson.*

%\[[https://youtu.be/v9cgvZXRRZU?si=xwylnSjCsaVA6hiA](https://youtu.be/v9cgvZXRRZU?si=xwylnSjCsaVA6hiA)