# Node JSON-RPC

* [ ] From: [https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls)

This article lists and details all of the calls the clients expect the server to support.

Most of them are are borrowed from the [Bitcoin Core developer reference](https://bitcoin.org/en/developer-reference) \(either verbatim or with slight changes\), though some have been borrowed from other sources \(e.g. `nodes`, `getheaders` etc. which were borrowed from the btcd API\).

**Deviations from the original calls are marked in red.**

The list of calls, categorized by their affinity with Bitcoin implementations, is available [here](https://docs.google.com/spreadsheets/d/1bOWO0KvUn0QVf299kxJDb4FwjMg8kKp-PNBuywVHRYQ/edit?usp=sharing).

Some global conventions:

* All hashes are represented in [RPC Byte order](https://bitcoin.org/en/glossary/rpc-byte-order)
* All serialized blocks are serialized as defined in [Serialized Block Format](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/143786359/Serialized+Block+Format)
* All serialized transactions are serialized as defined in [Serialized Transaction Format](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/143983036/Serialized+Transaction+Format)
* All of the above are **hex encoded**

## Nodes <a id="JSON-RPCAPIcalls-Nodes"></a>

### `addManualNode` <a id="JSON-RPCAPIcalls-addManualNode"></a>

`Back-end ticket:`  [`DEV-195`](https://daglabs.atlassian.net/browse/DEV-195) `- Getting issue details... STATUS`

Connects to a node. Adds the node to the [manual node list](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/134578256/Manual+Node+List) unless the `onetry` flag is set to `true`.

**Was originally called addnode**

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Default Value</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">node</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">N/A</td>
      <td style="text-align:left">
        <p>Node to add as a string of the form <code>&lt;address&gt;:&lt;port&gt;</code>
        </p>
        <p>The address may be:</p>
        <ul>
          <li>an IPv4 address,</li>
          <li>an <a href="https://en.wikipedia.org/wiki/IPv6#IPv4-mapped_IPv6_addresses">IPv4-as-IPV6 address</a>,
            or</li>
          <li>a resolvable hostname</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">oneTry</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">
        <p>If true:</p>
        <ul>
          <li>Immediately attempt connection, regardless of the state of the outgoing
            connection slots</li>
          <li>Only attempt once</li>
          <li>In no case this node will be added to the manual node list</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>**The design of this call is borrowed from the parallel `addnode` call in the Bitcoin core API, but has been split into the current call and the removemanualnode call \(which does not exist in Bitcoin core\)**

#### `Output` <a id="JSON-RPCAPIcalls-Output"></a>

| `null` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments"></a>

This returns null as to not to block the client while waiting for response from the other node.

#### Source <a id="JSON-RPCAPIcalls-Source"></a>

Based on [Bitcoin core addnode API call](https://bitcoin.org/en/developer-reference#addnode) with considerable modifications

### `getAllManualNodesInfo` <a id="JSON-RPCAPIcalls-getAllManualNodesInfo"></a>

Back-end ticket:  [DEV-195](https://daglabs.atlassian.net/browse/DEV-195) - Getting issue details... STATUS

Return the information one would receive from [getmanualnodeinfo ](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls#JSON-RPCAPIcalls-getmanualnodeinfo)for all nodes in the [manual node list](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/134578256/Manual+Node+List).

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.1"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| details | boolean | N | true | If set to false, display only the `<address>:<port>` string, otherwise, print full details as described in the output |

#### `Output` <a id="JSON-RPCAPIcalls-Output.1"></a>

| `[ (array of json objects)    //for each node in the manual node list, the output of getmanualnodeinfo for that node]` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.1"></a>

  
**Complements** [**getManualNodeInfo**](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls#JSON-RPCAPIcalls-getmanualnodeinfo)

### `getManualNodeInfo` <a id="JSON-RPCAPIcalls-getManualNodeInfo"></a>

Back-end ticket:  [DEV-195](https://daglabs.atlassian.net/browse/DEV-195) - Getting issue details... STATUS

`Returns information about a node in the` [`manual node list`](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/134578256/Manual+Node+List)`.`

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.2"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| node | string | Y | N/A | The node to get information about, as a string of the form `<address>:<port>` such as in [addnode ](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls#JSON-RPCAPIcalls-addnode) |
| details | boolean | N | true | If set to false, display only the `<address>:<port>` string, otherwise, print full details as described in the output |

**In Bitcoin core the Node field is optional, when omitted the node would return a list of all nodes. We deferred this behavior to the** [**getallmanualnodesinfo** ](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls#JSON-RPCAPIcalls-getallmanualnodesinfo)**call**

#### `Output` <a id="JSON-RPCAPIcalls-Output.2"></a>

| `{   (json object)    "manualNode":   "data", (string) the requested node as a string of the form <address>:<port> such as in` `addnode                                     empty string if` `the node was not found    //if Details is true and addednode is not empty        "connected":    b,  (boolean) true` `if` `and only if` `the node is currently connected        "addresses":    [ (array of json objects) the addresses belonging to the node            { (json object)                "address":      "data", (string) IP address (resolved) and port number of the node                "connected":    b,      (boolean) true` `iff we are connected to the node using this` `address                //if connected is true                    "outbound": b,  (boolean) true` `iff we established the connection            },            ...        ]}` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.2"></a>

**Was originally called `getaddednodeinfo`**

### `getPeerInfo` <a id="JSON-RPCAPIcalls-getPeerInfo"></a>

Returns information about the node's currently connected peers

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.3"></a>

This call gets no parameters

#### `Output` <a id="JSON-RPCAPIcalls-Output.3"></a>

| `[ (json array of json objects)     { (json object) a representation of a peer        "id":           n,      (numeric) the index number for` `this` `remote node in` `the local node address database        "addr":         "data", (string) the IP address and port number used to connect to this` `host        "services":     "data", (string) services supported by the remote nodes, currently set to 0x01        "lastSend":     n,      (numeric) UNIX epoch time of when we last successfully sent TCP data to this` `node        "lastRecv":     n,      (numeric) UNIX epoch time of when we last successfully received data from this` `node        "bytesSent":    n,      (numeric) Total bytes sent to this` `node        "bytesRecv":    n,      (numeric) Total bytes received from this` `node        "connTime":     n,      (numeric) UNIX epoch time when we connected to this` `node        "timeOffset":   n,      (numeric) The time offset in` `seconds        //if the peer was ever pinged:            "pingTime":     n,      (numeric) The number of microseconds this` `node took to respond to our last ping            "minPing":      n,      (numeric) The minimal number of microseconds this` `node took to respond to a ping        //if there is an outstanding ping to this peer:            "pingWait":     n,      (numeric) The number of microseconds this` `node took to respond to our last ping        "version":      n,      (numeric) The version of the protocol used by this` `node, currently set to 1        "subVersion":   "data", (string) the user agent this` `nodes advertises itself with        "inbound":      b,      (boolean) True iff the remote node initiated the connection        "startingSize": n,      (numeric) The size of the peer's DAG as reported when first initializing connection with` `the current node        "banScore":     n,      (numeric) The ban score assigned to the node based on a yet to be specified enforcement mechanism        "syncedHeaders":    TBD        "syncedBlocks":     TBD        "inflight":         [ (array of strings) hashes of blocks requested from this` `peer and not yet received                                "hash", block hash                                ...,                            ]        "whitelisted":  b,      (boolean) TBD        "bytesSentPerMsg": { (string:numeric dictionary)            messageType: n,     messageType is a type of message from the TBD list of types in` `the P2P protocol,                                n is the number of bytes in` `messages of this` `type sent to the peer        }        "bytesRecvPerMsg": { (string:numeric dictionary)            messageType: n,     messageType is a type of message from the TBD list of types in` `the P2P protocol,                                n is the number of bytes in` `messages of this` `type received from the peer        }    }, ... ]` |
| :--- |


**The original Bitcoin Core call also included a field called `addrlocal`** where it would state the IP the remote node uses to connect to the current node. This was done with the forsaken notion of [Pay-to-IP](https://en.bitcoin.it/wiki/IP_transaction). We have removed this altogether.

#### Comments <a id="JSON-RPCAPIcalls-Comments.3"></a>

Source

###  `node (empty)` <a id="JSON-RPCAPIcalls-node(empty)"></a>

### `removeManualNode` <a id="JSON-RPCAPIcalls-removeManualNode"></a>

Back-end ticket:  [DEV-195](https://daglabs.atlassian.net/browse/DEV-195) - Getting issue details... STATUS

`Removes a node from the` [`manual node list`](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/134578256/Manual+Node+List)`.`

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.4"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| node | string | Y | N/A | The node to remove, as a string of the form `<address>:<port>` such as in [addnode ](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/122945550/JSON-RPC+API+calls#JSON-RPCAPIcalls-addnode) |

#### `Output` <a id="JSON-RPCAPIcalls-Output.4"></a>

| `null` |
| :--- |


## `Transactions` <a id="JSON-RPCAPIcalls-Transactions"></a>

### `createrawtransaction (empty)` <a id="JSON-RPCAPIcalls-createrawtransaction(empty)"></a>

### `decoderawtransaction (empty)` <a id="JSON-RPCAPIcalls-decoderawtransaction(empty)"></a>

### `decodescript (empty)` <a id="JSON-RPCAPIcalls-decodescript(empty)"></a>

### `getrawtransaction (empty)` <a id="JSON-RPCAPIcalls-getrawtransaction(empty)"></a>

### `gettransaction (empty)` <a id="JSON-RPCAPIcalls-gettransaction(empty)"></a>

###  `getTxOut` <a id="JSON-RPCAPIcalls-getTxOut"></a>

`Fetches details about an unspent transaction output.`

`Back end ticket:`  [`DEV-152`](https://daglabs.atlassian.net/browse/DEV-152) `- Getting issue details... STATUS`

`Front end ticket:`  [`DEV-155`](https://daglabs.atlassian.net/browse/DEV-155) `- Getting issue details... STATUS`

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.5"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| `txId` | string | Y | N/A | The TXID of the transaction containing the desired output |
| outputIndex | int | Y | N/A | The index of the output within the transaction, the index of the first output being 0 |
| unconfirmed | boolean | N | false | If set to true unconfirmed outputs \(from mempool\) will also be displayed |

#### `Output` <a id="JSON-RPCAPIcalls-Output.5"></a>

| `{   (json object)    "bestBlock":        "data", (string) hash of the block which includes this` `transaction, encoded in` `hex    "confirmations":    n,      (numeric) number of confirmations received for` `this` `transaction    "value":            n,      (numeric) amount of coins spent to this` `output    //if there is a scriptPubKey        "scriptPubKey": {   (json object)            "asm":  "asm",  (string) pubkey script in` `decoded form            "hex":  "data", (string) pubkey script encoded in` `hex            "type": "data", (string) type of script (see below)            //if the script type is NOT nulldata or nonstandard                "reqSigs":      n,  (numeric) number of required signatures, always 1 for` `P2PK, P2PKH and P2SH                "addresses":    [ (array of strings) addresses used in` `this` `transaction                    "address",  (string) a P2PKH or P2SH address                    ...                ]        },    "version":  n,  (numeric) the transaction version, should be set to 1    "coinbase": b,  (boolean) true` `iff this` `is a coinbase transaction    ]}` |
| :--- |


`The type string should be set for one of the following:`

| Type of script | Expected value |
| :--- | :--- |
| P2PK | pubkey |
| P2PK | pubkeyhash |
| P2SH | scripthash |
| multisig | multisig |
| nulldata | nulldata |
| anything else | nonstandard |

#### Comments <a id="JSON-RPCAPIcalls-Comments.4"></a>

**Originally, the `type` field appeared after the `reqSigs` field.**

#### Source <a id="JSON-RPCAPIcalls-Source.1"></a>

Replicated from [a](https://github.com/btcsuite/btcd/blob/master/docs/json_rpc_api.md#searchrawtransactions)[ similar call in Bitcoin core](https://bitcoin.org/en/developer-reference#gettxout) with slight modifications.

###  `searchRawTransactions` <a id="JSON-RPCAPIcalls-searchRawTransactions"></a>

`Back end ticket:`  [`DEV-153`](https://daglabs.atlassian.net/browse/DEV-153) `- Getting issue details... STATUS`

`Front end ticket:`  [`DEV-156`](https://daglabs.atlassian.net/browse/DEV-156) `- Getting issue details... STATUS`

Fetches raw data for transactions involving the passed address.

Returns transactions from both the database and the mempool \(transactions pulled from the mempool can be identified by having 0 confirmations\).

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.6"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| `address` | string | Y | N/A | Bitcoin address |
| `verbose` | boolean\* | N | true | If True the transaction is returned as a JSON object rather than a hex-encoded string |
| `skip` | int | N | 0 | If set to n then the n leading transactions will be left out of the final response |
| `count` | int | N | 100 | The maximum number of transactions to return |
| `vinExtra` | boolean\* | N | false | If true then data from previous input will be included in the vin  |
| `reverse` | boolean | N | false | If set to True the transactions will returned in reverse chronological order \(first to last\) |

**boolean values marked with \* were int values in the btcd implementation**

#### `Return Value` <a id="JSON-RPCAPIcalls-ReturnValue"></a>

If `verbose` is **false**:

| `[ (json array of strings)     "serializedTx", ... hex-encoded bytes of the serialized transaction ]` |
| :--- |


  
If `verbose` is **true**:

| `[ (array of json objects)`     `{ (json object)        "hex":      "data", (string) hex-encoded transaction        "txId":     "hash", (string) the hash of the transaction        "version":  n,      (numeric) the transaction version        "lockTime": n,      (numeric) the transaction lock time`         `"inputs":   [ (array of json objects) the transaction inputs as json objects            //For coinbase transactions:            { (json object)                "coinbase":     "data", (string) the hex-encoded bytes of the signature script                "txInWitness":  "data", (string) the witness stack for` `the input                "sequence":     n,      (numeric) the script sequence number            }            //For non-coinbase transactions:            { (json object)                "txId": "hash", (string) the hash of the origin transaction                "outputIndex":  n,      (numeric) the index of the output being redeemed from the origin transaction                "scriptSig":    {       (json object) the signature script used to redeem the origin transaction                    "asm":  "asm",  (string) disassembly of the script                    "hex":  "data", (string) hex-encoded bytes of the script                }                //if vinextra is true:                        "prevOut": {    (json object) Data from the origin transaction output with` `index vout.                            "addresses":    ["value",...],  (array of string) previous output addresses                            "value":        n.nnn,          (numeric) previous output value                        }                "txInWitness":  "data", (string) the witness stack for` `the input                "sequence":     n,      (numeric) the script sequence number            }, ...        ]`         `"outputs":  [ (array of json objects) the transaction outputs as json objects            { (json object)                "value":    n,  (numeric) the value in` `BTC                "index":        n,  (numeric) the index of this` `transaction output                "scriptPubKey": {   (json object) the public key script used to pay coins                    "asm":      "asm",          (string) disassembly of the script                    "hex":      "data",         (string) hex-encoded bytes of the script                    "reqSigs":  n,              (numeric) the number of required signatures                    "type":     "scripttype",   (string) the type of the script (e.g. 'pubkeyhash')                    "addresses": [  (json array of string) the bitcoin addresses associated with` `this` `output                                        "address",  (string) the bitcoin address                                        ...                    ]                }            }, ...        ]`         `"blockHash":        "hash", (string) Hash of the block the transaction is part of.        "confirmations":    n,      (numeric) Number of confirmations of block.        "time":             t,      (numeric) Transaction time in` `seconds since the epoch.        "blockTime":        t,      (numeric) Block time in` `seconds since the epoch.`     `},...]` |
| :--- |


#### Usage of this RPC requires the optional `--addrindex` flag to be activated, otherwise all responses will simply return with an error stating the address index has not yet been built up. <a id="JSON-RPCAPIcalls-UsageofthisRPCrequirestheoptional--addrindexflagtobeactivated,otherwiseallresponseswillsimplyreturnwithanerrorstatingtheaddressindexhasnotyetbeenbuiltup."></a>

#### Comments <a id="JSON-RPCAPIcalls-Comments.5"></a>

* If the node is not synced with the network all requests will return an error response in order to avoid serving stale data.

#### Source <a id="JSON-RPCAPIcalls-Source.2"></a>

Replicated from [a similar call in btcd](https://github.com/btcsuite/btcd/blob/master/docs/json_rpc_api.md#searchrawtransactions) with slight modifications.

### `sendRawTransaction` <a id="JSON-RPCAPIcalls-sendRawTransaction"></a>

`Back end ticket:`  [`DEV-154`](https://daglabs.atlassian.net/browse/DEV-154) `- Getting issue details... STATUS`

`Front end ticket:`  [`DEV-157`](https://daglabs.atlassian.net/browse/DEV-157) `- Getting issue details... STATUS`

Validates a transaction and broadcasts it to the P2P network.

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.7"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| transaction | string | Y | N/A | The serialized transaction to broadcast encoded as hex |

**The Bitcoin core version of this call has a second parameters allowing for "insanely high fees" \(whatever that means\). As of now, this parameter doesn't actually do anything, and since it is also wallet oriented we decided to remove it altogether.**

#### `Return Value` <a id="JSON-RPCAPIcalls-ReturnValue.1"></a>

| `"hash"`  `(string) the hash of the transaction` |
| :--- |


###  `validateaddress (empty)` <a id="JSON-RPCAPIcalls-validateaddress(empty)"></a>

###  `verifymessage (empty)` <a id="JSON-RPCAPIcalls-verifymessage(empty)"></a>

## Blocks and DAG <a id="JSON-RPCAPIcalls-BlocksandDAG"></a>

### getBlock <a id="JSON-RPCAPIcalls-getBlock"></a>

Given a block hash, return the block info, either serialized or as JSON. The JSON version returns various metadata of the block.

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.8"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| headerHash | string | Y | N/A | The hash of the header of the desired block |
| serialized | boolean | N | false | If true, the block will be serialized |

#### `Output` <a id="JSON-RPCAPIcalls-Output.6"></a>

| `//if Serialized is true(string) the block in` `serialized form` `//if Serialized is false{   (json object)    "confirmations":    n,      (numeric) number of confirmations for` `this` `block    "medianTime":       n,      (numeric) the median time of the block    "headerData":       { (json object) the data in` `the header)        same as in` `getBlockHeader    },    "parentHashes": [ (string array) hashes of the parent blocks (if` `any)        "blockHash",    (string) hash of a block,        ...    ],    "tx":   [ (string array) transcation IDs of transactions in` `the block        "txId", (string) transaction ID,        ...    ],}` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.6"></a>

The transactions in "tx" must appear **in the same order** as in the serialized block.

Cf. [median time](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/141852690/Median+Time).

### getblockchaininfo `(empty)` <a id="JSON-RPCAPIcalls-getblockchaininfo(empty)"></a>

### getblockcount `(empty)` <a id="JSON-RPCAPIcalls-getblockcount(empty)"></a>

### getBlockHeader `(empty)` <a id="JSON-RPCAPIcalls-getBlockHeader(empty)"></a>

Translate a block hash to a [block header](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/142245901/Block+header), either serialized or as JSON. The JSON version returns various metadata of the block.

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.9"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| headerHash | string | Y | N/A | The hash of the header of the desired block |
| serialized | boolean | N | false | If true, the block will be returned serialized as a string |

#### `Output` <a id="JSON-RPCAPIcalls-Output.7"></a>

| `//if Serialized is true(string) the block header in` `serialized form` `//if Serialized is false{   (json object)    "confirmations":    n,      (numeric) number of confirmations for` `this` `block    "medianTime":       n,      (numeric) the median time of the block    "headerData":       { (json object) the data in` `the header)        "version":          n,      (numeric) the block structure version, should be set to 1        "parentHashes": [ (string array) hashes of the previous blocks            "blockHash",    (string) hash of a block,            ...        ],        "merkleRoot":       "data", (string) the value of the HashMerkleRoot        "time":             n,      (numeric) the Time field in` `the block header        "target":           n,      (numeric) the Target field from the block header        "nonce":            n,      (numeric) the Nonce field from the block header    },    "childrenHashes":   [ (string array) hashes of the next blocks (if` `any)        "blockHash",    (string) hash of a block,        ...    ],}` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.7"></a>

The previous blocks in the `HashPrevBlock` array in `headerdata` must appear **in the same order** as in the serialized block.

Cf. [median time](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/141852690/Median+Time).

### getblocktemplate `(empty)` <a id="JSON-RPCAPIcalls-getblocktemplate(empty)"></a>

### getheaders `(empty)` <a id="JSON-RPCAPIcalls-getheaders(empty)"></a>

### getinfo `(empty)` <a id="JSON-RPCAPIcalls-getinfo(empty)"></a>

### gettiphashes <a id="JSON-RPCAPIcalls-gettiphashes"></a>

Returns the hashes of all tips in the DAG.

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.10"></a>

`This call takes no parameters`

#### `Output` <a id="JSON-RPCAPIcalls-Output.8"></a>

| `[ (string) hashes of tips, ordered as described in` `the comments"hash", (string) hash of a tip...,]` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.8"></a>

The hashes should be ordered so that higher layers should appear last. Internally, tips of the same layer should be ordered by [median time](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/141852690/Median+Time).

### getTipHeaders <a id="JSON-RPCAPIcalls-getTipHeaders"></a>

Returns the headers of all tips in the DAG.

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.11"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| serialized | boolean | N | false | If true, the blocks will be returned serialized as strings |

#### `Output` <a id="JSON-RPCAPIcalls-Output.9"></a>

| `//if Serialized is true[ (string array) the tip headers serialized, ordered as described in` `the comments    (string) a serialized block    ...,]` `//if Serialized is false[ (json object array) for` `each tip, the output getBlockHeader would return` `on its hash, ordered as described in` `the comments]` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.9"></a>

The tips should be ordered so that higher layers should appear last. Internally, tips of the same layer should be ordered by [median time](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/141852690/Median+Time).

### getTips <a id="JSON-RPCAPIcalls-getTips"></a>

Returns the all tips in the DAG.

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.12"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| Serialized | boolean | N | false | If true, the blocks will be returned serialized as strings |

#### `Output` <a id="JSON-RPCAPIcalls-Output.10"></a>

| `//if Serialized is true[ (string array) the tips as serialized blocks, ordered as described in` `the comments    (string) a serialized block    ...,]` `//if Serialized is false[ (json object array) for` `each tip, the output getblock would return` `on its hash, ordered as described in` `the comments]` |
| :--- |


#### Comments <a id="JSON-RPCAPIcalls-Comments.10"></a>

The tips should be ordered so that higher layers should appear last. Internally, tips of the same layer should be ordered by [median time](https://daglabs.atlassian.net/wiki/spaces/SPEC/pages/141852690/Median+Time).

### invalidateblock `(empty)` <a id="JSON-RPCAPIcalls-invalidateblock(empty)"></a>

### submitblock `(empty)` <a id="JSON-RPCAPIcalls-submitblock(empty)"></a>

### verifychain `(empty)` <a id="JSON-RPCAPIcalls-verifychain(empty)"></a>

## Mining <a id="JSON-RPCAPIcalls-Mining"></a>

###  estimatefee `(empty)` <a id="JSON-RPCAPIcalls-estimatefee(empty)"></a>

### generate `(empty)` <a id="JSON-RPCAPIcalls-generate(empty)"></a>

### getdifficulty `(empty)` <a id="JSON-RPCAPIcalls-getdifficulty(empty)"></a>

### getgenerate `(empty)` <a id="JSON-RPCAPIcalls-getgenerate(empty)"></a>

### gethashespersec `(empty)` <a id="JSON-RPCAPIcalls-gethashespersec(empty)"></a>

### getmempoolinfo `(empty)` <a id="JSON-RPCAPIcalls-getmempoolinfo(empty)"></a>

### getmininginfo `(empty)` <a id="JSON-RPCAPIcalls-getmininginfo(empty)"></a>

### getrawmempool `(empty)` <a id="JSON-RPCAPIcalls-getrawmempool(empty)"></a>

### setgenerate `(empty)` <a id="JSON-RPCAPIcalls-setgenerate(empty)"></a>

## Network <a id="JSON-RPCAPIcalls-Network"></a>

### getconnectioncount `(empty)` <a id="JSON-RPCAPIcalls-getconnectioncount(empty)"></a>

### getcurrentnet `(empty)` <a id="JSON-RPCAPIcalls-getcurrentnet(empty)"></a>

### getnettotals `(empty)` <a id="JSON-RPCAPIcalls-getnettotals(empty)"></a>

### getnetworkhashps `(empty)` <a id="JSON-RPCAPIcalls-getnetworkhashps(empty)"></a>

### ping `(empty)` <a id="JSON-RPCAPIcalls-ping(empty)"></a>

### stop `(empty)` <a id="JSON-RPCAPIcalls-stop(empty)"></a>

## Server <a id="JSON-RPCAPIcalls-Server"></a>

### debuglevel `(empty)` <a id="JSON-RPCAPIcalls-debuglevel(empty)"></a>

### help `(empty)` <a id="JSON-RPCAPIcalls-help(empty)"></a>

### uptime `(empty)` <a id="JSON-RPCAPIcalls-uptime(empty)"></a>

### version `(empty)` <a id="JSON-RPCAPIcalls-version(empty)"></a>

## Irrelevant to non-wallet <a id="JSON-RPCAPIcalls-Irrelevanttonon-wallet"></a>

The original bitcoin core full node doubled as a wallet manager. This makes little sense, as it is rarely true that you want to keep your node on the same machine where you want to keep your wallet.

We decided to remove all wallet related RPC calls, which we list for completeness.

The calls are: 

`addmultisigaddress, addnode, createmultisig, dumpprivkey, encryptwallet, getaccount, getaccountaddress, getaddressesbyaccount, getbalance,` 

`getnewaddress, getrawchangeaddress, getreceivedbyaccount, getreceivedbyaddress, gettxoutsetinfo, importprivkey, keypoolrefill, listaccounts,` 

`listaddressgroupings, listlockunspent, listreceivedbyaccount, listreceivedbyaddress, listsinceblock, listtransactions, listunspent, lockunspent,` 

`move, sendfrom, sendmany, sendtoaddress, setaccount, settxfee, signmessage, signrawtransaction, walletlock, walletpassphrase, walletpassphrasechange.`

Template for future API calls

This API call has not yet been approved

#### `Parameters` <a id="JSON-RPCAPIcalls-Parameters.13"></a>

| Name | Type | Required | Default Value | Description |
| :--- | :--- | :--- | :--- | :--- |
|  |  |  |  |  |

#### `Output` <a id="JSON-RPCAPIcalls-Output.11"></a>

| `...` |
| :--- |


