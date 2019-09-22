---
description: Draft
---

# Kaspa btcctl bash commands

## addManualNode

###  "addr" \(onetry=false\)

## createRawTrasnaction

### \[{"txid":"value","vout":n},...\] {"address":amount,...} \(locktime\)



## debugLevel "levelspec"

## decodeRawTransaction "hextx"

## decodeScript "hexscript"

## flushDbCache

## generate numblocks

## getAllManualNodesInfo \(details=true\)

{% api-method method="get" host=":~/kaspacoin/btcd/cmd/btcctl$ go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getInfo --notls" path="" %}
{% api-method-summary %}
getInfo
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "version": 120000,
  "protocolVersion": 70002,
  "blocks": 111636,
  "timeOffset": 0,
  "connections": 0,
  "proxy": "",
  "difficulty": 1.25334631,
  "testNet": false,
  "devNet": true,
  "relayFee": 0.00001,
  "errors": ""
}

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=":~/kaspacoin/btcd/cmd/btcctl$ go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getBlockDagInfo --notls" path="" %}
{% api-method-summary %}
getBlockDagInfo
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "dag": "devnet",
  "blocks": 111636,
  "headers": 111636,
  "tipHashes": [
    "00001c0bd807e66ad7907fb2cc73ae3e1aabce2e20a4d03737b1d0ea718f0344"
  ],
  "difficulty": 1.25334631,
  "medianTime": 1566978717,
  "utxoCommitment": "6dcda0b0fc8b47e71b1039707f1e07d70297803283cf768b05dd0ca14b084ad7",
  "pruned": false,
  "softForks": null,
  "bip9SoftForks": {
    "dummy": {
      "status": "failed",
      "bit": 28,
      "startTime": 1199145601,
      "timeout": 1230767999,
      "since": 0
    }
  }
}

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="" path="" %}
{% api-method-summary %}

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

