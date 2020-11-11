# Scalability Bounds of P2P Pub/Sub (libp2p floodsub, gossipsub and episub)


## Background

Publish/subscribe (pub/sub) messaging systems have been proposed and traditionally used to disintermediate senders and receivers of messages. That is, in pub/sub systems, publishers of content do not send the published messages directly to one or a group of receivers but instead publish messages to a topic. The pub/sub system then needs to match subscribers' interests to published messages (otherwise known as events). This model of communication enhances asynchronous communication and can significantly reduce network traffic and bandwidth requirements.

Within the IPFS and libp2p ecosystems, pub/sub is being used to push naming record updates to the decentralised naming system of IPFS, acronymed [IPNS](https://docs.ipfs.io/guides/concepts/ipns/). As the IPFS system is evolving and growing, communicating new entries to IPNS is becoming an issue due to the increased network and node load requirements. The expected growth of the system to multiple millions of nodes is going to create significant performance issues which might render the system unusable.

Despite the significant amount of related literature on the topic of pub/sub, very few systems have been tested to that level of scalability, while those that have were mostly cloud-based, managed, and structured. Instead, in the case of IPNS (and IPFS more in general), the network is entirely decentralised and therefore unmanaged. This poses new challenges to the pub/sub protocol, which we want to explore through this RFP.


## Problem Statement

The solutions proposed for this RFP will be addressing our published open problem on [PubSub at Scale](https://github.com/libp2p/notes/blob/master/OPEN_PROBLEMS/PUBSUB_AT_SCALE.md). Please review the problem in detail for requirements and constraints.


## Scope of this RFP

With this RFP, we are looking for approaches that explore the scalability bounds of the existing pub/sub algorithms within libp2p, i.e. gossipsub and episub. IPFS already has hundreds of thousands of daily users and is expected to grow exponentially to multiple millions. ETH1.0 already has more than 16,000 nodes and when ETH2.0 arrives this number is expected to rise by several orders of magnitude. A thorough evaluation of the scalability performance of the existing protocols or their redesign is essential before they get deployed at those scales.

As such, the goal of this RFP is to answer questions such as:
* What changes need to be made in order for gossipsub and episub to scale to 10s of millions of users while retaining acceptable performance in terms of message delivery latency?
* What changes need to be made so that gossipsub and episub can operate efficiently under high rates of network churn (up to 40% per hour)?
* What are the latency guarantees that we can expect as the system scales from hundreds of nodes to millions of nodes?

Use cases of interest (10 ≤ n, m ≤ 10M):
* A blog (1 publisher,  n subscribers, 1 post/day)
* A collaborative document (1-50 publishers, 1-50 subscribers, 10-500 updates/minute)
* A forum (n publishers, m subscribers,  1-n posts/day)
* A live video stream (1 publisher, n subscribers, strict requirements of low latency)


## Objectives & Key Results

### `Objective 1:` For one or more of the use cases of interest, perform an evaluation (in a simulated environment using your favourite tool of choice) of the multiple pub/sub implementations (floodsub, gossipsub, and episub) that can:

- 1. Demonstrate the performance of the current design for multiple network sizes (10K, 100K, 1M, 10M).
- 2. Identify what are the scalability limits with regards thresholds of memory usage per peer (e.g. GB of memory), unacceptable delay (e.g. tens to hundreds of milliseconds), resilience to churn and other dimensions. you find relevant for each use case of interest.
- 3. Suggest what are the parts of the algorithm that can be the primary bottlenecks.

### `Objective 2:` Compare scalability limits against performance metrics through empirical tests

- 1. For the use cases of interest studied in Objective 1, verify (through realistic simulation using the testbed with the actual implementations) that theoretical limits are observed in the implementations of floodsub, gossipsub & episub).
- 2. Report the differences between the Simulations from Objective 1 & the results from Objective 2.a.
- 3. In case of any significant different, investigate and report potential causes.

### `Objective 3:` Design, develop, and evaluate improvements to the design of floodsub, gossipsub, episub (or the implementation of a new pub/sub algorithm) that can improve performance  for one of the use cases of interest in one or more of the following dimensions:

- 1. **Scalability** - Support a higher number of active nodes in the network without degrading the quality of service.
- 2. **Efficiency** -  Reduce aggregate computational and network load (e.g. CPU, memory, bandwidth overhead) across the network.
- 3. **Load Balancing** - Improve workload distribution so as to minimise inter-node load asymmetry (e.g. CPU, memory, bandwidth).
- 4. **Resiliency** - Support a broader range of node churn rates without compromising the system (e.g. be able to deal with churn from 10% to 80% per hour).
- 5. **Latency** - Achieve lower and more scalable propagation latency (e.g. keep latency below 1 second for up to 1M nodes and below 2 seconds for 5M nodes).


## RFP details

#### Application Deadline

Rolling: we will be reviewing applications in batches corresponding to calendar months. The call will close on 30 June 2020 or earlier if awarded.

#### Recommended team

* One or two full-time Post-Doctoral researchers for a period of 8-18 months.
* Experience with: distributed systems, publish/subscribe systems, DHTs, internet routing protocols, content-addressable networks.

We expect the technical depth of the work to be at the PhD level but smaller grants are also available to sponsor MSc level work.

#### Award

Up to $100,000 per proposal. Possibility of up to 20% paid in FIL.

#### Payout schedule

60% upon award and 40% on completion (adjustable to accommodate institutional requirements).

#### Point of contact

David Dias (@daviddias). We encourage you to reach out to rfp@protocol.ai if you’re considering applying or have any questions.


**Results are to be released as open source under the [Permissive License Stack](https://protocol.ai/blog/announcing-the-permissive-license-stack/) (Dual License Apache-2 + MIT).**
