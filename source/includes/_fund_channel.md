# Deposit BTC for miner fee

Alice tells her OBD to create a funding transaction. Since on Omnilayer/BTC, the miner fee is bitcoin, so that first we need to deposit small amount of bitcoin used in withdraw money, creating temporary multi-sig addresses for transactions in RSMC and HTLC.  

After that, we need to create the second part of transactions that deposit tokens which both sides agreed to fund the channel.  

This is the place that OBD differs from LND or other lightning implementation, since they are for BTC only, so there is no need for them to deposit extra bitcoin for fee.

<aside class="warning">Another difference is that we need to transactions for the miner fees, one for the channel, and the other for a internal temporary multi-sig address. So Alice may raise request twice, and Bob needs to answer twice.


It is not finalized yet.
</aside>

## funder create funding transaction

<!-- right -->
> Alice's data:

```shell
address: mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs  
privkey: cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy  
channel address：2N1DFjaE4yCcECdFwgLQcLmNrLV5zetgQtE  
```
 
> Alice requests:

```shell
{
    	"type":1009,
    	"data":{
			"fromBitCoinAddress":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
			"fromBitCoinAddressPrivKey":"cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy",
			"toBitCoinAddress":"2N1DFjaE4yCcECdFwgLQcLmNrLV5zetgQtE",
			"amount":0.0001,
			"minerFee":0.00001,
    	}
    
}
```

> OBD Responses:

```json
{
	"type":1009,
	"status":true,
	"from":"alice",
	"to":"alice",
	"result":
	{
		"hex":"0200000001c77e31c262301a13d0fa6cec9827b25017bd30fb1ad3793b2c8ff66461fc7ab5000000006a473044022047c441b3f34bcce960f667391941a4db29de16bb00dbee97d4c85f60995d4b5102201a4f2d13a0cc22e7e62ee3fe04d0cc7dd36e375108a9242ede7aa718a5c5578a0121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff02102700000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468748170f00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac00000000",
		"txid":"266b14b6c816fcf2525b1b009ea966f34b6b6a1774065501fa242dbc847e5a68"
	}
}
```
<!-- center -->

**Message Type: 1009**
 
**Alice Requests**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
fromBitCoinAddress         | ------- |   data    | address of funder, from where the BTC is transferred.
fromBitCoinAddressPrivKey  | ------- |   data    | private key.
toBitCoinAddress           | ------- |   data    | the channel multi-sig address.
amount			   | 0.0001  |   data    | 
minerFee		   | 0.00001 |   data    | 
 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
hex            | ------- |   result  | 
txid	       | ------- |   result  | transaction id


## funder notifies fundee

<!-- right -->
> Alice: 

```shell
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],
		"channel_address_private_key":"cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy",  			"funding_tx_hex":"0200000001c77e31c262301a13d0fa6cec9827b25017bd30fb1ad3793b2c8ff66461fc7ab5000000006a473044022047c441b3f34bcce960f667391941a4db29de16bb00dbee97d4c85f60995d4b5102201a4f2d13a0cc22e7e62ee3fe04d0cc7dd36e375108a9242ede7aa718a5c5578a0121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff02102700000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468748170f00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac00000000"
	}
}
``` 

> OBD Responses：

```json
{
	"type":-3400,
	"status":true,
	"from":"alice",
	"to":"alice",
	"result":
	{
		"amount":0.0001,
		"funding_txid":"266b14b6c816fcf2525b1b009ea966f34b6b6a1774065501fa242dbc847e5a68",
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183]
	}
}
```


<!-- center -->
Alice tells her OBD to notify Bob that she created the funding transaction by payloads packed in the message -3400.  

**Message Type: -3400**
 
**Alice Requests**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | 
channel_address_private_key  | ------- |   data    | private key of the channel that Alice holds
funding_tx_hex               | ------- |   data    | 

 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
amount         | ------- |   result  | 
funding_txid   | ------- |   result  | funding transaction id
temporary_channel_id  | ------- |   result  |


## fundee replies

<!-- right -->
> Bob relies: 

```shell
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],
		"channel_address_private_key":"cV6dif91LHD8Czk8BTgvYZR3ipUrqyMDMtUXSWsThqpHaQJUuHKA",
		"funding_txid":"266b14b6c816fcf2525b1b009ea966f34b6b6a1774065501fa242dbc847e5a68",
		"approval":true
    	}
}
``` 

> OBD Responses：

```json
{
	"type":-3500,
	"status":true,
	"from":"bob",
	"to":"alice",
	"result":
	{
		"channel_id":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
		"create_at":"2019-09-21T00:14:42.17012+08:00",
		"id":3,
		"owner":"alice",
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],
		"tx_hash":"0200000001685a7e84bc2d24fa01550674176a6b4bf366a99e001b5b52f2fc16c8b6146b2600000000d90047304402206724a55a5f58c8cc0325e5b2d7e9bce844c621194777a2bac4ae18f0794fe316022038beaa93be8cd70b7337b68c9fb48377c4833739a1165e3104db8b50a6d75573014730440220723785063cd4767cd275d6f861ab3147b29ba8035123ca140d6c09f329562cac02200d3e25d2b2e10975bcf98241715c87d8c380e92fa5aad7e8470da1d2e260ff3701475221021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952aeffffffff01581b0000000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac00000000","txid":"cd30f25260a7d5971949802974f57da45341146404ebb26db3917ef5bc841b9e"
	}
}
```


<!-- center -->
Bob tells his OBD to reply Alice that he knows the funding by message -3500. 


**Message Type: -3500**
 
**Bob Replies**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | 
channel_address_private_key  | ------- |   data    | private key of the channel that Bob holds
funding_txid                 | ------- |   data    | 
approval                     | ------- |   data    | true or false
 

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of fundee
to             | ------- |   body    | peer id of funder
channel_id            | ------- |   result  | 
create_at             | ------- |   result  | 
id                    | ------- |   result  | 
owner                 | ------- |   result  | the funder is the owner
temporary_channel_id  | ------- |   result  |
tx_hash               | ------- |   result  |
 
