# Optimize storage and convergence time in causal CmRDTs

**Brief description**
CRDTs provide a framework for creating eventually consistent data types that can be shared amongst replicas and that guarantees liveliness and monotonicity. Typically, operation-based CRDTs (or CmRDTs) propagate changes through the replication of operations. The storage requirements for these types of CRDTs is unbounded, as is the replication time for newly-joined fresh replicas or replicas that are severely diverged. We need to define protocols that a) bound the local storage dedicated to these and b) bound the time required for convergence.

**Problem Statement:** https://github.com/protocol/research/issues/9

**Application Deadline:** Rolling deadline

**Objectives & Key Results**
 - **Objective 1**: Improve causal CmRDT convergence time
    - Find a protocol that reduces the time required to synchronize any fresh replica without compromising eventual changes in offline replicas.
    - Formally define one such protocol.
    - Find a protocol that can optimize the time required for synchronizing any two replicas that have strongly diverged without compromising changes in offline replicas.
    - Formally define one such a protocol.
- **Objective 2**: Allow local “garbage collection” for CmRDTs
    - Find a protocol that allows defining an upper bound for the required total size of local storage dedicated to operations without compromising changes in offline replicas.
    - Formally define one such protocol.

Note: It is not necessary for an application to propose solutions to both objectives or multiple key results.  Furthermore, it’s worth noting that we ascribe slightly more importance to Objective 1.


**Award:** Up to $200,000 (USD) per grant

**Payout schedule:** Award winners receive the full disbursement shortly after selection

**Application Instructions**: [RFP Application Instructions](https://github.com/protocol/research-RFPs/blob/master/RFP-application-instructions.md)

**Results are to be released as open source under MIT license**
