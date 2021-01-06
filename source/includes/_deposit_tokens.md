## fundingAsset

### Simple Type -102120 Protocol

<!-- center -->
Alice starts to deposit omni assets to the channel. This is quite similar to the the btc funding procedure.

<!-- right -->
> Alice's example data:

```shell
address: mre4gBmjKiBm8gwZmpCNcnnHiDY7TXr2wD  
privkey: cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf  
channel address：2MvPieQLzkS2mhexzYExsSkXGTExJJzzANw  
```

### Websocket Request: Message Type -102120


<!-- right -->
> Request:
 
```json
{
    "type":-102120,
    "data":{
        "from_address":"mre4gBmjKiBm8gwZmpCNcnnHiDY7TXr2wD",
	"from_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf",
        "to_address":"2MvPieQLzkS2mhexzYExsSkXGTExJJzzANw",
        "amount":20,
        "property_id":137
    }
}
```


<!-- center -->

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
from_address   | ------- |   data    | address of funder, from where the BTC is transferred.
from_address_private_key  	| ------- |   data    | private key.
to_address           		| ------- |   data    | the channel multi-sig address.
amount			   	| ------- |   data    | 
property_id		  	| ------- |   data    | the omni asset id that has been funding.
 
### Websocket Response:

<!-- right -->
> OBD Responses:

```json 
{
    	"type":-102120,
    	"status":true,
    	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
    	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
    	"result":{
        		"hex":"0200000001e062124a8f5476cd4cff020894525d360f6d0ab4a8d2b357a90f328c3dabe2d9010000006a47304402202616b9a0c38865c5a3cba97cdb228698272d954f315771c1b1fbd9e1310d4d150220589c13f8e2e4fd6b6cfd09e59f62faa899ff898f7b040cc972d5a09d6a448c11012102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc66ffffffff034a070a00000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac0000000000000000166a146f6d6e69000000000000008900000000773594001c0200000000000017a9142283a07c4fc44706e540c02edf1a1f3a4c433bfc8700000000"
    	}
} 
```


<!-- center -->
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false.
from	       | ------- |   body    | peer id of funder.
to             | ------- |   body    | peer id of fundee.
hex            | ------- |   result  | funding transaction hex. 


## assetFundingCreated

### Simple Type -100034 Protocol

Type -100034 Protocol is used to notify the success of omni asset funding transaction 
to the counterparty of the channel.

### Websocket Request: Message Type -100034

<!-- right -->
> Request:
 
```json 

{
    "type":-100034,
    "recipient_node_peer_id":"QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
    "recipient_user_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
    "data":{
        "temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef",  		"funding_tx_hex":"0200000001c0c7ec0dabae15d2a5e3fd8e740952839c588cd68d8eafe00ed5d3c0f01bf229000000006a473044022045405d082bd9826e00819094cc693c0b2e40926b1a30cefc18e171c9c12e60be02206ba7479690f849cca828a6711a484a5149d79299fc9c4e179bffcad5e7bfb773012102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc66ffffffff03f61c0c00000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac0000000000000000166a146f6d6e69000000000000008900000000b2d05e001c0200000000000017a914fcbbb71abf965dfde49fbf7442a4696ce2dd6c5f8700000000",
        "temp_address_pub_key":"03985e8880628058da7c49b0968e4e7d2819240b60255a1c9b5f2407a4056b5f54",
        "temp_address_private_key":"cTLc9tx1Hihqfu6jXXWQ8pFeiQb5aJ8ck88ik8RJuutP95dqTuqo",
        "channel_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf"
    }
}
```

<!-- center -->
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | temporary channel id. 
funding_tx_hex               | ------- |   data    | asset funding transaction hex.
temp_address_pub_key         | ------- |   data    | temporary address pub key.
temp_address_private_key     | ------- |   data    | temporary address private key.
channel_address_private_key  | ------- |   data    | private key of the channel that Alice holds.
 
### Websocket Response: Message Type -110034

> OBD Responses：

```json  
{
	"type":-110034,
	"status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"to":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"result"{  		
		"c1a_rsmc_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a9000000009200473044022048d693d88f34ff98ae1ca34e56f3c88394569caa491ef3d08b7f0a06c0515e1b022020b155a9ca07be5cf854200157781169a1e402018b3dd9418f26651924176c59010047522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff03344700000000000017a9146816548153dd030f92540b24e9605c0c565cc378870000000000000000166a146f6d6e69000000000000008900000000773594001c0200000000000017a9146816548153dd030f92540b24e9605c0c565cc3788700000000",  			"funding_omni_hex":"0200000001e062124a8f5476cd4cff020894525d360f6d0ab4a8d2b357a90f328c3dabe2d9010000006a47304402202616b9a0c38865c5a3cba97cdb228698272d954f315771c1b1fbd9e1310d4d150220589c13f8e2e4fd6b6cfd09e59f62faa899ff898f7b040cc972d5a09d6a448c11012102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc66ffffffff034a070a00000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac0000000000000000166a146f6d6e69000000000000008900000000773594001c0200000000000017a9142283a07c4fc44706e540c02edf1a1f3a4c433bfc8700000000",
		"rsmc_temp_address_pub_key":"03985e8880628058da7c49b0968e4e7d2819240b60255a1c9b5f2407a4056b5f54",
		"temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef"
	}
}
``` 

<!-- center -->
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status           | ------- |   body    | true or false.
from	         | ------- |   body    | peer id of funder.
to               | ------- |   body    | peer id of fundee.    
c1a_rsmc_hex	 | ------- |   result  | hex of the first commitment transaction constructed.
funding_omni_hex | ------- |   result  | omni asset funding transaction hex.
rsmc_temp_address_pub_key	| ------- |   result  | pub key of the temporary address that used to accept tokens RSMC output.
temporary_channel_id		| ------- |   result  | temporary channel id. 

<aside class="notice">
<code>property id</code> and <code>amount</code> are all included in <code>funding_omni_hex</code>. The counterparty can easily validate the funding transaction by checking the hex. 
</aside>


## assetFundingSigned

### Simple Type -100035 Protocol  

<aside class="success">
<code>Bob</code> tells his OBD to reply <code>Alice</code> that he knows the asset funding transaction by message -100035, and Alice's OBD will creat commitment transactions (C1a & RD1a).
</aside>

### Websocket Request: Message Type -100035

> Request: 
 
```json
{
	"type":-100035,
	"recipient_node_peer_id":"QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef",
		"fundee_channel_address_private_key":"cTWBhAwXyDtM5XxBwibUxMzH5R2na7WHCTXcnL2xq3y25S4mpAMd",
		"signed_alice_rsmc_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......"
    }
}
``` 

<!-- center -->
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id    		| ------- |   data    | temporary channel id.
fundee_channel_address_private_key	| ------- |   data    | private key of the channel that Bob holds, used to sign the commitment transactions. 
signed_alice_rsmc_hex  | ------- |   data    | the hex signed at client side. 

### Websocket Response: Message Type -110035

> OBD Responses：

```json
{
	"type":-110035,
	"status":true,
	"from":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"result":{
		"channelId":"ea5096b1864bcfa398486ca659dfb5711506d851fc4626075ecd388a65b6cde9",
		"temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef"
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false.
from	       | ------- |   body    | peer id of fundee.
to             | ------- |   body    | peer id of funder.
channel_id            | ------- |   result  | officially generates the channel ID. 
temporary_channel_id  | ------- |   result  | temporary channel id, won't appear again in following messages.  
