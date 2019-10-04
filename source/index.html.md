---
title: OmniBOLT API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - golang

toc_footers:
  - <a href='https://github.com/LightningOnOmnilayer/LightningOnOmni'>Join our dev community</a>
  - <a href='https://github.com/LightningOnOmnilayer/Omni-BOLT-spec'>Help in OmniBOLT specification</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# OmniBOLT Daemon Websocket Messages Reference

Welcome to the Websocket API reference documentation for OmniBOLT Daemon (OBD), the daemon that communicates with light clients written in any program languages that support websocket connection, such as javascript, golang, shell or even C/C++. OBD runs as an independent process connecting to a full node of OmniCore(version 0.18), which provides the complete services of token transactions on bitcoin network. And current OBD implementation is deeply binded to OmniCore. 

We assume you have already gone through our [installation instruction](https://github.com/LightningOnOmnilayer/LightningOnOmni#installation), so that you are familar with:

 * pull source code and compile to get obdserver;  
 * Configure OmniCore node for OBD;  
 * Connect OBD with Omnicore correctly;  
 * Use a simple web based [websocket client](https://chrome.google.com/webstore/detail/websocket-test-client/fgponpodhbmadfljofbimhhlengambbn?hl=en) for Chrome to do experiments;  
 * OBD responses you correctly;  

For kernel developers in our community, the above steps are the easiest way to get started. Just open [LightningOnOmni](https://github.com/LightningOnOmnilayer/LightningOnOmni#step-4-test-channel-operations-using-websocket-testing-tool) project with your favorit golang editor, run OBD in debug mode, setup break points, and send messages from your websocket-test-client. You may intercept the messages and track message flows of OBD kernel.


# Wallet Preparation

> To test and debug the kernel, use OmniCore in REGTEST mode, and creat two users Alice and Bob:


```shell
# With shell, you can creat your addresses using omnicore-cli
Alice: mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs  
"pubkey": "021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d"  
"privatekey": "cUAdadTkjeVFsNz5ifhkETfAzk5PvhnLWtmdSKgbyTTjSCE4MYWy"  

Bob: myHRPQWTQ1yYbj7vr7raBW59CTeAFEsUXY  
"pubkey": "03efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce9"  
"privatekey":   cV6dif91LHD8Czk8BTgvYZR3ipUrqyMDMtUXSWsThqpHaQJUuHKA  

channel:  
(P2WHK)address: 2N1DFjaE4yCcECdFwgLQcLmNrLV5zetgQtE  
"channel_address_redeem_script":"5221021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d2103efd8923f1829ece87202892d31cd75c20b7a7b5adf888f7ba04fa2c1bc931ce952ae"  
"channel_address_script_pub_key":"a9145761a1d45b8a6e7caa10a4bcecca97630c67af4687"  

```

```javascript
To be added
```

> Make sure to replace pubkey and privatekey of `Alice/Bob` with values you create in your environment.

Use omnicore-cli to creat new users in REGTEST mode, and deposit tokens to their addresses respectively. Don't develop in real BTC network. A good start is [Use the raw transaction API to create a Simple Send transaction](https://github.com/OmniLayer/omnicore/wiki/Use-the-raw-transaction-API-to-create-a-Simple-Send-transaction)  

<aside class="notice">
After you created your users and their addresses, you shall replace <code>Alice</code> and <code>Bob</code> with your data.
</aside>

# Balance and Unspent

## get all balances for address

<!-- right -->
> Here we use the example data created before. Alice: 

```shell
./omnicore-cli omni_getallbalancesforaddress mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs  

``` 
> The above command returns JSON structured like this:

```json
{  
    "propertyid": 121,  
    "name": "OST-P1-Test",  
    "balance": "15.00000000",  
    "reserved": "0.00000000",  
    "frozen": "0.00000000"  
}  
```

> Bob: 

```shell
./omnicore-cli omni_getallbalancesforaddress myHRPQWTQ1yYbj7vr7raBW59CTeAFEsUXY  

``` 
> Response:

```json
{  
    "propertyid": 121,
    "name": "OST-P1-Test",
    "balance": "20.00000000",
    "reserved": "0.00000000",
    "frozen": "0.00000000"
}  
```

<!-- middle -->
`./omnicore-cli omni_getallbalancesforaddress <omni address>`

Parameter | Default | Description
--------- | ------- | -----------
omni address | N/A | Address created by omnicore-cli, same to the bitcoin core addresss.
 

## list unspent 

<!-- right -->
> Alice:

```shell
./omnicore-cli listunspent 0 999999  '["mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs"]'
```

> Response:

```json
{
    "txid": "b57afc6164f68f2c3b79d31afb30bd1750b22798ec6cfad0131a3062c2317ec7",
    "vout": 0,
    "address": "mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
    "account": "",
    "scriptPubKey": "76a914b5770ba3d6d34f5fdd8d582f81fb383975bb9c6d88ac",
    "amount": 0.01000000,
    "confirmations": 1,
    "spendable": true,
    "solvable": true
}
```

> Bob:

```shell
./omnicore-cli listunspent 0 999999  '["myHRPQWTQ1yYbj7vr7raBW59CTeAFEsUXY"]'
```

> Response:

```json
{
    "txid": "4a018fca34428ba622c9d105faba954e045dd977b61e52269b05d1cc5b8fc9f9",
    "vout": 0,
    "address": "myHRPQWTQ1yYbj7vr7raBW59CTeAFEsUXY",
    "account": "",
    "scriptPubKey": "76a914c2e30e5f058df787de0529e0742d7ef7a13231ab88ac",
    "amount": 0.00070467,
    "confirmations": 383,
    "spendable": true,
    "solvable": true
}
```

<!-- middle -->
`./omnicore-cli listunspent 0 999999  '["<omni address>"]'`

Parameter | Default | Description
--------- | ------- | -----------
omni address | N/A | Address created by omnicore-cli, same to the bitcoin core addresss.

<aside class="success">
Now, we have our environment ready. Let's start to communicate with OBD!
</aside>

# User Login

<!-- right -->
> Alice:

```shell
{
	"type":1,
	"data":{
        	"peer_id":"alice"
        	}
}
```
> Response:

```json
{
	"type":1,
	"status":true,
	"from":"alice",
	"to":"all",
	"result":"alice login"
}
```
> Bob:

```shell
{
	"type":1,
	"data":{
        	"peer_id":"bob"
        	}
}
```
> Response:

```json
{
	"type":1,
	"status":true,
	"from":"bob",
	"to":"all",
	"result":"bob login"
}
```


<!-- middle -->
This endpoint manages users createb by an OBD instance. Here we use Alice and Bob for testing purpose. The complete hirarchecal deterministic wallet system will be integrated soon, functions being including but not limited to: generat user mnemonic words, public/private key paires, PIN code, restore account.

**Websocket Request:**
 
{  
	"type":1,  
	"data":{  
        	"peer_id":"<user ID>"  
        	}  
}  
 

**Websocket Response：**
 
{  
	"type":1,  
	"status":true or false,  
	"from":"<user ID>",  
	"to":"all",  
	"result":"response data"  
}  
 
Parameter | Description
--------- | -----------
user ID   | Global ID for a user in OBD network

<aside class="warning">This is not ready for production environment.</aside>

 
# create channel

## request to create

> Alice requests:

```shell
{
    	"type":-32,
    		"data":{
		"funding_pubkey":"021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d"
    		},
	"recipient_peer_id":"bob"
}
```
> OBD Response:

```json
{
	"type":-32,
	"status":true,
	"from":"alice",
	"to":"bob",
	"result":
	{
		"chain_hash":"1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P",
		"channel_reserve_satoshis":0,
		"delayed_payment_base_point":"",
		"dust_limit_satoshis":0,
		"fee_rate_per_kw":0,
		"funding_address":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
		"funding_pubkey":"021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d",
		"funding_satoshis":0,
		"htlc_base_point":"",
		"htlc_minimum_msat":0,
		"max_accepted_htlcs":0,
		"max_htlc_value_in_flight_msat":0,
		"payment_base_point":"",
		"push_msat":0,
		"revocation_base_point":"",
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],"to_self_delay":0
	}
}
```

<!-- middle -->
Alice sends request to Bob for creating a channel between them

### Websocket Request
```
{
    	"type":-32,
    		"data":{
		"funding_pubkey":"021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d"
    		},
	"recipient_peer_id":"bob"
}
```

### Websocket Response：
```
{
	"type":-32,
	"status":true,
	"from":"alice",
	"to":"bob",
	"result":
	{
		"chain_hash":"1EXoDusjGwvnjZUyKkxZ4UHEf77z6A5S4P",
		"channel_reserve_satoshis":0,
		"delayed_payment_base_point":"",
		"dust_limit_satoshis":0,
		"fee_rate_per_kw":0,
		"funding_address":"mx4TDCXP2DedxcuA8RXaQ6c4q2GKAimUPs",
		"funding_pubkey":"021d475729c52f86df24b36aa231945bd090f9c23ccbfb91e4ade6813b2419d32d",
		"funding_satoshis":0,
		"htlc_base_point":"",
		"htlc_minimum_msat":0,
		"max_accepted_htlcs":0,
		"max_htlc_value_in_flight_msat":0,
		"payment_base_point":"",
		"push_msat":0,
		"revocation_base_point":"",
		"temporary_channel_id":[43,207,125,166,133,84,214,91,184,177,149,10,111,209,133,201,147,178,48,245,6,18,162,239,207,45,105,158,251,200,138,183],"to_self_delay":0
	}
}
```

## Websocket Request 
```
{
	"type":1,
	"data":{
        	"peer_id":"<user ID>"
        	}
}
```

## Websocket Response：
```
{
	"type":1,
	"status":true or false,
	"from":"<user ID>",
	"to":"all",
	"result":"response data"
}
```
Parameter | Description
--------- | -----------
user ID   | Global ID for a user in OBD network

<aside class="warning">This is not ready for production environment.</aside>


 

