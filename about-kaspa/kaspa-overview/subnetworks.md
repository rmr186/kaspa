# Subnetworks

Subnetworks is a mechanism in Kaspa's consensus layer that allows nodes with a common interest to form nested networks with their own rules within the greater Kaspa network. The beautiful thing about subnetworks in Kaspa is that the allow parties to run custom code and smart contracts, yet, they do not force all nodes in the Kaspa network to spend computational resources on verifying subnetwork-specific data and code, unless they want to.

To enable subnetworks, Kaspa introduces three new special fields inside the transaction:

* `subnetwork` is the subnetwork ID. You may think of it as a transaction type.
* `payload` is a field to store arbitrary data, and code relevant to the subnetwork.
* `payloadHash` is the hash of the arbitrary data in the field `payload`.

{% hint style="success" %}
While the `payload` field is part of the Kaspa transaction hash, the `payloadHash` field is part of the transaction hash. This allows nodes uninterested in certain subnetworks to ignore the payload of transactions in those subnetworks.
{% endhint %}

## Kaspa Reserved Subnetworks

There are three predefined subnetworks in Kaspa:

* `subnetwork 0` - Kaspa native subnetwork
* `subnetwork 1` - Kaspa block rewards subnetwork
* `subnetwork 2` - Kaspa registry subnetwork

Q: Are there reserved subnetworks, e.g. for stable Kaspa and WoT networks?

### Kaspa Native Subnetwork

The Kaspa native subnetwork is used to send all native Kaspa transactions. Native Kaspa transactions do not use the payload field, and this field must be empty.

### Kaspa Block Rewards Subnetwork

The Kaspa block rewards subnetwork is used to create transactions that pay block rewards to miners. Block rewards include coinbase transactions \(minting of new Kaspa when discovering new blocks\) and transaction fees. In Kaspa, the block rewards for a given block are paid to their creating miner by later blocks. At the time of creating a block, it is not known if some included transactions were also included in a parallel block, and which block comes first according to the PHANTOM ghostDAG consensus. Thus, the exact transaction fee to be paid to the miner is unsettled. Therefore, instead of the traditional way where each block charges the fees of its included transactions, it specifies in the `payload` field the public address to pay transaction fees to. You can think of it as, a block telling the next block that points to it "pay my block rewards here". In order for blocks to be valid, they must pay the required fees to blocks they point to.

### Kaspa Registry Subnetwork

The Kaspa registry subnetwork is used to create new subnetworks. In order to create a new subnetwork, one needs to makes a transaction on `subnetwork 2`; the resulting hash of this transaction is the new subnetwork ID. When creating the registration transaction, the `payload` field should include special instructions for the new subnetwork, such as gas limit per block.

In addition to the existing block size limit, every subnetwork has a gas limit: an upper limit on the sum of the claimed gas of all of that subnetwork's transactions witin a block. In other words: the subnetwork's gas limit is an instruction by the creator of the subnetwork to all miners, not to include transactions \(of their subnetwork\) with a gas sum higher than the subnetwork's gas limit within any block.

This agreement is in place to limit miners from including transactions and charging their corresponding fees in their blocks, that the subnetworks will not be willing nor capable to handle. Miners want to insert as many transactions to each block as possible, in order to get more fees. Subnetworks on the other hand don't want blocks to include too many transactions because they cannot necessarily process more than a certain amount of transactions per block, because each of them involves a cetrain gas expenditure as a result of running the code included in the `payload` field. Therefore the subnetworks instruct the miners to limit transactions of their subnetwork in any block to the gas limit. If miners break this agreement, they will be banned from the network.

