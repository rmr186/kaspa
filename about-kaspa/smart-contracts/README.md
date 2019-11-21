# Kaspa Smart Contract Layer

## Introduction

Kaspa is designed as a multi-layered cryptocurrency system with Nakamoto-style security on the base layer and EVM-style capabilities on the application layer. The community needs this new architectural concept because Ethereum is failing to achieve sound money conviction \(due to severe scalability issues, governance woes, and its move to proof of stake\) and Bitcoin is not useful for crypto-finance applications. Our system’s base consensus layer uses PHANTOM, a blockDAG protocol which is a generalization of Nakamoto consensus.

In Ethereum, everyone runs every computation, which compromises its scalability. With layer 2 scaling solutions, only interested parties need to run computations, reverting to the universal-computation main chain only upon a dispute. Solutions differ in how data is committed to the chain in normal operations, what backs/collateralizes the process, and what operations are taken by the base consensus upon a dispute.

In this document we are suggesting a solution design in which:

* Data is recorded on chain \(for data availability and proof of publication\).
* Data is usually computed and interpreted only by interested parties, who also deposit stake as collateral.
* Any dispute will initiate a bisection process which ultimately outputs the individual VM instruction that caused the dispute; the base consensus then runs this individual instruction and resolves the dispute; the stake of the lying party is taken as penalty.

This construction is inspired by Arbitrum, an EVM-compatible tailored VM solution, optimized for dispute days \(which was inspired from Truebit, a simpler WASM-based solution\).

It has many key deviations from Arbitrum which are described in detail in the following document, along with a deep dive into the design of the interaction between the smart contract layer and the base consensus layer.

For more information about PHANTOM, see the original paper \([https://eprint.iacr.org/2018/104.pdf](https://eprint.iacr.org/2018/104.pdf)\) and co-author Aviv Zohar’s presentation at Scaling Bitcoin 2018 \([https://youtu.be/3Hksieg5GdM?t=1803](https://youtu.be/3Hksieg5GdM?t=1803)\).

For more information about Arbitrum, see the original paper \([https://www.usenix.org/node/217514](https://www.usenix.org/node/217514)\) and github \([https://github.com/OffchainLabs](https://github.com/OffchainLabs)\).

For more information about TrueBit, see the original paper \([https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf)\) and github \([https://github.com/TrueBitFoundation](https://github.com/TrueBitFoundation)\).

## High-Level Overview

The smart contract layer is an environment for operating virtual machine instances. A virtual machine instance can be thought of, simply, as a set of interrelated smart contracts that are bootstrapped together and form this instance’s state. Henceforth we refer to these virtual machine instances as "silos".

A silo is validated and secured by a permissionless set of stakers \(or "managers"\)—the consensus layer does not regularly interpret or verify silo data. The stakers use the base layer consensus only for ordering silo transactions and resolving disputes. If a dispute regarding the state of a silo arises, a challenging process is initiated on chain, which ultimately produces a concise representation of the dispute that the consensus layer can easily verify.

### Base Layer Roles

The base layer provides native support for smart contract consensus: it supports staking, dispute resolution, transfer of transaction fees to silos, and more. Crucially, it supports two-way pegging, rendering the native blockDAG currency as the financial railing of all financial applications in the smart contract layer. We use the term "bridge" when referring to base-layer components which are built to support the smart contract layer.

### Two-Way Pegging

In order to support two-way pegging, silo managers publish short commitments to silo state on-chain. This commitment is in the form of an accumulated hash to full silo state. Any silo manager can validate this commitment against its own accumulated hash; if the hashes are different, the manager can challenge the false commitment and start a dispute process. The bridge’s main responsibility is to identify the correct commitments and to achieve _finality_ regarding these commitments and their outcomes. Once a commitment is finalized, i.e. it was agreed by all honest silo managers, it can also be trusted for unpegging currency from silo back to base-layer.

## Deviations From Arbitrum

### Permissionless Manager Set

We design a system where the manager set for a silo is completely dynamic and permissionless and requires only putting stake as deposit for participation. This means successful maintenance of a secure silo relies on free market forces influenced by silo-specific incentivization policy and on community interest in the silo.

This is unlike the current Arbitrum design, where the set of managers for each silo is fixed \(i.e., a known group of managers defined at silo initialization\). The main motivation for Arbitrum’s design choice is that unanimous off-chain agreement can be easily achieved between all known managers, which is beneficial since it avoids the need for frequent on-chain communication and reduces the dependency on speed of the consensus mechanism. However, our DAG-based consensus layer provides high throughput and fast confirmation times and is less of a concern, and so we opt out of this design in favor of a dynamic market-driven system.

### Finality Rules

In relation to permissionless silo governance, we also extend the finality rules to incorporate active confirmation of silo execution, taking into account the diversity of the dynamic manager set. This is as opposed to Arbitrum’s model which relies only on passive confirmation as time passes. Active confirmation is provided by uniformly-chosen random managers making subsequent commitments and hence actively confirming correctness.

Additionally, we design a set of rules for managing complex situations of multiple disputes along the commitment chain, which may indicate a coordinated attack. The system is designed such that the attack does not delay the progress of the silo, but only affects its finality. Details are specified in the “Detailed Description” section of this document.

### UTXO Base Layer

While Arbitrum is built on Ethereum, we opt for a more scalable base layer, a blockDAG consensus layer \(specifically, PHANTOM\) that inherits the security of Nakamoto’s UTXO-based design. Over this UTXO layer we design a mechanism for the smart contract layer of unpegging/pegging from/to native UTXO coins; Arbitrum is a more abstract design and so lacks such base-layer dependent specific details. Additionally, in our system the blockDAG is the only reference for content and order of input transactions, and no explicit message inbox is maintained and used by silo managers.

## Proof of Stake vs. Staking

Proof of stake suffers from inherent flaws, stemming from its coupling of economics and consensus, rendering it unsuitable for base-layer consensus. In contrast, we use the process of staking in a very restricted and defined manner: staking is used for incentivizing correct commitments to the silo state and strongly disincentivizing incorrect commitments. It is explicitly not used for ordering events and reorg resistance, and, as described below, the staking protocol’s security does not rely on the honest majority assumption that proof of stake does.

### The Any-Trust Model

In contrast to proof of stake, the security of a smart contract silo relies on the dispute mechanism. The ability to use miners as referees in case of a dispute, allows us to rely on the any-trust assumption: It suffices to assume that a single honest manager exists and is online to provably finalize silo state commitments. This stems from the fact that any honest manager can use the dispute mechanism to challenge and penalize any false claim, hence by remaining silent an honest manager passively confirms correctness of claimed state. Below we discuss in more detail the exact criteria required to reach _state finalization_ in this model.

## Detailed Description

In this section we expand on the details of the smart contract bridge and the smart contract platform, including some aspects of the virtual machine design.

### Commitments

A commitment to silo state needs to specify the state it started executing from and the set of transactions it processed. We therefore require that each commitment points to a previous commitment which represents the state it started executing from, and that it specifies a reference block in the base-layer's DAG, meaning it includes all silo transactions from the previous commitment to the reference block.

#### Base-Layer DAG

To simplify the discussion we abstract the base-layer's DAG structure into a _blockchain_ by looking only at the _selected chain_ of the DAG. When referring to "reference block transactions" we actually mean "the set of all transactions accepted by the reference block". Commitments to a silo will only use reference blocks from the selected chain. This simplifies commitment management by providing a clear point of reference for synchronizing commitments made by different managers. Since silo commitments must be executed sequentially anyway, there is no apparent serialization cost by restricting commitments to the selected chain; high throughput is still achieved by parallel transaction mining and due to multiple silos operating independently in parallel.

#### Tree Structure

When a silo is created, it defines its initial state and provides a commitment to it. We refer to this initial commitment as the _genesis_ commitment for this silo. Following the genesis commitment, any other commitment must point at some previous commitment as its logical ancestor, thus forming a tree structure. A fork in this tree means that two or more commitments in the tree are pointing to the same parent commitment. A fork can occur if \(i\) two _identical_ commitments were made by two different managers, in which case we can simply treat them as one; \(ii\) two different _unsynced_ commitments were made \(i.e., pointing at different reference blocks\), in which case we must provide a mechanism to eventually sync and logically unify them; and \(iii\) two different _synced_ commitments were made, which means the two committing managers disagree regarding silo state and the disagreement must be resolved by a dispute resolution process.

At times of normal execution, when no malicious managers are involved, the commitment tree is expected to immediately converge into a chain. If an attack occurs, the dispute process will allow us to eventually ignore and abandon the false commitment branches. A commitment node is hence considered finalized when it has no siblings in the tree and finality confirmation criteria have been reached for it. Finality rules imply that a commitment is finalized only if all its ancestors are finalized, thus forming an ever growing finality chain at the upper part of the tree, starting from the genesis.

#### Logical Fork Merging

In the following sections we describe the lottery mechanism by which a silo manager "wins a ticket" to perform a commitment \(and to get a fee for its work\). For now it suffices to say that lottery should result in a single commitment per reference block in expectation. However, we must deal with the cases where no commitments are made for a reference block \(i.e., no winning ticket\), or multiple commitments were made \(multiple simultaneous winners\).

To address these cases, rather than sending a single commitment for the current reference block only, a commiting manager should explicitly specify commitments to all not-yet-committed-to reference blocks. More formally, we define the following commitment rule:

* Scenario: A manager wins a ticket for committing to reference block _j+k_ and the last commitment transaction it locally observes in the DAG is for reference block _j_.
* Action: the manager reports _k-1_ commitments starting from reference block _j+1_ and ending with reference block _j+k_.
  * There is no need to report a commitment for a reference block if it contains no transactions for this silo. 

This way, when commitment transactions are received on-chain, any gap in the commitment chain can be immediately filled, and identical commitments can instantly be identified and merged in the tree.

If, however, two synced commitments are found to be different, we must use the dispute process. The management of forks in the tree data structure in such cases is described below.

### Finality

Commitments are confirmed by the fact that no honest manager successfully challenged their correctness. This requires some sort of finality criteria. That is, when is a commitment considered finalized such that we can apply its implications \(such as unpegging currency\) back to the base layer.

#### Passive Confirmation \(Minimum Time\)

Essentially, this means that finalization is mainly a function of time passing. We require that a minimal amount of time passes, so that honest managers get the chance to challenge malicious commitments, henceforth _passively_ confirming it if they do not.

#### Active Confirmation \(Minimum Distinct Stake\)

Commitments form a tree structure by pointing to previous commitments. By pointing to a previous commitment, the following commitment declares it confirms it, and can possibly lose its stake if that previous commitment is challenged. We use a policy where commitments can only be made according to a uniform lottery. This way, as more distinct, uniformly-chosen managers _actively_ participate in a specific commitment branch, our confidence in that branch increases and we can thus finalize faster.

#### Maximum Time

Finality should eventually be reached also at times with no silo activity if enough time has passed.

### Staking

To incentivize correct execution of a silo, miners must be able to penalize \(or reward\) silo managers for making \(or challenging\) false commitments. To support this, we define a staking mechanism in the base layer. Staking is also used to enforce an economically fair lottery for making commitments to a silo.

#### Staking unit

When a silo is created it specifies in its configuration a _staking unit_ \(in the native currency\). This staking unit reflects an amount of currency that will be used to penalize false commitments and is high enough to discourage malicious behavior for this silo.

#### Staking transaction

The staking transaction is a special transaction for locking native coins for the purpose of participating in silo management. The output for a staking transaction is in the form of a wrapped staking token named USTO \(Unspent Staking Transaction Output\), which holds the value of a single staking unit and grants permission to perform commitments/challenges within the silo. The staking transaction receives as input a target silo and native UTXO coins, and outputs one or more USTOs according to the pre-configured staking unit of the target silo \(i.e. \# USTO outputs &lt; sum of UTXO inputs / staking unit\).

A USTO token is the primary input for commitment transactions. USTO tokens should be included in the UTXO set of the DAG, however, they are not considered "spent" when used for commitments. The token should be considered spent \(and thus extracted from the UTXO set\) only when being slashed following a false commitment or when requested to be unlocked by its owner. To unlock its stake, a silo manager must call the _unlock stake transaction_. The unlocking should be rejected if the specified token was recently used by any not-yet-finalized commitment.

#### Lottery

Staking tokens are also used for managing a lottery of commitment "tickets". For each token and for each reference block we define a lottery over the combined hash of the token and the reference block. The token wins the lottery if the resulting hash is small enough. More specifically, the lottery function is defined as the result of the following predicate:

![equation](https://latex.codecogs.com/gif.latex?%5Cfrac%7B%5Ctext%7Bxor%7D%28%5Ctext%7Bhash%7D%28%5Ctext%7Btoken%7D%29%2C%20%5Ctext%7Bhash%7D%28%5Ctext%7Bblock%7D%29%29%7D%7B%5Ctext%7Bmaxhash%7D%7D%20%3C%20%5Cfrac%7B1%7D%7Bn%7D)

where _n_ is the overall number of current staking tokens for this silo. This lottery results in a single commitment ticket for each reference block in expectation \(a _Binomial_ distribution with _n_ trials and _p = 1/n_\). Overall, the chance to win a ticket grows linearly with the number of staking tokens one holds. In the long run this results in a fair share of commitment fees to all managers, where each manager earns fees proportionally to the number of tokens he owns, i.e. proportional to the amount of stake it proved to have and was willing to lock as deposit.

### Dispute Management

At any time, a commitment to silo state can be challenged by any other interested party. Commitments can only be challenged while not yet finalized. The challenger must provide stake for backing up his challenge. This stake can be in the form of an already existing staking token or with native coins \(equal to a staking unit\), in which case the challenging transaction should output a staking token for future usage/unlocking. Once a challenge was declared and processed by the base layer, a dispute process begins. Following the dispute process rules, which will be described below, one of the arguing parties loses and its stake is slashed. The slashing must be initiated by the winning party as a special _request slashing transaction_ which should "spend" the loser’s staking token, pay the winner half a staking unit in native coins, and burn the other half.

A challenge must contain, of course, an alternative commitment to silo state. This commitment immediately creates a fork in the commitment tree. Alternatively, a dispute can begin when two conflicting commitments are made. In this case, we consider both commitments as pending until one of them is challenged.

Challenges to a commitment should be made within the finality time window. If multiple challenges are made within this time, the challenged manager must address them one-by-one, until he loses once or wins them all. In order to decide what order challenges should be addressed, we follow the consensus order provided by the base layer \(i.e., by the consensus order of challenging transactions\).

Technical note: the process of managing the commitment tree and the disputes is always "on demand". The miners never initiate a test for non-responsiveness of challenged parties, etc. Rather, the miners merely respond to events such as "committer _x_ should have responded to my challenge according to consensus order, but did not respond for _y_ time; please slash his staking token for my benefit".

#### Multiple Challenges

Two cases to consider if multiple challenges are made:

* All challengers make the same commitment: In this case, if the first challenger wins, there is no need to run additional disputes. However if it loses, the other challengers should get a chance to defend their claim as well.  
* Challengers make different commitments: In this case, we must run multiple disputes to eliminate all false commitments. 

#### Commitment Branch

A challenged commitment may already have a commitment chain following it. All following committers to this chain implicitly agree about all previous commitments. Hence, if a commitment is challenged and loses, all participating managers should be slashed as well.

However, even if the commitment loses a dispute, this can be an _impostor attack_ by which the committer deliberately loses the dispute although the commitment is indeed correct. To address this we must allow any other following committer to stand and defend the commitment as well. Likewise, any losing commitment can be reclaimed by any other manager willing to challenge the former challenger.

### Dispute Process

Once a dispute process begins between two parties, we use a dispute data structure to follow the state of the interaction between them. The dispute proceeds by the challenged manager \(the "prover"\) sending the overall number of instructions it executed, _T_, and _c-1_ commitments representing intermediate silo states at _c-1_ predefined locations with _T/c_ instruction intervals between each. Note that _c_ is a parameter which affects the overall number of interactions required to resolve the dispute \(_log\_c\(T\)_ interactions\), and is a predefined parameter which depends on the delay between rounds and the cost of large vs. multiple transactions. The challenger then points at the first false commitment of the _c_ commitments, and the process continues recursively within the interval between the false commitment and the one before. This recursive logic continues until the challenger points at two consecutive commitments at times _t, t+1_ which are the exact points of conflict. The prover then needs to send a proof \(see sections 4.2 and 4.3 of [Arbitrum’s paper](https://www.usenix.org/system/files/conference/usenixsecurity18/sec18-kalodner.pdf) for a detailed explanation of such “one-step proofs”\) showing the correct execution of that step. If the proof is correct, the prover wins the dispute, otherwise the challenger wins. Each of the parties must respond at his turn within a specified time period, otherwise he loses.

All dispute interactions are performed as special on-chain transactions. The data sent should be saved in the dispute data structure so that the ending proof can be validated against prior commitments made.

### State Commitment and Silo Outbox

While executing silo function calls, the user may request, via the smart contract logic, to perform base-layer transactions. Such transactions may include transferring coins back to the base-layer \(unpegging\), or making function calls to other silos \(composability\). We call such transactions the "output transactions" of a silo function call. Output transactions can only be published on chain when the commitment including their initiating function call reaches finality. We now describe the process of post-finality publication of these transactions.

During function execution, any output transaction is appended to a dedicated list within the silo. When finished processing a group of function calls \(all calls from a specific reference block\), the hash of all output transactions is combined in a Merkle tree with the hash of the silo's memory space, and together they form a "commitment" to silo state. Additionally, this list of transactions is appended to a pending-finality transaction queue \(the "outbox"\) to be published on-chain when the corresponding commitment is finalized. Upon reaching finality, these transactions are dequeued from the outbox and published on-chain referencing the originating commitment. The published transaction must include a Merkle witness showing that the operation was indeed included in the claimed commitment.

The responsibility for publishing outbox transactions should be on the managers who made the commitment \(or committing manager with the lowest lottery hash, to avoid the possible redundant double publication\). These managers have an incentive to publish the transactions, since they include their own commitment fees. If the committing managers are offline at the time finality is reached, we can move the responsibility on to managers who made subsequent commitments.

### Pegging and Unpegging

#### The Silo Native Account

Each silo includes a special native account. This native account functions as a money pool representing all shared resources pegged to silo control, where the internal partitioning of this pool is managed within internal silo state. This account also functions as a bridge for transferring fees from miners \(when mining silo function calls\) to silo managers \(once commitments to those function calls are finalized\).

Sending currency _to_ this account is as simple as any currency transfer \(signed by sender to public address of the account\); however, sending currency _from_ this account back to the user requires that all silo managers agree with the transaction \(any-trust model\), which is accomplished when silo state commitments reach finality as described above.

To make sure the silo account has no private key which can be used to sign transactions without involvement of silo managers, we can either:

* Use a special type of address which is inherently different from usual user addresses and is processed differently; or
* Use a usual public address, but make sure it is generated in such a way that no one knows a private key for it.

#### Pegging

Pegging is the process of transferring currency from base-layer to silo control. From the base layer's perspective it's a simple signed transaction passing native coins from a user address to the public address of the silo. However this transfer must be accompanied with a special silo function call for reporting the transfer to the internal state of the silo. In order to link the two operations atomically, we must use a special transaction that combines the native coin transfer with a silo function call in its payload for reporting the currency deposit.

#### Unpegging

Unpegging is the reverse operation of sending native currency from the silo account to a user account. As explained above, unpegging requires waiting for finality of the initiating commitment and thus passes through the outbox of a silo.

There is, however, a challenge in managing such unpegging transactions. For instance, consider a basic scenario where a silo manages an account-based model and the user would like to pull currency from its internal silo account to its native public address. The user calls a function in the silo's contract for requesting the operation. However, in order to create a native transaction, the silo contract code must access UTXO coins, which are associated with the silo’s native account, to use them as inputs. This is problematic since it requires access to data external to the silo state \(to enforce deterministic execution which is required for the dispute mechanism, I/O should not be possible within a silo\). Another problem is that those coins must be "locked" until the transaction is actually published on-chain post finality.

Instead of preparing actual transactions in the outbox, the outbox operation should only include the required metadata for the operation. This should be in the form "user _x_ should get paid _y_ coins from silo account". Later, when finality is reached, a manager can group all these operations into a single native _unpegging transaction_, passing currency from the silo to the list of specified users with the specified amounts, along with the corresponding witnesses to the source commitment. At this stage, the manager can easily access any required UTXO inputs, since this step is not executed from within silo contract code. To prevent double spending of such a transaction \(replay\), a boolean field attached to each finalized commitment should indicate if its unpegging transaction has already been processed. Another option would be to manage an increasing nonce for each silo for tracking all unpegging transactions.

### Commitment Fees

Each commitment to silo state should reward the manager making the commitment with a fee \(once the commitment is accepted as finalized\). The fee should be collected from the fees paid by users when publishing their function call transactions which were included in the commitment.

#### Fee Funding

When a miner adds a function call transaction to a block it should push half of the fee for the transaction to the corresponding native silo account \(grouped for all transactions in the block\).

#### Fee Destination

Eventually, the fee should be equally split between managers/challenger which committed the finalized commitment. Usually, the manager\(s\) publishing the commitment would be those who won the lottery tickets to commit. However when challenges are involved, the fee should be paid to the winning challenger \(although he had no ticket to commit\).

#### Fee Transaction

Although fee payment is essentially an unpegging operation \(pulling currency from the silo account\), unlike usual unpegging transactions, the manager cannot explicitly state the destination for the fee in the outbox operation. This is because \(i\) specifying his own address will render the commitment different for each manager \(since outbox operations are part of state commitment\), and \(ii\) the amount of fee to be paid depends on the number of managers making the same commit \(i.e. \# winning tickets\), which is unknown in advance. Instead, the operation should only specify the amount of fee to be paid without the destination. Following finality, a fee payment transaction should be published with a witness to the fee amount, and the fee should be equally split between all committers. This fee payment can also be part of the single unpegging transaction for that commitment, it just differs from other payments in the transaction by the verification mechanism used.

### Composability

Outbox transactions may contain function calls to other silos. Replay attacks and authorization are the concerns of the receiving silo. This means that function calls to other silos may be published before finality \(some function calls may include pegging native coins to the other silo and hence require post-finality\).

### Base-Layer Reorg

Silo finality is logically "internal" to base-layer finality. This means a base-layer reorg may undo operations which were recorded on-chain post silo finality. This happens if the reorg affects the validity of the source commitment. In order to be able to reverse back silo state in case of a reorg, managers must keep checkpoints of the complete silo state backwards in time to a point where base-layer finality was already achieved. See details below.

### Syncing

Syncing to silo state requires that one or multiple managers holding the complete silo state must send it to the interested incoming manager. The receiving manager can check the validity of the state by hashing and comparing to on-chain silo commitments. The new manager should avoid making commitments to the silo until the state he received was proven as finalized. Another option is that he receives a checkpoint from a point at time which has been finalized already, so he can re-execute from that point on his own and make commitments asap.

### Checkpoints

Silo state must be periodically saved in checkpoints to allow recovery from base-layer reorgs. At the very least, we must keep a single checkpoint with the following properties: \(i\) the checkpoint is for some state in the past which already reached base-layer finality, and \(ii\) all transactions in the DAG from that step on are still available, so we can replay the execution according to the new order. If the selected chain of the DAG gets modified often by slight reorgs , we should also keep checkpoints which are much closer to the current time.

### Virtual Machine

The virtual machine is the execution environment for the silo's smart contracts. The environment consists of a memory space representing silo state, smart contracts representing silo logic, and an interpreter for executing contract functions over silo state. In order to function properly over the base-layer bridge and to support the anytrust model, the machine needs to \(i\) have completely deterministic execution, so distinct distributed managers can agree on its state; \(ii\) support hashing its entire state to a constant-size hash which will be used by managers to make on-chain commitments; and \(iii\) be able to provide proof for correct execution of a single step of the machine. Below we extend on these requirements.

#### Deterministic Execution

The virtual machine must be completely deterministic. That is, every code execution must lead to the exact same results, with out dependency on the actual environment/OS/node the machine is running on. To ensure deterministic execution:

* I/O should not be supported within silo code, since access to external data may lead to non-deterministic results. Silos may receive external data only through function call transactions which are agreed upon by base-layer consensus and can be validated by miners at dispute times
* Usage of floating point operations etc. may be restricted 
* Randomization can be allowed only if fixed seeds are used

#### State Commitments and One-Step Proofs

The machine needs to support hashing of its memory. That is, the memory space needs to be hashed so that managers can make commitments to the machine state on chain. The design must support the following:

* Hash of memory space between function calls; this is for ongoing peacetime commitments on chain
* Hash of memory space during function execution, including runtime memory such as the program counter/call stack/heap/etc.; this is for dispute times, where conflicting managers are negotiating to find the exact point of disagreement

During a dispute, the bisection process results in two consecutive commitments made by the prover, where the challenger agrees with the first commitment but claims the second one is wrong. At this stage, the prover needs to provide a proof of correct execution of this single step. To support such one-step proofs, we need the following:

* Hash needs to be such that a witness can be provided to a specific memory location \(Vector Commitment, supported by Merkle trees\)
* PC \(program counter\) and other data segments \(such as the data stack\) must have a predetermined known memory location 

The one-step proof will work as follows: 1. The prover reveals the content of the PC with a witness According to PC, miners read the next instruction to execute from the smart contract binaries 2. According to this instruction the prover reveals other data segments, for instance if the instruction to execute is "add", the prover will read the location of the stack head and the content of the two top values in the stack. 3. The miners can now validate that only the correct data was modified and that the following state commitment is consistent with the executed instruction

#### Peacetime Commitments vs. Dispute Time Negotiation

We make the following observation which can help optimize machine execution at peacetime \(when no dispute occurs\). Naturally, a virtual machine environment should consist of long-term persistent memory \(a "DB"\), and short-term runtime memory \(the "RAM"\). Long-term persistent memory should hold silo entities with durable state \(e.g., user accounts, global state\). The machine executes smart contract function calls using the run-time memory space, which includes the call stack, data stack, registers, program counter, etc., and updates entities in the persistent memory whenever required.

It is possible to distinguish between state commitment during usual execution \(peace days\), and at times of dispute. At peacetime, the machine needs to support incremental hashing only for the persistent memory space, which will be used to commit to silo state on chain. If _w_ is the number of writes to persistent state and _T_ is the overall number of executed instructions, it is reasonable to assume that _w &lt;&lt; T_, so the number of hash updates is relatively small.

During dispute times, we need to narrow the conflict down to a single instruction, so the silo needs to support commitments to the runtime memory space as well. This means that during dispute negotiation the machine must incrementally update the hash value of _all_ memory spaces following every instruction \(note that writes to runtime memory are in the order of _T_\). The cost for each such incremental hash update should be at _most_ logarithmic in the size of the space.

