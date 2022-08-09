# RFP-014: Private retrieval of data

## Background

### Motivation
The options available today for interactive (low latency) communication with privacy guarantees are very limited. Solutions developed to date have focused on the traditional web model of a single trusted origin publisher for data (as opposed to content-addressed systems where data may be replicated in advance), and have drawbacks both in their incurred latency and threat model.

We are soliciting proposals to explore and develop viable mechanisms for reader-private communications under the hypothesis that it is possible to design a scalable system that does not sacrifice latency for privacy.

### Existing Approaches
The most widely deployed approaches to reader privacy rely on mixnet and mixnet-like multi-hop systems ([Nym](https://nymtech.net/), [Tor](https://www.torproject.org/)), or on trusted hardware ([SGX](https://www.intel.com/content/www/us/en/developer/tools/software-guard-extensions/overview.html)).

| System | Anonymity | Scale |
| ------ | --------- | ------ |
| [Tor](https://torproject.org) | Onion Routing | 10k routers, [3m](https://metrics.torproject.org/userstats-relay-country.html) users |
| [Nym](https://nymtech.net/) | Mixnet | 500 routers, ~[20k](https://etherscan.io/token/0x525A8F6F3Ba4752868cde25164382BfbaE3990e1#balances) users |
| [Freenet](https://freenetproject.org/index.html) | [Proactive-Replication](https://freenetproject.org/papers/ddisrs.pdf) | ~[25k](https://www.reddit.com/r/Freenet/comments/ek7vz/huge_jump_in_freenet_user_count_probably_thanks/) users |
| [Signal](https://signal.org/) | Centralized. SGX | ~[40m](https://www.businessofapps.com/data/signal-statistics/) users |


There are tradeoffs associated with each of these solutions. Mixnets and onion routing incur higher latency costs. The additional network hops used by these systems make them difficult to ever compete on speed with web2 CDNs. Multihop systems today have not considered how to optimize performance for non-web cases with self-authenticating, cacheable content.

Hardware solutions like SGX have been used as a privacy solution in a number scenarios, and notably by Signal. This abstraction offers low latency, but rests on trust in a centralized server operator and hardware manufacturer.

Other potential improvements to the security/privacy equilibrium have been developed in the academic literature, but have not yet crossed the innovation chasm into production. These include cryptosystems like [Fully homomorphic encryption](http://cs.cmu.edu/~odonnell/hits09/gentry-homomorphic-encryption.pdf) (FHE), the centerpiece of  DARPA’s [DPRIVE program](https://www.darpa.mil/news-events/2020-03-02) and massively-scalable anonymous communication systems like [PIR-Tor](https://www.usenix.org/conference/usenix-security-11/pir-tor-scalable-anonymous-communication-using-private-information). Promising techniques from blinded tokens to zero knowledge proofs and statistical data structures may also play roles in a practical system.

### Further Reading
The research community has made many advances in the last 3 decades in the spirit of this RFP. Here we briefly note several examples of such research to provide context, but do not view any specific paper as prescriptive or specifically the kind of research we are seeking in response to this RFP.

- [Freenet: A distributed anonymous information storage and retrieval system - Designing privacy enhancing technologies - 2001](http://snap.stanford.edu/class/cs224w-readings/clarke00freenet.pdf)
- [Express: Lowering the Cost of Metadata-hiding Communication with Cryptographic Privacy - Usenix Security 2021](https://www.usenix.org/conference/usenixsecurity21/presentation/eskandarian)
- [Blinder: MPC Based Scalable and Robust Anonymous Committed Broadcast - CCS 2021] (https://eprint.iacr.org/2020/248)
- [Karaoke: Distributed Private Messaging Immune to Passive Traffic Analysis - OSDI 2018](https://www.usenix.org/conference/osdi18/presentation/lazar)
- [PIR-Tor: Scalable Anonymous Communication Using Private Information Retrieval](https://www.usenix.org/conference/usenix-security-11/pir-tor-scalable-anonymous-communication-using-private-information)
- [Oblivious DNS: Practical Privacy for DNS Queries](https://petsymposium.org/2019/files/papers/issue2/popets-2019-0028.pdf)
- [CryptDB: processing queries on an encrypted database](https://dl.acm.org/doi/abs/10.1145/2330667.2330691)

## Problem Statement
The solutions proposed for this RFP will address our published [Open Problem on Private Retrieval](https://github.com/protocol/research#private-retrieval).

## Scope of this RFP
This request for proposals is intended to fund the **research and development of interactive private communication mechanisms, with the goal of creating deployable prototypes**. We are particularly excited to develop work that can be applied in the contexts of libp2p, IPFS, and Filecoin, but we are considering all projects which can be generalized to content-addressed data

We are interested in funding projects which:
- Explore new mechanisms for private communication (e.g. with cryptographic, information theoretic, or statistical basis)
- Relax the traditional ‘web’ assumptions of a single origin to engage with the possibilities of pre-distributed CDN or content-addressed data.
- Prototype the use of novel network-layer privacy technologies in real systems.

## Program Objectives
This call for proposals seeks both **research leading to deployment-ready design sketches** for private communication mechanisms as well as **development activities implementing prototypes** from existing  designs. Applicants should specify which of these objectives their proposal addresses.

**Objective 1 (Research):**
Design of a mechanism or architecture for private/anonymous low-latency communication
- Designs should be fully specified and compatible with implementation in a Web3 context.
- Solutions should be accompanied by experimental/simulation results demonstrating reduced latency and equivalent security (under reasonable model parameters) to existing mechanisms. This includes a demonstration of the scaling performance of the system.

**Objective 2 (Implementation):**
Development of a deployment-ready prototype of a mechanism or architecture for private/anonymous low-latency communication from existing designs.
- The design to be implemented should be referenced and fully specified in the proposal.
- Solutions should be accompanied by experimental/simulation results comparing the performance of the system to that  projected in the original research result, and to other relevant extant solutions.

## RFP Details

### Application Deadline
This RFP will be awarded in phases to accommodate iterative developmental work referencing prior designs.

**Rolling:** we will be reviewing applications in batches corresponding to calendar months. Phase 1 will close on **1 March 2023** or earlier if awarded.

### Recommended team

#### Research
We expect that a team of 1-2 postdoctoral researchers/PhD students supported by their PI working for 1 year should be appropriate for the technical depth of the work, but we will consider other proposals. Renewals are possible for more technically complex projects.

#### Implementation
We expect that a team of 2-3 research engineers/developers working for 1-2 years will be sufficient to carry out the prototype development projects envisioned in this call,  but we will consider other proposals. Renewals are possible.

### Award
Commensurate with scope, up to USD 150k per award for research grants and USD 300k per award for implementation grants.

### Payout schedule
60% upon award and 40% on completion (adjustable to accommodate institutional requirements). Awardees may choose to receive any amount of their award in FIL.

### Point of contact
@willscott

We encourage you to reach out to research-grants@protocol.ai or visit #private-retrieval in the [Lodestar Discord](https://discord.gg/y63FHBWv) if you’re considering applying or have any questions.

### Applications
Please submit your proposal using our application management system at https://grants.protocol.ai/.

**Results are to be released as open source under the [Permissive License Stack](https://protocol.ai/blog/announcing-the-permissive-license-stack/) (Dual License Apache-2 + MIT).**
