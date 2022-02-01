# RFP-011: Changing the Internet

## Background

The Internet’s core architecture has remained largely unchanged for over 40 years. While the longevity of its original design indicates that there are important lessons to be learned, changes in use cases, constraints, and technologies warrant large-scale change. However changing the Internet has proved difficult: piecemeal solutions have proved insufficient while clean-slate designs have proved undeployable.

## Problem Statement

The solutions proposed in response to this RFP should address one or more of the five core challenges identified below. Changing the Internet is a broad topic, one that cannot be easily summarized in a short statement, nor can the possible solutions be specified in advance.

1. **Incentives for New Infrastructure**. The Internet currently consists of a complex mix of large-scale infrastructure providers. Many of these companies are legacies of past national / centralized telecommunications efforts, and are slow to innovate today. However, decentralized or bottom-up approaches to infrastructure deployment have struggled in part due to misaligned incentives, high barriers to entry (cost, business relationships, pre-existing infrastructure, regulation, etc.), and a lack of architectural primitives to enable their use. We call for research that proposes new incentive mechanisms for the deployment of any types of Internet infrastructure outside of existing players, where infrastructure reflects the physical deployment of resources at any layer of the stack, from the physical infrastructure that transports bits to the compute, storage, and networking facilities that enable services.

2. **Incentives for New Services**. Today a few large companies dominate the Web and the Internet as a whole. The power and breadth of these companies has continued to expand, and with it service innovation has begun to stagnate. New network service offerings are difficult to introduce and the incentives for doing so at small scale are not present. We call for research that proposes new incentive mechanisms for the deployment of new network services that can be invoked by any party in the Internet and cannot be thwarted by existing players. Furthermore, such new services must be deployable today, not clean slate, and thus must have an incremental deployment path.

3. **Architecture for Service Introduction**. The last two decades have witnessed a dramatic shift from end hosts to the network. Today most Internet traffic is intermediated by middleboxes, whether hardware or software, but the introduction of these boxes and services on them is complex, expensive, slow, and non-democratic. We call for research that proposes mechanisms for introducing new services into the network. The service architecture must be compatible with the Internet as it exists today (i.e., the service architecture should not be clean-slate) and should enable bottom-up innovation and service introduction.

4. **Mechanisms for Service Invocation**. Once new Internet services are available and properly incentivized, there must be a way for the services to be invoked by applications and/or users, whether or not their proximate network provider or local government wishes for them to do so. We call for research that proposes mechanisms for service invocation from end hosts and applications. Such service invocation should not be easily limited by uncooperative, adversarial, or legacy intermediaries.

5. **Trust Mechanisms for Decentralized Infrastructure and Services**. As new infrastructure and services are deployed, users will face a problem of trust. Today’s Internet runs on implicit (if unearned) trust in large infrastructure and service providers. The future Internet, one with many newly-introduced infrastructural elements and network services, will necessarily have more distinct organizational entities involved and thus require dispersion of trust among these entities. We call for research that proposes trust mechanisms for these newly introduced infrastructure and service elements. These mechanisms should not make assumptions about centralized systems of enforcement or control.

## Further Reading

The networking research community has made many advances in the last 3 decades along the lines of this RFP. Here we briefly note several examples of such research to provide context, but do not view any specific paper as prescriptive or specifically the kind of research we are looking to be proposed in response to this RFP.

- [Deployment and scalability of an inter-domain multi-path routing infrastructure](https://doi.org/10.1145/3485983.3494862), ACM CoNEXT ‘21
- [Enabling a permanent revolution in internet architecture](https://doi.org/10.1145/3341302.3342075), ACM SIGCOMM ‘19
- [Balancing accountability and privacy in the network](https://doi.org/10.1145/2740070.2626306), ACM SIGCOMM ‘14
- [XIA: architecting a more trustworthy and evolvable internet](https://doi.org/10.1145/2656877.2656885), ACM SIGCOMM CCR ‘14
- [Networking is IPC: a guiding principle to a better internet](https://doi.org/10.1145/1544012.1544079), ACM CoNEXT ‘08
- [Plutarch: an argument for network pluralism](https://doi.org/10.1145/972426.944763), ACM SIGCOMM FDNA ‘03
- [Tussle in cyberspace: defining tomorrow's internet](https://doi.org/10.1145/633025.633059), ACM SIGCOMM ‘02
- [Distributed algorithmic mechanism design: Recent results and future directions](https://doi.org/10.1145/570810.570812), ACM DIALM ‘02

## Scope of this RFP

The goal of this RFP is to stimulate improvements to the Internet that can pave the way for innovative uses and designs for future network technologies including but not limited to Web3. We are funding research that addresses one or more of the five themes listed above.

Successful projects will result in a deployable prototype or prototypes that can be further developed towards commercialization.

## RFP Details
### Application Deadline
**Rolling**: we will be reviewing applications in batches corresponding to calendar months. The call will close on **August 30, 2022** or earlier if awarded.

### Recommended Team
Likely team size will range from 1-2 PIs/Co-PIs and 1-4 students/researchers, as justified in project scope description.

### Award
Commensurate with team and scope, up to $200k USD. Possibility of up to 20% paid in FIL.

### Payout Schedule
60% upon award and 40% on completion (adjustable to accommodate institutional requirements).

### Point of Contact
We encourage you to reach out to research-grants@protocol.ai if you are considering applying or have any questions.

### Applications
Submit your proposal using our application management system at https://grants.protocol.ai/.

** Results are to be released as open source under the Permissive License Stack (Dual License Apache-2 + MIT).

