---
description: List of API request calls
---

# API Request Calls

* [ ] \[1h\] Write introduction paragraph
* [ ] \[2h\] Explain about the use case for each method
* [x] \[1h\] Fix responses
* [x] \[4h\] Give an example for each request & response

This page lists the calls that can be requested from the API.

All API calls return 200 OK if no error was encountered.

If an error occurs, an error code is returned with the following body:

```go
{
    errorCode       // int       HTTP error code
    errorMessage    // string    Human-readable error description
}
```

Example:

```javascript
{
    "errorCode":404
    "errorMessage":"No transaction was found for the given address."
}
```

## List of API Calls

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transaction/id/{txid}" %}
{% api-method-summary %}
/transaction/id/ {txid}
{% endapi-method-summary %}

{% api-method-description %}
Searches for a transaction by its ID.  
Response: Transaction
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="txid" type="string" required=true %}
64bit integer for requested transaction ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a single transaction object.
{% endapi-method-response-example-description %}

```javascript
{
    "transactionId":""
    "transactionHash":""
    "acceptingBlockHash":""
    "acceptingBlockBlueScore":
    "subnetworkId":""
    "lockTime":
    "gas":
    "payloadHash":""
    "payload":""
    "inputs":
    [
        {
            "previousTransactionId":""
            "previousTransactionOutputIndex":""
            "scriptSig":""
            "sequence":""
        },
        {
            "previousTransactionId":""
            "previousTransactionOutputIndex":""
            "scriptSig":""
            "sequence":""
        }
    ]
    "outputs":
    [
        {
            "value":""
            "scriptPubKey":""
            "address":""
        },
        {
            "value":""
            "scriptPubKey":""
            "address":""
        }
    ]
    "mass":
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404
    "errorMessage":"No trasnaction with the given txid was found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422
    "errorMessage":"The given txid is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transaction/hash/{txhash}" %}
{% api-method-summary %}
/transaction/hash/ {txhash}
{% endapi-method-summary %}

{% api-method-description %}
Searches for transaction by its hash.  
Response: Transaction
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="txhash" type="string" required=true %}
Hex-encoded 32 bytes, representing the 64 character transaction ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a single transaction object.
{% endapi-method-response-example-description %}

```javascript
{
    "transactionId":""
    "transactionHash":""
    "acceptingBlockHash":""
    "acceptingBlockBlueScore":
    "subnetworkId":""
    "lockTime":
    "gas":
    "payloadHash":""
    "payload":""
    "inputs":
    [
        {
            "previousTransactionId":""
            "previousTransactionOutputIndex":""
            "scriptSig":""
            "sequence":""
        },
        {
            "previousTransactionId":""
            "previousTransactionOutputIndex":""
            "scriptSig":""
            "sequence":""
        }
    ]
    "outputs":
    [
        {
            "value":""
            "scriptPubKey":""
            "address":""
        },
        {
            "value":""
            "scriptPubKey":""
            "address":""
        }
    ]
    "mass":
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404
    "errorMessage":"No transactions for the given txhash was found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422
    "errorMessage":"The given txhash is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/transactions/address/{address}\[&skip=txid&limit=100\]" %}
{% api-method-summary %}
/transactions/address/ {address}
{% endapi-method-summary %}

{% api-method-description %}
Searches for all transactions where the given address is either an input or an output.    
Returns: Array of Transaction, sorted by accepting block date in ascending order \(earliest tx first\).
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
A Bech32-encoded address.
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="integer" required=false %}
If provided, skips the first given number of transactions, ordered by ID \(default 0\).
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="integer" required=false %}
If provided, limits the response to the given number of  transactions \(default 100, max 1000\). If more than 1000 is specified, returns an error.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is an array of transaction objects. In the following example, the array consists of two transaction objects.
{% endapi-method-response-example-description %}

```javascript
[
    {
        "transactionId":""
        "transactionHash":""
        "acceptingBlockHash":""
        "acceptingBlockBlueScore":
        "subnetworkId":""
        "lockTime":
        "gas":
        "payloadHash":""
        "payload":""
        "inputs":
        [
            {
                "previousTransactionId":""
                "previousTransactionOutputIndex":""
                "scriptSig":""
                "sequence":""
            },
            {
                "previousTransactionId":""
                "previousTransactionOutputIndex":""
                "scriptSig":""
                "sequence":""
            }
        ]
        "outputs":
        [
            {
                "value":""
                "scriptPubKey":""
                "address":""
            },
            {
                "value":""
                "scriptPubKey":""
                "address":""
            }
        ]
        "mass":
    },
    {
        "transactionId":""
        "transactionHash":""
        "acceptingBlockHash":""
        "acceptingBlockBlueScore":
        "subnetworkId":""
        "lockTime":
        "gas":
        "payloadHash":""
        "payload":""
        "inputs":
        [
            {
                "previousTransactionId":""
                "previousTransactionOutputIndex":""
                "scriptSig":""
                "sequence":""
            },
            {
                "previousTransactionId":""
                "previousTransactionOutputIndex":""
                "scriptSig":""
                "sequence":""
            }
        ]
        "outputs":
        [
            {
                "value":""
                "scriptPubKey":""
                "address":""
            },
            {
                "value":""
                "scriptPubKey":""
                "address":""
            }
        ]
        "mass":
    }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":400
    "errorMessage":"Limit higher than 1000 or lower than 0 was requested."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404
    "errorMessage":"No transactions for the given address was found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422
    "errorMessage":"The given address is not a well-formatted P2PKH or P2SH address, or the given txid in the skip parameter is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/utxos/{address}" %}
{% api-method-summary %}
/utxos/address/ {address}
{% endapi-method-summary %}

{% api-method-description %}
Searches for all unspent transaction outputs for the given address.  
Response: Array of TransactionOutput, omitting the address field.
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
The response is an array of transactionOutput objects. In the following example the array consists of two transactionOutput objects. The address field in each transactionOutput object is omitted.
{% endapi-method-response-example-description %}

```javascript
[
    {
        "transactionId":""
        "value":""
        "scriptPubKey":""
        "acceptingBlockHash":""
        "acceptingBlockBlueScore":""
        "isCoinbase":""
        "confirmations":""
        "isSpendable":
    },
    {
        "transactionId":""
        "value":""
        "scriptPubKey":""
        "acceptingBlockHash":""
        "acceptingBlockBlueScore":""
        "isCoinbase":""
        "confirmations":""
        "isSpendable":
    }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404
    "errorMessage":"No UTXOs were found for the given address."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422
    "errorMessage":"The given address is not a well-formatted P2PKH or P2SH address."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/block/{hash}" %}
{% api-method-summary %}
/block/ {hash}
{% endapi-method-summary %}

{% api-method-description %}
Searches for a block by its hash.  
Response: Block
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
The response is a single block object.
{% endapi-method-response-example-description %}

```javascript
{
    "blockHash":""
    "version":
    "hashMerkleRoot":""
    "acceptedIdMerkleRoot":""
    "utxoCommitment":""
    "timestamp":
    "bits":
    "nonce":
    "acceptingBlockHash":""
    "blueScore":
    "isChainBlock":
    "mass":
}                         
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":404
    "errorMessage":"No Block with the given hash found.."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":422
    "errorMessage":"The given hash is not a hex-encoded 32-byte hash."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/blocks\[?order=asc/desc&skip=0&limit=25\]" %}
{% api-method-summary %}
/blocks
{% endapi-method-summary %}

{% api-method-description %}
Searches for all blocks.  
Response: Array of Block
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="order" type="string" required=false %}
If provided \(asc or desc\), sorts the blocks by ascending or descending order respectively \(default: desc\)
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="string" required=false %}
If provided, skips the first given number of blocks ordered by ID \(default: 0\).
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="string" required=false %}
If provided, limits the response to the given number of blocks \(default: 25, max 100\). If more than 100 is specified, returns an error.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is an array of block objects. In the following example, the array consists of two block objects.
{% endapi-method-response-example-description %}

```javascript
[
    {
        "blockHash":""
        "version":
        "hashMerkleRoot":""
        "acceptedIdMerkleRoot":""
        "utxoCommitment":""
        "timestamp":
        "bits":
        "nonce":
        "acceptingBlockHash":""
        "blueScore":
        "isChainBlock":
        "mass":
    },
    {
        "blockHash":""
        "version":
        "hashMerkleRoot":""
        "acceptedIdMerkleRoot":""
        "utxoCommitment":""
        "timestamp":
        "bits":
        "nonce":
        "acceptingBlockHash":""
        "blueScore":
        "isChainBlock":
        "mass":
    }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":400
    "errorMessage":"Limit higher than 100 or lower than 0 was requested."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.kas.pa" path="/dev/v1/fee-estimates" %}
{% api-method-summary %}
/fee-estimates
{% endapi-method-summary %}

{% api-method-description %}
Returns updated fee estimates.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response is a feeEstimates object.
{% endapi-method-response-example-description %}

```javascript
{
    "highPriority":900
    "normalPriority":300
    "lowPriority":100
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "errorCode":500
    "errorMessage":"A server error occurred."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.kas.pa" path="/dev/v1/transaction" %}
{% api-method-summary %}
/transaction
{% endapi-method-summary %}

{% api-method-description %}
This method takes a Hex-encoded rawTransaction data as input, and sends it to a node for execution.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="rawTransaction" type="string" required=true %}
Hex-encoded raw transaction data
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

## List of MQTT Topics

* [ ] Explanation what this is for
* [ ] Explanation that we use RabbitMQ and how to subscribe to topics

### **transactions/{address}**

Subscribing to this topic will send a notification of type Transaction, whenever a transaction for the given address is seen in a new block.

### **transactions/accepted/{address}**

Subscribing to this topic will send a notification of type Transaction, whenever a transaction for the given address is accepted.

### **transactions/unaccepted/{address}**

Subscribing to this topic will send a notification of type Transaction whenever a transaction for the given address is unaccepted. This can happen when there is a re-organization of the blocks in the DAG. In this case, a transaction that was previously by a specific block may become unaccepted from that block, and may or may not be accepted in a different block \(receivable in the _transaction/accepted_ topic\).

### **dag/selected-tip**

Subscribing to this topic will send a notification of type Block whenever the longest parent chain becomes longer, as a result of a new block extending it, or a re-organization.

