## fundingBitcoin

### Simple Type -102109 Protocol

Type -102109 Protocol is used for depositing bitcoin into a channel. Since the basic Omnilayer protocal uses BTC as miner fee in constructing transactions, this message -102109 is mandatory for depositing a little BTC into a channel as miner fee.  


### Websocket Request: Message Type -102109

> Request:

```json
{
	"type":-102109,
	"data":{
		"from_address":"mre4gBmjKiBm8gwZmpCNcnnHiDY7TXr2wD",
		"from_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf",
		"to_address":"2MvPieQLzkS2mhexzYExsSkXGTExJJzzANw",
		"amount":0.0002,
		"miner_fee":0.00001,
	}
}
```
 

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
from_address | ------- |   data    | address of funder, from where the BTC is transferred.
from_address_private_key  	| ------- |   data    | private key.
to_address           		| ------- |   data    | the channel multi-sig address.
amount			   	| 0.0001  |   data    | amount of BTC.
miner_fee		   	| 0.00001 |   data    | miner fee in BTC for each time transaction.
 
### Websocket Response: Message Type -102109

> OBD Responses:

```json
{
	"type":-102109,
	"status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"result":
	{
		"hex":"02000000019c35dde24b4b3ca57b51c20e4093ce0f412e9cec18b767acf92e0544a11dd363000000006a473044022047985ce08d3dcdbd80c4c9f87b919660a94a2680b192ea90e70f8cb3c36b1b7b0220519bc547641574ade1275c864db58bd97682296649222bc1dccb11298721a40f012102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc66ffffffff02204e00000000000017a9142283a07c4fc44706e540c02edf1a1f3a4c433bfc872eb90a00000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac00000000",
		"txid":"a9e2e14939f5edadabd8675be6d160c73762e9d562b35594328150db968e32c3"
	}
}
```
  

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false.
from	       | ------- |   body    | peer id of funder.
to             | ------- |   body    |  
hex            | ------- |   result  | transaction hex.
txid	       | ------- |   result  | transaction id.


## bitcoinFundingCreated

### Simple Type -100340 Protocol

Type -100340 Protocol is used to notify the success of funding BTC to the counterpart of the channel.

<aside class="notice">
<code>Alice</code> tells her OBD to notify <code>Bob</code> that she created the funding transaction by payloads packed in this message -100340. 
</aside>

### Websocket Request: Message Type -100340

> Request:

```json
{
	"type":-100340,
	"recipient_node_peer_id":"QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"recipient_user_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
	"data":{
		"temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef",
		"channel_address_private_key":"cVV22tLgBbLv1K1uW6z2doR4Copat1mejjND1jtW8CVkRLUSpPxf",  			"funding_tx_hex":"0200000001f2ad47c834d41507642d8007ed3e43d408ad29ad2bf1d563d98ff7694db40ef7000000006a473044022051bde2fea20a92f7ce71a99cfda2055bb5e0fc47af3e9ed429c70b6d00e5be2302201dbab200fcfa15431cb541bfe1cd6055c95f12825f29d211d412d4171378cf30012102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc66ffffffff02f82a00000000000017a914f222f1986a60a01e33f6f813a13c7507492f4ac28732bf0a00000000001976a91484902564ba3ce47952d86a0d53c17402b3cce96588ac00000000"
	}
}
```  
 

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | temporary channel id.
channel_address_private_key  | ------- |   data    | private key of the channel that Alice holds.
funding_tx_hex               | ------- |   data    | funding transaction hex.


### Websocket Response: Message Type -110340

> OBD Responses：

```json
{
	"type":-110340,
	"status":true,
	"from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"to":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"result":{
        "funding_btc_hex":"02000000019c35dde24b4b3ca57b51c20e4093ce0f412e9cec18b767acf92e0544a11dd363000000006a473044022047985ce08d3dcdbd80c4c9f87b919660a94a2680b192ea90e70f8cb3c36b1b7b0220519bc547641574ade1275c864db58bd97682296649222bc1dccb11298721a40f012102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc66ffffffff02204e00000000000017a9142283a07c4fc44706e540c02edf1a1f3a4c433bfc872eb90a00000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac00000000",
        "funding_redeem_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a900000000920047304402206cc5fa2e47a9d7772eb114ba2223e62481ac76a9f34c98349d0b306490280e3c02207cd41cfd3aa98758b76d08f867424e6e17f95925f7fc4c5412996203243fe059010047522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff0150490000000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac00000000",
        "funding_txid":"a9e2e14939f5edadabd8675be6d160c73762e9d562b35594328150db968e32c3",
        "temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef"
    }
}
```

 

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false.
from	       | ------- |   body    | peer id of funder.
to             | ------- |   body    | peer id of fundee.
funding_btc_hex  | ------- |   result  | btc funding transaction hex.
funding_redeem_hex | ------- |   result  | 
funding_txid   | ------- |   result  | funding transaction id.
temporary_channel_id  | ------- |   result  | temporary channel id. 


## bitcoinFundingSigned

### Simple Type -100350 Protocol
 
 
<aside class="success">
<code>Bob</code> tells his OBD to reply Alice that he knows the BTC funding by message -100350.
</aside>


### Websocket Request: Message Type -100350

> Request:
 
 
```json
{
	"type":-100350,
	"recipient_node_peer_id":"QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
	"recipient_user_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
	"data":{
		"temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef",
		"channel_address_private_key":"cTWBhAwXyDtM5XxBwibUxMzH5R2na7WHCTXcnL2xq3y25S4mpAMd",
		"funding_txid":"603bfad3a62e7bf5480bc851e763c53adb276b16b02edc9451be6dd23cc07ba4",
		"signed_miner_redeem_transaction_hex":"c851e763c53adb27dc9451be6dd23cc07ba4......",
		"approval":true
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id         | ------- |   data    | temporary channel id. 
channel_address_private_key  | ------- |   data    | private key of the channel that Bob holds.
funding_txid                 | ------- |   data    | funding transaction id. 
signed_miner_redeem_transaction_hex  | ------- |   data    | the hex signed at client side. 
approval                     | ------- |   data    | true or false.

 
### Websocket Response: Message Type -110350

> OBD Responses：

 
```json
{
    	"type":-110350,
    	"status":true,
    	"from":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
    	"to":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2@/ip4/127.0.0.1/tcp/4001/p2p/QmVEoTmyofsbEnsoFwQXHngafECHJuVfEgGyb2bZtyiont",
    	"result":{
        	"approval":true,  			"funding_redeem_hex":"0200000001c3328e96db5081329455b362d5e96237c760d1e65b67d8abadedf53949e1e2a900000000d90047304402206cc5fa2e47a9d7772eb114ba2223e62481ac76a9f34c98349d0b306490280e3c02207cd41cfd3aa98758b76d08f867424e6e17f95925f7fc4c5412996203243fe05901473044022069466e582dcd465dfeb4407f1d65138cbb7fb16d8aed4309a6ac99049aeb0f9402201149356dd472e1f032fc574c5d6e138661c0da830a50b48fd6b6b24b9b6a7bc70147522102c57b02d24356e1d31d34d2e3a09f7d68a4bdec6c0556595bb6391ce5d6d4fc6621028f49447e1a20d4211c8c621976cff3d2ae00d4af79be3e74e05f1ed4e23f319e52aeffffffff0150490000000000001976a9147a019f584f6a65d114d5f17264c9eb32f763d72c88ac00000000",
        	"funding_txid":"a9e2e14939f5edadabd8675be6d160c73762e9d562b35594328150db968e32c3",
        	"temporary_channel_id":"38e41ef5ba61c11642b2fa3ea93e8026ab7b057b06b64215f255669acf8dc0ef"
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false.
from	       | ------- |   body    | peer id of fundee.
to             | ------- |   body    | peer id of funder. 
funding_redeem_hex    | ------- |   result  | funding redeemscript hex.
funding_txid	      | ------- |   result  | funding tx id.
temporary_channel_id  | ------- |   result  | temporary channel id
 
