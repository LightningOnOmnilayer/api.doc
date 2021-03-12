# HTLC Payment
 
## wsAddInvoice

### Simple Type -100402 Protocol

create an invoice.

### Websocket Request: Message Type -100402

>Request:

```json
req
{
    "type":-100402,
    "data":{
        "property_id":137,
        "amount":4,
        "h":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933",
        "expiry_time":"2020-07-15",
        "description":"description",
        "is_private":false
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
property_id         | ------- |   data    | assets id
amount	            | ------- |   data    | amount of transfer
h         | ------- |   data    | the hash of r used to lock a HTLC.
expiry_time	    | ------- |   data    | expiry time.
description	    | ------- |   data    | short memo.
is_private          | ------- |   data    | true to private channel or false to open channel.

### Websocket Response: Message Type 100402

> OBD Responses:

```json
{
	"type":-100402,
	"data": { 
		"invoice":"obtb400000000s1pqzyfnpwQmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiontuzq1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2hzz035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933xq8p0sm45qdqtdescriptionjf4"
    }
}

```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
invoice   | ------- |   data    | the invoice string encoded by beth32. 


## payInvoice

### A SDK Function

Pay an invoice. This function firstly seeks a full path of nodes, decides which path is the optimistic one, in terms of hops, node's histroy service quility, and fees. Then it construct an HTLC and pay to the payee who creates the invoice.  


### Request: 

>Request:

```json
{
	"data": {
		"invoice":"obtb400000000s1pqzyfnpwQmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiontuzq1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2hzz035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933xq8p0sm45qdqtdescriptionjf4"
	}
}
```
 
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
invoice   | ------- |   data    | the invoice string encoded by beth32. 


## HTLCFindPath

### Simple Type -100401 Protocol

This protocol is the first step of a payment, which seeks a full path of nodes, decide which path is the optimistic one, in terms of hops, node's histroy service quility, and fees. 

There is two ways to do it. One is through an invoice and the other is through some detail data.

### Websocket Request: Message Type -100401

>Request:

```json
{
	"type":-100401,
	"data": {
		"invoice":"obtb400000000s1pqzyfnpwQmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiontuzq1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2hzz035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933xq8p0sm45qdqtdescriptionjf4"
	}
}

OR

{
    "type":-100401,
    "recipient_node_peer_id":"Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"recipient_user_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
	"data"{
        "property_id":137,
        "amount":4,
		"h":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933",
        "expiry_time":"2020-07-15",
        "description":"description",
        "is_private":false
	}
}
```
 
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
invoice   | ------- |   data    | the invoice string encoded by beth32. 

OR

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
property_id         | ------- |   data    | assets id
amount	            | ------- |   data    | amount of transfer
h         | ------- |   data    | the hash of r used to lock a HTLC.
expiry_time	    | ------- |   data    | expiry time.
description	    | ------- |   data    | short memo.
is_private          | ------- |   data    | true to private channel or false to open channel.

### Websocket Response: Message Type -100401

>Response:

```json
{
	"type":-100401,
	"status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/3001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"result":{
		"min_cltv_expiry":1,
		"next_node_peerId":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
		"routing_packet":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9"
    	}
}
``` 

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
min_cltv_expiry   | ------- |   result    | min cltv expiry.
next_node_peerId  | ------- |   result    | next hop.
routing_packet	  | ------- |   result    | routing packet.


## addHTLC

### Simple Type -100040 Protocol

Add an HTLC.

<!-- right -->
> current stage before adding a new HTLC:

```shell
H and R
{
        "address":"mgxXToovRhRictkY1oDZfPn36Kx6hu2rtV",
        "index":16,
        "pub_key":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933",
        "wif":"cUAa45B7TWp6FbK6N15WLq5pXDCKUJj98RSXm4QTV5WsQNeF6tmn"
}
alice's previous temp rsmc 

{
        "address":"mhAMwJW2bMtJ5iW4dp1EUT8nGi9LF1PXgj",
        "index":6,
        "pub_key":"03985e8880628058da7c49b0968e4e7d2819240b60255a1c9b5f2407a4056b5f54",
        "wif":"cTLc9tx1Hihqfu6jXXWQ8pFeiQb5aJ8ck88ik8RJuutP95dqTuqo"
}
alice's current rsmc
{
        "address":"mirZzX7MRF87hpw7ST3JS8RvnKh7EZ1P4s",
        "index":13,
        "pub_key":"0253a778da18945a91fccfa40ad09ac4d276991ab70ad6e5b1009b68434d57b8b3",
        "wif":"cNwgTtsRaWdDWdNVA4aPEoumS9prpYirE3Q1BuARrphjH9THVeH2"
}
alice's htlc
{
        "address":"mudzJLK5ncD1awLbZf2FhWJgbfe3sWKDMj",
        "index":14,
        "pub_key":"03f40b44a99393408d705806b09531c47d042ab332e7cd59c39e2aa67df0bc926c",
        "wif":"cRVqzokvBvtzw91Nv4zkvmad7uRBf5eMfr5t5EDL7q2U6VxBTmoy"
}
alice's ht1a
{
        "address":"mj8Pressjgn2mfzPTB2G7U2unvtVmSh6Aa",
        "index":15,
        "pub_key":"036266f3a46ef8e413211c55d9c48a54b237ce5479470535ddc82880e19740f7a8",
        "wif":"cUxBiu8jVgk9gDhdgpF3S4wFrpFBkAFujCEVgCRYBsQaTDtUdVMb"
}
```

### Websocket Request: Message Type -100040

> Request:

```json
{
	"type":-100040,
	"recipient_node_peer_id":"Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"recipient_user_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
	"data":{  
		// "property_id":137,
		"amount":4,
		"memo":"memo",
		"h":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933",
		"routing_packet":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
		"cltv_expiry":1,
		"channel_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf",
		"last_temp_address_private_key":"cTLc9tx1Hihqfu6jXXWQ8pFeiQb5aJ8ck88ik8RJuutP95dqTuqo",
		"curr_rsmc_temp_address_pub_key":"0253a778da18945a91fccfa40ad09ac4d276991ab70ad6e5b1009b68434d57b8b3",
		"curr_rsmc_temp_address_private_key":"cNwgTtsRaWdDWdNVA4aPEoumS9prpYirE3Q1BuARrphjH9THVeH2",
		"curr_htlc_temp_address_pub_key":"03f40b44a99393408d705806b09531c47d042ab332e7cd59c39e2aa67df0bc926c",
		"curr_htlc_temp_address_private_key":"cRVqzokvBvtzw91Nv4zkvmad7uRBf5eMfr5t5EDL7q2U6VxBTmoy",
		"curr_htlc_temp_address_for_ht1a_pub_key":"036266f3a46ef8e413211c55d9c48a54b237ce5479470535ddc82880e19740f7a8",
		"curr_htlc_temp_address_for_ht1a_private_key":"cUxBiu8jVgk9gDhdgpF3S4wFrpFBkAFujCEVgCRYBsQaTDtUdVMb"
	}
}

```

Payer requests his OBD by sending the created pub/priv keys and other info about the payee.  

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
<!-- property_id         | ------- |   data    | assets id. -->
amount	            | ------- |   data    | amount of transfer.
memo   		    | ------- |   data    | short memo for this tranfer.
h  		    | ------- |   data    | preimage of r to Lock a HTLC.
routing_packet	    | ------- |   data    | routing info of next hop.
cltv_expiry	    | ------- |   data    | cltv expiry.
channel_address_private_key	| ------- |   data    | payer's private key of channel address.
last_temp_address_private_key	| ------- |   data    | previous private key of temporary multi-sig address that stores money of pay-to-rsmc.
curr_rsmc_temp_address_pub_key	| ------- |   data    | rsmc temp address for current HTLC.
curr_rsmc_temp_address_private_key		| ------- |   data    | private key of rsmc temp address for current HTLC.
curr_htlc_temp_address_pub_key			| ------- |   data    | pub key of htlc temp address.
curr_htlc_temp_address_private_key		| ------- |   data    | private key of htlc temp address.
curr_htlc_temp_address_for_ht1a_pub_key		| ------- |   data    | pub key of ht1a temp address.
curr_htlc_temp_address_for_ht1a_private_key	| ------- |   data    | private key of ht1a temp address.


### Websocket Response: Message Type -110040

> OBD Responses:

```json
{
	"type":-110040,
	"status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"to":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"result":{
		"amount":4,
		"channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
		"cltv_expiry":1,
		"commitment_tx_hash":"4a5854c6cc0d1930500e05353aff416b8cdd1859dc2c2af44b46089e23938023", 			"counterparty_tx_hex":"0200000002ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cdeb020000009200473044022042a2b6efa34c52409cdbd4971526824830e6fb1d49a7747634f9ab5ece6f28ad02206f40b96b2f13144eee9e2d686eaa7e880e8da29999c5a840662b04f221e537ad010047522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aefffffffffcabe9c10966c7640e41fcbc1e597991d4da4edc2f6e257c6159c1d7c34a8b8200000000920047304402200f7c74406f0c6553fd141caf19305c42d9dcdf487dda56e30144a02325a76ebd02205eba8119193b05fa1829c4d3117c9582987347e7a2e5cdccd7cd151a2a8e5128010047522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03d7470000000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac0000000000000000166a146f6d6e6900000000000000890000000011e1a30022020000000000001976a914ceb05f96f5b3cbd12d7bff5f9f45252b3f67bf7088ac00000000",
		"curr_htlc_temp_address_for_ht1a_pub_key":"036266f3a46ef8e413211c55d9c48a54b237ce5479470535ddc82880e19740f7a8",
		"curr_htlc_temp_address_pub_key":"03f40b44a99393408d705806b09531c47d042ab332e7cd59c39e2aa67df0bc926c",
		"curr_rsmc_temp_address_pub_key":"0253a778da18945a91fccfa40ad09ac4d276991ab70ad6e5b1009b68434d57b8b3",
		"h":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933", 			"htlc_tx_hex":"0200000001e062124a8f5476cd4cff020894525d360f6d0ab4a8d2b357a90f328c3dabe2d900000000920047304402204709f197d1def131fdee07957ff110fca149a33db87ec8e59032000034668c99022009e0dfe7af7c0b065c1770e94f7733684b9df1f000ad85d105824238b0e8d232010047522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a9147abd515aab0f074a6ae0b993e75f10107dd382bb870000000000000000166a146f6d6e6900000000000000890000000017d784001c0200000000000017a9147abd515aab0f074a6ae0b993e75f10107dd382bb8700000000",
		"last_temp_address_private_key":"cTLc9tx1Hihqfu6jXXWQ8pFeiQb5aJ8ck88ik8RJuutP95dqTuqo",
		"memo":"memo",
		"routing_packet":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",  			"rsmc_tx_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a900000000920047304402205868611dc0c9437adc3fc6ef9e2b14b1d8449f66c8d20758a58287733118324f0220454da7c12c60ae0639af6fd0d4f87cf8608318bd00ecd75e455efbad8d479b08010047522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a9143e19c938d5330b5d5a244573c778acfaf7e674c3870000000000000000166a146f6d6e690000000000000089000000004d7c6d001c0200000000000017a9143e19c938d5330b5d5a244573c778acfaf7e674c38700000000"
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
amount         | ------- |   result  | amount of transfer
channel_id     | ------- |   result  | channel id
cltv_expiry    | ------- |   result  | cltv expiry
commitment_tx_hash	| ------- |   result  | cltv expiry 
counterparty_tx_hex	| ------- |   result  | cltv expiry 
curr_htlc_temp_address_for_ht1a_pub_key		| ------- |   result  | pub key of curr temp address for ht1a
curr_htlc_temp_address_pub_key 			| ------- |   result  | pub key of curr htlc temp address 
curr_rsmc_temp_address_pub_key			| ------- |   result  | pub key of curr rsmc temp address
h						| ------- |   result  | preimage of r to Lock a HTLC.
htlc_tx_hex					| ------- |   result  | hex of the htlc transaction.
last_temp_address_private_key			| ------- |   result  | private key of the previous temp address
memo						| ------- |   result  | short memo for this tranfer.
routing_packet					| ------- |   result  | routing info of next hop.
rsmc_tx_hex					| ------- |   result  | rsmc transaction hex


## HTLCSigned

### Simple Type -100041 Protocol

Type -100041 Protocol is used to response an incoming HTLC.

<!-- right -->
> current stage before signing a new incoming HTLC:

```shell
bob's lastest temp  rsmc 
{
        "address":"mo9sVgTYnrx1rdxxj3XYswAcUwnQKmGKoN",
        "index":7,
        "pub_key":"03b2e7ecc5ff62feb342943a1364f555e8302f507f78c6392c82b9e12c95ccb40b",
        "wif":"cPnHVbg3ZZoXhvgu5pWSx18zf9Dw5jfp7wMaDEx4JzHd9zqh1etk"
}

bob's rsmc
{
        "address":"myz18rSHrtZR4DSHGy1WUPTK1QCrsxJJow",
        "index":25,
        "pub_key":"0360917f53381f2b05bb3ec299b6bf7e7446a5c6ed287d65cfc6e38858c3800172",
        "wif":"cTWtQC6WWosXNaqFbnotZdqHi5Y74vEARaqGoqPHX3285QKdz1wD"
    }

bob's htlc
{
        "address":"n39P3nHJbS4BMm8tuMwoFmrfgMBQzZG3TJ",
        "index":26,
        "pub_key":"0218da225a18821ec4353cd4d4c10735e5ce56552166c072e43d977bb50da30f18",
        "wif":"cUYShY9qJdX3S1d74dK9Q3QYAypKPMVG9gYvB3iZe3J9GyLCG9G6"
}
```


### Websocket Request: Message Type -100041

> Request:

```json
{
	"type":-100041,
	"recipient_node_peer_id":"Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"payer_commitment_tx_hash":"4a5854c6cc0d1930500e05353aff416b8cdd1859dc2c2af44b46089e23938023",
        "c3a_complete_signed_rsmc_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......",
        "c3a_complete_signed_counterparty_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......",
        "c3a_complete_signed_htlc_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......",
        "channel_address_private_key":"cTWBhAwXyDtM5XxBwibUxMzH5R2na7WHCTXcnL2xq3y25S4mpAMd",
		"curr_rsmc_temp_address_pub_key":"0360917f53381f2b05bb3ec299b6bf7e7446a5c6ed287d65cfc6e38858c3800172",
		"curr_rsmc_temp_address_private_key":"cTWtQC6WWosXNaqFbnotZdqHi5Y74vEARaqGoqPHX3285QKdz1wD",
		"curr_htlc_temp_address_pub_key":"0218da225a18821ec4353cd4d4c10735e5ce56552166c072e43d977bb50da30f18",
		"curr_htlc_temp_address_private_key":"cUYShY9qJdX3S1d74dK9Q3QYAypKPMVG9gYvB3iZe3J9GyLCG9G6",
		"last_temp_address_private_key":"cPnHVbg3ZZoXhvgu5pWSx18zf9Dw5jfp7wMaDEx4JzHd9zqh1etk"
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
payer_commitment_tx_hash		| ------- |   data    | payer's commitment tx hash.
c3a_complete_signed_rsmc_hex  | ------- |   data    | the hex signed at client side. 
c3a_complete_signed_counterparty_hex  | ------- |   data    | the hex signed at client side. 
c3a_complete_signed_htlc_hex  | ------- |   data    | the hex signed at client side. 
channel_address_private_key		| ------- |   data    | payee's private key of the channel address.
curr_rsmc_temp_address_pub_key		| ------- |   data    | payee's pub key of current rsmc temp address.
curr_rsmc_temp_address_private_key	| ------- |   data    | payee's private key of current rsmc temp address.
curr_htlc_temp_address_pub_key		| ------- |   data    | payee's pub key of current htlc temp address.
curr_htlc_temp_address_private_key	| ------- |   data    | payee's private key of current htlc temp address.
last_temp_address_private_key		| ------- |   data    | private key of payee's last temp address.

### Websocket Response: Message Type -41

> OBD Responses:

```json
{
	"type":-110041,
	"status":true,
	"from":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"result":{
        "amount_to_counterparty":3,
        "amount_to_htlc":4,
        "amount_to_rsmc":13,
        "begin_block_height":1764129,
        "channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
        "create_at":"2020-06-09T16:17:12.2961533+08:00",
        "create_by":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
        "curr_hash":"4a5854c6cc0d1930500e05353aff416b8cdd1859dc2c2af44b46089e23938023",
        "curr_state":11,  		"from_counterparty_side_for_me_tx_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a900000000d900473044022027a4d0d62644f35397259b935fb1b5a12e7fb5ba4d2ecb95c6594462b03f0bf1022055a667b71720c1d69f0cc9276e73abbe037aaf4d273c4182d727326e87093fbd0147304402204b82eae5542ed8359463cef4fbb195b5c0b0f5e7f48fa359342d041ae163254702207ce784d75958d20137b0990cd112d2a4156c2fe34a5b92f7db31262de64e01000147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a9143990d4cce9619dcaf5bb619fb19f36d0f43c38ac870000000000000000166a146f6d6e6900000000000000890000000011e1a3001c0200000000000017a9143990d4cce9619dcaf5bb619fb19f36d0f43c38ac8700000000",
        "htlc_cltv_expiry":1,
        "htlc_h":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933",
        "htlc_multi_address":"2N4SDDibGaVVxggNTs9wufh22qQH39Tj3gq",
        "htlc_multi_address_script_pub_key":"a9147abd515aab0f074a6ae0b993e75f10107dd382bb87",
        "htlc_r":"",  		"htlc_redeem_script":"522103f40b44a99393408d705806b09531c47d042ab332e7cd59c39e2aa67df0bc926c21028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52ae",
        "htlc_routing_packet":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
        "htlc_sender":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
        "htlc_temp_address_pub_key":"03f40b44a99393408d705806b09531c47d042ab332e7cd59c39e2aa67df0bc926c",  		"htlc_tx_hex":"0200000001e062124a8f5476cd4cff020894525d360f6d0ab4a8d2b357a90f328c3dabe2d900000000d90047304402204709f197d1def131fdee07957ff110fca149a33db87ec8e59032000034668c99022009e0dfe7af7c0b065c1770e94f7733684b9df1f000ad85d105824238b0e8d2320147304402204875292df9ca67b03c5f1337574a4eb65814077097779ed6423ad64e2cc4722c02204936d4b6b923e004fcea4d4746b29a8e409d2b89d7232c938cbfe814d35c378d0147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a9147abd515aab0f074a6ae0b993e75f10107dd382bb870000000000000000166a146f6d6e6900000000000000890000000017d784001c0200000000000017a9147abd515aab0f074a6ae0b993e75f10107dd382bb8700000000",
        "htlc_txid":"3f3a44ce9e42b6e38b9dc6aad770ab4e453211427d6e2c1cb169898fc7d0e871",
        "id":30,
        "input_amount":20,
        "input_txid":"ebcdb6658a38cd5e072646fc51d8061571b5df59a66c4898a3cf4b86b19650ea",
        "input_vout":2,
        "last_commitment_tx_id":29,
        "last_edit_time":"2020-06-09T16:17:12.2961533+08:00",
        "last_hash":"1ea1bf809c292a594d40fff5a701b7544cf24478522d77af7fe0d045b7ba89e0",
        "owner":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
        "peer_id_a":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
        "peer_id_b":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
        "property_id":137,
        "rsmc_input_txid":"",
        "rsmc_multi_address":"2MxuampceMui3WgELn4SoXxohNvdyvdQebn",
        "rsmc_multi_address_script_pub_key":"a9143e19c938d5330b5d5a244573c778acfaf7e674c387",     		"rsmc_redeem_script":"52210253a778da18945a91fccfa40ad09ac4d276991ab70ad6e5b1009b68434d57b8b321028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52ae",
        "rsmc_temp_address_pub_key":"0253a778da18945a91fccfa40ad09ac4d276991ab70ad6e5b1009b68434d57b8b3",        		"rsmc_tx_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a900000000d90047304402205868611dc0c9437adc3fc6ef9e2b14b1d8449f66c8d20758a58287733118324f0220454da7c12c60ae0639af6fd0d4f87cf8608318bd00ecd75e455efbad8d479b0801473044022071b1ab8ce14e612a595741cb02bba57d3ab2c114c8fc0fe9eb2be6baeae11d54022003e9979b46f45a6629f50028563cb59b319a28569afb462bf5350ab7ffa941f40147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a9143e19c938d5330b5d5a244573c778acfaf7e674c3870000000000000000166a146f6d6e690000000000000089000000004d7c6d001c0200000000000017a9143e19c938d5330b5d5a244573c778acfaf7e674c38700000000",
        "rsmc_txid":"2904a9967cbbb5a43a0376f50342e75555d1a71986bbc6cff26142f2309a12a1",
        "send_at":"0001-01-01T00:00:00Z",
        "sign_at":"2020-06-09T16:20:15.1304179+08:00", 		"to_counterparty_tx_hex":"0200000002ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cdeb02000000d900473044022042a2b6efa34c52409cdbd4971526824830e6fb1d49a7747634f9ab5ece6f28ad02206f40b96b2f13144eee9e2d686eaa7e880e8da29999c5a840662b04f221e537ad0147304402204151fdb70c4ad5b4732268d9ab0f9c6fbb1539322224ffa277f7f1f3e4573a1e022034cee39ccacbaffaa2d85e1361290a3ab9a281b51f9fc70e098c230d0e3f58140147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aefffffffffcabe9c10966c7640e41fcbc1e597991d4da4edc2f6e257c6159c1d7c34a8b8200000000d90047304402200f7c74406f0c6553fd141caf19305c42d9dcdf487dda56e30144a02325a76ebd02205eba8119193b05fa1829c4d3117c9582987347e7a2e5cdccd7cd151a2a8e51280147304402201b0034af1fdb7c4134eb32a01bc605e6010101eb39cb4dc53e7f4c8f1dfaaa7302201be53089f8fae856404e31a73e60acf744d05eef188bf0e00f1180b83a0761380147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03d7470000000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac0000000000000000166a146f6d6e6900000000000000890000000011e1a30022020000000000001976a914ceb05f96f5b3cbd12d7bff5f9f45252b3f67bf7088ac00000000",
        "to_counterparty_txid":"4fa372d11bb2cbb5637e67fe2e68b9b21265ac04456a921d9aa599ff12dd72a1",
        "tx_type":1
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
approval       | ------- |   result  | true or false
amount_to_counterparty		| ------- |   result  | portion of this payment to the counterparty.
amount_to_htlc   		| ------- |   result  | portion of this payment to the HTLC temporary address.
amount_to_rsmc			| ------- |   result  | portion of this payment to the RSMC temporary address.
begin_block_height		| ------- |   result  | current block height using in timing.

A HTLC has three output: to counterparty, to htlc, to rsmc. The three fields are filled by payee's obd when constructing HTLC transactions on payee's obd. The remaining fields are for notifying purpose when obd module developers debug. For wallet developers, these fields shall be ignored.  


## forwardR

### Simple Type -100045 Protocol

Type -100045 Protocol is used to forward R to a user.

<aside class="succeed">
<code>Carol</code> (destination) Send R (Preimage_R) to <code>Bob</code> (middleman).
</aside>

Bob asks Carol if Carol can present an unknown 20-byte random R subject to Hash(R) = H, within two days for example, then Bob will settle the contract by paying assets. This message is from Bob's client to Bob's OBD.  
 
### Websocket Request: Message Type -100045

> Request:

```json
{
	"type":-100045,
	"recipient_node_peer_id":"Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
		"r":"cUAa45B7TWp6FbK6N15WLq5pXDCKUJj98RSXm4QTV5WsQNeF6tmn",
		"channel_address_private_key":"cTWBhAwXyDtM5XxBwibUxMzH5R2na7WHCTXcnL2xq3y25S4mpAMd"
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id   | ------- |   data    | the channel id.
r            | ------- |   data    | Preimage_R
channel_address_private_key  | ------- |   data    | bob's private key of channel address. 

### Websocket Response: Message Type -110045

> OBD Responses:

```json
{
	"type":-110045,
	"status":true,
    	"from":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"result":{
        	"channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
        	"he1b_temp_pub_key":"02d93e3a855ec5603d74c6f379da184c5fcc05372ca48b4791e1afa1d6587db715",
		"he1b_tx_hex":"02000xxxxxxxxxxxxxxxxxbf97538700000000",
		"herd1b_tx_hex":"6f6d6xxxxxxxxxxxxxxx001976a914ceb05f",
        	"msg_hash":"5f28fc568453f3c5625a5dc4e918a3d5b3aa75f7a447ee44166880698d2dc61e",
        	"r":"cUAa45B7TWp6FbK6N15WLq5pXDCKUJj98RSXm4QTV5WsQNeF6tmn"
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
id             | ------- |   result  | 
channel_id		| ------- |   result  | 
he1b_temp_pub_key  	| ------- |   result  | temp pub key of HE1B.
he1b_tx_hex  		| ------- |   result  | transaction hex of HE1B.
herd1b_tx_hex  		| ------- |   result  | transaction hex of HERD1B. 
msg_hash  		| ------- |   result  | 
r			| ------- |   result  | the preimage.


## signR

### Simple Type -100046 Protocol

Type -100046 Protocol is used to accept R and notify the sender.

### Websocket Request: Message Type -100046

> Request:

```json
{
	"type":-100046,
	"recipient_node_peer_id":"QmVQZ6byysojM1mYyxNrDXy8gG96BbH8ZDW9pnuHFGWazn",
	"recipient_user_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
	"data":{
        "channel_id":"80d34c072c35b83a70f82fcb791025b4756db6f8aad37d6f50fbde763168abf5",
        "c3b_htlc_herd_complete_signed_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......",
        "c3b_htlc_hebr_partial_signed_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......",
		"channel_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf"
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id                         | ------- |   data    | hash of this HTLC request 
c3b_htlc_herd_complete_signed_hex  | ------- |   data    | the hex signed at client side. 
c3b_htlc_hebr_partial_signed_hex   | ------- |   data    | the hex signed at client side. 
channel_address_private_key        | ------- |   data    | bob's private key of channel address. 
 

### Websocket Response: Message Type -110046

> OBD Responses:

```json
{
	"type":-110046,
	"status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVQZ6byysojM1mYyxNrDXy8gG96BbH8ZDW9pnuHFGWazn",
	"to":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/QmVQZ6byysojM1mYyxNrDXy8gG96BbH8ZDW9pnuHFGWazn",
	"result":{
		"commitmentTxInfo":{
			"amount_to_counterparty":13,
			"amount_to_htlc":4,
			"amount_to_rsmc":3,
			"begin_block_height":1764129,
			"channel_id":"80d34c072c35b83a70f82fcb791025b4756db6f8aad37d6f50fbde763168abf5",
			"create_at":"2020-06-09T16:19:57.4116087+08:00",
			"create_by":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
			"curr_hash":"191947428ead76964da30881524696a8b3ebbaee2bc365c8cd29467a54e011ae",
			"curr_state":12,
			"from_counterparty_side_for_me_tx_hex":"0200000 xxxxxxxxx 7088ac00000000",
			"htlc_cltv_expiry":1,
			"htlc_h":"035be1bc8f26ac7318d83663bd5dab10c843a74d11e573731a6a9abee5b9d46933",
			"htlc_multi_address":"2NAS4M4CCzew1BFnq59nuDJBNGoigo1oAyV",
			"htlc_multi_address_script_pub_key":"a914bc86aa4fe56c2efda016bb1b0fa928372b8c51ab87",
			"htlc_r":"cUAa45B7TWp6FbK6N15WLq5pXDCKUJj98RSXm4QTV5WsQNeF6tmn",
            "htlc_redeem_script":"52210218da225a18   xxxxxxx   95bb6391ce5d6d4fc6652ae",
            "htlc_routing_packet":"80d34c072c35b83a70f82fcb791025b4756db6f8aad37d6f50fbde763168abf5",
            "htlc_sender":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
            "htlc_temp_address_pub_key":"0218da225a18821ec4353cd4d4c10735e5ce56552166c072e43d977bb50da30f18",
            "htlc_tx_hex":"0200000 xxxxx c51ab8700000000",
            "htlc_txid":"14b1caad63717b4a2b8f7bf51964f33c707b4f2adbb145aad21777ac5da64516",
            "id":19,
            "input_amount":20,
            "input_txid":"ebcdb6658a38cd5e072646fc51d8061571b5df59a66c4898a3cf4b86b19650ea",
            "input_vout":2,
            "last_commitment_tx_id":18,
            "last_edit_time":"2020-06-09T16:19:57.4116087+08:00",
            "last_hash":"6ec57103f76f17e0c3b7d3d21ae2e35682be87a52d324bff58934c3a7c22717c",
            "owner":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
            "peer_id_a":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
            "peer_id_b":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
            "property_id":137,
            "rsmc_input_txid":"",
            "rsmc_multi_address":"2MxVc1GcmtyHgn6s2gzHD8Vjv94YV4bkwqd",
            "rsmc_multi_address_script_pub_key":"a9143990d4cce9619dcaf5bb619fb19f36d0f43c38ac87",
            "rsmc_redeem_script":"522103609 xxxxx 95bb6391ce5d6d4fc6652ae",
            "rsmc_temp_address_pub_key":"0360917f53381f2b05bb3ec299b6bf7e7446a5c6ed287d65cfc6e38858c3800172",
            "rsmc_tx_hex":"0200000001c33 xxxxxx c8700000000",
            "rsmc_txid":"058f36e8fd80e970f299c84b843de16fe909d4dfdf046a2d200ac2f3f3a13fc0",
            "send_at":"0001-01-01T00:00:00Z",
            "sign_at":"0001-01-01T00:00:00Z",
            "to_counterparty_tx_hex":"0200000 xxxxxxxx 8ac00000000",
            "to_counterparty_txid":"66f327f8df014c641839081397211561def01757ca19392b44dff8bf1f434027",
            "tx_type":1
        }
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
id             | ------- |   result  | 
commitmentTxInfo		| ------- |   result  | returns the params in this commitment transaction.
 
 

## closeHTLC

### Simple Type -100049 Protocol

Type -100049 message is used to close a HTLC.
 

### Websocket Request: Message Type -100049

> Request:

```json
{
	"type":-100049,
	"recipient_node_peer_id":"Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"recipient_user_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
	"data":{
		"channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
		"channel_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf",
		"last_rsmc_temp_address_private_key":"cNwgTtsRaWdDWdNVA4aPEoumS9prpYirE3Q1BuARrphjH9THVeH2",
		"last_htlc_temp_address_private_key":"cRVqzokvBvtzw91Nv4zkvmad7uRBf5eMfr5t5EDL7q2U6VxBTmoy",
		"last_htlc_temp_address_for_htnx_private_key":"cUxBiu8jVgk9gDhdgpF3S4wFrpFBkAFujCEVgCRYBsQaTDtUdVMb",
		"curr_rsmc_temp_address_pub_key":"02fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed8",
		"curr_rsmc_temp_address_private_key":"cTiDwaM3y5LE2HuWWgvRTC9mgHiovf2zntjSgCPyLLeuUTmKk1BY"
    }
}

```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id   | ------- |   data    | Id of a channel
channel_address_private_key              | ------- |   data    | Alice's private key of the channel address.
last_rsmc_temp_address_private_key       | ------- |   data    | Alice's private key of last rsmc temp address.
last_htlc_temp_address_private_key		| ------- |   data    | Alice's private key of last htlc temp address.
last_htlc_temp_address_for_htnx_private_key	| ------- |   data    | Alice's private key of last htlc temp address for htnx.
curr_rsmc_temp_address_pub_key			| ------- |   data    | Alice's pub key of current rsmc temp address. 
curr_rsmc_temp_address_private_key		| ------- |   data    | Alice's private key of curr rsmc temp address.	


### Websocket Response: Message Type -100049

> OBD Responses:

```json
{
    "type":-100049,
    "status":true,
    "from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/3001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
    "to":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/3001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
    "result":{
        "channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
        "commitment_tx_hash":"7a9f1db251531e6846812638cb7d7b2d1e9a25bab93a0c6d8f2ecb404754fdab",
        "curr_rsmc_temp_address_pub_key":"02fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed8",
        "last_htlc_temp_address_for_htnx_private_key":"cUxBiu8jVgk9gDhdgpF3S4wFrpFBkAFujCEVgCRYBsQaTDtUdVMb",
        "last_htlc_temp_address_private_key":"cRVqzokvBvtzw91Nv4zkvmad7uRBf5eMfr5t5EDL7q2U6VxBTmoy",
        "last_rsmc_temp_address_private_key":"cNwgTtsRaWdDWdNVA4aPEoumS9prpYirE3Q1BuARrphjH9THVeH2",
        "msg_hash":"ed80ad44aeeca449e6f686d3adbf6c231fa9667c9e560d20d874df8b92ec02d6",
	"rsmc_hex":"020xxxxxxxxxxxxxxxxxd7dbe2d391d22d19014e6cb4a8b6c33251d28700000000",
	"to_counterparty_tx_hex":"02000xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxc988ac00000000"
    }
}

```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
channel_id              			| ------- |   result  | channel id.
commitment_tx_hash            			| ------- |   result  | commitment tx hash.
curr_rsmc_temp_address_pub_key            	| ------- |   result  | curr rsmc temp address pub key. 
last_htlc_temp_address_for_htnx_private_key 	| ------- |   result  | 
last_htlc_temp_address_private_key		| ------- |   result  | 
last_rsmc_temp_address_private_key		| ------- |   result  | 
msg_hash					| ------- |   result  | 
rsmc_hex					| ------- |   result  | 
to_counterparty_tx_hex				| ------- |   result  | 


## closeHTLCSigned

### Simple Type -100050 Protocol

Type -49 Protocol is used to response the request of close HTLC .

 
### Websocket Request: Message Type -100050

> Request:

```json
{
	"type":-100050,
	"recipient_node_peer_id":"Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"msg_hash":"45620c2a09d610e7eaa85d4f7fe48d38cf9a5b801e0196259ff89da41bba6a47",
		"channel_address_private_key":"cTWBhAwXyDtM5XxBwibUxMzH5R2na7WHCTXcnL2xq3y25S4mpAMd",
		"last_rsmc_temp_address_private_key":"cTWtQC6WWosXNaqFbnotZdqHi5Y74vEARaqGoqPHX3285QKdz1wD",
		"last_htlc_temp_address_private_key":"cUYShY9qJdX3S1d74dK9Q3QYAypKPMVG9gYvB3iZe3J9GyLCG9G6",
		"last_htlc_temp_address_for_htnx_private_key":"cVpq1dvQdh16iyR1j1FbCDCMxXD27GXMRU7mna3Pe6yPvSVrmgUG",
		"curr_rsmc_temp_address_pub_key":"0298bdca47bbb76b1022eb7d18534961a12ce6dd80308c839576602b771e324fba",
		"curr_rsmc_temp_address_private_key":"cU78aif2a4YR5xK8HxBTrPKjdjhD8W4SSZNTw4yFEdwi59JMrYQY"
    	}
}

```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
msg_hash   | ------- |   data    | hash of this closing request
channel_address_private_key		| ------- |   data    | Bob's private key of the channel address.
last_rsmc_temp_address_private_key	| ------- |   data    | Bob's private key of previous rsmc temp address.
last_htlc_temp_address_private_key	| ------- |   data    | Bob's private key of previous htlc temp address.
last_htlc_temp_address_for_htnx_private_key      | ------- |   data  | Bob's private key of previous htlc temp address for htnx.
curr_rsmc_temp_address_pub_key      | ------- |   data    | Bob's pub key of curr rsmc temp address.
curr_rsmc_temp_address_private_key  | ------- |   data    | Bob's private key of curr rsmc temp address.

### Websocket Response: Message Type -110050

> OBD Responses:

```json
{
	"type":-110050,
	"status":true,
	"from":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/Qmd3a2GWMwJuC98x1sthzvqXXhaVSz5rFzJYCekBU5ihPP",
	"result":{
		"latestCommitmentTxInfo":{
		    "amount_to_counterparty":7,
		    "amount_to_htlc":0,
		    "amount_to_rsmc":13,
		    "begin_block_height":0,
		    "channel_id":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
		    "create_at":"2020-06-09T16:24:09.9204628+08:00",
		    "create_by":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
		    "curr_hash":"f08463542b83b17da8328624dc06443184bac742937f40ddbbaeccbd059561df",
		    "curr_state":10,
		    "from_counterparty_side_for_me_tx_hex":"",
		    "htlc_cltv_expiry":0,
		    "htlc_h":"",
		    "htlc_multi_address":"",
		    "htlc_multi_address_script_pub_key":"",
		    "htlc_r":"",
		    "htlc_redeem_script":"",
		    "htlc_routing_packet":"",
		    "htlc_sender":"",
		    "htlc_temp_address_pub_key":"",
		    "htlc_tx_hex":"",
		    "htlc_txid":"",
		    "id":31,
		    "input_amount":20,
		    "input_txid":"ebcdb6658a38cd5e072646fc51d8061571b5df59a66c4898a3cf4b86b19650ea",
		    "input_vout":2,
		    "last_commitment_tx_id":30,
		    "last_edit_time":"2020-06-09T16:24:09.9204628+08:00",
		    "last_hash":"4a5854c6cc0d1930500e05353aff416b8cdd1859dc2c2af44b46089e23938023",
		    "owner":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
		    "peer_id_a":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
		    "peer_id_b":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
		    "property_id":137,
		    "rsmc_input_txid":"a9e2e14939f5edadabd8675be6d160c73762e9d562b35594328150db968e32c3",
		    "rsmc_multi_address":"2N1CTdYvAfopS1qWQLGjqwTTqmKyWrSfuVY",
		    "rsmc_multi_address_script_pub_key":"a914573b2587a9f4672b6a2964c234d9b9d34d188dba87", 			 				"rsmc_redeem_script":"522102fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed821028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52ae",
			"rsmc_temp_address_pub_key":"02fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed8",  				"rsmc_tx_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a900000000d900473044022037fa3ac0917d3bc965ede51cb330f0baf5cfe4da91e54988a521205378e66e4c022034d5c8cc347d617dd54f0f66fd4a2b47fb1aa8d417f575bbb45d5fa20add0c080147304402207c0f867a8fa3f2a02d0c09fe37af564170cda0090650974ca0eac993ff84ba1902206910dea102e9fcec49bd6453fb33579457e80f66cbaaaec46c7a4bb50613f0440147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a914573b2587a9f4672b6a2964c234d9b9d34d188dba870000000000000000166a146f6d6e690000000000000089000000004d7c6d001c0200000000000017a914573b2587a9f4672b6a2964c234d9b9d34d188dba8700000000",
            		"rsmc_txid":"fa7751984b5991d025c07a59045e437477e6e2904e3141e0d2fd144210822c9f",
           		"send_at":"0001-01-01T00:00:00Z",
            		"sign_at":"2020-06-09T16:26:01.9381622+08:00",  			"to_counterparty_tx_hex":"0200000003e062124a8f5476cd4cff020894525d360f6d0ab4a8d2b357a90f328c3dabe2d900000000d90047304402207449af13fff3ea7cd979e348722859b1c49a59b163637276c83e2f625df4aece0220290e7b3e0208b1d0f896f554db33f097b5dac30c6fe825bfd4cc02789ea72e640147304402205121ded0913dc12e87cd1df3b1974f1bf3f14bd0c0d43bd9b05d74d67d143b9202207315984b7bdc16b623e7bd540c6d16f1c1a2412fd3d26cbe8d65b661caf13c100147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffffea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cdeb02000000d90047304402200aca4baf1229076ba939196e709be7eeb7efecba53ea935b3927dc2f238aa75c02205dac78ef8955dcd705e2dae5e3ae0947ab7196255b473e2fdc6ed268668ceb290147304402205c8fa21796390bbe4d3bb242f1b4b1bc660390824105d31dee1a685be1e94b69022035da7fba16c599735b0ae29d21ef8292b846e1800ef606c3e3f57fac5a3501d80147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aefffffffffcabe9c10966c7640e41fcbc1e597991d4da4edc2f6e257c6159c1d7c34a8b8200000000d900473044022045c297ed55c1909324b5c9ab3b4d4c8b7666c74eaada24c586324814b5e854bf02205b96cb0f81bbae680d228d5c023c20ed14d1a6eeb1aed98da4ef1330afecab40014730440220110a42b75af5e395d61aaf777ea8acf4eecf734f958f3cd9465d48e2a504c199022077fbd0618f7ba25b6d8de0a1a783d300f5621eba44700ed03ecb33c60499d2540147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03ea930000000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac0000000000000000166a146f6d6e6900000000000000890000000029b9270022020000000000001976a914ceb05f96f5b3cbd12d7bff5f9f45252b3f67bf7088ac00000000",
            		"to_counterparty_txid":"1a3960c81384be425d7364a6f10c416c4f49d15497b8012e63731593b2cf0a3d",
            		"tx_type":0
        	}
    	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
latestCommitmentTxInfo            | ------- |   result  | the latest Commitment transaction infomation, for debugging purpose only.


## closeChannel

### Simple Type -100038 Protocol

Type -100038 Protocol is used to close a channel.

### Websocket Request: Message Type -100038

> Request:

```json
{
    "type": -100038,
    "recipient_node_peer_id":"QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef"
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id   | ------- |   data    | 


### Websocket Response: Message Type -110038

> OBD Responses:

```json
{
    "type": -110038,
    "status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"to be updated": to be updated according to latest obd release. 
	}
}

```


## closeChannelSigned

### Simple Type -100039 Protocol

Type -100039 Protocol is used to response the request of closing a channel.

### Websocket Request: Message Type -100039

> Request:

```json
{
    "type":-100039,
    "recipient_node_peer_id":"QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef",		"request_close_channel_hash":"4453e70ba9f2a805433b3696e43fd4175cabf45daf36aa59368ecf210f5773cb",
		"approval":true
	}
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id  | ------- |   data    | 
request_close_channel_hash  | ------- |   data    | hash of channel
approval    | ------- |   data    | true or false


### Websocket Response: Message Type -100039

> OBD Responses:

```json
{
    "type": -100039,
    "status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"to be updated": to be updated according to latest obd release. 
	}
}

```
