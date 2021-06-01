# Proof of Space and Useful Space

## **Background**

A **Proof of Space** is a protocol that allows a prover to convince a
verifier that he has a minimum specified amount of space (ie, used
storage).

In Filecoin we are interested in proving *useful* space, that is storage
space that can be used to keep real-world data. Therefore, we want the
advice *A* of the PoS to encode some real data *D*, instead of just
being a random incompressible sequence of bytes. This is the informal
new property we are interested in each time we will talk about **Proof
of Useful Space.**

A more formal way to capture the security requirements of the Filecoin
decentralized storage network is using a proof of useful space that is
actually a **Proof of Replication**.

Informally, this means that in addition to the space hardness property
seen before for a PoS , the replica (that is the advice that now
contains encoded data) has the extraction property. In other words,
there is an extraction algorithm that can recover the original data from
the interaction with a successful prover during the execution phases.

Please see the related [**Open Problem
Statement**](https://github.com/protocol/CryptoNetLab/blob/main/open_problems/Proof-of-Space_and_Useful_Space_Open_Problems.md),
the [**Proof of Space
report**](https://eprint.iacr.org/2013/796.pdf), and the [**Proof
of Replication report**](https://eprint.iacr.org/2018/678) for
further details.

## **Problem Statement**

The solutions proposed for this RFP will address our published open
problems on [**Proof of Space and Useful
Space**](https://github.com/protocol/CryptoNetLab/blob/main/open_problems/Proof-of-Space_and_Useful_Space_Open_Problems.md).
Please review the problems in detail for requirements and constraints.

## **Scope of this RFP**

The goal of this RFP is to stimulate improvements to the Proof of Space
constructions used in Filecoin and other Web3 protocols. We are funding
research that addresses any of the **eight Open Problems** listed in the
above [Open Problem
Statement](https://github.com/protocol/CryptoNetLab/blob/main/open_problems/Proof-of-Space_and_Useful_Space_Open_Problems.md):

-   **Problem 1:** Simple graph-labeling based PoS in the time model

-   **Problem 2:** Graph-labeling based PoS in the cost model

-   **Problem 3:** Fewer communication rounds for repeated audits

-   **Problem 4:** Proof of Useful Space from hash-based PoS

-   **Problem 5:** Proof of Useful Space with data updatability

-   **Problem 6:** Tight hash-table based PoS construction

-   **Problem 7:** Incremental cost for parameters upgrades

-   **Problem 8:** Verifiable capacity-bound functions

Successful applications will include an explicit formulation of the
objectives they propose to deliver, a description of the use-cases for
which their solution is optimized, and a description of how the
performance or correctness of the solution will be demonstrated and
evaluated relative to the current state-of-the-art.

Solutions (or impossibility results) should take the form of a
scientific paper or technical report. Additional follow-on funding is
available to support the preparation of submissions to open-access
journals and conference proceedings, including travel and presentation
costs.

## **RFP Details**

### **Application Deadline**

**Rolling:** we will be reviewing applications in batches corresponding
to calendar months. The call will close on **August 30, 2021** or
earlier if awarded.

### **Recommended team**

We expect the technical depth of the work to be at the level of a PhD
student. We suggest that a team of one PI and one PhD part time for 9-12
months or one PI and one postdoctoral researcher part time for 6-8
months is appropriate for most problems.

Applicants may build a collaborative project with researchers from
multiple institutions if desired.

-   **Recommended expertise:** Mathematicians and/or Computer Scientists
 familiar with Applied Cryptography, Codes, Information Theory,
 Proof of Storage

### **Award**

Up to $50,000 USD per proposal. Possibility of up to 20% paid in FIL.

### **Payout schedule**

60% upon award and 40% on completion (adjustable to accommodate
institutional requirements).

### **Point of contact**

**Irene Giacomelli (@irenegia) and Luca Nizzardo (@lucaniz)**. We
encourage you to reach out to **rfp@protocol.ai** if you are considering
applying or have any questions.


### **Applications** 

Submit your proposal using our application management system at 
https://grants.protocol.ai/.

**Results are to be released as open source under the** **[Permissive
License
Stack](https://protocol.ai/blog/announcing-the-permissive-license-stack/)
(Dual License Apache-2 + MIT).**
