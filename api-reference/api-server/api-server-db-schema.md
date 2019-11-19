---
description: List of tables used by the API Server to store data
---

# Database Schema

## Blocks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| BlockHash | string | Index |
| AcceptingBlockID | int64 | References Block\(ID\) |
| Version | int32 |  |
| HashMerkleRoot | string |  |
| AcceptedIDMerkleRoot | string |  |
| UTXOCommitment | string |  |
| Timestamp | dateTime | Index |
| Bits | uint32 |  |
| Nonce | uint64 |  |
| BlueScore | uint64 |  |
| IsChainBlock | bool | Index |

## ParentBlocks

One-to-Many relation table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| ParentBlockID | int64 | Index, References Block\(ID\) |

## AcceptingBlocks

One-to-Many relation table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| AcceptingBlockID | int64 | Index, References Block\(ID\) |

## RawBlocks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| BlockData | blob |  |

## Subnetworks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| SubnetworkID | string | Index |

## Transactions

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| AcceptingBlockID | int64 | Index, References Block\(ID\) |
| TransactionHash | string | Index |
| TransactionID | string | Index. Actual TransactionID as defined by Kaspa Protocol |
| LockTime | uint64 |  |
| SubnetworkID | int64 |  |
| Gas | uint64 |  |
| PayloadHash | string |  |
| Payload | blob |  |
| Mass | uint64 |  |

## TransactionsToBlocks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| TransactionID | int64 | Primary Key |
| BlockID | int64 | Primary Key |
| Index | int | Index |

{% hint style="info" %}
The primary key is compound.
{% endhint %}

## TransactionInputs

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| TransactionID | int64 | Index, References Transaction\(ID\) |
| TransactionOutputID | int64 | Index, References TransactionOutput\(ID\) |
| Index | uint32 |  |
| SigScript | blob |  |
| Sequence | uint64 |  |

## TransactionOutputs

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| TransactionID | int64 | Index. References Transaction\(ID\) |
| Index | uint32 |  |
| Value | uint64 |  |
| IsSpent | bool | Index |
| PkScript | blob |  |

## UTXOs

Stores the current UTXO-Set. Updates whenever there's a chain update.

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| TransactionOutputID | int64 | Primary Key |
| AcceptingBlockID | int64 | IndexOperations |

