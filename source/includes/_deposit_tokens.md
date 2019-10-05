# Deposit Omni Assets

Alice starts to deposit omni assets to the channel. This is quit similar to the the btc funding procedure.

## funder creates token funding transaction

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
    	"type":2001,
    	"data":{
			"fromBitCoinAddress":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
			"fromBitCoinAddressPrivKey":"cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy",
			"toBitCoinAddress":"2N1DFjaE4yCcECdFwgLQcLmNrLV5zetgQtE",
			"amount":6,
			"propertyId": 121
                
	}
}
```

> OBD Responses:

```json
{
	"type":2001,
	"status":true,
	"from":"alice",
	"to":"bob",
	"result":{
		"hex":"02000000016aabc30d6ef0d7e1d02572ca30450421a7541b7bf3d9b9937240b977b228215c010000006a473044022077352945e8cbc96475ba6e8af7f808980eeb93560ecca416dc5e94e561af0ebe022044a1c15a436594bade0db2a955e05b629d8611f15b3603aea13100b0ec2798900121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff0384b30e00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac0000000000000000166a146f6d6e6900000000000000790000000023c346001c0200000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468700000000"
		"txid":"missing??? our development team?"	
	}
}
```

<!-- center -->

**Message Type: 2001**
 
**Alice Requests**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
fromBitCoinAddress         | ------- |   data    | address of funder, from where the BTC is transferred.
fromBitCoinAddressPrivKey  | ------- |   data    | private key.
toBitCoinAddress           | ------- |   data    | the channel multi-sig address.
amount			   | 0.0001  |   data    | 
propertyId		   | ------- |   data    | the omni asset id that has been funding.
 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
hex            | ------- |   result  | 
txid	       | ------- |   result  | transaction id


## funder notifies fundee the token funding is created

<!-- right -->
> Alice data: 

```shell
address  n2gj8MDzUU7JZ6eVF5VpXcL4wUfaDXzTfJ
privkey   cSgTisoiZLzH5vrwHBMAXLC5nvND2ffcqqDtejMg12rEVrUMeP5R
pubkey   03ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d
```

> Alice sends: 

 
```shell
{
	"type":-34,
	"data":{
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],  			"funding_tx_hex":"02000000016aabc30d6ef0d7e1d02572ca30450421a7541b7bf3d9b9937240b977b228215c010000006a473044022077352945e8cbc96475ba6e8af7f808980eeb93560ecca416dc5e94e561af0ebe022044a1c15a436594bade0db2a955e05b629d8611f15b3603aea13100b0ec2798900121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff0384b30e00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac0000000000000000166a146f6d6e6900000000000000790000000023c346001c0200000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468700000000",		
		"temp_address_pub_key":"03ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d",
		"temp_address_private_key":"cSgTisoiZLzH5vrwHBMAXLC5nvND2ffcqqDtejMg12rEVrUMeP5R",
		"channel_address_private_key":"cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy"
	}
}
```


> OBD Responses：

```json
{
	"type":-34,
	"status":true,
	"from":"alice",
	"to":"bob",
	"result":{
		"amount_a":6,
		"amount_b":0,
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197],
		"channel_info_id":2,
		"create_at":"2019-09-21T00:58:55.448353+08:00",
		"create_by":"alice",
		"curr_state":10,
		"fundee_sign_at":"0001-01-01T00:00:00Z",
		"funder_address":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
		"funder_pub_key_2_for_commitment":"03ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d",
		"funding_output_index":2,"funding_tx_hex":"02000000016aabc30d6ef0d7e1d02572ca30450421a7541b7bf3d9b9937240b977b228215c010000006a473044022077352945e8cbc96475ba6e8af7f808980eeb93560ecca416dc5e94e561af0ebe022044a1c15a436594bade0db2a955e05b629d8611f15b3603aea13100b0ec2798900121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff0384b30e00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac0000000000000000166a146f6d6e6900000000000000790000000023c346001c0200000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468700000000",
		"funding_txid":"c734f981de217a95764e565ffeb6464507fda588118754df3d4ec5cbe487fd3b",
		"id":2,
		"peer_id_a":"alice",
		"peer_id_b":"bob",
		"property_id":121
	}
}
```


<!-- center -->
Alice tells her OBD to notify Bob that she created the token funding transaction by payloads packed in the message -34.  

**Message Type: -34**
 
**Alice sends**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | 
funding_tx_hex               | ------- |   data    | 
temp_address_pub_key         | ------- |   data    | 
temp_address_private_key     | ------- |   data    | 
channel_address_private_key  | ------- |   data    | private key of the channel that Alice holds
 
 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status           | ------- |   body    | true or false
from	         | ------- |   body    | peer id of funder
to               | ------- |   body    | peer id of fundee
amount_a         | ------- |   result  | how much has been funded
amount_b         | ------- |   result  | none required
channel_id       | ------- |   result  | channel officially created
channel_info_id  | ------- |   result  | 
create_at        | ------- |   result  | 
create_by        | ------- |   result  | created by the funder
curr_state       | ------- |   result  | 
fundee_sign_at   | ------- |   result  | 
funder_address   | ------- |   result  | 
funder_pub_key_2_for_commitment   | ------- |   result  | 
funding_output_index"             | ------- |   result  |
funding_tx_hex                    | ------- |   result  | 
funding_txid                      | ------- |   result  | 
id                                | ------- |   result  | 
peer_id_a                         | ------- |   result  | 
peer_id_b                         | ------- |   result  | 
property_id                       | ------- |   result  | 


## fundee replies knowing the token has been funded


<!-- right -->
> Bob sends: 

```shell
{
	"type":-35,
	"data":{
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197],
		"fundee_channel_address_private_key":"cV6dif91LHD8Czk8BTgvYZR3ipUrqyMDMtUXSWsThqpHaQJUuHKA",
		"approval":true
	}
}
``` 

> OBD Responses：

```json
{
	"type":-35,
	"status":true,
	"from":"bob",
	"to":"bob",
	"result":{
		"amount_a":6,
		"amount_b":0,
		"channel_id":[59,253,135,228,203,197,78,61,223,84,135,17,136,165,253,7,69,70,182,254,95,86,78,118,149,122,33,222,129,249,52,197],
		"channel_info_id":2,
		"create_at":"2019-09-21T00:58:55.448353+08:00",
		"create_by":"alice",
		"curr_state":20,
		"fundee_sign_at":"2019-09-21T01:01:53.970256+08:00",
		"funder_address":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
		"funder_pub_key_2_for_commitment":"03ea01f8b137df5744ec2b0b91bc46139cabf228403264df65f6233bd7f0cbd17d",
		"funding_output_index":2,
		"funding_tx_hex":"02000000016aabc30d6ef0d7e1d02572ca30450421a7541b7bf3d9b9937240b977b228215c010000006a473044022077352945e8cbc96475ba6e8af7f808980eeb93560ecca416dc5e94e561af0ebe022044a1c15a436594bade0db2a955e05b629d8611f15b3603aea13100b0ec2798900121021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32dffffffff0384b30e00000000001976a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac0000000000000000166a146f6d6e6900000000000000790000000023c346001c0200000000000017a9145761a1d45b8a6e7caa10a4bcecca97630c67af468700000000",
		"funding_txid":"c734f981de217a95764e565ffeb6464507fda588118754df3d4ec5cbe487fd3b",
		"id":2,
		"peer_id_a":"alice",
		"peer_id_b":"bob",
		"property_id":121
	}
}
```


<!-- center -->
Bob tells his OBD to reply Alice that he knows the funding by message -35, and OBD has created commitment transactions (C1a & RD1a)

**Message Type: -35**
 
**Bob Replies**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id         			| ------- |   data    | 
fundee_channel_address_private_key  	| ------- |   data    | private key of the channel that Bob holds
funding_txid                		| ------- |   data    | transaction id missing???
approval                    		| ------- |   data    | true or false
 

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of fundee
to             | ------- |   body    | peer id of funder
channel_id            | ------- |   result  | 
amount_a 	      | ------- |   result  | how much the funder deposited
amount_b	      | ------- |   result  | none required
channel_id            | ------- |   result  | 
channel_info_id       | ------- |   result  | 
create_at             | ------- |   result  | 
create_by             | ------- |   result  | 
curr_state            | ------- |   result  | 
fundee_sign_at        | ------- |   result  | 
funder_address        | ------- |   result  | 
funder_pub_key_2_for_commitment         | ------- |   result  | 
funding_output_index                    | ------- |   result  | 
funding_tx_hex                          | ------- |   result  | 
funding_txid                            | ------- |   result  | 
id                                      | ------- |   result  | 
peer_id_a                               | ------- |   result  | 
property_id                             | ------- |   result  | 
 
 
