# DAG Consensus

## Introduction

### Consensus in a Distributed System

#### What is a Distributed System?

A distributed system is a system in which participants, who are geographically separate one from the other, coordinate by passing messages one between the other in order to achieve a common objective.

Three significant characteristics of distributed systems are:

* Concurrency
* [Lack of a global clock](https://en.wikipedia.org/wiki/Clock_synchronization)
* Independent failure of components

Concurrency means that messages and events in the distributed system take place at the same time by different participants, without any coordination.

Lack of global clock means that there is no one unified clock, on which all participants can rely, for the purpose of determining the order between events by different participants.

Independent failure of components means that different participants may fail, not send or receive messages, come and leave the system without warning and be byzantine \(misbehave\) - with or without malicious intentions.

### The Nakamoto Consensus

## Phantom Consensus

### Differences of BlockDAG over Blockchain

Phantom consensus is a generalization of Nakamoto consensus, where instead of a chain of blocks, the ledger consists of a directional acyclic graph of blocks. Instead of pointing to one previous block as in the blockchain, in a blockDAG each block points to all previous recent blocks; formally, these are the tips of the block DAG that the mining Node observed locally at the time of the new block’s creation.

