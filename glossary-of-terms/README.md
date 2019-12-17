# Glossary of Terms

## **Money**

Money is any item or verifiable record that is generally accepted as [payment](https://en.wikipedia.org/wiki/Payment) for [goods and services](https://en.wikipedia.org/wiki/Goods_and_services) and repayment of [debts](https://en.wikipedia.org/wiki/Debt). The main functions of money are : a [medium of exchange](https://en.wikipedia.org/wiki/Medium_of_exchange), a [unit of account](https://en.wikipedia.org/wiki/Unit_of_account), a [store of value](https://en.wikipedia.org/wiki/Store_of_value) and sometimes, a [standard of deferred payment](https://en.wikipedia.org/wiki/Standard_of_deferred_payment). ****Any item or verifiable record that fulfills these functions can be considered as money.

To fulfill its various functions, money must have certain properties:

* [Fungibility](https://en.wikipedia.org/wiki/Fungibility): its individual units must be capable of mutual substitution \(i.e., interchangeability\).
* [Durability](https://en.wikipedia.org/wiki/Durability): able to withstand repeated use.
* Portability: easily carried and transported.
* Cognizability: its value must be easily identified.
* Stability of value: its value should not fluctuate.

#### Creation of money

In current economic systems, money is created by two procedures:

* Legal tender, or narrow money \(M0\) is the cash money created by a Central Bank by minting coins and printing banknotes.
* Bank money, or broad money \(M1/M2\) is the money created by private banks through the recording of loans as deposits of borrowing clients, with partial support indicated by the cash ratio. Currently, bank money is created as electronic money.

## **Medium of Exchange**

When money is used to intermediate the exchange of goods and services, it is performing a function as a medium of exchange. It thereby avoids the inefficiencies of a [barter system](https://en.wikipedia.org/wiki/Barter), such as the "[coincidence of wants](https://en.wikipedia.org/wiki/Coincidence_of_wants)" problem. Money's most important usage is as a method for comparing the values of dissimilar objects.

## **Unit of Account**

A unit of account is a standard monetary unit of measurement of value/cost of goods, services, or assets. It is one of three well-known [functions of money](https://en.wikipedia.org/wiki/Money#functions). It lends meaning to profits, losses, liability, or assets. 

The accounting monetary unit of account suffers from the pitfall of not being a stable unit of account over time. Inflation destroys the assumption that money is stable which is the basis of classic accountancy. In such circumstances, historical values registered in accountancy books become heterogeneous amounts measured in different units. The use of such data under traditional accounting methods without previous correction often leads to invalid results.

## Store of Value

To act as a store of value, a money must be able to be reliably saved, stored, and retrieved – and be predictably usable as a medium of exchange when it is retrieved. The value of the money must also remain stable over time. Some have argued that inflation, by reducing the value of money, diminishes the ability of the money to function as a store of value.

## Inflation

An increase in the quantity of money, leading to a decline in the value of existing money, and thus to an increase in the general level of prices or in the cost of living. In government controlled money, inflation can occur when the central bank prints more money, causing an increase in the quantity of money.

## **Distributed Ledger**

Stemming from the verb _"to lay"_, a ledger was originally a book laying regularly in one place, openly accessible, in which transactions, measured in a unit of account were permanently recorded by date.

A digital ledger is a digital file, or collection of files, or a database storing these records.

The distributed ledger is a digital ledger spread across several nodes \(devices\) on a peer-to-peer network, where each replicates and saves an identical copy of the ledger and updates itself independently, without a central authority. When a ledger update happens, each node constructs the new transaction, and then the nodes vote by a consensus protocol on which copy is correct. Once a consensus has been reached, all the other nodes update themselves with the new, correct copy of the ledger.

## **Consensus Protocol**

## **Bitcoin**

## Blockchain

## **Kaspa**

## **Digital Currency**

## Consensus Stack

## Ethereum

## **Smart Contracts**

## **Turing Complete**

## **Non-Turing Complete** 

## **Bitcoin Script**

## **Proof of Work \(PoW\)**

## Optical Proof of Work \(OPoW\)

## Transaction Output

## Spent Transaction Output

## Unspent Transaction Output

## Directed Acyclic Graph \(DAG\)

Kaspa's consensus layer is a generalization of Nakamoto consensus, where instead of a chain of blocks, the ledger consists of a directional acyclic graph \(DAG\) of blocks. Instead of pointing to one previous block as in the blockchain, in a blockDAG each block points to all previous recent blocks; formally, these are the tips of the block DAG that the mining node observed locally at the time of the new block’s creation.

![Directional Acyclic Graph \(DAG](https://lh6.googleusercontent.com/W-v03qdqQp_1rQsHFz00A5p14z3Bklo3Ag09-a16aJNlXXpbOOEzhCdpTtnhROEO_A9e1TDghXRhTD21wVt4oO9lUhfezsGt6F8NQXSwzmWL-bvwvuMPEp4iPX5zn1U1CwFjHhwT)

## PHANTOM

The natural topology of a [DAG](./#directed-acyclic-graph-dag) already induces a partial ordering over the blocks: if there is a path from block X to block Y in the DAG, then block Y was provably created before block X and so should precede X in the global order. Thus, we only need to define an order over blocks created in parallel, i.e. any set of blocks such that there is no path that connects them.

PHANTOM takes as input a blockDAG and outputs an ordering of its blocks. It does so by identifying a cluster of well-connected \(blue\) blocks and ordering the DAG in a way that favors them over the other \(red\) blocks.

![Depiction of Blue vs Red Blocks DAG](https://lh4.googleusercontent.com/ryec3BWdfGLVasVyG569W7DvvmV5ItBRkv91rLyCK7Ao9m6AutzGcijHdZEmHc5UanV5kp-vPKmV3S_zUdw1kB5bsdnOtpOjJ1vJqZAkqhNd--rEN69bqqK3pAIOLHbpW_t5ec58)

## GhostDAG

While PHANTOM is a group of algorithms, the implementation of some of which are non-Turing complete, GhostDAG is a greedy implementation of PHANTOM that is Turing complete.

## Selected Parent Chain

The Selected Parent chain is the chain with the most blue blocks in its past. Without going specifics of the algorithm that finds the Selected Parent Chain, we will just say that it allows to create a view of the DAG where the blocks are “reordered” as a chain, where each block has branches coming out of it. Every block in the Selected Parent Chain "accepts" all transactions in its branches, forming a structure that is very similar to a linear blockchain.

![Selected Parent Chain \(DAG &#x201C;reordered&#x201D; as a chain with branches\)](https://docs.google.com/drawings/u/0/d/s2wRdG6YtGDVzlwJqko0FHw/image?w=622&h=423&rev=518&ac=1&parent=1ksZXr5a2IldgSNKB8xen9SMEjHM88Siv-RVJa1I-Jv0)





