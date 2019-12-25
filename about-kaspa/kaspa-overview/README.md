---
description: Introducing Kaspa blockDAG money
---

# Kaspa Overview

## What is Kaspa?

Kaspa is a high throughput, photonic proof-of-work, expressive, sound money.

Kaspa is a cryptocurrency that inherits its defining properties and values \(e.g. security, consensus, governance and more\) from Nakamoto's Bitcoin, yet allows expressiveness and programmability over it, much like Ethereum.

Kaspa is optimized for sound and scalable store of value, payments, settlements, transfer of digital assets, proof of publication, and smart contracts.

## How Does Kaspa Work?

Rather than storing the distributed ledger in a block_chain_, Kaspa's distributed ledger is stored in a block_DAG_ - a wide, directed acyclic graph \(DAG\) of blocks. Many blocks can be created in parallel. This high block throughput enables near-immediate transaction confirmations. Globally ordering transactions from parallel blocks and restoring consensus is accomplished by Kaspa's [Phantom ghostDAG](https://eprint.iacr.org/2018/104.pdf), a highly scalable and decentralized blockDAG consensus protocol, that generalizes over the [Nakamoto](https://bitcoin.org/bitcoin.pdf) consensus.

Kaspa intends to use optical proof of work \(OPoW\) - an implementation of traditional proof of work \(PoW\) with analog photonic ASICs, instead of traditional digital ASICs. A consensus protocol based on OPoW retains the same security properties of consensus protocols using traditional PoW \(i.e. an attacker needs to get hold of more than 50% of the entire network's computational resources for a prolonged time to attack the network - something that is infeasible to achieve, given the permissionless nature of decentralized consensus\). OPoW mining with Photonic-ASICs is more energy efficient and "green" than traditional ASIC mining, allowing for more sustainable and geographically decentralizing mining. Coupling that with the rapid block creation rate of the blockDAG, it is much more likely for a miner to find blocks and get the rewards associated with finding blocks. This makes Kaspa mining fair game for smaller miners, further contributing to its decentralization potential.

Kaspa enables a[ smart contract](../smart-contracts/) computation layer, inspired by [Arbitrum](https://www.usenix.org/node/217514) and [TrueBit](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf). Kaspa's smart contract later is decoupled from the consensus layer, allowing the native currency - Kaspa to remain lightweight and scalable, while supporting and enhancing upper-layer use cases, such as stable tokens, decentralized credit systems, and other decentralized financial \(DeFi\) apps.

## Next Steps

You are encouraged to read how the blockDAG consensus works and how it generalizes over the Bitcoin Nakamoto consensus.

* [ ] Read the [Kaspa Whitepaper Draft](https://github.com/kaspanet/documentation/blob/master/whitepaperdraft.md)
* [ ] Read the [Kaspa Smart Contracts Spec](https://github.com/kaspanet/documentation/blob/master/Smart%20Contract%20Layer/Smart-Contract-Layer.md)
* [ ] Read the [Phantom GhostDAG Paper](https://eprint.iacr.org/2018/104.pdf) by Yonatan Sompolinsky and Aviv Zohar

If you want to get your hands dirty and play with Kaspa, you may join the Kaspa TestNet and:

* [ ] Create a wallet and send Kaspa with the [Kaspa CLI Wallet](../../try-kaspa/untitled.md)
* [ ] Join the decentralized network and [run a Kaspa node](../../try-kaspa/running-a-node/) or a [Kasparov API Server](../../try-kaspa/api-server/)
* [ ] Be one of the first to [mine Kaspa](../../try-kaspa/untitled-1/).

