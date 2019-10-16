---
description: >-
  The API Server provides network data via HTTPS Get calls via a dedicated
  server connected to a Kaspa node.
---

# API Server

### Setting up your own API Server Node

### Connecting to the Kaspa network

### List API Server Calls

### List of API Server Data Structures

## How it Works: Internal Server Operations

### API Server Database Schema

#### Blocks Table

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

#### ParentBlocks Table

One-To-Many relation table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| ParentBlockID | int64 | Index, References Block\(ID\) |

#### AcceptingBlocks Table

One-To-Many relation table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| AcceptingBlockID | int64 | Index, References Block\(ID\) |

#### RawBlocks Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| BlockData | blob |  |

#### Subnetworks Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Ke |
| SubnetworkID | string | Index |

#### Transactions Table

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

#### TransactionsToBlocks Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| TransactionID | int64 | Primary Key |
| BlockID | int64 | Primary Key |
| Index | int | Index |

Note: Primary Key is compound

#### TransactionOutputs Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| TransactionID | int64 | Index, References Transaction\(ID\) |
| Index | uint32 |  |
| Value | uint64 |  |
| PkScript | blob |  |

#### TransactionInputs Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| TransactionID | int64 | Index, References Transaction\(ID\) |
| TransactionOutputID | int64 | Index, References TransactionOutput\(ID\) |
| Index | uint32 |  |
| SigScript | blob |  |
| Sequence | uint64 |  |

#### UTXOs Table

Stores the current UTXO-Set. Updates whenever there's a chain update.

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| TransactionOutputID | int64 | Primary Key |
| AcceptingBlockID | int64 | IndexOperations |

## Internal API Server Operations

### Bootstrapping Procedure

1. API-Server starts with an empty database.
2. API-Server connects to Full-Node through JSON-RPC WebSocket.
3. API-Server subscribes to Block and ChainUpdate notifications. For now storing the notifications without acting upon them until step 4 is complete.
4. API-Server calls GetChainFromBlock\(nil, true\). 
   1. API-Server populates Blocks and related tables using the Blocks part of the response, similar to when receiving a BlockNotification.
   2. API-server converts SelectedParentChain to a ChainUpdateNotification with all the SelectedParentChain blocks as AddedChainBlocks, and then acts upon it as standard ChainUpdateNotification.
5. API-Server acts upon all notifications stored in step 3, and resumes normal operation.

### Restarting After Downtime Procedure

* Same as Bootstrapping, except that in step 4 - call GetChainFromBlock\(B, true\) where B is the last finalized block known to API-Server.
* Since this is a rare event, there is no need for the API server to track what is finalized and what is not.  
* Instead, whenever this is needed, find the finality point using a similar algorithm to that of the full-node.

### On Block Notification Procedure

1. Add the new block to the Blocks table.
2. Add the new block transactions to the Transactions and relevant tables.

### On Chain Update Notification Procedure

1. Iterate over RemovedChainBlockHashes and for each block hash in it: 
   1. Find all UTXOs with AcceptingBlockID = ID of corresponding block.
   2. Find all related transactions.
   3. Remove found UTXOs.
   4. Re-add all UTXOs these transactions removed.
2. Iterate over AddedChainBlocks for each chainBlock in it: 
   1. For each acceptedBlock 
      1. Find all accepted transactions by the AcceptedTxID.
      2. Remove their inputs from the UTXO set.
      3. Add their outputs to the UTXO set.

## API Server Examples



