# RFP-004: Data laced with permissions: Decentralised Access Control in CRDTs

**Brief description**
Conflict-free replicated data types (CRDTs) provide a framework for creating eventually consistent data types that can be shared amongst replicas and that guarantees liveliness and monotonicity. To allow replica creators and owners to keep control over who has read and write access to a CRDT instance, we seek a decentralised Access control list (ACL) type that has typical ACL security properties and that, when applied to a CRDT instance, still guarantees CRDT safety (that it provably converges to the same value on all replicas).

**Problem Statement:** https://github.com/protocol/research/issues/8

**Application Deadline:** ~rolling deadline~ (closed)

**Key Results**
 - Prove that there is an eventually consistent ACL type that guarantees security and, when applied to a CRDT, guarantees safety (where safety and security are defined in the problem statement)
 - Define and formally specify such a type
 - Prove that there is an strongly consistent ACL type that guarantees security and, when applied to a CRDT, guarantees safety (where safety and security are defined in the problem statement)
 - Define and formally specify such a type

**Award:** Up to $200,000 (USD) per grant

**Payout schedule:** Award winners receive the full disbursement shortly after selection

**Results are to be released as open source under MIT license**
