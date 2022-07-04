# RFP-012: Checkpointing Filecoin onto Bitcoin

## Motivation
Blockchains based on a reusable resource (such as proof-of-stake or proof-of-space) are not as secure as those based on proof-of-work. Specifically, they are vulnerable to long-range attacks (LRA), where an adversary can create a long fork very cheaply.

Long-range attacks rely on the inability of a user who disconnects from the system at time $t_1$ and reconnects at a later time to tell that validators that were legitimate at time $t_1$ and left the system (by e.g. transferring their stake) are not to be trusted anymore.

In a PoS system,  where the creation of blocks is costless and timeless, these validators could create a fork that starts from the past, i.e. at time $t_1$, and runs until the present. This is unlike, for example, proof-of-work (PoW) systems, where creating blocks requires time and money (e.g., performing actual computation) and not just using cryptographic keys. A user would be unable to recognize the attack as they are presented with a "valid" chain fork. Because the past keys of a given chain fork do not hold value in the present on that fork, previous validators can easily be bribed by an adversary intending to perform this attack.

We propose to design a checkpointing mechanism that leverages the security of proof-of-work blockchains by anchoring the state of the Filecoin chain onto the Bitcoin blockchain. In the case of long-range attacks, the Bitcoin chain can be used to determine the honest chain. We present our current design below, before explaining its limitations and some associated open problems. In short, we came up with a design that leverages Schnorr threshold signatures supported by the 2021 Bitcoin Taproot upgrade. However, we still need to scale the solution to accommodate PoS and related weighted consensus protocols (such as Filecoin's Expected Consensus), which feature possibly thousands of miners/validators.

## Existing design

### Intuition
We designed a solution to LRA, inspired by [Steinhoff et al.](https://arxiv.org/abs/2109.03913) (who showed how to do this on Ethereum (Eth 1.0)) using Bitcoin's PoW, as Ethereum is moving to proof-of-stake. The implementation and design of such a scheme on Bitcoin is more challenging than the implementation on Eth 1.0 of Steinhoff et al. because Bitcoin's expressivity is considerably more limited. Besides, the approach designed by Steinhoff et al. leverages multi-signatures for anchoring, which can quickly bloat the transaction size, making it, at worst, impossible to anchor PoS networks with a large number of validators, and, at best, very costly to do so.

Our approach to addressing this constraint is to use the capabilities enabled by the recent Taproot upgrade to Bitcoin, which allows for more efficient Schnorr threshold signatures. As Bitcoin does not allow for stateful smart contracts, we instead use an aggregated public key to represent the set of validators in the PoS system. When the set changes, the aggregated key must be updated in the Bitcoin blockchain. This is done by having a transaction transfer the funds associated with the aggregated key of the previous validators to the new aggregated key. Instead of having each validator send a transaction to the Bitcoin network, this transaction is signed interactively, off-chain, and all the signatures are aggregated into one constant-size signature.

We note that since our work is based on Schnorr threshold signatures and uses Bitcoin's Taproot, it could be of independent interest to any project looking to implement large-scale threshold signing transactions on Bitcoin (for example, [sidechains](https://gist.github.com/mappum/da11e37f4e90891642a52621594d03f6)).

### High-level protocol description
Each configuration $C_i$ (i.e. set of participants) is associated with a [Taproot](https://en.bitcoin.it/wiki/BIP_0341) public key $Q_i$ that consists of an internal key, in this case an aggregate public key $pk_i$, which participants computed with an interactive DKG protocol and a tweaked part (see figure below).

![](https://i.imgur.com/nirWuWc.jpg)

We choose to tweak the internal key using a commitment to the PoS chain (i.e., the hash of the state of the PoS blockchain), i.e., we have: $Q_{i} = pk_{i} + H_{TapTweak}(pk_{i}||ckpt)G$. Each player $j$ in the configuration then knows a share of the secret key associated with $pk_i$, $s_{i,j}$, such that $t_i$ of the shares are enough to compute a valid signature on any message, but fewer than $t_i$ participants cannot compute a signature.

Configuration $C_i$ is responsible for anchoring the state of the PoS chain at this point in time in the Bitcoin blockchain, which also includes updating the new configuration. In order to do so, the new configuration $C_{i+1}$ must first compute their aggregated public key $pk_{i+1}$ using the DKG algorithm.

This key is then tweaked using a commitment $ckpt$ to the PoS chain (i.e., the hash of the PoS chain at that time). The tweaked key becomes $Q_{i+1} = pk_{i+1} + H_{TapTweak}(pk_{i+1}||ckpt)G$.

Note that only the tweaked key will appear on the blockchain, and so the hash $ckpt$ will not be visible to anyone looking at the blockchain without external knowledge. However, anyone with access to $pk_{i+1}$ and $ckpt$ can easily reconstruct $Q_{i+1}$ to verify that their view of the PoS chain is correct.

To update the configuration from $C_i$ to $C_{i+1}$, a transaction from $Q_i$ to $Q_{i+1}$ must be included in the Bitcoin blockchain. The transaction needs to be signed by $t_i$ participants from configuration $C_i$ where $t_i$ is chosen to be strictly more than $f|C_i|$ as this ensures that at least one honest participant signs, preventing an adversary from signing an illegitimate transaction. Our current prototype uses the [FROST](https://eprint.iacr.org/2020/852) algorithm for signing.

Since we assume that online validators can distinguish an LRA chain, it is enough to have the transaction signed by $t_i$ participants as no honest validators can be fooled into signing an illegitimate transaction. If forks were allowed in the case of an adversary with only $f$ fraction of the power (i.e., outside of LRA forks), this would be more problematic, as two conflicting transactions could then be signed, and we would require at least two-thirds of the participants to sign the transaction, for $f=1/3$ (this could be fixed by considering a block in the past, i.e. one that has been finalized).

In addition to the transfer of coins from $Q_i$ to $Q_{i+1}$, the transaction spent by configuration $C_i$ will have a second output that does not receive any bitcoins and is unspendable but contains an identifier $cid$ used to retrieve the full details of the configuration. This is done using the [$OP\_RETURN$](https://en.bitcoin.it/wiki/OP_RETURN) opcode of Bitcoin that allows storing of extra information in the chain.

This identifier is useful when a user does not have access to the right PoS chain (i.e., does not have the correct value for $pk_{i+1}$ and $c$ due to an LRA). In this case, the content identifier $cid$ can be used, together with content-addressable decentralized storage, for example, IPFS (or alternative content-addressable storage implemented on PoS network validators) to retrieve the identities of the nodes in the correct configuration.

The transaction updating the configuration will look as follows:
$tx_i:Q_i\rightarrow((\textsf{amount},Q_{i+1}),(0,OP\_RETURN=cid))$, meaning that $\textsf{amount}$ is transferred to $Q_{i+1}$ and 0 is transferred to $OP\_RETURN=cid$ (unspendable output).
This information is then publicly available.

## Limitations and open problems
The approach presented above is limited mainly due to the high communication cost of the threshold DKG, where each participant must share a secret with the rest. This limitation is even more severe when considering a non-flat model, where a participant must hold a number of keys proportional to their amount of stake/power in the system. With a blockchain like Filecoin, we could quickly end up with hundreds of thousands or even millions of keys. This solution is hence not viable for large blockchains. We are thus interested in developing a solution that could scale to up to **500k keys** or that can incorporate the power (weight) of participants without dramatically increasing the number of keys and scale to up to **10k (weighted) participants**.

Although scalable threshold DKG and signatures schemes have recently been proposed, none of them are currently compatible with Bitcoin (e.g. BLS signature, [Mithril](https://eprint.iacr.org/2021/916)). Solving the following open problems would help scale our solution to thousands or millions of nodes:

* Non-interactive DKG compatible with Schnorr threshold signing schemes on the secp256k1 curve and scalable to 500k keys.
* Threshold DKG and Schnorr signing schemes on the secp256k1 curve that incorporate the weight of participants and scale to 10k nodes.
* Parallelized DKG: sort the participants into different subgroups of the same "size" (i.e., power) and have them compute a DKG inside their subgroup, then merge the keys of all the subgroups; this approach is an open problem as the interactions between subgroups to compute the final threshold key would be highly complex.
* Efficient share aggregation: aggregate the shares associated with one participant, such that the complexity of our protocol is quadratic in the number of participants and not the number of "unit of power" (i.e., moving from a flat to a non-flat model should not significantly increase the communication complexity of the algorithm); some shares can currently be aggregated but not to the point of being equivalent to a flat model.
* Sampling: similar to the approach used in [Mithril](https://eprint.iacr.org/2021/916), one could elect a subset of the participants to perform the signing instead of having everyone contribute; the issue with this approach is that, in our case, the sample elected to perform the signing must be known ahead of time (as they need to compute the DKG beforehand) and hence could be corrupted by an adversary prior to the signature. This is unlike the approach in Mithril, where the sample elected is revealed at the time of creating the signature.

## Scope of this RFP

The goal of this RFP is to fund research that addresses one or more of the open problems above while meeting the requirements/constraints of the overall project and driving progress towards the goals. Proposals should seek to maintain compatibility with portions of the solution that they do not replace or improve upon.

## RFP Details

### Application deadline
**Rolling**: we will be reviewing applications in batches corresponding to calendar months. The call will close on **September 30, 2022**, or earlier if awarded.

### Recommended team
A team of one PhD or postdoctoral researcher supported by one PI should be appropriate for the technical depth of the work, but we will consider other proposals.

### Award
Commensurate with scope, up to USD 100k per award. We will generally consider projects lasting up to one year, but faster delivery (through scope restriction or parallelization) is highly prized.

### Payout schedule
60% upon award and 40% on completion (adjustable to accommodate institutional requirements).

### Point of contact
**@sa8**. We encourage you to reach out to research-grants@protocol.ai or visit #consensus in the [Filecoin Slack](https://filecoin.io/slack/) if you are considering applying or have any questions.

### Applications
Please submit your proposal using our application management system at https://grants.protocol.ai/.

** Results are to be released as open-source under the Permissive License Stack (Dual License Apache-2 + MIT).
