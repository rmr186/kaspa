---
description: Community project requirements
---

# Block Explorer

The purpose of a Block Explorer is to allow searching for blocks, transactions, addresses, and arbitrary data in the blockDAG.

### A DAG-based Block Explorer: Major Differences from Blockchain

Because of the nature of a block-DAG topology, there are some major differences that should be accounted for when designing the database schema for a DAG-based block explorer.

1. Each block may point to several previous blocks as opposed to one previous block.
2. Each block is either in the Selected Parent Chain or not.
3. Each chain block has a list of other blocks that it accepted; and thereby, each block has an accepting chain block.
4. New blocks and Red blocks necessarily don’t have an accepting block.
5. Each transaction might appear in multiple blocks as opposed to one block.
6. Each transaction is accepted \(a new “property”\) by zero to one block \(zero in case of double spend\).

#### Block Explorer Duties

The block explorer shall have the following pages:

1. Recent Blocks - Returns a list of recent blocks.
   1. Listen to new blocks info after loading the page.
   2. Show the number of new incoming blocks as a sticky notification at the top of the blocks list, when clicked populate their info in the recent blocks list \(with indication what is new\)
2. Recent Transactions - Returns a list of recent transactions.
3. Block Info - Returns a specific block’s info.
4. Address - Returns an address’s info and a list of transactions to and from it.
5. Transaction Info - Returns a specific transaction’s info.
6. The block explorer shall demonstrate the high block creation rate of the blockDAG network, by showing an ever-quickly-growing number of new blocks since last refresh.

#### What’s not in this document \(yet\)?

* Details of each of the above pages, input, output, layout and design specifics.

