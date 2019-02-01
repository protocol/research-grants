# Secret Single-Leader Election (SSLE)

## Brief description

A Secret Single-Leader Election (SSLE) for a given clock blockchain round `r` is a function that *fairly* and *secretly* selects a single leader from a known set of participants. Only when the selected participant proposes a block for round `r` will that miner reveal itself to other protocol participants as the verifiable leader for the block at round `r`. 

In the context of Filecoin, SSLE selects a block leader from a weighted set of miners where the probability that a miner is selected is proportional to their power in the Filecoin Power Table. An SSLE protocol would elect exactly one individual as leader in a way that no one (eavesdroppers and other participants in the protocol) can tell who was elected other than the leader. Then, once the leader announces the next block, the validity of the election can be verified by anyone. Filecoin currently uses an alternative consensus mechanism, Expected Consensus (EC). For a given round `r`, EC facilitates a secret election where on expectation there is one leader (implying that zero or multiple leaders may also be elected in any round). SSLE will be an evolutionary improvement to EC.

We discuss specific requirements for SSLE and distinguish it from existing consensus protocols below.

## Problem Statement

### What should SSLE address?

`Fork grinding` - Having multiple block leaders for a given round may result in opportunities for miners to selectively mine a branch. This process may potentially bias the protocol through grinding. A construction where only one block leader exists at any round `r`minimizes this form of grinding thereby helping secure the protocol.

`Faster Convergence` - Having a single leader elected at every round will enable faster blockchain consensus without needing to consider network latency, synchronicity, or other factors that may cause consensus to fail.

`Simpler Protocol` - The Filecoin consensus protocol currently relies on *tipsets* (set of blocks mined in a given round). `tipsets` specify how miners should act when multiple leaders are mined in a round. A SSLE construction should simplify the protocol and result in stronger security guarantees. For example, SSLE should reduce the number of possible canonical forks to one and results in implications to finality, liveness, and potential double spending opportunities.

Existing considerations that SSLE must preserve

`Security` - We want to protect the Filecoin network from malicious actors. These attacks may affect liveness or finality and include but are not limited to Sybil, Eclipse, and Denial of Service (DoS). For instance, if a malicious actor were able to learn the identity of a miner who became a block leader, that actor may attempt to deny the leader's access to the Filecoin network (using a denial of service attack, for instance) before they publish their block; this would affect liveness and finality at least temporarily.

### Requirements & Constraints

The following are the current set of protocol requirements for SSLE:

1. *Fair* - Recall that in a Proof-of-Stake or Power Fault Tolerance system, each miner has an associated *power* `p` which is related to the amount of physical storage a miner makes available to the Filecoin network. Since we want to incent miners to add more storage, each miner’s chance of becoming the canonical block leader should to be proportional to their power.

2. *Secret* - Only the canonical block leader at a round `r` can know that they are the leader until they broadcast a new block to the other miners. All miners should be able to verify the canonical block leader non-interactively.

3. *Unpredictable* - No observer or set of observers smaller than a threshold *m of n* should be able to predict which miner will become a block leader at a given round `r_i` with probability greater than `eps`. In other words, no observer or collection of observers should be able to predict block leaders with any advantage greater than `eps`.

4. *Verifiable* - For a protocol to be verifiable, we claim that a protocol must produce some output that any miner can use to check the fairness of the leader selection.

Or in other words, SSLE is a function *S* that maps a set of miners *M* to a value *w* which secretly represents a miner *m_win*:

- *w* is secret to all but the miner *m_win*
- the relationship between *w* and a miner *m_win* can be publicly verified
- *S* is verifiably fair
- *S* cannot be influenced by any group of miners smaller than some threshold *T*
- *S* is not predictable

Furthermore, we require that any solution meet the following performance constraints:

1. `On-chain efficiency` -  We constrain SSLE to require no more than O(log[n]) bits stored on chain per block, where `n` is the number of active protocol participants. Moreover, we constrain the system to support no fewer than O(log[k]^2) new participants per round, where `k` is the total number of miners that have ever participated.

2. `Reasonable communication complexity` - In the worst case, SSLE related communication should scale with similar state of the art solutions. Ideally, the number of SSLE related messages would scale with O(n) or better, where `n` is the number of miners participating in the election.

3. `Computationally efficient` - Any participant should expect no more than `block time`/`thresh_prop` seconds of compute time for an average commercially available CPU in the network. `thresh_prop` is a security parameter that is tuned to ensure validation propagation and confirmation race attacks are not possible. A validation propagation attack is a form of DoS that occurs when a block takes approximately the same time to validate as it does to mine.  

4. `Scalable in the number of miners` - The protocol should not impact the performance or security guarantees of the Filecoin network with a large set of active miners. An ideal SSLE protocol would scale with 2^14 or more miners.

5. `Liveness` - The protocol must be robust in the face of sporadic participation in a given round. The protocol should guarantee progress if the majority of SSLE participants are honest and progress given sporadic partitions.

### Reasonable assumptions

- The public keys of miners who have participated in at least one block are known by all SSLE participants.
- A current accounting of power is available to all miners
- There is a source of randomness which is maintained on-chain. This might take the form of a Randomness Beacon or Oracle with short output. The state of randomness is stored on chain and updated each block. This source of randomness must adhere to the constraints and requirements above and may be generated by the SSLE protocol itself.
- A majority of the network’s power is controlled by honest participants.

Many consensus protocols in the literature, such as `Snow White`, `Algorand,` `Ouroboros`, and others fulfill some but not all of the requirements for SSLE. For instance,

- `Snow White` does not guarantee a single leader in a given round.

- `Algorand` does not canonically elect a single leader and is near the limit of communication complexity. 

- `Ouroboros` elections are not secret.


### High-Level Example Protocol with Multiparty Computation

Suppose a block `B_n` is the most recent addition to the Filecoin blockchain and winner `m_win` has not yet submitted its block. Furthermore, suppose that the expected time in which `m_win` should respond has not yet passed. `B_n` must contain  `m_win` as well as a list of members `c_i` that will select the next block leader. Each `c_i` has similar properties to `m_win`. We assume that all quantities are verified by each participant as they receive them.

Phase 1: From `B_n` we deduce a committee `C_n` whose function is to select the next block leader as well as a new committee for block `B_n+1`. At this time, all `c_i` collude together to generate (1) `C_n+1` (2) `m_win+1`. Once this has happened, each `c_i` publishes their result.

Phase 2: Upon receipt of `C_n+1` and `m_win+1`, `m_win` generates, signs, and publishes the new lead block for the Filecoin blockchain.

Requirements this method meets:

If the MPC is fair, then this protocol will be fair, secret, unpredictable, and Verifiable
Note that this protocol looks similar to Algorand with the key difference that there is only one leader possible for a given round.

Possible issues with this method:

MPC often presents communication challenges that might not allow it to have reasonable communication complexity.
Certain MPC methods can be computationally expensive to set up.
MPC methods often scale poorly with the number of participants.

### High-Level Example Protocol with Functional Encryption

Suppose there exists some functional encryption scheme with a trusted setup, 
`Fe(seed, {miners}) = E_winner`
Where the trusted setup results in a public key associated with `Fe`. `Fe` takes a per-block `seed` and weighted list of miners as input. `Fe` outputs an encrypted ticket which the winner will use to prove that they are the elected leader for a specific block. In this construction,  `Fe` would randomly select a winner from `{miners}` and subsequently encrypt a signature over the winners public key,
    `E_sk(Sign_Fe(pk_winner))`
Where sk and pk are the secret and public key of the winner and the encryption is randomized.

Requirements this method meets:

If `Fe` is secure, then this protocol will be fair, secret, unpredictable, and Verifiable

Possible issues with this method:

This construction requires a trusted setup which may require updating if miners are added or removed from the network. This could lead to communication challenges as additional communication would need to performed in order to update `{miners}`.
Functional encryption schemes are often computationally expensive.
Depending on how `Fe` is constructed, this could lead to significant on-chain storage of `Fe` state.

## RFP details

**Application Deadline:**  Wednesday, February 27th, 2019

**Recommended team**

 - Any number of Mathematicians and/or Computer Scientists
 - Familiarity with Applied Cryptography, Distributed Systems, and Consensus

**Objectives & Key Results**

 - **Objective 1:** *Determine an SSLE construction meeting the above requirements and constraints*
    - Generate documentation stating the operation and assumptions of the construction
    - Generate a proof of correctness
    - Generate a proof of security
- **Objective 2:** *Build a reference implementation*
    - Generate a pseudocode protocol with detailed specifications for each variable and message
    - Submit benchmarks validating the above constraints
    - Submit highly commented source code (code should be simple and need not be optimized)
- **(optional) Objective 3:** *Determine an SSLE construction that can be performed without multiparty computation (MPC)*
    - Proof of correctness and security under non-MPC model
    - Theoretical assessment of computational complexities

**Alternative Objectives & Key Results**

- **Objective 1:** Prove that there cannot exist an SSLE construction that satisfies the above requirements and constraints.

**Award:** Up to $200,000 (USD) per grant with up to 20% payable in Filecoin.

**Payout schedule:** Award winners receive the majority of the disbursement shortly after selection, with the remainder presented upon completion of the work.

**Application Instructions**: [RFP Application Instructions](https://github.com/protocol/research-RFPs/blob/master/RFP-application-instructions.md)

**Results are to be released as open source under MIT license**
