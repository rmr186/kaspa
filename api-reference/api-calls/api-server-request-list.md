# API Server Request List

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

{% endapi-method-response-example-description %}

```

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

{% endapi-method-response-example-description %}

```

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

