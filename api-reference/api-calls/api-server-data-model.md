---
description: 'Describing the API Server data types, database schema, etc.'
---

# API Server Data Model



## List of API Server Response Types

All hashes and IDs are hex-encoded SHA-256 hashes, unless specified.

### Transaction Type

```text
{
    TransactionHash            // string   Hex-encoded hash
    TransactionID              // string   Hex-encoded hash
    AcceptingBlockHash         // string   Present only if transaction is accepted
    AcceptingBlockBlueScore    // int        Present only if transaction is accepted
    SubnetworkID               // string   Hex-encoded RIPEMD160 hash
    LockTime                   // int      
    Gas                        // int      Present only if SubnetworkID is not the native subnetwork
    PayloadHash                // string   Present only if SubnetworkID is not the native subnetwork
    Payload                    // string   Base64-encoded data. Present only if SubnetworkID is not the native subnetwork
    Inputs                     // Array of TransactionInput
    Outputs                    // Array of TransactionOutput
    Mass                       // int 
}
```

### TransactionOutput Type

```text
{
    TransactionID             // string   Hex-encoded hash. Omitted when part of a Transaction object
    Value                     // int
    ScriptPubKey              // string   Base64-encoded data
    Address                   // string   Bech32-encoded address. Present only if ScriptPubKey is P2PKH or P2SH
}
```

### TransactionInput Type

```text
{
    TransactionID                   // string   Hex-encoded hash. Omitted when part of a Transaction object
    PreviousTransactionID           // string
    PreviousTransactionOutputIndex  // int
    ScriptSig                       // string   Base64-encoded data
    Sequence                        // int
}
```

```text
{
    BlockHash                // string    Hex-encoded hash
    Version                  // int        
    HashMerkleRoot           // string    Hex-encoded hash
    AcceptedIDMerkleRoot     // string    Hex-encoded hash
    UTXOCommitment           // string    Hex-encoded hash
    Timestamp                // int        Unix-timestamp. Seconds since 01/01/1970    
    Bits                     // int        
    Nonce                    // int        
    AcceptingBlockHash       // string    Hex-encoded hash. Present only if this block is accepted
    BlueScore                // int        
    IsChainBlock             // bool        
    Mass                     // int         
}
```

### FeeEstimate Type

```text
{
    HighPriority                   // int        
    NormalPriority                 // int        
    LowPriority                    // int        
}
```

{% hint style="info" %}
* HighPriority is the estimated fee, for the transaction to be included in the next 10 blocks, with a probability of 90%.
* NormalPriority is the estimated fee, for the transaction to be included in the next 100 blocks, with a probability of 90%.
* LowPriority is the estimated fee, for the transaction to be included in the next 300 blocks, with a probability of 90%.

All values are in Satoshi per transaction gram.  Numbers are bound to change during TestNet.
{% endhint %}



## Database Schema

### Blocks Table

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

### ParentBlocks Table

One-To-Many relation table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| ParentBlockID | int64 | Index, References Block\(ID\) |

### AcceptingBlocks Table

One-To-Many relation table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| AcceptingBlockID | int64 | Index, References Block\(ID\) |

### RawBlocks Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| BlockID | int64 | Primary Key, References Block\(ID\) |
| BlockData | blob |  |

### Subnetworks Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Ke |
| SubnetworkID | string | Index |

### Transactions Table

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

### TransactionsToBlocks Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| TransactionID | int64 | Primary Key |
| BlockID | int64 | Primary Key |
| Index | int | Index |

Note: Primary Key is compound

### TransactionOutputs Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| TransactionID | int64 | Index, References Transaction\(ID\) |
| Index | uint32 |  |
| Value | uint64 |  |
| PkScript | blob |  |

### TransactionInputs Table

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| ID | int64 | Primary Key |
| TransactionID | int64 | Index, References Transaction\(ID\) |
| TransactionOutputID | int64 | Index, References TransactionOutput\(ID\) |
| Index | uint32 |  |
| SigScript | blob |  |
| Sequence | uint64 |  |

### UTXOs Table

Stores the current UTXO-Set. Updates whenever there's a chain update.

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| TransactionOutputID | int64 | Primary Key |
| AcceptingBlockID | int64 | IndexOperations |

API Server Examples

