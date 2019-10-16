---
description: Dedicated server-run tools for interacting with the Kaspa networking
---

# API Server Reference

Server API is a dedicated server providing a set of tools for exploring the Kaspa Ledger.  The Server API  does not currently offer all of the tools offered by our JSON-RPC API \(e.g. validation, mining functions, etc.\) 

{% hint style="info" %}
Please make sure you are referencing the Server API code, [as opposed to the JSON-RPC API reference](../object-types/).  JSON-RPC API calls can be made by any authenticated JSON-RPC client without running a dedicated serer.
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

## Operations

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

### Block Type

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
All values are in Satoshi per transaction gram.  
{% endhint %}

{% hint style="info" %}
Numbers are bound to change during TestNet.
{% endhint %}

* HighPriority is the estimated fee, for the transaction to be included in the next 10 blocks, with a probability of 90%.
* NormalPriority is the estimated fee, for the transaction to be included in the next 100 blocks, with a probability of 90%.
* LowPriority is the estimated fee, for the transaction to be included in the next 300 blocks, with a probability of 90%.

## List of API Calls

{% hint style="info" %}
All API calls return 200 OK if no error was encountered.
{% endhint %}

If an error occurs, an error code is returned with the following body:

```text
{
    ErrorCode      // int      HTTP error code
    ErrorMessage   // string   Human-readable description of what went wrong
}
```

{% api-method method="get" host="https://api.kas.pa" path="/v1/transaction/id/{txid}" %}
{% api-method-summary %}
transaction/id
{% endapi-method-summary %}

{% api-method-description %}
Searches for a transaction by its ID.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="txid" type="integer" required=true %}
64bit integer for requested transaction ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

    TransactionHash            // string   Hex-encoded hash
    TransactionID              // string   Hex-encoded hash
    AcceptingBlockHash         // string   Present only if transaction is accepted
    AcceptingBlockBlueScore    // int      Present only if transaction is accepted
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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No trasnaction with the given txid was found
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
The given txid is not a hex-encoded 32-byte hash
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/v1/transaction/hash/{txhash}" %}
{% api-method-summary %}
transaction/hash
{% endapi-method-summary %}

{% api-method-description %}
Searches for transaction by its Hash
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="txhash" type="string" required=true %}
Hex-encoded hash
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
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
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No transactions for the given txhash was found
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
The given txhash is not a hex-encoded 32-byte hash
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/v1/transactions/address/{address}\[?skip=txid&limit=100\]" %}
{% api-method-summary %}
/transactions/address
{% endapi-method-summary %}

{% api-method-description %}
Searches for all transactions where the given address is either an input or an output.    
Returns an array of transactions.  
Sorted by accepting block date ascending.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="skip" type="integer" required=false %}
skips all transactions up to and including the transaction with given txid.  
Ignored if txid is not found or not related to given address. 
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="integer" required=false %}
response limited to this number of transactions per response \(default=100, maximum is 1000, if limit is set to more than 1000, it will be set to 1000\)
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
Bech32-encoded address
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This call returns an array of transactions, each with the following structure:
{% endapi-method-response-example-description %}

```
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
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No transactions for the given address was found.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
The given address is not a well-formatted P2PKH or P2SH address, or the given txid in the skip parameter is not a hex-encoded 32-byte hash
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred.
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/v1/utxos/{address}" %}
{% api-method-summary %}
/utxos/{address}
{% endapi-method-summary %}

{% api-method-description %}
Searches for all unspent transaction outputs for a given address.  
Returns an array of TransactionOutput
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
Bech32-encoded address
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns an array of transaction outputs, based on the following structure:
{% endapi-method-response-example-description %}

```
TransactionID1, Value1, ScriptPubKey1, Address1, ...
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No Block with the given hash found.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
The given hash is not a hex-encoded 32-byte hash.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred.
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



{% api-method method="get" host="https://api.kas.pa" path="/v1/block/{hash}" %}
{% api-method-summary %}
/block/{hash}
{% endapi-method-summary %}

{% api-method-description %}
Searches for a block by its Hash.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="hash" type="string" required=true %}
Hex-encoded hash
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a Block object:
{% endapi-method-response-example-description %}

```
    BlockHash                
    Version                   
    HashMerkleRoot               
    AcceptedIDMerkleRoot          
    UTXOCommitment                
    Timestamp                
    Bits                    
    Nonce                    
    AcceptingBlockHash            
    BlueScore                    
    IsChainBlock                    
    Mass                          
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No Block with the given hash found.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
The given hash is not a hex-encoded 32-byte hash.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred.
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/v1/blocks\[?order=ascending/descending&skip=blockhash&limit=25\]" %}
{% api-method-summary %}
/blocks
{% endapi-method-summary %}

{% api-method-description %}
Searches for all blocks.  
Returns an array of blocks.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="limit" type="string" required=false %}
limits the number of blocks to return, default=25, max=100.
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="string" required=false %}
Skips all blocks up to and including the block with given blockhash.  Ignored if blockhash is not found.
{% endapi-method-parameter %}

{% api-method-parameter name="order" type="string" required=false %}
sorts by given order.  Defaults to descending, limited to \[limit\] results per page \(default=25, max=-100.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns an array of blocks, each with the following structure:
{% endapi-method-response-example-description %}

```
    BlockHash                
    Version                   
    HashMerkleRoot               
    AcceptedIDMerkleRoot          
    UTXOCommitment                
    Timestamp                
    Bits                    
    Nonce                    
    AcceptingBlockHash            
    BlueScore                    
    IsChainBlock                    
    Mass                          
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No Block with the given hash found.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
The given hash is not a hex-encoded 32-byte hash.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred.
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/v1/fee-estimates" %}
{% api-method-summary %}
/fee-estimates
{% endapi-method-summary %}

{% api-method-description %}
Returns updated fee estimates.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns FeeEstimate type as follows:
{% endapi-method-response-example-description %}

```
HighPriority        
NormalPriority 
LowPriority
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.kas.pa" path="/v1/transaction" %}
{% api-method-summary %}
/transaction
{% endapi-method-summary %}

{% api-method-description %}
posts a raw transaction to the network.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="RawTransaction" type="integer" required=false %}
Base64-encoded raw transaction data
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
-
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
The given transaction is not valid.
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
A server error occurred
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## \(Draft\) List of MQTT Topics

### **transactions/{address}**

Sends a notification of type Transaction whenever a transaction for given address is seen in a new block.

### **transactions/accepted/{address}**

Sends a notification of type Transaction whenever a transaction for given address is accepted.

