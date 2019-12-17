---
description: Introducing Kaspa blockDAG money
---

# Kaspa Overview

## What is Kaspa?

Kaspa is a high throughput, photonic proof-of-work, expressive, sound money.

Kaspa is optimized for sound and scalable store of value, payments, settlements, transfer of digital assets, proof of publication, and smart contracts.

## How Does Kaspa Work?

Rather than storing the distributed ledger in a block_chain_, Kaspa's distributed ledger is stored in a block_DAG_ - a wide, directed acyclic graph \(DAG\) of blocks. Many blocks can be created in parallel. This allows for high block throughput and thus for near-immediate transaction confirmations. In order to order events from blocks created in parallel and restore consensus, Kaspa uses [Phantom ghostDAG](https://eprint.iacr.org/2018/104.pdf), a highly scalable and decentralized blockDAG consensus protocol, that generalizes over the [Nakamoto](https://bitcoin.org/bitcoin.pdf) consensus.

Kaspa intends to use optical proof of work \(OPoW\) - an implementation of traditional proof of work \(PoW\) with analog photonic processors rather than digital silicone processors. An OPoW based consensus retains the same security properties of traditional PoW \(i.e. an attacker needs 51% of the entire network's computational resources\). OPoW mining with Photonic-ASICs is more energy efficient and "green" than traditional ASIC mining. This allows for more sustainable and geographically decentralizing mining. Couple that with the rapid block creation rate of the blockDAG, that means it is much more likely for a miner to find blocks and get their attached rewards. This makes Kaspa mining fair game for smaller miners.

Kaspa enables a[ smart contract](../smart-contracts/) computation layer, inspired by [Arbitrum](https://www.usenix.org/node/217514) and [TrueBit](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf), and decoupled from the consensus layer, allowing the native currency to remain lightweight and scalable, while supporting and enhancing upper-layer use cases, such as stable tokens and decentralized credit systems.

## Next Steps

You are encouraged to read how the blockDAG consensus works and how it generalizes over the Bitcoin Nakamoto consensus.



After getting a feel for Kaspa \(and maybe reading the GhostDAG protocol's white paper\), you will most likely next start getting to know how Kaspa packages work as you start to interact with the Kaspa Devnet.

[Next, try running your own local full node.]()

After you've got your node running, [check out this brief tutorial on node operations]() to learn how to generate transactions.

Finally, you can dig deeper into [Kaspa transactions](), or dive into our [codebase reference](../../reference/kaspa-full-node/code-ref/).

