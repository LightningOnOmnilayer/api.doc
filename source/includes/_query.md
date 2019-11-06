# Query

This section collects all the messages used in querying the latest status of the channel, as well as specific transactions and their status. Each party may use this tool set to surveil the behavior of his counterparty. 

## Query list of commitment transactions

<!-- right -->
> either Alice or Bob:

```shell
{
    	"type": -35101,
    	"data":{
    		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
    	}
    }
```
 

> OBD Responses:

```json
{
	"type": -35101,
	"status":true,
	"from":"alice",
	"to":"alice",
	"result":[{
		"amount_b":0,
		"amount_m":6,
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197],
		"create_at":"2019-09-21T01:01:54.262628+08:00",
		"create_by":"bob",
		"curr_hash":"8a55f150c34e2720a587dde3809e81959701e1dd3f78c1e1fc9960c132566fe0",
		"curr_state":10,
		"id":4,
		"input_amount":6,
		"input_txid":"c734f981de217a95764e565ffeb6464507fda588118754df3d4ec5cbe487fd3b",
		"input_vout":2,
		"last_commitment_tx_id":0,
		"last_edit_time":"2019-09-21T01:01:54.262628+08:00",
		"last_hash":"",
		"multi_address":"2N3zxWFsT2nocWPiM7Qpc8rngQDv7PEX6yz",
		"owner":"alice",
		"peer_id_a":"alice",
		"peer_id_b":"bob",
		"property_id":121,
		"redeem_script":"522103ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952ae",
		"script_pub_key":"a91475f6a2aa9461985bed4b847104ca81b73cf2f1ac87",
		"send_at":"0001-01-01T00:00:00Z",
		"sign_at":"2019-09-21T01:01:54.704956+08:00",
		"temp_address_pub_key":"03ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d",
		"transaction_sign_hex_to_other":"",
		"transaction_sign_hex_to_temp_multi_address":"0200000001685a7e84bc2d24fa01550674176a6b4bf366a99e001b5b52f2fc16c8b6146b2600000000d900473044022008df14db1d40c4c53dbf87903ed3604e6075a80d96c8efbdc0899218c813d1df02203dde10f2139286ba4a533ea28f3b25c1394a359e1bf7db0a8b4245981a2aab3b014730440220194ed787685360c7c3b1dc67328db680e8e8d9e201753550c585efcf355beee1022023bf97e53ae59110f0c5f4160a2df723747ba61d65db5d6af7cc93ba70cc5ace01475221021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952aeffffffff033c1900000000000017a91475f6a2aa9461985bed4b847104ca81b73cf2f1ac870000000000000000166a146f6d6e6900000000000000790000000023c346001c0200000000000017a91475f6a2aa9461985bed4b847104ca81b73cf2f1ac8700000000",
		"txid_to_other":"",
		"txid_to_temp_multi_address":"8656156b4e70670745f0f9e46b64a84c7366bbad3182ebf135b6a5b60f10b1f1"
	}]
}

```

<!-- center -->

What is this message really querying about? The latest channel status?

**Message Type: -35101**
 
**Either Alice or Bob Requests:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 

 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of fundee
to             | ------- |   body    | peer id of funder
amount_m       | ------- |   result  | total tokens in this channel
channel_id            | ------- |   result  | 
create_at             | ------- |   result  | 
create_by             | ------- |   result  | 
curr_hash             | ------- |   result  | 
curr_state            | ------- |   result  | 
id                    | ------- |   result  | 
input_amount          | ------- |   result  | 
input_txid            | ------- |   result  | 
input_vout            | ------- |   result  | 
last_commitment_tx_id | ------- |   result  | 
last_edit_time        | ------- |   result  | 
last_hash             | ------- |   result  | 
multi_address         | ------- |   result  | 
owner                 | ------- |   result  | 
peer_id_a             | ------- |   result  | 
peer_id_b             | ------- |   result  | 
property_id           | ------- |   result  | 
redeem_script         | ------- |   result  | 
script_pub_key        | ------- |   result  | 
send_at               | ------- |   result  | 
sign_at               | ------- |   result  | 
temp_address_pub_key                                | ------- |   result  | 
transaction_sign_hex_to_other                       | ------- |   result  | 
transaction_sign_hex_to_temp_multi_address          | ------- |   result  | 
txid_to_other                                       | ------- |   result  | 
txid_to_temp_multi_address                          | ------- |   result  | 
 


## Latest commitment transaction

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35104,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
```


> OBD Responses：

```json
{
	"type": -35104,
	"status":true,
	"from":"alice",
	"to":"alice",
	"result":{
		"amount_b":0,
		"amount_m":6,
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197],
		"create_at":"2019-09-21T01:01:54.262628+08:00",
		"create_by":"bob",
		"curr_hash":"8a55f150c34e2720a587dde3809e81959701e1dd3f78c1e1fc9960c132566fe0",
		"curr_state":10,
		"id":4,
		"input_amount":6,
		"input_txid":"c734f981de217a95764e565ffeb6464507fda588118754df3d4ec5cbe487fd3b",
		"input_vout":2,
		"last_commitment_tx_id":0,
		"last_edit_time":"2019-09-21T01:01:54.262628+08:00",
		"last_hash":"",
		"multi_address":"2N3zxWFsT2nocWPiM7Qpc8rngQDv7PEX6yz",
		"owner":"alice",
		"peer_id_a":"alice",
		"peer_id_b":"bob",
		"property_id":121,
		"redeem_script":"522103ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952ae","script_pub_key":"a91475f6a2aa9461985bed4b847104ca81b73cf2f1ac87",
		"send_at":"0001-01-01T00:00:00Z",
		"sign_at":"2019-09-21T01:01:54.704956+08:00",
		"temp_address_pub_key":"03ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d",
		"transaction_sign_hex_to_other":"",
		"transaction_sign_hex_to_temp_multi_address":"0200000001685a7e84bc2d24fa01550674176a6b4bf366a99e001b5b52f2fc16c8b6146b2600000000d900473044022008df14db1d40c4c53dbf87903ed3604e6075a80d96c8efbdc0899218c813d1df02203dde10f2139286ba4a533ea28f3b25c1394a359e1bf7db0a8b4245981a2aab3b014730440220194ed787685360c7c3b1dc67328db680e8e8d9e201753550c585efcf355beee1022023bf97e53ae59110f0c5f4160a2df723747ba61d65db5d6af7cc93ba70cc5ace01475221021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952aeffffffff033c1900000000000017a91475f6a2aa9461985bed4b847104ca81b73cf2f1ac870000000000000000166a146f6d6e6900000000000000790000000023c346001c0200000000000017a91475f6a2aa9461985bed4b847104ca81b73cf2f1ac8700000000",
		"txid_to_other":"",
		"txid_to_temp_multi_address":"8656156b4e70670745f0f9e46b64a84c7366bbad3182ebf135b6a5b60f10b1f1"
	}
}

```


<!-- center -->
 

**Message Type: -35104**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 
 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status           | ------- |   body    | true or false
from	         | ------- |   body    | peer id of funder
to               | ------- |   body    | peer id of fundee
amount_b         | ------- |   result  | none required 
amount_m         | ------- |   result  | how much has been funded
channel_id       | ------- |   result  | 
create_at        | ------- |   result  | 
create_by        | ------- |   result  | created by the funder
curr_hash        | ------- |   result  | 
curr_state       | ------- |   result  | 
id               | ------- |   result  | 
input_amount     | ------- |   result  | 
input_txid       | ------- |   result  | 
input_vout       | ------- |   result  | 
last_commitment_tx_id           | ------- |   result  | 
last_edit_time      		| ------- |   result  | 
last_hash      			| ------- |   result  | 
multi_address      		| ------- |   result  | 
owner      			| ------- |   result  | 
peer_id_a   		        | ------- |   result  | 
peer_id_b     		        | ------- |   result  | 
property_id     		| ------- |   result  | 
redeem_script        		| ------- |   result  | 
script_pub_key      		| ------- |   result  | 
send_at      			| ------- |   result  | 
sign_at      			| ------- |   result  | 
temp_address_pub_key     	| ------- |   result  | 
transaction_sign_hex_to_other      		| ------- |   result  | 
transaction_sign_hex_to_temp_multi_address      | ------- |   result  | 
txid_to_other    				| ------- |   result  | 
txid_to_temp_multi_address     			| ------- |   result  | 



## Latest Revockable Delivery Transaction

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35105,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
``` 


<!-- center -->
 

**Message Type: -35105**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 

## Latest Breach Remedy Transaction

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35106,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
``` 


<!-- center -->
 

**Message Type: -35106**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 

## List all Revockable Delivery Transactions

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35108,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
``` 


<!-- center -->
 

**Message Type: -35108**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 
 
## List all Breach Remedy Transactions

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35109,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
``` 


<!-- center -->
 

**Message Type: -35109**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 

## List all Channels

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -3202, 
}
``` 


<!-- center -->
 

**Message Type: -3202**
 
**Either Alice or Bob can fetch the list of all channels**

This message has no arguments.

## List all Commitment Transactions Broadcasted

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35110,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
``` 


<!-- center -->
 

**Message Type: -35110**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 

## List all Commitment Transactions Broadcasted

<!-- right -->
 
> Either Alice or Bob can query: 

 
```shell
{
	"type": -35110,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197]
	}
}
``` 


<!-- center -->
 

**Message Type: -35110**
 
**Either Alice or Bob can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
 

<!-- Added by Kevin 2019-11-6 -->

## Get HTLC base data and the H (Hash_Preimage_R)

<!-- right -->
 
> Only HTLC creator can query: 

```json
{
	"type": -4001
}
``` 

<!-- center -->
 
**Message Type: -4001**
 
**Only HTLC creator can query:**

This message has no arguments.

## Get HTLC base data and the R (Preimage_R)

<!-- right -->
 
> Only HTLC Receiever can query: 

```json
{
	"type": -4101
}
``` 

> OBD Responses:

```json
{
    "type": -4101, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": [
        {
            "amount": 5, 
            "create_at": "2019-11-04T15:02:25.2004375+08:00", 
            "create_by": "alice", 
            "curr_state": 20, 
            "h": "83519233492eb05ddd547757f2c3d151ad9392b2ebf48fc1a88e07e61dd82a45", 
            "id": 1, 
            "property_id": 121, 
            "r": "2de142c8006a3462241e96a610b59f3d92d8259c", 
            "recipient_peer_id": "carol", 
            "request_hash": "742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636", 
            "sender_peer_id": "alice", 
            "sign_at": "2019-11-04T15:08:31.5417759+08:00", 
            "sign_by": "carol"
        }, 
        {
            "amount": 5, 
            "create_at": "2019-11-06T08:02:50.8302374+08:00", 
            "create_by": "alice", 
            "curr_state": 20, 
            "h": "e7626f2b7207006d6515399c587c09c3bfb5ed3b12f63c12b0d40e634f9dd9a3", 
            "id": 2, 
            "property_id": 121, 
            "r": "2de142c8006a3462241e96a610b59f3d92d8259c", 
            "recipient_peer_id": "carol", 
            "request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9", 
            "sender_peer_id": "alice", 
            "sign_at": "2019-11-06T08:03:41.3545586+08:00", 
            "sign_by": "carol"
        }
    ]
}
```

<!-- center -->
 
**Message Type: -4101**
 
**Only HTLC Receiever can query:**

This message has no arguments.

## The middleman node get the R (Preimage_R) by channel id

<!-- right -->
 
> Only middleman node can query: 

```json
{
	"type":-4103,
	"data":{
		"channel_id":[223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51]
	}
}
``` 

<!-- center -->
 
**Message Type: -4103**
 
**Only middleman node can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 

## HTLC Receiever get the R by H

<!-- right -->
 
> HTLC Receiever can query: 

```json
{
	"type":-4105,
	"data":"e7626f2b7207006d6515399c587c09c3bfb5ed3b12f63c12b0d40e634f9dd9a3"
}
``` 

> OBD Responses:

```json
{
    "type": -4105, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": "\"2de142c8006a3462241e96a610b59f3d92d8259c\""
}
```

<!-- center -->
 
**Message Type: -4105**
 
**HTLC Receiever can query:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
H         | ------- |   data    | Hash_Preimage_R

