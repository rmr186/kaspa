# Phantom Consensus

## Introduction

### Consensus in a Distributed System

#### What is a Distributed System?

* [ ] Finish explaining about [distributed systems](https://medium.com/s/story/lets-take-a-crack-at-understanding-distributed-consensus-dad23d0dc95) and distributed consensus

A distributed system is a system in which participants, who are geographically separate one from the other, coordinate by passing messages one between the other in order to achieve a common objective.

Three significant characteristics of distributed systems are:

* Concurrency
* [Lack of a global clock](https://en.wikipedia.org/wiki/Clock_synchronization)
* Independent failure of components

Concurrency means that messages and events in the distributed system take place at the same time by different participants, without any coordination.

Lack of global clock means that there is no one unified clock, on which all participants can rely, for the purpose of determining the order between events by different participants.

Independent failure of components means that different participants may fail, not send or receive messages, come and leave the system without warning and be byzantine \(misbehave\) - with or without malicious intentions.

### The Nakamoto Consensus

* [ ] Explain about the Nakamoto consensus

## Phantom Consensus

### Differences of BlockDAG over Blockchain

* [ ] What is a DAG?
* [ ] Explain incentive for using wide DAG rather than a DAG of one chain.
* [ ] What's the new challenge of achieving consensus in a DAG?

Phantom consensus is a generalization of Nakamoto consensus, where instead of a chain of blocks, the ledger consists of a directional acyclic graph of blocks. Instead of pointing to one previous block as in the blockchain, in a blockDAG each block points to all previous recent blocks; formally, these are the tips of the blockDAG that the mining node observed locally at the time it created the new block.

![Directional Acyclic Graph - The BlockDAG](https://lh6.googleusercontent.com/W-v03qdqQp_1rQsHFz00A5p14z3Bklo3Ag09-a16aJNlXXpbOOEzhCdpTtnhROEO_A9e1TDghXRhTD21wVt4oO9lUhfezsGt6F8NQXSwzmWL-bvwvuMPEp4iPX5zn1U1CwFjHhwT)

Unlike a blockchain which preserves consistency by construction \(every block in the chain adds transactions that are consistent with its predecessors in the chain\), a blockDAG incorporates blocks from different “branches” and so may contain conflicting transactions. We therefore need a method to recover consistency; in other words, a blockDAG system requires replacing Satoshi’s longest chain rule with a new consensus protocol.

### **Phantom GhostDAG - the New Consensus Kid on the Block**

* [ ] What is Phantom?
* [ ] How does Phantom achieve consensus in a DAG?

The natural topology of a DAG already induces a partial ordering over the blocks: if there is a path from block X to block Y in the DAG, then block Y was provably created before block X and so should precede X in the global order. Thus, we only need to define an order over blocks created in parallel, i.e. any set of blocks such that there is no path that connects them.

Phantom takes as input a blockDAG and outputs an ordering of its blocks. It does so by identifying a cluster of well-connected \(blue\) blocks and ordering the DAG in a way that favors them over the other \(red\) blocks.

![Depiction of Blue vs Red Blocks in the DAG](https://lh4.googleusercontent.com/ryec3BWdfGLVasVyG569W7DvvmV5ItBRkv91rLyCK7Ao9m6AutzGcijHdZEmHc5UanV5kp-vPKmV3S_zUdw1kB5bsdnOtpOjJ1vJqZAkqhNd--rEN69bqqK3pAIOLHbpW_t5ec58)

In Phantom, each node extracts transaction consistency from its locally observed blockDAG by running the Phantom algorithm on it, which assigns a linear ordering to the blocks, and thus, a linear ordering to the transactions, from which double spends can be eliminated. Currently, the red blocks are not part of the consensus.

