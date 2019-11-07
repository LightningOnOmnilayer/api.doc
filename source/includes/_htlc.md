# HTLC in OBD

Hashed Timelock Contract (HTLC) Testing in OBD.

A bidirectional payment channel only permits secure transfer of funds inside a channel. 
To be able to construct secure transfers using a network of channels across multiple hops 
to the final destination requires an additional construction, a Hashed Timelock Contract (HTLC).

The purpose of an HTLC is to allow for global state across multiple nodes via hashes. 

## Prepare Data for Client

<!-- right -->

> Alice's data:

```shell
adderss: muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit
pubkey:  029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c
privkey: cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8
```

> Bob's data:

```shell
adderss: mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8
pubkey:  03da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f
privkey: cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt
```

> Carol's data:

```shell
adderss: mgoiBkppoJMc8cC8XRYNvFEjath5DrKqj8
pubkey:  034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a
privkey: cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV
```

<!-- center -->

**For testing, we generate three client data of Alice, Bob, Carol**

## Prepare Data for Channel Address

<!-- right -->

> Channel A2B:

```shell
channel_address：2NFhMhDJT9TsnBCG6L2amH3eDXxgwW6EJh7
redeem_script：5221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52ae
channelId: [174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75]
```

> Channel B2C:

```shell
channel_address：2MzQW254vB6mHsUvLHxCnKZ73Gcw7kSrvsd
redeem_script：522103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f21034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a52ae
channel:
[223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51]
```

<!-- center -->

**For testing, we generate two multisig addresses.**

## Login

<!-- right -->

> Alice login:

```json
{
	"type":1,
	"data":{
        "peer_id":"alice"
    }
}
```

> Bob login:

```json
{
	"type":1,
	"data":{
        "peer_id":"bob"
    }
}
```

> Carol login:

```json
{
	"type":1,
	"data":{
        "peer_id":"carol"
    }
}
```

<!-- center -->

Three client login.

**Message Type: 1**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
peer_id   | ------- |   data    | id of a client

## Open Channel between Alice and Bob（A2B）

<!-- right -->

> Alice send the request:

```json
{
	"type":-32,
    "data":{
        "funding_pubkey":"029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c"
    },
    "recipient_peer_id":"bob"
}
```

> OBD Responses:

```json
[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164]
```

> Bob replies:

```json
{
	"type":-33,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"funding_pubkey":"03da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -33, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "accept_at": "2019-11-04T10:59:51.1997943+08:00", 
        "address_a": "muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit", 
        "address_b": "mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8", 
        "chain_hash": "1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P", 
        "channel_address": "2NFhMhDJT9TsnBCG6L2amH3eDXxgwW6EJh7", 
        "channel_address_redeem_script": "5221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52ae", 
        "channel_address_script_pub_key": "a914f64403be27af8af0a8abc21aed584b06f80adf3087", 
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "channel_reserve_satoshis": 0, 
        "close_at": "0001-01-01T00:00:00Z", 
        "create_at": "2019-11-04T10:58:08.2357582+08:00", 
        "create_by": "alice", 
        "curr_state": 20, 
        "delayed_payment_base_point": "", 
        "dust_limit_satoshis": 0, 
        "fee_rate_per_kw": 0, 
        "funding_address": "muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit", 
        "funding_pubkey": "029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c", 
        "funding_satoshis": 0, 
        "htlc_base_point": "", 
        "htlc_minimum_msat": 0, 
        "id": 1, 
        "max_accepted_htlcs": 0, 
        "max_htlc_value_in_flight_msat": 0, 
        "payment_base_point": "", 
        "peer_id_a": "alice", 
        "peer_id_b": "bob", 
        "pub_key_a": "029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c", 
        "pub_key_b": "03da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f", 
        "push_msat": 0, 
        "revocation_base_point": "", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],
        "to_self_delay": 0
    }
}
```

<!-- center -->

Alice sends request to her OBD instance, her OBD helps her complete the message, and routes her request to Bob's OBD for creating a channel between them. 

**Message Type: -32**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
funding pubkey    | ------- |   data    | public key of funder, who wish to deposite BTC and other tokens to the channel
recipient_peer_id | ------- |   body    | peer id of the fundee.
 
<br/>

**OBD Response：**  
This response is simutanously sent to Bob's OBD. Response is a temporary_channel_id.

<br/>

Bob replies to accept, his OBD completes his message and routes it to Alice's OBD. Then Alice sees the response of acceptance. 

**Message Type: -33**
 
Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id  | ------- |   body    | the temporary channel id, used before real funding completes.
funding pubkey        | ------- |   body    | public key of fundee(Bob), who wish to deposite BTC and other tokens to the channel. Current implementation of OBD supports one-way deposite. Fundee does not need to fund the channel.
approval              | ------- |   body    | true or false to deny.
 
<br/>

**OBD Response：**  
This response is simutanously sent to Alice's OBD.

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status    | ------- |    body   | true or false
from      | ------- |    body   | peer id of funder
to        | ------- |    body   | peer id of fundee
accept_at | ------- |    result   | time of acceptance
address_a | ------- |    result   | address of Alice
address_b | ------- |    result   | address of Bob
chain_hash             |  1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P     |    result   | identify the public chain. Default is OmniLayer
channel_address                | ------- |    result   | the p2whk address of the channel
channel_address_redeem_script  | ------- |    result   | redeem script of the multi-sig address
channel_address_script_pub_key | ------- |    result   | the script pubkey
channel_id                     | ------- |    result   | the global unique channel id
channel_reserve_satoshis       | ------- |    result   | reserved satoshis in the channel
close_at                       | ------- |    result   | close time
create_at                      | ------- |    result   | create time
create_by                      | ------- |    result   | the funder who created this channel. Alice in this case.
curr_state                     | ------- |    result   | 
delayed_payment_base_point | ------- |    result   | 
dust_limit_satoshis        | ------- |    result   | 
fee_rate_per_kw            | ------- |    result   | 
funding_address            | ------- |    result   | Funder's address from where he send money to the channel
funding_pubkey             | ------- |    result   | public key of funder, who wish to deposite BTC and other tokens to the channel 
funding_satoshis           | ------- |    result   | 
htlc_base_point            | ------- |    result   | 
htlc_minimum_msat          | ------- |    result   | 
max_accepted_htlcs         | ------- |    result   | 
max_htlc_value_in_flight_msat      | ------- |    result   | 
payment_base_point                 | ------- |    result   | 
peer_id_a                          | ------- |    result   | peer id of funder
peer_id_b                          | ------- |    result   | peer id of fundee
pub_key_a                          | ------- |    result   | public key of funder
pub_key_b                          | ------- |    result   | public key of fundee. Current OBD implementation does not requir fundee to deposit money to a channel.
push_msat                          | ------- |    result   | 
revocation_base_point              | ------- |    result   | 
temporary_channel_id               | ------- |    result   | 
to_self_delay                      | ------- |    result   | 


## Open Channel between Bob and Carol（B2C）

<!-- right -->

> Bob send the request:

```json
{
	"type":-32,
    "data":{
        "funding_pubkey":"03da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f"
    },
    "recipient_peer_id":"carol"
}
```

> OBD Responses:

```json
[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86]
```

> Carol replies:

```json
{
	"type":-33,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"funding_pubkey":"034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -33, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "accept_at": "2019-11-04T11:07:27.4224459+08:00", 
        "address_a": "mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8", 
        "address_b": "mgoiBkppoJMc8cC8XRYNvFEjath5DrKqj8", 
        "chain_hash": "1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P", 
        "channel_address": "2MzQW254vB6mHsUvLHxCnKZ73Gcw7kSrvsd", 
        "channel_address_redeem_script": "522103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f21034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a52ae", 
        "channel_address_script_pub_key": "a9144e8a01887f51a04610909ceaddb596fbfe109b8f87", 
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "channel_reserve_satoshis": 0, 
        "close_at": "0001-01-01T00:00:00Z", 
        "create_at": "2019-11-04T11:06:50.261993+08:00", 
        "create_by": "bob", 
        "curr_state": 20, 
        "delayed_payment_base_point": "", 
        "dust_limit_satoshis": 0, 
        "fee_rate_per_kw": 0, 
        "funding_address": "mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8", 
        "funding_pubkey": "03da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f", 
        "funding_satoshis": 0, 
        "htlc_base_point": "", 
        "htlc_minimum_msat": 0, 
        "id": 2, 
        "max_accepted_htlcs": 0, 
        "max_htlc_value_in_flight_msat": 0, 
        "payment_base_point": "", 
        "peer_id_a": "bob", 
        "peer_id_b": "carol", 
        "pub_key_a": "03da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f", 
        "pub_key_b": "034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a", 
        "push_msat": 0, 
        "revocation_base_point": "", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],
        "to_self_delay": 0
    }
}
```

<!-- center -->

Bob sends request to his OBD instance, his OBD helps he complete the message, and routes his request to Carol's OBD for creating a channel between them. 

**Message Type: -32**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
funding pubkey      | ------- |   data    | public key of funder, who wish to deposite BTC and other tokens to the channel
recipient_peer_id | ------- |   body    | peer id of the fundee.
 
<br/>

**OBD Response：**  
This response is simutanously sent to Bob's OBD. Response is a temporary_channel_id.

<br/>

Carol replies to accept, her OBD completes her message and routes it to Bob's OBD. Then Bob sees the response of acceptance. 

**Message Type: -33**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
temporary_channel_id  | ------- |   body    | the temporary channel id, used before real funding completes.
funding pubkey        | ------- |   body    | public key of fundee(Bob), who wish to deposite BTC and other tokens to the channel. Current implementation of OBD supports one-way deposite. Fundee does not need to fund the channel.
approval              | ------- |   body    | true or false to deny.
 
<br/>

**OBD Response：**
Same to above.

<!-- ## Alice Deposit Assets to Channels A2B -->

## Alice deposit BTC to channel A2B for miner fee

<!-- right -->

> Alice send 1009 first:

```json
{
    "type":1009,
    "data":{
        "from_address":"muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit",
        "from_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
        "to_address":"2NFhMhDJT9TsnBCG6L2amH3eDXxgwW6EJh7",
        "amount":0.0001,
        "miner_fee":0.00001
    }
}
```

> OBD Responses:

```json
{
    "type": 1009, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "hex": "0200000002634ad0a2468850f4bed537ffc2a28aa6395cb2c34efe54b321135bae298d5d79020000006a4730440220421bc0e7cbeaebb5ad7e0559d5be01f50143816243904bed8c4fe2972717ad0b02206bf8b4f3dc911b6e8c6748b248e6e59dd4b43e4b5a197379863040f374a979a10121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffffe2fa86ac404f71b97e6b34010549a82c9f3ee1a150c54b7daec99c0da83f8482010000006a47304402207d52f8a36791ce26361809b9a7b9da4175bf3b5ad6004d6a46ecb4a329dfdb370220217c481b71bcafbf3e93fe72837c28e48f38474ea1341bae030867e37be5c08c0121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff02102700000000000017a914f64403be27af8af0a8abc21aed584b06f80adf30876a190f00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "txid": "b18aab6599f1661963763281c83ddd7f6de51813881b2ee563008c021d31fcd4"
    }
}
```

> Alice send a notification to bob:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",		"funding_tx_hex":"0200000002634ad0a2468850f4bed537ffc2a28aa6395cb2c34efe54b321135bae298d5d79020000006a4730440220421bc0e7cbeaebb5ad7e0559d5be01f50143816243904bed8c4fe2972717ad0b02206bf8b4f3dc911b6e8c6748b248e6e59dd4b43e4b5a197379863040f374a979a10121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffffe2fa86ac404f71b97e6b34010549a82c9f3ee1a150c54b7daec99c0da83f8482010000006a47304402207d52f8a36791ce26361809b9a7b9da4175bf3b5ad6004d6a46ecb4a329dfdb370220217c481b71bcafbf3e93fe72837c28e48f38474ea1341bae030867e37be5c08c0121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff02102700000000000017a914f64403be27af8af0a8abc21aed584b06f80adf30876a190f00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000"
	}
}
```

> OBD Responses:

```json
{
    "type": -3400, 
    "status": true, 
    "from": "alice", 
    "to": "bob", 
    "result": {
        "amount": 0.0001, 
        "funding_txid": "b18aab6599f1661963763281c83ddd7f6de51813881b2ee563008c021d31fcd4", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164]
    }
}
```

> Bob replies:

```json
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",		"funding_txid":"b18aab6599f1661963763281c83ddd7f6de51813881b2ee563008c021d31fcd4",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -3500, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "create_at": "2019-11-04T13:05:52.1148727+08:00", 
        "id": 1, 
        "owner": "alice", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],
        "tx_hash": "0200000001d4fc311d028c0063e52e1b881318e56d7fdd3dc8813276631966f19965ab8ab100000000d9004730440220167874a0697aeebb170adfd418cbab33a39a837099be4a829d8c71d4a1933e0c0220145afac21e206ed8b37e38aaa13788b91dc7819bc2caeb760c5b13fb62a34d820147304402201fcc09eff15e178704bcda267bcc221fa677ff3f9c0f04139a458696b121ef780220502e7ff9f919d4fe165f0b4abf39000779c7c49b47064b3c752db2668a81750601475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffff01581b0000000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "txid": "17405cc66b345247ded162a21ecfbd98a6e9b85a6d2dfba32b2421b9670efe4f"
    }
}
```

> Alice send 1009 second:

```json
{
    "type":1009,
    "data":{
		"from_address":"muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit",
		"from_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
		"to_address":"2NFhMhDJT9TsnBCG6L2amH3eDXxgwW6EJh7",
		"amount":0.0001,
		"miner_fee":0.00001
	}
}
```

> OBD Responses:

```json
{
    "type": 1009, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "hex": "0200000001d4fc311d028c0063e52e1b881318e56d7fdd3dc8813276631966f19965ab8ab1010000006a473044022000dbea4c6684f8d03571c57f3a37e9b7d51b4bebabc26eba062754bfafbed36502200bdf324eeba7c0ecb272e1ba3210a6f1aa17e1ddab9289caafa2414e6cd570180121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff02102700000000000017a914f64403be27af8af0a8abc21aed584b06f80adf308772ee0e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "txid": "c657197ed731441da5ac13032707639039de15cf0d5f644e291c007c0f97b6d4"
    }
}
```

> Alice send a notification to bob:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",		"funding_tx_hex":"0200000001d4fc311d028c0063e52e1b881318e56d7fdd3dc8813276631966f19965ab8ab1010000006a473044022000dbea4c6684f8d03571c57f3a37e9b7d51b4bebabc26eba062754bfafbed36502200bdf324eeba7c0ecb272e1ba3210a6f1aa17e1ddab9289caafa2414e6cd570180121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff02102700000000000017a914f64403be27af8af0a8abc21aed584b06f80adf308772ee0e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000"
	}
}
```

> OBD Responses:

```json
{
    "type": -3400, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "amount": 0.0001, 
        "funding_txid": "c657197ed731441da5ac13032707639039de15cf0d5f644e291c007c0f97b6d4", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164]
    }
}
```

> Bob replies:

```json
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",		"funding_txid":"c657197ed731441da5ac13032707639039de15cf0d5f644e291c007c0f97b6d4",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -3500, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "create_at": "2019-11-04T13:08:22.4007767+08:00", 
        "id": 2, 
        "owner": "alice", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],
        "tx_hash": "0200000001d4b6970f7c001c294e645f0dcf15de39906307270313aca51d4431d77e1957c600000000d9004730440220276a1bec9fc2662c328911f8f0a8af59d56732e5ca62abb3a9ba3b67343967c102203ab6c8c47ffa39ba6013e4d36afd74b408c60d4c2e32c757ef68587f214c91860147304402200766a31cc399d4441fe41f3fbb6e7c8f0dc00b34b388309201e474298f6ffac202202f6e6cbf3c03ffbaa0fb92fed4637a9585b7b91e819f2b5a031bab255214c6f301475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffff01581b0000000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "txid": "70cd0630e5efcdedd699a950ff8c9391f8afd682fffcb90c25ef6453d5d477ec"
    }
}
```

> Alice send 1009 third:

```json
{
    "type":1009,
    "data":{
		"from_address":"muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit",
		"from_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
		"to_address":"2NFhMhDJT9TsnBCG6L2amH3eDXxgwW6EJh7",
		"amount":0.0001,
		"miner_fee":0.00001
	}
}
```

> OBD Responses:

```json
{
    "type": 1009, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "hex": "0200000001d4b6970f7c001c294e645f0dcf15de39906307270313aca51d4431d77e1957c6010000006a473044022038611f8b4239633c53286d22a643f53ed9e280e90c49f6f6e88ccd31af34e08a02202d20172c87191dad26271ed26bdbdb7c410500a9cb24629fffb3253a2ffa9f190121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff02102700000000000017a914f64403be27af8af0a8abc21aed584b06f80adf30877ac30e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "txid": "f13d17bdbf7e933c921621ae7abfa6d56d47cacced8aee77987c374a3e337fe4"
    }
}
```

> Alice send a notification to bob:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",		"funding_tx_hex":"0200000001d4b6970f7c001c294e645f0dcf15de39906307270313aca51d4431d77e1957c6010000006a473044022038611f8b4239633c53286d22a643f53ed9e280e90c49f6f6e88ccd31af34e08a02202d20172c87191dad26271ed26bdbdb7c410500a9cb24629fffb3253a2ffa9f190121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff02102700000000000017a914f64403be27af8af0a8abc21aed584b06f80adf30877ac30e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000"
	}
}
```

> OBD Responses:

```json
{
    "type": -3400, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "amount": 0.0001, 
        "funding_txid": "f13d17bdbf7e933c921621ae7abfa6d56d47cacced8aee77987c374a3e337fe4", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164]
    }
}

```

> Bob replies:

```json
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",		"funding_txid":"f13d17bdbf7e933c921621ae7abfa6d56d47cacced8aee77987c374a3e337fe4",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -3500, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "create_at": "2019-11-04T13:09:51.4411834+08:00", 
        "id": 3, 
        "owner": "alice", 
        "temporary_channel_id": [68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],
        "tx_hash": "0200000001e47f333e4a377c9877ee8aedccca476dd5a6bf7aae2116923c937ebfbd173df100000000d900473044022015e42277609a892d988ed4662b5ed7a3ae2400fc9fa211cbf59447875d5de9b3022072c5e9c3322e497cc1c116c959233720ebd0587eb33206267e45c5361e2588e6014730440220327f8918baadec25dcd9780fcca5ef7eff78518c4aec3276d4a648ef71e1671f02200a2e7a6e1173feff589352bcbc880b7030ef78700f9e27320f45e1564bb972aa01475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffff01581b0000000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "txid": "2cd75a9e21cbc6e4b972ac40c10f5246b77dd74724bbadd145a00eec3371c6b0"
    }
}
```

<!-- center -->

*First deposit BTC for miner fee*

**Message Type: 1009**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
from_address         		| ------- |   data    | address of funder, from where the BTC is transferred.
from_address_private_key  	| ------- |   data    | private key.
to_address           		| ------- |   data    | the channel multi-sig address.
amount			   	| 0.0001  |   data    | 
miner_fee		   	| 0.00001 |   data    | 
 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
hex            | ------- |   result  | 
txid	       | ------- |   result  | transaction id

<br/>

*Send a notification to bob*

**Message Type: -3400**

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

<br/>

*Bob replies*

**Message Type: -3500**

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

<br/>

*Second deposit BTC*

**Message Type: 1009**

Parameters and OBD Responses same to above. 

<br/><br/><br/><br/><br/>

*Send a notification to bob*

**Message Type: -3400**

Parameters and OBD Responses same to above. 

<br/>

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Bob replies*

**Message Type: -3500**

Parameters and OBD Responses same to above. 

<br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Third deposit BTC*

**Message Type: 1009**

Parameters and OBD Responses same to above. 

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Send a notification to bob*

**Message Type: -3400**

Parameters and OBD Responses same to above. 

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Bob replies*

**Message Type: -3500**

Parameters and OBD Responses same to above. 


## Alice deposit USDT to channel A2B for transfer

<!-- right -->

> Alice send:

```json
{
	"type":2001,
	"data":{
        "from_address":"muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit",			
        "from_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
        "to_address":"2NFhMhDJT9TsnBCG6L2amH3eDXxgwW6EJh7",
        "amount":50,
        "property_id": 121
	}
}
```

> OBD Responses:

```json
{
    "type": 2001, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "hex": "0200000001e47f333e4a377c9877ee8aedccca476dd5a6bf7aae2116923c937ebfbd173df1010000006a47304402205d721941d28ec7a6a427d0da51cf89e70772548d0829b627919a3ebf8722e96a02207658e909db233dfa6d4ca4f1a3a08fc0325c045c519d2e0e8f6af56ea6e334f00121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff03a6b50e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac0000000000000000166a146f6d6e690000000000000079000000012a05f2001c0200000000000017a914f64403be27af8af0a8abc21aed584b06f80adf308700000000"
    }
}
```

> Alice2's temp address data:

```shell
address:  mq8t9iEHoYzw4EzByo9p1uCVBWDnFU4JwW
privkey:  cVBFoaRumDJYntRRV244KUj7kyrGauGhT6bZcf15xfhGCh9mAbVp
pubkey:   0380874d124f259b31ee8cf3256d784f0269ae9cf3b577e5c271c452572f8b28e5
```

> Alice send a notification to bob:

```json
{
    "type":-34,
	"data":{
		"temporary_channel_id":[68,9,34,176,221,163,195,216,120,239,152,94,138,101,252,83,99,125,195,221,146,3,0,128,166,224,203,99,101,48,20,164],		"funding_tx_hex":"0200000001e47f333e4a377c9877ee8aedccca476dd5a6bf7aae2116923c937ebfbd173df1010000006a47304402205d721941d28ec7a6a427d0da51cf89e70772548d0829b627919a3ebf8722e96a02207658e909db233dfa6d4ca4f1a3a08fc0325c045c519d2e0e8f6af56ea6e334f00121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff03a6b50e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac0000000000000000166a146f6d6e690000000000000079000000012a05f2001c0200000000000017a914f64403be27af8af0a8abc21aed584b06f80adf308700000000",	
		"temp_address_pub_key":"0380874d124f259b31ee8cf3256d784f0269ae9cf3b577e5c271c452572f8b28e5",		"temp_address_private_key":"cVBFoaRumDJYntRRV244KUj7kyrGauGhT6bZcf15xfhGCh9mAbVp",		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8"
	}
}
```

> OBD Responses:

```json
{
    "type": -34, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "amount_a": 50, 
        "amount_b": 0, 
        "channel_id": [174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
        "channel_info_id": 1, 
        "create_at": "2019-11-04T13:17:15.1972409+08:00", 
        "create_by": "alice", 
        "curr_state": 10, 
        "fundee_sign_at": "0001-01-01T00:00:00Z", 
        "funder_address": "muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit", 
        "funder_pub_key_2_for_commitment": "0380874d124f259b31ee8cf3256d784f0269ae9cf3b577e5c271c452572f8b28e5", 
        "funding_output_index": 2, 
        "funding_tx_hex": "0200000001e47f333e4a377c9877ee8aedccca476dd5a6bf7aae2116923c937ebfbd173df1010000006a47304402205d721941d28ec7a6a427d0da51cf89e70772548d0829b627919a3ebf8722e96a02207658e909db233dfa6d4ca4f1a3a08fc0325c045c519d2e0e8f6af56ea6e334f00121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff03a6b50e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac0000000000000000166a146f6d6e690000000000000079000000012a05f2001c0200000000000017a914f64403be27af8af0a8abc21aed584b06f80adf308700000000", 
        "funding_txid": "492be5f12083719e668a98fbcb531c42d8879c1151c93d20ed3a4c91679a24ae", 
        "id": 1, 
        "peer_id_a": "alice", 
        "peer_id_b": "bob", 
        "property_id": 121
    }
}
```

> Bob replies:

```json
{
	"type":-35,
	"data":{
		"channel_id":[174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],		"fundee_channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -35, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "amount_a": 50, 
        "amount_b": 0, 
        "channel_id": [174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
        "channel_info_id": 1, 
        "create_at": "2019-11-04T13:17:15.1972409+08:00", 
        "create_by": "alice", 
        "curr_state": 20, 
        "fundee_sign_at": "2019-11-04T13:18:19.6544966+08:00", 
        "funder_address": "muYrqVWTKnkaVAMuqn59Ta6GL912ixpxit", 
        "funder_pub_key_2_for_commitment": "0380874d124f259b31ee8cf3256d784f0269ae9cf3b577e5c271c452572f8b28e5", 
        "funding_output_index": 2, 
        "funding_tx_hex": "0200000001e47f333e4a377c9877ee8aedccca476dd5a6bf7aae2116923c937ebfbd173df1010000006a47304402205d721941d28ec7a6a427d0da51cf89e70772548d0829b627919a3ebf8722e96a02207658e909db233dfa6d4ca4f1a3a08fc0325c045c519d2e0e8f6af56ea6e334f00121029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3cffffffff03a6b50e00000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac0000000000000000166a146f6d6e690000000000000079000000012a05f2001c0200000000000017a914f64403be27af8af0a8abc21aed584b06f80adf308700000000", 
        "funding_txid": "492be5f12083719e668a98fbcb531c42d8879c1151c93d20ed3a4c91679a24ae", 
        "id": 1, 
        "peer_id_a": "alice", 
        "peer_id_b": "bob", 
        "property_id": 121
    }
}
```

<!-- center -->

*Deposit USDT for transfer to Bob*

**Message Type: 2001**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
from_address         		| ------- |   data    | address of funder, from where the BTC is transferred.
from_address_private_key  	| ------- |   data    | private key.
to_address           		| ------- |   data    | the channel multi-sig address.
amount			   	| 0.0001  |   data    | 
property_id		  	| ------- |   data    | the omni asset id that has been funding.
 
**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
hex            | ------- |   result  | 
txid	       | ------- |   result  | transaction id

<br/>

*Send a notification to bob*

**Message Type: -34**

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

<br/>

*Bob replies*

**Message Type: -35**

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


<!-- ## Bob Deposit Assets to Channels B2C -->

## Bob deposit BTC to channel B2C for miner fee

<!-- right -->

> Bob send 1009 first:

```json
{
    "type":1009,
    "data":{
		"from_address":"mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8",
		"from_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"to_address":"2MzQW254vB6mHsUvLHxCnKZ73Gcw7kSrvsd",
		"amount":0.0001,
		"miner_fee":0.00001
	}
}
```

> OBD Responses:

```json
{
    "type": 1009, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "hex": "0200000001602db1aaf84e110d8a980ce6edabfd7dd6ff9e015c1386ccaeb0950e172d095d010000006a47304402205f58bfbaa958111bdc5cd3f2b7fef54345994f01390b5bedb7616a651cf06970022020caac0ad91031648b6276eeaee890137066c835253f48c32ae90d5be135946c012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff02102700000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8748170f00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000", 
        "txid": "af8797e691644774cd6bb93f527e1ddcdfb291412fa1fa64e96f5efdd50db660"
    }
}
```

> Bob send a notification to Carol:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",		"funding_tx_hex":"0200000001602db1aaf84e110d8a980ce6edabfd7dd6ff9e015c1386ccaeb0950e172d095d010000006a47304402205f58bfbaa958111bdc5cd3f2b7fef54345994f01390b5bedb7616a651cf06970022020caac0ad91031648b6276eeaee890137066c835253f48c32ae90d5be135946c012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff02102700000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8748170f00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000"
	}
}
```

> OBD Responses:

```json
{
    "type": -3400, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "amount": 0.0001, 
        "funding_txid": "af8797e691644774cd6bb93f527e1ddcdfb291412fa1fa64e96f5efdd50db660", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86]
    }
}
```

> Carol replies:

```json
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",		"funding_txid":"af8797e691644774cd6bb93f527e1ddcdfb291412fa1fa64e96f5efdd50db660",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -3500, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "create_at": "2019-11-04T13:28:56.0383895+08:00", 
        "id": 4, 
        "owner": "bob", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],
        "tx_hash": "020000000160b60dd5fd5e6fe964faa12f4191b2dfdc1d7e523fb96bcd74476491e69787af00000000d900473044022003164086a5b474ddf75ebd878f49282cbae075667c5b23bd992c4a8c6dcdec1b02202d8d9f98a930c72fba0387cc0a352d81f1d61827e7ff9ac7e2974fd48f712c590147304402203ecde9676432a283864b74229d1b4c0922eabfa7086c66f4f3470fe09ca62a9602202806bd98359694c30296d5aba05c8b1a4ffa1d84105409f8e067c4c1cf4790f80147522103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f21034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a52aeffffffff01581b0000000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000", 
        "txid": "399b0ac5fb7d6ec736a9eca7c30830adcc38ce0dfe61615cb6085c71f206ace2"
    }
}
```

> Bob send 1009 second:

```json
{
    "type":1009,
    "data":{
		"from_address":"mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8",
		"from_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"to_address":"2MzQW254vB6mHsUvLHxCnKZ73Gcw7kSrvsd",
		"amount":0.0001,
		"miner_fee":0.00001
	}
}
```

> OBD Responses:

```json
{
    "type": 1009, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "hex": "020000000160b60dd5fd5e6fe964faa12f4191b2dfdc1d7e523fb96bcd74476491e69787af010000006a47304402204d0f507845d7ea2bf2d566c2808417bc47ae4d4f1f8f64826ef4429ae723444202201d73bbc627c6b5a862d8568b518190d03731437f24278b5142d460a172a1feac012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff02102700000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8750ec0e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000", 
        "txid": "3ac771c333ed4171a725cc6e554de66a937dafaaf087bed4bdb5927f219212ce"
    }
}
```

> Bob send a notification to Carol:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",		"funding_tx_hex":"020000000160b60dd5fd5e6fe964faa12f4191b2dfdc1d7e523fb96bcd74476491e69787af010000006a47304402204d0f507845d7ea2bf2d566c2808417bc47ae4d4f1f8f64826ef4429ae723444202201d73bbc627c6b5a862d8568b518190d03731437f24278b5142d460a172a1feac012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff02102700000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8750ec0e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000"
	}
}
```

> OBD Responses:

```json
{
    "type": -3400, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "amount": 0.0001, 
        "funding_txid": "3ac771c333ed4171a725cc6e554de66a937dafaaf087bed4bdb5927f219212ce", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86]
    }
}
```

> Carol replies:

```json
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",		"funding_txid":"3ac771c333ed4171a725cc6e554de66a937dafaaf087bed4bdb5927f219212ce",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -3500, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "create_at": "2019-11-04T13:30:36.8618003+08:00", 
        "id": 5, 
        "owner": "bob", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],
        "tx_hash": "0200000001ce1292217f92b5bdd4be87f0aaaf7d936ae64d556ecc25a77141ed33c371c73a00000000d900473044022018b420fca2ed9a82a27ad4637a801accdde63ffc4804e372435ff735129d1d8d02202bde9712ccf41471972b97c09a67f3d3e8a9d9af2ad6f547d449f2065025c1420147304402207a4c07d9e4e9802aa0b55a960726e59a06ab27d75436445bcb8093b78d0277d502201bee2a6d14b192ba59258e8e3cdf81c8cbf874399d5bfe3aa4bb70e838971ab10147522103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f21034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a52aeffffffff01581b0000000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000", 
        "txid": "5f5c918df87d039de2d5747884a2ffce86de448b0a7e803870db75a737c8de4c"
    }
}
```

> Bob send 1009 third:

```json
{
    "type":1009,
    "data":{
		"from_address":"mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8",
		"from_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"to_address":"2MzQW254vB6mHsUvLHxCnKZ73Gcw7kSrvsd",
		"amount":0.0001,
		"miner_fee":0.00001
	}
}
```

> OBD Responses:

```json
{
    "type": 1009, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "hex": "0200000001ce1292217f92b5bdd4be87f0aaaf7d936ae64d556ecc25a77141ed33c371c73a010000006a4730440220381b2144239eff8636a0ead31719a178fc92f3acaf891b852533f81e8532dbfc02201b49cba9639dec92ea0499ca1c0c3e15761675d79a0059bec43167c7c66ff763012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff02102700000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8758c10e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000", 
        "txid": "aa94902e186168fcc293e597a5d5b56f4dec3a7fb16d4ed84c7d3530dea53388"
    }
}
```

> Bob send a notification to Carol:

```json
{
	"type":-3400,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",		"funding_tx_hex":"0200000001ce1292217f92b5bdd4be87f0aaaf7d936ae64d556ecc25a77141ed33c371c73a010000006a4730440220381b2144239eff8636a0ead31719a178fc92f3acaf891b852533f81e8532dbfc02201b49cba9639dec92ea0499ca1c0c3e15761675d79a0059bec43167c7c66ff763012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff02102700000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8758c10e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000"
	}
}
```

> OBD Responses:

```json
{
    "type": -3400, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "amount": 0.0001, 
        "funding_txid": "aa94902e186168fcc293e597a5d5b56f4dec3a7fb16d4ed84c7d3530dea53388", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86]
    }
}
```

> Carol replies:

```json
{
	"type":-3500,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",		"funding_txid":"aa94902e186168fcc293e597a5d5b56f4dec3a7fb16d4ed84c7d3530dea53388",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -3500, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "channel_id": [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        "create_at": "2019-11-04T13:32:04.058052+08:00", 
        "id": 6, 
        "owner": "bob", 
        "temporary_channel_id": [26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],
        "tx_hash": "02000000018833a5de30357d4cd84e6db17f3aec4d6fb5d5a597e593c2fc6861182e9094aa00000000d90047304402203e356be9b2b17b516c7ecf4aff3d8f62b4ea4e64081ba1a09fc636eb469e73b20220549f4dd8078303f8dce70f0989fe6a8218288598c87e5ced096230da04b8db140147304402203dce406a77df31c8f64650b95a9ffb2a7961983b8ef5dc663c326df624c0829402202ce9205d170c72e9d6d88a9a4dcf43892bd481c9f859c61d0783d8eb9a7165090147522103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f21034094927aa69a96d82d7e67146cf9b8dcd775919d1373d5319454e6004c0cdf7a52aeffffffff01581b0000000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac00000000", 
        "txid": "efb844c25f1a151034f1209b9bf039de178464da8e9f81562226e52044caaf25"
    }
}
```

<!-- center -->

*First deposit BTC*

**Message Type: 1009**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Send a notification to Carol*

**Message Type: -3400**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Carol replies*

**Message Type: -3500**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Second deposit BTC*

**Message Type: 1009**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Send a notification to Carol*

**Message Type: -3400**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Carol replies*

**Message Type: -3500**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Third deposit BTC*

**Message Type: 1009**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Send a notification to Carol*

**Message Type: -3400**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Carol replies*

**Message Type: -3500**

## Bob deposit USDT to channel B2C for transfer

<!-- right -->

> Bob send:

```json
{
	"type":2001,
	"data":{
		"from_address":"mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8",
		"from_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"to_address":"2MzQW254vB6mHsUvLHxCnKZ73Gcw7kSrvsd",
		"amount":40,
		"property_id": 121
	}
}
```

> OBD Responses:

```json
{
    "type": 2001, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "hex": "02000000018833a5de30357d4cd84e6db17f3aec4d6fb5d5a597e593c2fc6861182e9094aa010000006a47304402207d3254f94e441a094b998751e67ef98159f06befc8af9917896d73433135c07f02200f5a2805771f1f4a0fd7ca8f5a1b97543de171b0a973f130cd499deded58d6e7012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff0384b30e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac0000000000000000166a146f6d6e69000000000000007900000000ee6b28001c0200000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8700000000"
    }
}
```

> Bob2's temp address data:

```shell
address:  muqmS2XAX91TvtrUmYnz8qCCTdgFq1mtdj
privkey:  cNzMo4uyZZHBEsxzfK6QRzVcshR5mjFLbwY9p1n921i6PQAewSBD
pubkey:   02759f5703ca8f229ae9815345eaf1cbb7acdd5dc67c49d49a2c6ce4b74c52bf0f
```

> Bob send a notification to Carol:

```json
{
	"type":-34,
	"data":{
		"temporary_channel_id":[26,200,121,127,242,0,84,191,162,35,118,90,99,71,229,123,238,190,22,226,54,211,38,113,229,165,241,132,153,48,99,86],		"funding_tx_hex":"02000000018833a5de30357d4cd84e6db17f3aec4d6fb5d5a597e593c2fc6861182e9094aa010000006a47304402207d3254f94e441a094b998751e67ef98159f06befc8af9917896d73433135c07f02200f5a2805771f1f4a0fd7ca8f5a1b97543de171b0a973f130cd499deded58d6e7012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff0384b30e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac0000000000000000166a146f6d6e69000000000000007900000000ee6b28001c0200000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8700000000",	
		"temp_address_pub_key":"02759f5703ca8f229ae9815345eaf1cbb7acdd5dc67c49d49a2c6ce4b74c52bf0f",		"temp_address_private_key":"cNzMo4uyZZHBEsxzfK6QRzVcshR5mjFLbwY9p1n921i6PQAewSBD",		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt"
	}
}
```

> OBD Responses:

```json
{
    "type": -34, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "amount_a": 40, 
        "amount_b": 0, 
        "channel_id": [223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51],
        "channel_info_id": 2, 
        "create_at": "2019-11-04T13:35:57.7498842+08:00", 
        "create_by": "bob", 
        "curr_state": 10, 
        "fundee_sign_at": "0001-01-01T00:00:00Z", 
        "funder_address": "mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8", 
        "funder_pub_key_2_for_commitment": "02759f5703ca8f229ae9815345eaf1cbb7acdd5dc67c49d49a2c6ce4b74c52bf0f", 
        "funding_output_index": 2, 
        "funding_tx_hex": "02000000018833a5de30357d4cd84e6db17f3aec4d6fb5d5a597e593c2fc6861182e9094aa010000006a47304402207d3254f94e441a094b998751e67ef98159f06befc8af9917896d73433135c07f02200f5a2805771f1f4a0fd7ca8f5a1b97543de171b0a973f130cd499deded58d6e7012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff0384b30e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac0000000000000000166a146f6d6e69000000000000007900000000ee6b28001c0200000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8700000000", 
        "funding_txid": "316dac6f679c413ee93148bec0c5b73001c0f79e01f2ee919b2f16bab94bb1df", 
        "id": 2, 
        "peer_id_a": "bob", 
        "peer_id_b": "carol", 
        "property_id": 121
    }
}
```

> Carol replies:

```json
{
	"type":-35,
	"data":{
		"channel_id":[223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51],		"fundee_channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -35, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "amount_a": 40, 
        "amount_b": 0, 
        "channel_id": [223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51],
        "channel_info_id": 2, 
        "create_at": "2019-11-04T13:35:57.7498842+08:00", 
        "create_by": "bob", 
        "curr_state": 20, 
        "fundee_sign_at": "2019-11-04T13:37:05.2307075+08:00", 
        "funder_address": "mtSJixJ8eCguXDAdkGGoQu3nG1n77a6td8", 
        "funder_pub_key_2_for_commitment": "02759f5703ca8f229ae9815345eaf1cbb7acdd5dc67c49d49a2c6ce4b74c52bf0f", 
        "funding_output_index": 2, 
        "funding_tx_hex": "02000000018833a5de30357d4cd84e6db17f3aec4d6fb5d5a597e593c2fc6861182e9094aa010000006a47304402207d3254f94e441a094b998751e67ef98159f06befc8af9917896d73433135c07f02200f5a2805771f1f4a0fd7ca8f5a1b97543de171b0a973f130cd499deded58d6e7012103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91fffffffff0384b30e00000000001976a9148db89be1a801f2b6d1f66703982dc1642a87f7af88ac0000000000000000166a146f6d6e69000000000000007900000000ee6b28001c0200000000000017a9144e8a01887f51a04610909ceaddb596fbfe109b8f8700000000", 
        "funding_txid": "316dac6f679c413ee93148bec0c5b73001c0f79e01f2ee919b2f16bab94bb1df", 
        "id": 2, 
        "peer_id_a": "bob", 
        "peer_id_b": "carol", 
        "property_id": 121
    }
}
```

<!-- center -->

*Deposit USDT for transfer to Carol*

**Message Type: 2001**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Send a notification to Carol*

**Message Type: -34**

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

*Carol replies*

**Message Type: -35**

## Create RSMC Commitment Transaction between Alice and Bob

<!-- right -->

> Prepare Data:

```shell
last_temp_address：     mq8t9iEHoYzw4EzByo9p1uCVBWDnFU4JwW
last_temp_private_key： cVBFoaRumDJYntRRV244KUj7kyrGauGhT6bZcf15xfhGCh9mAbVp

alice2 data
curr_temp_address：     moKypvXXnui8obVqFNcJkLGL9w483AqUvQ
curr_temp_private_key： cVsKRbL4ijWULmbU78nghKYL79GLYo7q9ccgmSR5c6zJWKfEEdJN
curr_temp_pub_key:      028cda5a6cadd592f8b8728b7842d7f5045609895eb27bc87ef8bbde3bdc6b96f3
```

> Alice launch the request:

```json
{
	"type":-351,
	"data":{
        "channel_id":[174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
        "amount":24,
        "curr_temp_address_pub_key":"028cda5a6cadd592f8b8728b7842d7f5045609895eb27bc87ef8bbde3bdc6b96f3",
        "curr_temp_address_private_key":"cVsKRbL4ijWULmbU78nghKYL79GLYo7q9ccgmSR5c6zJWKfEEdJN",
        "channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
        "last_temp_address_private_key":"cVBFoaRumDJYntRRV244KUj7kyrGauGhT6bZcf15xfhGCh9mAbVp"
    }
}
```

> OBD Responses:

```json
{
    "type": -351, 
    "status": true, 
    "from": "alice", 
    "to": "alice", 
    "result": {
        "amount": 24, 
        "channel_address_private_key": "", 
        "channel_id": [174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
        "curr_temp_address_private_key": "", 
        "curr_temp_address_pub_key": "028cda5a6cadd592f8b8728b7842d7f5045609895eb27bc87ef8bbde3bdc6b96f3", 
        "last_temp_address_private_key": "", 
        "property_id": 0, 
        "request_commitment_hash": "f290579190da44c3c9950c3322710ad7f8ea9287d9a6ebebb8284874bfcae50d"
    }
}
```

> Bob2's temp address data:

```shell
last_temp_pub_key： 

Bob2
curr_temp_address：     mnVf4N8yur9bowFokfz8Ypn8C2kqNmp9Hg
curr_temp_private_key： cURFDvYsF49hazcGK3i4344H1r3pSjHwdqL2yQ85qxdzpub3rozx
curr_temp_pub_key：     02ef5cc2ab1dd7a840834991fcdf0554e59c550a7d5ff6934eff09c53099e5273b
```

> Bob replies:

```json
{
	"type":-352,
	"data":{
		"channel_id":[174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
		"curr_temp_address_pub_key":"02ef5cc2ab1dd7a840834991fcdf0554e59c550a7d5ff6934eff09c53099e5273b",
		"curr_temp_address_private_key":"cURFDvYsF49hazcGK3i4344H1r3pSjHwdqL2yQ85qxdzpub3rozx",
		"last_temp_private_key":"",
		"request_commitment_hash":"f290579190da44c3c9950c3322710ad7f8ea9287d9a6ebebb8284874bfcae50d",
		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -352, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "amount_to_htlc": 0, 
        "amount_to_other": 26, 
        "amount_to_rsmc": 24, 
        "channel_id": [174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
        "create_at": "2019-11-04T14:37:47.5732+08:00", 
        "create_by": "bob", 
        "curr_hash": "1c7b75fd5db490b818113a2c67532b017dfa66c227a5c44b85b04afafb67ba71", 
        "curr_state": 10, 
        "htlc_h": "", 
        "htlc_multi_address": "", 
        "htlc_multi_address_script_pub_key": "", 
        "htlc_redeem_script": "", 
        "htlc_sender": "", 
        "htlc_temp_address_pub_key": "", 
        "htlc_tx_hash": "", 
        "htlc_txid": "", 
        "id": 4, 
        "input_amount": 50, 
        "input_txid": "492be5f12083719e668a98fbcb531c42d8879c1151c93d20ed3a4c91679a24ae", 
        "input_vout": 2, 
        "last_commitment_tx_id": 0, 
        "last_edit_time": "2019-11-04T14:37:47.5732+08:00", 
        "last_hash": "", 
        "owner": "bob", 
        "peer_id_a": "alice", 
        "peer_id_b": "bob", 
        "property_id": 121, 
        "rsmc_multi_address": "2NCqpcN5cZXztLxnuojGDdfASVaiVUgNHms", 
        "rsmc_multi_address_script_pub_key": "a914d6f56f035723dab75cb42168a78c3950a51463f787", 
        "rsmc_redeem_script": "522102ef5cc2ab1dd7a840834991fcdf0554e59c550a7d5ff6934eff09c53099e5273b21029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c52ae", 
        "rsmc_temp_address_pub_key": "02ef5cc2ab1dd7a840834991fcdf0554e59c550a7d5ff6934eff09c53099e5273b", 
        "rsmc_tx_hash": "0200000001d4b6970f7c001c294e645f0dcf15de39906307270313aca51d4431d77e1957c600000000d900473044022029b293c9a41afa157e40a8dc01cf1c37b36f0370e5f352d785637c3517f6987602206c65e2997645a52546aed3df51e3b3dc31b4f754dabf87d7e6575dabbc9ffb350147304402207f081c590e044cfd62eb61f0babff619451f2e2b97b5e84126a7e4ce606773730220032a6eb2e4a7f109886aa2253e80bfc9f44e4c55d6906ee1192323dd0c95785901475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffff033c1900000000000017a914d6f56f035723dab75cb42168a78c3950a51463f7870000000000000000166a146f6d6e690000000000000079000000008f0d18001c0200000000000017a914d6f56f035723dab75cb42168a78c3950a51463f78700000000", 
        "rsmc_txid": "79b9da6165f26514c311e9deb166c267a152d3b2739d1b6d20570524b470c530", 
        "send_at": "0001-01-01T00:00:00Z", 
        "sign_at": "2019-11-04T14:37:49.5936687+08:00", 
        "to_other_tx_hash": "0200000003ae249a67914c3aed203dc951119c87d8421c53cbfb988a669e718320f1e52b4902000000d900473044022044bce17ff3e559eb261e58cfd74c75dcce484bca1e036383eccda0af585a867a022076f7c6ae5d115cb59e7346b700d4d5a3dbd0a1ff7b0ad25db0772300334061720147304402206135de8ec7b2d61a93425d07d13e811592d392ca5afd4bef7595f7edae7d7e85022025e2970a19bf19a6ade5e9a5a73d3a669648bbff17c6edf95795613d3229f72601475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffffd4fc311d028c0063e52e1b881318e56d7fdd3dc8813276631966f19965ab8ab100000000d90047304402201a7a29be4f434fa296ea25324d032eba37e468baf80af5acb7692e41c75f47f602203992ad31a27388025783752daa4b515ac4c9da70f0d2d11212c529b3d90519740147304402206abe4622ed65af415694e932050f671e1f9c11dcaa31afbc19d4bf85a2163baa022035b9a2272cd050b9a9dfa12f96cfec21c5c31b9fadf647ce6802fb519d2220dc01475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffffe47f333e4a377c9877ee8aedccca476dd5a6bf7aae2116923c937ebfbd173df100000000d900473044022017961e760fb5e9542d27453ab1e8c511189e49053290f5f1e95c8a1d33cdc67d022071e040e3d62c26b0c90b6e85e9d2c89f60ee90b51f5c935ace323cdafe58ab9601473044022017899dd5eab611e951beb18e2b5d5fc6403349c3f84db6a06bce8fae0e367c0e02207c673ff269dd43eb4aabcc8ad3b9b9a5aa67faaedccf9489dc121e4f51a5a0ee01475221029cf4b150da0065d5c08bf088e8a5367d35ff72e4e79b39efb401530d19fa3f3c2103da1b78a5ab4a5e4e13515e5104dbfc1d2d349d89039c15eda9c0118b7edaa91f52aeffffffff0362420000000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac0000000000000000166a146f6d6e690000000000000079000000009af8da0022020000000000001976a91499ee096d15bc8ae4ac8856a918ff2c4572877fa488ac00000000", 
        "to_other_txid": "6d7f75233f9ba125ee0c7bba31e910fd546703f89c2693298d4becf25096683c", 
        "tx_type": 0
    }
}
```

<!-- center -->

*Alice transfer assets to Bob*

**Message Type: -351**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id         		| ------- |   data    | 
amount  			| ------- |   data    | amount to pay
curr_temp_address_pub_key       | ------- |   data    | temporary multi-sig address
curr_temp_address_private_key   | ------- |   data    | the private key of the temporary multi-sig address currently in use. 
property_id		   	| ------- |   data    | the omni asset id that has been funding.
channel_address_private_key     | ------- |   data    | Alice's private key of the channel
last_temp_address_private_key   | ------- |   data    | the private key of the last temporary multi-sig address

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | peer id of funder
to             | ------- |   body    | peer id of fundee
amount 	       | ------- |   result  | 
channel_address_private_key  		| ------- |   result    | 
channel_id 				| ------- |   result    | 
curr_temp_address_private_key 		| ------- |   result    | 
curr_temp_address_pub_key 		| ------- |   result    | 
last_temp_address_private_key 		| ------- |   result    | 
property_id 				| ------- |   result    | 
request_commitment_hash 		| ------- |   result    | 

<br/>

*Bob replies*

**Message Type: -352**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id         			| ------- |   data    | 
curr_temp_address_pub_key               | ------- |   data    | to be added
curr_temp_address_private_key           | ------- |   data    | to be added
last_temp_private_key     		| ------- |   data    | to be added
request_commitment_hash			| ------- |   data    | to be added
channel_address_private_key 		| ------- |   data    | private key of the channel that Bob holds
approval

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status           | ------- |   body    | true or false
from	         | ------- |   body    | peer id of funder
to               | ------- |   body    | peer id of fundee
amount_b         | ------- |   result  | how much has been payed
amount_m         | ------- |   result  | the balance of the sender 
channel_id       | ------- |   result  | channel officially created
create_at        | ------- |   result  | 
create_by        | ------- |   result  | created by the funder
curr_state       | ------- |   result  | 
id               | ------- |   result  | 
input_amount 	 | ------- |   result  | 
input_txid 	 | ------- |   result  | 
input_vout	 | ------- |   result  | 
last_commitment_tx_id 	| ------- |   result  | to be added
last_edit_time 		| ------- |   result  | 
last_hash 		| ------- |   result  | 
multi_address 		| ------- |   result  | to be added
owner 			| ------- |   result  | 
peer_id_a 		| ------- |   result  | 
peer_id_b 		| ------- |   result  | 
property_id 		| ------- |   result  | 
redeem_script 		| ------- |   result  | to be added
script_pub_key 		| ------- |   result  | to be added
send_at 		| ------- |   result  | 
sign_at 		| ------- |   result  | 
temp_address_pub_key 	| ------- |   result  | to be added
transaction_sign_hex_to_other 	| ------- |   result  | to be added


## Launch a HTLC

<!-- right -->

> Alice launch a request to transfer to Carol:

```json
{
	"type": -40,
	"data": {
		"property_id": 121,
		"amount": 5,
		"recipient_peer_id": "carol"
	}
}
```

> OBD Responses:

```json
{
    "type": -40, 
    "status": true, 
    "from": "alice", 
    "to": "carol", 
    "result": {
        "amount": 5, 
        "approval": false, 
        "property_id": 121, 
        "request_hash": "742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636"
    }
}
```

> Carol send H to Alice:

```json
{
	"type": -41,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"property_id": 121,
		"amount": 5,
		"approval":true
	}
}
```

> OBD Responses:

```json
{
    "type": -41, 
    "status": true, 
    "from": "carol", 
    "to": "alice", 
    "result": {
        "approval": true, 
        "h": "83519233492eb05ddd547757f2c3d151ad9392b2ebf48fc1a88e07e61dd82a45", 
        "id": 1, 
        "request_hash": "742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636"
    }
}
```

<!-- center -->

We will launch a HTLC (Hashed Timelock Contract) transfer for testing purpose. 
It tests Alice transfer assets to Carol through Bob (middleman).

a) There IS NOT a direct channel between Alice and Carol.

b) There is a direct channel between Alice and Bob.

c) There is a direct channel between Bob and Carol.

<br/>

*Alice launch a request to transfer to Carol*

**Message Type: -40**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
property_id         | ------- |   data    | assets id
amount	            | ------- |   data    | amount of transfer
recipient_peer_id   | ------- |   data    | peer id of receiever

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
amount         | ------- |   result  | amount of transfer
approval       | ------- |   result  | true or false
property_id    | ------- |   result  | assets id
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Carol receieved request from Alice, and Carol generate a preimage R as a secret.

Carol send H (Hash of the preimage R) to Alice. Alice will looking for a middleman
that can transfer assets directly to Carol. Alice will tell the middleman if can 
produce to Alice an unknown 20-byte random input data R from a known hash H, 
within three days, then Alice will settle the contract by paying the middleman assets.

<br/>

*Carol send H (Hash_Preimage_R) to Alice*

**Message Type: -41**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash  | ------- |   data    | hash of this HTLC request
property_id   | ------- |   data    | assets id
amount	      | ------- |   data    | amount of transfer
approval      | ------- |   data    | true or false

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
approval       | ------- |   result  | true or false
h              | ------- |   result  | Hash_Preimage_R
id             | ------- |   result  | 
request_hash   | ------- |   result  | hash of this HTLC request


## Alice looking for a path to transfer to Carol and Send H to Bob

<!-- right -->

> Alice send H to Bob:

```json
{
	"type": -42,
	"data": {
		"h":"83519233492eb05ddd547757f2c3d151ad9392b2ebf48fc1a88e07e61dd82a45"
	}
}
```

> OBD Responses:

```json
{
    "type": -42, 
    "status": true, 
    "from": "alice", 
    "to": "bob", 
    "result": {
        "h": "83519233492eb05ddd547757f2c3d151ad9392b2ebf48fc1a88e07e61dd82a45", 
        "request_hash": "742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636"
    }
}
```

> Bob's temp address data for create HTLC commitment transactions:

```shell
Bob RSMC
address:  mqtffoYNN7J3pZnkeVgoXfpQ1pz11ZuyX2
privkey:  cN3o7Se2qcSq7Z3wYL9DLxn1cr5Du5rmmxsQeoeF6LC7yXcZZotN
pubkey:   03c5d2756dea6d6259080d7e1ab3f8597e7e9a83b5667eff70ea49ca3addb6f293

Bob HTLC
address:  mnznzruCDWz3QQUCw4wvC3NoNup2kdTdkU
privkey:  cPRy5pB8Ek2DabfQ74x8giqDdPtwTvptgRnq8qEP7KduzdmMFmJM
pubkey:   03d2edfe1f0a527f70473dbacb386e4e6a9cc0ea0cabf71f6c0a3dd516a8e6099f
```

> Bob replies:

```json
{
	"type": -44,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"approval":true,
		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"last_temp_address_private_key":"cURFDvYsF49hazcGK3i4344H1r3pSjHwdqL2yQ85qxdzpub3rozx",
		"curr_rsmc_temp_address_pub_key":"03c5d2756dea6d6259080d7e1ab3f8597e7e9a83b5667eff70ea49ca3addb6f293",
		"curr_rsmc_temp_address_private_key":"cN3o7Se2qcSq7Z3wYL9DLxn1cr5Du5rmmxsQeoeF6LC7yXcZZotN",
		"curr_htlc_temp_address_pub_key":"03d2edfe1f0a527f70473dbacb386e4e6a9cc0ea0cabf71f6c0a3dd516a8e6099f",
		"curr_htlc_temp_address_private_key":"cPRy5pB8Ek2DabfQ74x8giqDdPtwTvptgRnq8qEP7KduzdmMFmJM"
	}
}

```

> OBD Responses:

```json
{
    "type": -44, 
    "status": true, 
    "from": "bob", 
    "to": "alice", 
    "result": {
        "approval": true, 
        "request_hash": "742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636"
    }
}
```

> Alice's temp address data for create HTLC commitment transactions:

```shell
Alice RSMC
address  mmvTaVWx9EtwRHMkmhLbZF2JBGZc12ym2o
privkey cSYJ3vwcgMPDqegXFJ2YgCYNNgKS9tNxCaRkZnn3ourQSdGNkJCk
pubkey 03dab6d7b005e8b15a2dc8d7005b45111876813c24a54ff15316a76ba376cf020f

Alice HTLC
address  mtj8ChNcwkJi3ktB4apPTakknpDdiErTDX
privkey cR7wXNwPjMrDCpnJoinTiMK384YyKNTfyctLQ2CCdQobdanEqgAs
pubkey 03d16de84b72460055b18e6d572b49c4ab0e1d889c0bf0705becb22e16b65ca916

Alice HTna
address  mvYRwC7zTVhxNWeEgnUrdazMERvbP2yZpP
privkey cNzNyejXtgC4ySXCVXqa6egVinYLDtGhkRkGd271ZW6AJrVKYZ2w
pubkey 030cb3034f7374d5bb614e27169df99d346748a1a7a365a27b1138f5db7ad2b0f3
```

> Alice create HTLC commitment transactions:

```json
{
	"type": -45,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
		"last_temp_address_private_key":"cVsKRbL4ijWULmbU78nghKYL79GLYo7q9ccgmSR5c6zJWKfEEdJN",
		"curr_rsmc_temp_address_pub_key":"03dab6d7b005e8b15a2dc8d7005b45111876813c24a54ff15316a76ba376cf020f",
		"curr_rsmc_temp_address_private_key":"cSYJ3vwcgMPDqegXFJ2YgCYNNgKS9tNxCaRkZnn3ourQSdGNkJCk",
		"curr_htlc_temp_address_pub_key":"03d16de84b72460055b18e6d572b49c4ab0e1d889c0bf0705becb22e16b65ca916",
		"curr_htlc_temp_address_private_key":"cR7wXNwPjMrDCpnJoinTiMK384YyKNTfyctLQ2CCdQobdanEqgAs",
        "curr_htlc_temp_address_for_ht1a_pub_key":"030cb3034f7374d5bb614e27169df99d346748a1a7a365a27b1138f5db7ad2b0f3",
		"curr_htlc_temp_address_for_ht1a_private_key":"cNzNyejXtgC4ySXCVXqa6egVinYLDtGhkRkGd271ZW6AJrVKYZ2w"
	}
}
```

> OBD Responses:

```json
{
    "type": -45, 
    "status": true, 
    "from": "alice", 
    "to": "bob", 
    "result": {
        "h": "e7626f2b7207006d6515399c587c09c3bfb5ed3b12f63c12b0d40e634f9dd9a3", 
        "h_and_r_info_request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

<!-- center -->

Alice will looking for a path to reach Carol by pathfinding algorithm.
For testing purpose now, there is three client Alice, Bob, Carol, and 
they are all connected to an OBD server. 

The real situation should be that a network of many OBD servers is 
spread all over the world, a large number of clients are connected 
to these OBDs, and then we need to find the shortest path to transfer 
assets from one client to another.

<br/>

*Alice found a path and launch a request to middleman Bob*

*Alice send H to Bob*

**Message Type: -42**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
h         | ------- |   data    | Hash of the preimage R

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
h              | ------- |   result  | Hash of the preimage R
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Bob (middleman) receieved request from Alice and agree.

*Bob replies*

**Message Type: -44**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
approval       | ------- |   data    | true or false
channel_address_private_key         | ------- |   data    | 
last_temp_address_private_key       | ------- |   data    | 
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key  | ------- |   data    | 
curr_htlc_temp_address_pub_key      | ------- |   data    | 
curr_htlc_temp_address_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
approval       | ------- |   result  | true or false
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Bob agree request, Alice create related commitment transactions between Alice and Bob.

*Alice create HTLC commitment transactions*

**Message Type: -45**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
channel_address_private_key         | ------- |   data    | 
last_temp_address_private_key       | ------- |   data    | 
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key  | ------- |   data    | 
curr_htlc_temp_address_pub_key      | ------- |   data    | 
curr_htlc_temp_address_private_key  | ------- |   data    | 
curr_htlc_temp_address_for_ht1a_pub_key      | ------- |   data    | 
curr_htlc_temp_address_for_ht1a_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
h              | ------- |   result  | Hash of the preimage R
h_and_r_info_request_hash   | ------- |   result  | hash of this HTLC request


## Bob Send H to Carol through the Path

<!-- right -->

> Bob (middleman) Send H to Carol (destination):

```json
{
	"type": -43,
	"data": {
		"h":"83519233492eb05ddd547757f2c3d151ad9392b2ebf48fc1a88e07e61dd82a45",
        "h_and_r_info_request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636"
	}
}
```

> OBD Responses:

```json
{
    "type": -43, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "h": "e7626f2b7207006d6515399c587c09c3bfb5ed3b12f63c12b0d40e634f9dd9a3", 
        "request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

> Carol's temp address data for create HTLC commitment transactions:

```shell
Carol RSMC
address  mwFhZchMtq4y9jSvmkAaoyAve1vZ4gfCvB
privkey cTTFKJ3N8W4qHcpwj19NVVaEYAXBxf8DmBj9g7owLTWQ3mXXfD51
pubkey 03dd26ec67e15bde83b527be45a1c64f420821ba78ebc5eb9d3fe1a8ae3cd1f6d9

Carol HTLC
address  mmsNgwiBhJLcv7Pup7gvsNKg82ECcLMjdF
privkey cVW35aEjd56ZFHTSDtq9iUzHAfH6rWGVJcmYBn9QEp9ygSfkgZeG
pubkey 03e96c6692bef50af7c6c777ff1bd65b1134d18c98be801e00f8e6247db65950b8
```

> Carol replies:

```json
{
	"type": -44,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"approval":true,
		"channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",
		"last_temp_address_private_key":"",
		"curr_rsmc_temp_address_pub_key":"03dd26ec67e15bde83b527be45a1c64f420821ba78ebc5eb9d3fe1a8ae3cd1f6d9",
		"curr_rsmc_temp_address_private_key":"cTTFKJ3N8W4qHcpwj19NVVaEYAXBxf8DmBj9g7owLTWQ3mXXfD51",
		"curr_htlc_temp_address_pub_key":"03e96c6692bef50af7c6c777ff1bd65b1134d18c98be801e00f8e6247db65950b8",
		"curr_htlc_temp_address_private_key":"cVW35aEjd56ZFHTSDtq9iUzHAfH6rWGVJcmYBn9QEp9ygSfkgZeG"
	}
}
```

> OBD Responses:

```json
{
    "type": -44, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "approval": true, 
        "request_hash": "742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636"
    }
}
```

> Bob's temp address data for create HTLC commitment transactions:

```shell
Bob RSMC
address  mxHpHW9Sc7JByG3m9nmjC77jexKcMe8mwi
privkey cTMZ3csaDWvods2jnMkNJZpz98DRWmFQXb4C1vDFTibcST5g3SNb
pubkey 033c6284ac5c2409cbf2a49103ff05715f5a0497a0490cdc038248ba37c10e8ccb

Bob HTLC
address  mqkWnkNfhUR7niBVehuBfdXDmhHCL71ohG
privkey cNon6RZ9uLq6EPpGYt8tDZjQLMWVZDAXcrFy3LH1ZmHQJDbKWnye
pubkey 025c7cab6f5724a507ad7268bfb6820a3b6902b09a99e1b37241a6b8ede33cc2f1

Bob HTnb
address  mzQmxkY35FaXfzDKyTQPWzxG7k3vZJqkeP
privkey cUoDGr5cdNarcv43YXFdXBY2zf9721y6u6MiDnk56TSJWCGKvTbL
pubkey 02977ddeffc04ac0c99c74db308a4db39e60b338d99f3d1661f5ae24f3e17ad414
```

> Bob create HTLC commitment transactions:

```json
{
	"type": -45,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"last_temp_address_private_key":"cNzMo4uyZZHBEsxzfK6QRzVcshR5mjFLbwY9p1n921i6PQAewSBD",
		"curr_rsmc_temp_address_pub_key":"033c6284ac5c2409cbf2a49103ff05715f5a0497a0490cdc038248ba37c10e8ccb",
		"curr_rsmc_temp_address_private_key":"cTMZ3csaDWvods2jnMkNJZpz98DRWmFQXb4C1vDFTibcST5g3SNb",
		"curr_htlc_temp_address_pub_key":"025c7cab6f5724a507ad7268bfb6820a3b6902b09a99e1b37241a6b8ede33cc2f1",
		"curr_htlc_temp_address_private_key":"cNon6RZ9uLq6EPpGYt8tDZjQLMWVZDAXcrFy3LH1ZmHQJDbKWnye",
        "curr_htlc_temp_address_for_ht1a_pub_key":"02977ddeffc04ac0c99c74db308a4db39e60b338d99f3d1661f5ae24f3e17ad414",
		"curr_htlc_temp_address_for_ht1a_private_key":"cUoDGr5cdNarcv43YXFdXBY2zf9721y6u6MiDnk56TSJWCGKvTbL"
	}
}
```

> OBD Responses:

```json
{
    "type": -45, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "h": "e7626f2b7207006d6515399c587c09c3bfb5ed3b12f63c12b0d40e634f9dd9a3", 
        "h_and_r_info_request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

<!-- center -->

Setp 2 of the path, Bob (middleman) has got the H and send it to Carol. 

That's mean if Carol can produce to Bob an unknown 20-byte random input 
data R from a known hash H, within two days, then Bob will settle the 
contract by paying Carol assets.

<br/>

*Bob (middleman) Send H to Carol (destination)*

**Message Type: -43**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
h         | ------- |   data    | Hash of the preimage R
h_and_r_info_request_hash | ------- |   data    | hash of this HTLC request

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
h              | ------- |   result  | Hash of the preimage R
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Carol (destination) receieved request from Bob and agree.

*Carol replies*

**Message Type: -44**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
approval       | ------- |   data    | true or false
channel_address_private_key         | ------- |   data    | 
last_temp_address_private_key       | ------- |   data    | 
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key  | ------- |   data    | 
curr_htlc_temp_address_pub_key      | ------- |   data    | 
curr_htlc_temp_address_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
approval       | ------- |   result  | true or false
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Carol agree request, Bob create related commitment transactions between Bob and Carol.

*Bob create HTLC commitment transactions*

**Message Type: -45**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
channel_address_private_key         | ------- |   data    | 
last_temp_address_private_key       | ------- |   data    | 
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key  | ------- |   data    | 
curr_htlc_temp_address_pub_key      | ------- |   data    | 
curr_htlc_temp_address_private_key  | ------- |   data    | 
curr_htlc_temp_address_for_ht1a_pub_key      | ------- |   data    | 
curr_htlc_temp_address_for_ht1a_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
h              | ------- |   result  | Hash of the preimage R
h_and_r_info_request_hash   | ------- |   result  | hash of this HTLC request


## Carol Send R to Bob through the Path

<!-- right -->

> Carol's temp address data for create HTLC commitment transactions:

```shell
Carol HTLC HEnb commitment transaction:

address  mt3esQqTd8udMNQK8Vm8EDka2N3uCdquCa
privkey cR14XVjQ4yXunTnpqXZ1FMangq5bZNqsQ4gnsVpJ1KAMkxZVqo3F
pubkey 020eaa8f0c0f2761215af43dd7fccb11df8cafffcff4e8f186bd1b8a8a11e5f680
```

> Carol (destination) Send R (Preimage_R) to Bob (middleman):

```json
{
	"type": -46,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
        "r":"d77d7df74ca33a672802387fffc6575a09dc7c45",
		"channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",
		"curr_htlc_temp_address_private_key":"cVW35aEjd56ZFHTSDtq9iUzHAfH6rWGVJcmYBn9QEp9ygSfkgZeG",		
        "curr_htlc_temp_address_for_he1b_pub_key":"020eaa8f0c0f2761215af43dd7fccb11df8cafffcff4e8f186bd1b8a8a11e5f680",
		"curr_htlc_temp_address_for_he1b_private_key":"cR14XVjQ4yXunTnpqXZ1FMangq5bZNqsQ4gnsVpJ1KAMkxZVqo3F"
	}
}
```

> OBD Responses:

```json
{
    "type": -46, 
    "status": true, 
    "from": "carol", 
    "to": "bob", 
    "result": {
        "id": 4, 
        "r": "2de142c8006a3462241e96a610b59f3d92d8259c", 
        "request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

> Bob replies and create rest HTLC commitment transactions:

```json
{
	"type": -47,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"r":"d77d7df74ca33a672802387fffc6575a09dc7c45",
		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"curr_htlc_temp_address_private_key":"cNon6RZ9uLq6EPpGYt8tDZjQLMWVZDAXcrFy3LH1ZmHQJDbKWnye"
	}
}

```

> OBD Responses:

```json
{
    "type": -47, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "r": "2de142c8006a3462241e96a610b59f3d92d8259c", 
        "request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

<!-- center -->

So, from previous step (Setp 2), Bob ask Carol if Carol can produce to Bob an unknown 20-byte random input data R from a known hash H, within two days, then Bob will settle the contract by paying assets.

Of course Carol has the preimage R, because she generated it. Then now, Carol send R to Bob.

<br/>

*Carol (destination) Send R (Preimage_R) to Bob (middleman)*

**Message Type: -46**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
r              | ------- |   data    | Preimage_R
channel_address_private_key         | ------- |   data    | 
curr_htlc_temp_address_private_key      | ------- |   data    | 
curr_htlc_temp_address_for_he1b_pub_key  | ------- |   data    | 
curr_htlc_temp_address_for_he1b_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
id             | ------- |   result  | 
r              | ------- |   result  | Preimage_R
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Bob receieved the R, and check out if R is correct.
If correct, then create rest HTLC commitment transactions.

*Bob replies and create rest HTLC commitment transactions.*

**Message Type: -47**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
r              | ------- |   data    | Preimage_R
channel_address_private_key       | ------- |   data    | 
curr_htlc_temp_address_private_key      | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
r              | ------- |   result  | Preimage_R
request_hash   | ------- |   result  | hash of this HTLC request


## Bob Send R to Alice through the Path

<!-- right -->

> Bob's temp address data for create rest HTLC commitment transactions:

```shell
Bob HTLC HEnb commitment transaction:

address  mhDr57jhEWeYg2eYQf7LYHoxZ8ZgXEunaT
privkey cTeQ2e9Hw6y1RHjCCF9x3MR7pn3yPySgxSYy5rtvmVvM7ZNh9jUZ
pubkey 03ebdfc067f822e9ae0d76759c422bfd3aee342e21ca716dc16b81b335da73d69e
```

> Bob (middleman) Send R (Preimage_R) to Alice (launcher):

```json
{
	"type": -46,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
        "r":"d77d7df74ca33a672802387fffc6575a09dc7c45",
		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"curr_htlc_temp_address_private_key":"cPRy5pB8Ek2DabfQ74x8giqDdPtwTvptgRnq8qEP7KduzdmMFmJM",		
        "curr_htlc_temp_address_for_he1b_pub_key":"03ebdfc067f822e9ae0d76759c422bfd3aee342e21ca716dc16b81b335da73d69e",
		"curr_htlc_temp_address_for_he1b_private_key":"cTeQ2e9Hw6y1RHjCCF9x3MR7pn3yPySgxSYy5rtvmVvM7ZNh9jUZ"
	}
}
```

> OBD Responses:

```json
{
    "type": -46, 
    "status": true, 
    "from": "bob", 
    "to": "alice", 
    "result": {
        "id": 4, 
        "r": "2de142c8006a3462241e96a610b59f3d92d8259c", 
        "request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

> Alice replies and create rest HTLC commitment transactions:

```json
{
	"type": -47,
	"data": {
		"request_hash":"742db9677d53316b8faef7c9f40766e4f39dd6b82487c103960e9170de8ce636",
		"r":"d77d7df74ca33a672802387fffc6575a09dc7c45",
		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
		"curr_htlc_temp_address_private_key":"cR7wXNwPjMrDCpnJoinTiMK384YyKNTfyctLQ2CCdQobdanEqgAs"
	}
}
```

> OBD Responses:

```json
{
    "type": -47, 
    "status": true, 
    "from": "alice", 
    "to": "bob", 
    "result": {
        "r": "2de142c8006a3462241e96a610b59f3d92d8259c", 
        "request_hash": "1fe82bc9152741670c4ee2b4853df9346c1cc63fce6d1c896e7eeca8cc62c9d9"
    }
}
```

<!-- center -->

Bob send R to Alice, and Alice will create rest HTLC commitment transactions to pay assets to Bob.

*Bob (middleman) Send R (Preimage_R) to Alice (launcher)*

**Message Type: -46**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
r              | ------- |   data    | Preimage_R
channel_address_private_key         | ------- |   data    | 
curr_htlc_temp_address_private_key      | ------- |   data    | 
curr_htlc_temp_address_for_he1b_pub_key  | ------- |   data    | 
curr_htlc_temp_address_for_he1b_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
id             | ------- |   result  | 
r              | ------- |   result  | Preimage_R
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Alice receieved the R, and check out if R is correct.
If correct, then create rest HTLC commitment transactions.

*Alice replies and create rest HTLC commitment transactions.*

**Message Type: -47**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_hash   | ------- |   data    | hash of this HTLC request
r              | ------- |   data    | Preimage_R
channel_address_private_key       | ------- |   data    | 
curr_htlc_temp_address_private_key      | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
r              | ------- |   result  | Preimage_R
request_hash   | ------- |   result  | hash of this HTLC request


## Close HTLC

<!-- right -->

> Alice's temp address data for create commitment transactions:

```shell
Alice RSMC 
address  mkPtXTRyA53ddhknMnVqNCDdeN2FsXmtwe
privkey cTiDwaM3y5LE2HuWWgvRTC9mgHiovf2zntjSgCPyLLeuUTmKk1BY
pubkey 02fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed8
```

> Alice launch a request to close HTLC channel A2B:

```json
{
	"type": -48,
	"data": {
        "channel_id":[174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
		"last_rsmc_temp_address_private_key":"cSYJ3vwcgMPDqegXFJ2YgCYNNgKS9tNxCaRkZnn3ourQSdGNkJCk",
		"last_htlc_temp_address_private_key":"cR7wXNwPjMrDCpnJoinTiMK384YyKNTfyctLQ2CCdQobdanEqgAs",
		"last_htlc_temp_address_for_htnx_private_key":"cNzNyejXtgC4ySXCVXqa6egVinYLDtGhkRkGd271ZW6AJrVKYZ2w",
		"curr_rsmc_temp_address_pub_key":"02fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed8",
		"curr_rsmc_temp_address_private_key":"cTiDwaM3y5LE2HuWWgvRTC9mgHiovf2zntjSgCPyLLeuUTmKk1BY"
	}
}
```

> OBD Responses:

```json
{
    "type": -48, 
    "status": true, 
    "from": "alice", 
    "to": "bob", 
    "result": {
        "channel_id": [174,36,154,103,145,76,58,237,32,61,201,81,17,156,135,216,66,28,83,203,251,152,138,102,158,113,131,32,241,229,43,75],
        "create_at": "2019-11-06T08:45:40.4120228+08:00", 
        "create_by": "alice", 
        "curr_rsmc_temp_address_pub_key": "02fed65567b2ab00e2cbb28b46a687ce8fd0894486989cba54975b45bbc6a85ed8", 
        "curr_state": 10, 
        "id": 5, 
        "request_hash": "fa6cdcd0974eeabbeffca3d70d0a66cd7549b002de7cd56eee1c7e60b94dc0be"
    }
}
```

> Bob's temp address data for create commitment transactions:

```shell
Bob RSMC 
address  n2wKQgrfM5fFXmmA6xNWjWPPFPktJHnqEj
privkey cU78aif2a4YR5xK8HxBTrPKjdjhD8W4SSZNTw4yFEdwi59JMrYQY
pubkey 0298bdca47bbb76b1022eb7d18534961a12ce6dd80308c839576602b771e324fba
```

> Bob replies and create BR & a newer commitment transactions:

```json
{
	"type": -49,
	"data": {
		"request_close_htlc_hash":"fa6cdcd0974eeabbeffca3d70d0a66cd7549b002de7cd56eee1c7e60b94dc0be",
		"channel_address_private_key":"cToieuvo3JjkEUKa3tjd6J98RXKDTo1d2hUSVgKpZ1KwBvGhQFL8",
		"last_rsmc_temp_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"last_htlc_temp_address_private_key":"cPRy5pB8Ek2DabfQ74x8giqDdPtwTvptgRnq8qEP7KduzdmMFmJM",
		"last_htlc_temp_address_for_htnx_private_key":"cTeQ2e9Hw6y1RHjCCF9x3MR7pn3yPySgxSYy5rtvmVvM7ZNh9jUZ",
		"curr_rsmc_temp_address_pub_key":"0298bdca47bbb76b1022eb7d18534961a12ce6dd80308c839576602b771e324fba",
		"curr_rsmc_temp_address_private_key":"cU78aif2a4YR5xK8HxBTrPKjdjhD8W4SSZNTw4yFEdwi59JMrYQY"
	}
}
```

> OBD Responses:

```json
{
    "type": -49, 
    "status": true, 
    "from": "bob", 
    "to": "bob", 
    "result": {
        "msg": "close htlc success"
    }
}
```

> Carol's temp address data for create commitment transactions:

```shell
Carol RSMC 
address  mrWGmCyzEQxKWmBQoGmKDSH9Avo7yb6Vz6
privkey cNDBq3ZKKQEduVyygfcQRzxbhTS3Gt2zz6VEizkp6WyRXn8RdBtH
pubkey 03080445b531e1df053ce9f1e3d01cdf679f693b23a991ce74145cb0b2e29a2b2d
```

> Carol launch a request to close HTLC channel B2C:

```json
{
	"type": -48,
	"data": {
        "channel_id":[223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51],
		"channel_address_private_key":"cMxR8h9z5oKrdyuXVR9uzBbyyaJz1karxH1FW5xezhKzxQc7sCJV",
		"last_rsmc_temp_address_private_key":"cTTFKJ3N8W4qHcpwj19NVVaEYAXBxf8DmBj9g7owLTWQ3mXXfD51",
		"last_htlc_temp_address_private_key":"cVW35aEjd56ZFHTSDtq9iUzHAfH6rWGVJcmYBn9QEp9ygSfkgZeG",
		"last_htlc_temp_address_for_htnx_private_key":"cR14XVjQ4yXunTnpqXZ1FMangq5bZNqsQ4gnsVpJ1KAMkxZVqo3F",
		"curr_rsmc_temp_address_pub_key":"03080445b531e1df053ce9f1e3d01cdf679f693b23a991ce74145cb0b2e29a2b2d",
		"curr_rsmc_temp_address_private_key":"cNDBq3ZKKQEduVyygfcQRzxbhTS3Gt2zz6VEizkp6WyRXn8RdBtH"
	}
}
```

> OBD Responses:

```json
{
    "type": -48, 
    "status": true, 
    "from": "carol", 
    "to": "carol", 
    "result": {
        "channel_id": [223,177,75,185,186,22,47,155,145,238,242,1,158,247,192,1,48,183,197,192,190,72,49,233,62,65,156,103,111,172,109,51],
        "create_at": "2019-11-06T08:49:19.3717723+08:00", 
        "create_by": "carol", 
        "curr_rsmc_temp_address_pub_key": "03080445b531e1df053ce9f1e3d01cdf679f693b23a991ce74145cb0b2e29a2b2d", 
        "curr_state": 10, 
        "id": 7, 
        "request_hash": "9491ad7b9ff1003d6404b6f60845dfb0423d6233f42a3dbe118650c6c0e10232"
    }
}
```

> Bob's temp address data for create commitment transactions:

```shell
Bob RSMC 
address  mimQmQxqBVSCbUjVEir5d9Fi9ij9jqPEdP
privkey cRNxX8S287DA1hkMZHVwnQiMdQVwBBdqpaGYDP1wrRdzT7pSm5kU
pubkey 02a08635fb1c664aa2bc1a87e76f8dc0b3170c0d45d0f899b3f192093afa1bcd8c
```

> Bob replies and create BR & a newer commitment transactions:

```json
{
	"type": -49,
	"data": {
		"request_close_htlc_hash":"9491ad7b9ff1003d6404b6f60845dfb0423d6233f42a3dbe118650c6c0e10232",
		"channel_address_private_key":"cTr7SHQ7FR6Ej8ZU8vDpbt3y9GuF3hnBqn81Tv9Tdi5TeakqKavt",
		"last_rsmc_temp_address_private_key":"cTMZ3csaDWvods2jnMkNJZpz98DRWmFQXb4C1vDFTibcST5g3SNb",
		"last_htlc_temp_address_private_key":"cTeQ2e9Hw6y1RHjCCF9x3MR7pn3yPySgxSYy5rtvmVvM7ZNh9jUZ",
		"last_htlc_temp_address_for_htnx_private_key":"cUoDGr5cdNarcv43YXFdXBY2zf9721y6u6MiDnk56TSJWCGKvTbL",
		"curr_rsmc_temp_address_pub_key":"02a08635fb1c664aa2bc1a87e76f8dc0b3170c0d45d0f899b3f192093afa1bcd8c",
		"curr_rsmc_temp_address_private_key":"cRNxX8S287DA1hkMZHVwnQiMdQVwBBdqpaGYDP1wrRdzT7pSm5kU"
	}
}
```

> OBD Responses:

```json
{
    "type": -49, 
    "status": true, 
    "from": "bob", 
    "to": "carol", 
    "result": {
        "msg": "close htlc success"
    }
}
```

<!-- center -->

For continue using OBD to transfer assets OR some reasons
Alice need to close this HTLC channel between she and Bob.
Then Alice launch the closing request.

*Alice (launcher) request to close HTLC of A2B channel*

**Message Type: -48**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id   | ------- |   data    | Id of a channel
channel_address_private_key              | ------- |   data    | 
last_rsmc_temp_address_private_key       | ------- |   data    | 
last_htlc_temp_address_private_key      | ------- |   data    | 
last_htlc_temp_address_for_htnx_private_key      | ------- |   data    | 
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key      | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
channel_id              | ------- |   result  | Id of a channel
create_at   | ------- |   result  | time of created
create_by   | ------- |   result  | creator
curr_rsmc_temp_address_pub_key   | ------- |   result  | 
curr_state   | ------- |   result  | 
id   | ------- |   result  | 
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Bob agree the closing request, and create BR (Breach Remedy)
& a newer commitment transactions (known as Cxa or Cxb).

*Bob replies*

**Message Type: -49**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_close_htlc_hash   | ------- |   data    | hash of this closing request
channel_address_private_key              | ------- |   data    | 
last_rsmc_temp_address_private_key       | ------- |   data    | 
last_htlc_temp_address_private_key      | ------- |   data    | 
last_htlc_temp_address_for_htnx_private_key      | ------- |   data  |
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
msg            | ------- |   result  | success or failed

<br/>

For continue using OBD to transfer assets OR some reasons
Carol need to close this HTLC channel between she and Bob.
Then Carol launch the closing request.

*Carol (destination) request to close HTLC of B2C channel*

**Message Type: -48**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id   | ------- |   data    | Id of a channel
channel_address_private_key              | ------- |   data    | 
last_rsmc_temp_address_private_key       | ------- |   data    | 
last_htlc_temp_address_private_key      | ------- |   data    | 
last_htlc_temp_address_for_htnx_private_key      | ------- |   data    | 
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key      | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
channel_id              | ------- |   result  | Id of a channel
create_at   | ------- |   result  | time of created
create_by   | ------- |   result  | creator
curr_rsmc_temp_address_pub_key   | ------- |   result  | 
curr_state   | ------- |   result  | 
id   | ------- |   result  | 
request_hash   | ------- |   result  | hash of this HTLC request

<br/>

Bob agree the closing request, and create BR (Breach Remedy)
& a newer commitment transactions (known as Cxa or Cxb).

*Bob replies*

**Message Type: -49**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
request_close_htlc_hash   | ------- |   data    | hash of this closing request
channel_address_private_key              | ------- |   data    | 
last_rsmc_temp_address_private_key       | ------- |   data    | 
last_htlc_temp_address_private_key      | ------- |   data    | 
last_htlc_temp_address_for_htnx_private_key      | ------- |   data  |
curr_rsmc_temp_address_pub_key      | ------- |   data    | 
curr_rsmc_temp_address_private_key  | ------- |   data    | 

<br/>

**OBD Responses:**

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status         | ------- |   body    | true or false
from	       | ------- |   body    | sender
to             | ------- |   body    | receiever
msg            | ------- |   result  | success or failed

<!-- 十二、关闭HTLC状态下的通道 -->