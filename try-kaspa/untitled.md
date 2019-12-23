# CLI Wallet

The purpose of the Command Line Interface \(CLI\) Wallet is to allow basic user interaction with the Kaspa network. The CLI Wallet allows to create a wallet, to view its address and balance and to send a transaction.

The CLI Wallet creates a wallet \(private-public key pair\) on the local machine. It uses a Kasparov API server to send transactions and to get the address balance.

## Installing the CLI Wallet

## Creating a New Wallet

To create a new wallet, enter the command `wallet create`, with no input:

```
$ wallet create
```

{% hint style="success" %}
A Bech32 cashaddr private key \(PK\), and a set of public addresses \(PA\), one for each of the networks DevNet, TestNet and MainNet, will be created locally on the machine and output to console. **Keep the private key safe**, as it allows access to all funds in all three addresses.
{% endhint %}

```bash
$ wallet new
Private Key:              tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt
Public Address (DevNet):  kaspadev:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2
Public Address (TestNet): kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
Public Address (MainNet): kaspa:pr6m7j9njldwwzlg9v7v53unlr4jkmx6ey65nvtks5
```

## Checking the Balance

To check the balance of an address, enter the command `wallet balance` with the following input parameters:

* `address` - the public address to return the balance of.
* `api_address` - the address of the Kasparov API Server to use for relaying the balance request to the network.

```text
$ wallet balance --address=kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
                 --api_address=api.kas.pa
```

{% hint style="success" %}
The CLI Wallet will send a balance request on the input address, relayed through the input Kasparov API Server, using the REST API method [GET /utxos/{address}](../reference/kasparov-api-server/requests-list.md#utxos-address-address). The method returns a list of [TransactionOutput](../reference/kasparov-api-server/response-types.md#transactionoutput)s.
{% endhint %}

```text
$ wallet balance --address=kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
                 --api_address=api.kas.pa
Balance: KAS 0.123456789
```

{% hint style="info" %}
_The balance is the sum of the subset of the UTXO, belonging to the input_ `address`  
_If the UTXO does not contain any outputs belonging to the input_ `address`, _then balance = 0._
{% endhint %}

{% hint style="warning" %}
In case of an error, the error code and message received from Kasparov API Server will be output:
{% endhint %}

```text
$ wallet balance --address=kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t
                 --api_address=api.kas.pa
Error 403: Access denied.
```

Possible errors:

* Error 422: Unprocessable Entity - &lt;forwarded error from the Kaspa node&gt;
* Error 500: Internal Server Error - &lt;forwarded error from the API Server&gt;

## Receiving Funds

If you have just created your wallet, your wallet address will have a balance of Zero. In order to send a transaction, you need a positive balance. To get some Kaspa in your wallet, you can do one of three things:

* [ ] [Set up a miner](untitled-1/), and let it mine Kaspa into your wallet address.
* [ ] Get someone to send you some Kaspa \(ask on [Discord](https://discord.gg/WmGhhzk)\).
* [ ] Use the [Faucet](faucet.md) to drop you some Kaspa.

## Sending a Transaction

To send a transaction, enter the command `wallet send` with the following input parameters:

* `private_key` - the private key to access the unspent Kaspa on the source address.
* `to_address` - the public address to send the unspent Kaspa to.
* `send_amount` - the amount of Kaspa to send \(decimal number with up to 9 digits after the decimal point, e.g. 1,234.123456789\).
* `api_address` - the address of the Kasparov API Server, to relay the transaction to the network through.

{% hint style="info" %}
_Input fee is currently not supported. The fee will be determined by the difference between the inputs used by the transaction and_ send\_amount_._
{% endhint %}

```text
$ wallet send --private_key=tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt
              --to_address=kaspatest:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2
              --send_amount=1,234.12345678
              --api_address=api.kas.pa
```

{% hint style="info" %}

{% endhint %}

The **system** builds a transaction locally.

The **system** chooses unspent outputs to use as _inputs for the transaction_.  
E.g. by adding unspent outputs from oldest to newest, until their running sum is larger or equal to the send\_amount.

The **system** builds a transaction that uses the _inputs for the transaction_ to pay the send\_amount to the to\_address

_The result of the above process is a Transaction ID, generated locally._

The **system** sends the built transaction to the API Server available at the address specified by the inputapi\_address.

The REST API method is POST /transaction

The method returns 200 OK on success or an error code.

The **system** waits for a reply from the API Server.

In case of an error, outputs the error code and message received from the API Server to console.`1 2 3 4 5` `$ wallet send --private_key=tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt --to_address=kaspadev:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2 --send_amount=1,234.12345678 --api_address=api.kas.pa Error 403: Access denied.`

In case of success, outputs the \(hash of the\) Transaction ID to the console.`1 2 3 4 5` `$ wallet send --private_key=tb1qw508d6qejxtdg4y5r3zarvary0c5xw7kxpjzsxawgt --to_address=kaspatest:qr6m7j9njldwwzlg9v7v53unlr4jkmx6eylep8ekg2 --send_amount=1,234.12345678 --api_address=api.kas.pa Transaction ID: feee746c516ee9e97ef71c913186425efa8ff273ec6a1d2c58a3f0a449adf683`

