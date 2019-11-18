---
description: List of objects returned in API call responses
---

# API Response Types

Below is a list of objects that can appear in responses to API Server calls. Each object includes the JSON object structure, an explanation of each returned value and an example. Some fields may be omitted from some objects, depending on the API call response.

Unless specified otherwise, all hashes and IDs are hex-encoded SHA-256 hashes.

## Transacion

The transaction object contains information about the transaction and a list of sources and destinations for the transferred money \(inputs and outputs\). Some of the fields are optional and appear only if the transaction is accepted by a block, and some only if the transaction is non-native \(used by a sub network other than '1'\).

```go
{
	transactionId	         	// string   Hex-encoded hash
	transactionHash	        // string   Hex-encoded hash
	acceptingBlockHash      // string   Present only if transaction is accepted
	acceptingBlockBlueScore // int 	 		Present only if transaction is accepted
	subnetworkId            // string   Hex-encoded RIPEMD160 hash
	lockTime								// int      The earliest time or blue score the 
													//          transaction may be included in a block
	gas				   	 	 				// int      Present only if SubnetworkID is not the 
													//          native subnetwork
	payloadHash	       		 	// string   Present only if SubnetworkID is not the 
													//          native subnetwork
	payload		       		 		// string   Base64-encoded data. Present only if 
													//          SubnetworkID is not native
	inputs                  // Array of TransactionInput
	outputs  		   		 		  // Array of TransactionOutput
	mass                    // int 
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded transaction ID</td>
    </tr>
    <tr>
      <td style="text-align:left">transactionHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded transaction hash</td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>The hash of the accepting block</p>
        <p><em>Present only if the transaction is accepted</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockBlueScore</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">
        <p>The blue score of the accepting block</p>
        <p><em>Present only if the transaction is accepted</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">subnetworkId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded RIPEMD160 hash</td>
    </tr>
    <tr>
      <td style="text-align:left">lockTime</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">
        <p>The earliest time or blue score the transaction may be included in a block.
          <br
          />If less than &#x2049;, lockTime is parsed as blue score,</p>
        <p>Otherwise, lockTime is parsed as Unix epoch time.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">gas</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The gas allocated to running the transaction
        <br /><em>Present only if subnetworkId is not 1 -- the native subnetwork</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">payloadHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">The hash of the payload
        <br /><em>Present only if subnetworkId is not</em>  <em>1 -- the native subnetwork</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">payload</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Base64-encoded data.
        <br /><em>Present only if subnetworkId is not 1 -- the native subnetwork</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">inputs</td>
      <td style="text-align:left">array of transactionInput</td>
      <td style="text-align:left">An array of inputs used as money sources by the transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">outputs</td>
      <td style="text-align:left">array of transactionOutput</td>
      <td style="text-align:left">An array of outputs, used as money destinations by the transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">mass</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">The transaction&apos;s mass</td>
    </tr>
  </tbody>
</table>### Example

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

## TransactionInput

The transactionInput object contains information about one source of money used in a transaction \(an input\). The field transactionId is omitted when the transactionInput is part of the Transaction.

```go
{
    transactionId                   // string   Hex-encoded hash. Omitted when 
                                    //          part of a Transaction object
    previousTransactionId           // string
    previousTransactionOutputIndex  // int
    scriptSig                       // string   Base64-encoded data
    sequence                        // int
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Hex-encoded transaction ID</p>
        <p>Omitted, when returned as part of Transaction object</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">previousTransactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded transaction ID</td>
    </tr>
    <tr>
      <td style="text-align:left">previousTransactionOutputIndex</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">&#x2049;</td>
    </tr>
    <tr>
      <td style="text-align:left">scriptSig</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Base64-encoded data</td>
    </tr>
    <tr>
      <td style="text-align:left">sequence</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">&#x2049;</td>
    </tr>
  </tbody>
</table>### Example

```javascript
{
    "transactionId":""
    "previousTransactionId":""
    "previousTransactionOutputIndex":""
    "scriptSig":""
    "sequence":""
}
```

## TransactionOutput

The transactionOutput object contains information about one destination for the money in a transaction \(an output\). Some fields are omitted depending on the request.

```go
{
    transactionId    // string   Hex-encoded hash. Omitted when part of a 
                     //          Transaction object
    value            // int
    scriptPubKey     // string   Base64-encoded data
    address          // string   Bech32-encoded address. Present only if 
                     //          ScriptPubKey is P2PKH or P2SH
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionId</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Hex-encoded transaction ID</p>
        <p>Omitted when returned as part of Transaction object</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Amount of money to move to this output</td>
    </tr>
    <tr>
      <td style="text-align:left">scriptPubKey</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Base64-encoded data</td>
    </tr>
    <tr>
      <td style="text-align:left">address</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Bech32-encoded address</p>
        <p>Omitted if scriptPubKey is neither P2PKH nor P2SH</p>
        <p>Omitted in the get UTXOs by address response</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockBlueScore</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">isCoinbase</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">confirmations</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Omitted except in the get UTXOs by address response</td>
    </tr>
    <tr>
      <td style="text-align:left">isSpendable</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">
        <p>Omitted except in the get UTXOs by address response</p>
        <p>This can also be calculated by the requesting client. True if #Confirmations
          &gt;= 100, where #Confirmations = SelectedTip.BlueScore - AcceptingBlock.BlueScore</p>
      </td>
    </tr>
  </tbody>
</table>### Example

```javascript
{
    "transactionId":""
    "value":""
    "scriptPubKey":""
    "address":""
    "acceptingBlockHash":""
    "acceptingBlockBlueScore":""
    "isCoinbase":""
    "confirmations":""
    "isSpendable":
}
```

## Block

The block object contains information about a block in the DAG. The field acceptingBlockHash is present only if the block is accepted.

```go
{
    blockHash               // string    Hex-encoded hash
    version                 // int        
    hashMerkleRoot          // string    Hex-encoded hash
    acceptedIdMerkleRoot    // string    Hex-encoded hash
    utxoCommitment          // string    Hex-encoded hash
    timestamp               // int       Unix epoch timestamp. Seconds since 
                            //           01/01/1970    
    bits                    // int        
    nonce                   // int        
    acceptingBlockHash      // string    Hex-encoded hash. Present only if 
                            //           this block is accepted
    blueScore               // int        
    isChainBlock            // bool        
    mass                    // int         
}
```

### Fields and Data types

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">blockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">version</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">hashMerkleRoot</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">acceptedIdMerkleRoot</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">utxoCommitment</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Hex-encoded hash</td>
    </tr>
    <tr>
      <td style="text-align:left">timestamp</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Unix epoch timestamp (seconds since 01/01/1970)</td>
    </tr>
    <tr>
      <td style="text-align:left">bits</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">nonce</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">acceptingBlockHash</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>Hex-encoded hash</p>
        <p>Omitted unless the block is accepted</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">blueScore</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">the number of blue blocks in this block&apos;s selected parent chain since
        the origin block &#x2049;</td>
    </tr>
    <tr>
      <td style="text-align:left">isChainBlock</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">True if this block is part of the selected parent chain</td>
    </tr>
    <tr>
      <td style="text-align:left">mass</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>### Example

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

## FeeEstimate

The feeEstimate object contains information about the estimated fee costs to include a transaction in a block. The values are in Satoshi per gram of transaction mass.

```go
{
    highPriority      // int    estimated fee for near-immediate confirmation
    normalPriority    // int    estimated fee for confirmation in about 1 minute
    lowPriority       // int    estimated fee for confirmation within 5 minutes
}
```

### Fields and Data types

| Name | Type | Description |
| :--- | :--- | :--- |
| highPriority | int | The estimated fee in Satoshi per gram of transaction mass, for the transaction to be included within the next 10 blocks, with a probability of 90%. With 1 block per second, this translates to near immediate confirmation. |
| normalPriority | int | The estimated fee in Satoshi per gram of transaction mass, for the transaction to be included within the next 100 blocks, with a probability of 90%. With 1 block per second, this translates to confirmation in about a minute. |
| lowPriority | int | The estimated fee in Satoshi per gram of transaction mass, for the transaction to be included within the next 300 blocks, with a probability of 90%. With 1 block per second, this translates to confirmation within 5 minutes. |

{% hint style="warning" %}
Numbers are bound to change during TestNet.

The fee estimates is currently implemented as a stub.
{% endhint %}

### Example

```javascript
{
    "highPriority":900
    "normalPriority":300
    "lowPriority":100
}
```

