# SendBitcoin

## Simple Type 1009 Protocol

Type 1009 Protocol is used for transfering bitcoin.

## Websocket Request: Message Type 1009

> Request:

```json
{
	"type":1009,
	"data":{
		"from_address":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
		"from_address_private_key":"cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy",
		"to_address":"2N1DFjaE4yCcECdFwgLQcLmNrLV5zetgQtE",
		"amount":0.0001,
		"miner_fee":0.00001,
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
from_address | ------- |   data    | address of funder, from where the BTC is transferred.
from_address_private_key  	| ------- |   data    | private key.
to_address           		| ------- |   data    | the channel multi-sig address.
amount			   	| 0.0001  |   data    | 
miner_fee		   	| 0.00001 |   data    | 
 
## Websocket Response:

> OBD Responses:

```json
{
	"type":1009,
	"status":true,
	"from":"<user_id>",
	"to":"<user_id>",
	"result":
	{
		"hex":"0200000001c77e31c262301a13d0fa6cec9827b25017bd30fb1ad3793b2c8ff66461fc7ab5000000006a473044022047c441b3f34bcce960f667391941a4db29de16bb00dbee97d4c85f60995d4b5102201a4f2d13a0cc22e7e62ee3fe04d0cc7dd36e375108a9242ede7aa718a5c5578a0121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff02102700000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468748170f00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac00000000",
		"txid":"266b14b6c816fcf2525b1b009ea966f34b6b6a1774065501fa242dbc847e5a68"
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
hex            | ------- |   result  | 
txid	       | ------- |   result  | transaction id


# NotifySendingBitcoin

## Simple Type -3400 Protocol

Type -3400 Protocol is used to notify the transfering bitcoin event 
to the partner of the channel .

*Alice tells her OBD to notify Bob that she created the funding transaction by payloads packed in the message -3400.*

## Websocket Request: Message Type -3400

> Request:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],
		"channel_address_private_key":"cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy",  			"funding_tx_hex":"0200000001c77e31c262301a13d0fa6cec9827b25017bd30fb1ad3793b2c8ff66461fc7ab5000000006a473044022047c441b3f34bcce960f667391941a4db29de16bb00dbee97d4c85f60995d4b5102201a4f2d13a0cc22e7e62ee3fe04d0cc7dd36e375108a9242ede7aa718a5c5578a0121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff02102700000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468748170f00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac00000000"
	}
}
``` 

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | 
channel_address_private_key  | ------- |   data    | private key of the channel that Alice holds
funding_tx_hex               | ------- |   data    | 

## Websocket Response:

> OBD Responses：

```json
{
	"type":-3400,
	"status":true,
	"from":"<user_id>",
	"to":"<user_id>",
	"result":
	{
		"amount":0.0001,
		"funding_txid":"266b14b6c816fcf2525b1b009ea966f34b6b6a1774065501fa242dbc847e5a68",
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183]
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
amount         | ------- |   result  | 
funding_txid   | ------- |   result  | funding transaction id
temporary_channel_id  | ------- |   result  |


# ResponseSendingBitcoin

## Simple Type -3500 Protocol

Type -3500 Protocol is used to response the transfering bitcoin event 
by the partner of the channel .

*Bob tells his OBD to reply Alice that he knows the funding by message -3500.*

## Websocket Request: Message Type -3500

> Request:

```json
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

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | 
channel_address_private_key  | ------- |   data    | private key of the channel that Bob holds
funding_txid                 | ------- |   data    | 
approval                     | ------- |   data    | true or false
 
## Websocket Response:

> OBD Responses：

```json
{
	"type":-3500,
	"status":true,
	"from":"<user_id>",
	"to":"<user_id>",
	"result":
	{
		"channel_id":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
		"create_at":"2019-09-21T00:14:42.17012+08:00",
		"id":3,
		"owner":"<user_id>",
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],
		"tx_hash":"0200000001685a7e84bc2d24fa01550674176a6b4bf366a99e001b5b52f2fc16c8b6146b2600000000d90047304402206724a55a5f58c8cc0325e5b2d7e9bce844c621194777a2bac4ae18f0794fe316022038beaa93be8cd70b7337b68c9fb48377c4833739a1165e3104db8b50a6d75573014730440220723785063cd4767cd275d6f861ab3147b29ba8035123ca140d6c09f329562cac02200d3e25d2b2e10975bcf98241715c87d8c380e92fa5aad7e8470da1d2e260ff3701475221021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952aeffffffff01581b0000000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac00000000",
		"txid":"cd30f25260a7d5971949802974f57da45341146404ebb26db3917ef5bc841b9e"
	}
}
```

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
tx_hash               | ------- |   result  | hash of transaction
txid                  | ------- |   result  | transaction id
 