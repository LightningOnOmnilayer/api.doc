# OBD gRPC API Reference
 
# HTLC

## AddInvoice

### Unary RPC

 AddInvoice attempts to add a new invoice to the invoice database. Any duplicated invoices are rejected, therefore all invoices *must* have a unique payment preimage.

```shell
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

```javascript
const fs = require('fs');
const grpc = require('grpc');
const protoLoader = require('@grpc/proto-loader');
const loaderOptions = {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
};
const packageDefinition = protoLoader.loadSync('rpc.proto', loaderOptions);
const lnrpc = grpc.loadPackageDefinition(packageDefinition).lnrpc;
const macaroon = fs.readFileSync("LND_DIR/data/chain/bitcoin/simnet/admin.macaroon").toString('hex');
process.env.GRPC_SSL_CIPHER_SUITES = 'HIGH+ECDSA';
const lndCert = fs.readFileSync('LND_DIR/tls.cert');
const sslCreds = grpc.credentials.createSsl(lndCert);
const macaroonCreds = grpc.credentials.createFromMetadataGenerator(function(args, callback) {
  let metadata = new grpc.Metadata();
  metadata.add('macaroon', macaroon);
  callback(null, metadata);
});
let creds = grpc.credentials.combineChannelCredentials(sslCreds, macaroonCreds);
let lightning = new lnrpc.Lightning('localhost:10009', creds);
let request = { 
  memo: <string>, 
  r_preimage: <bytes>, 
  r_hash: <bytes>, 
  value: <int64>, 
  value_msat: <int64>, 
  settled: <bool>, 
  creation_date: <int64>, 
  settle_date: <int64>, 
  payment_request: <string>, 
  description_hash: <bytes>, 
  expiry: <int64>, 
  fallback_addr: <string>, 
  cltv_expiry: <uint64>, 
  route_hints: <array RouteHint>, 
  private: <bool>, 
  add_index: <uint64>, 
  settle_index: <uint64>, 
  amt_paid: <int64>, 
  amt_paid_sat: <int64>, 
  amt_paid_msat: <int64>, 
  state: <InvoiceState>, 
  htlcs: <array InvoiceHTLC>, 
  features: <array FeaturesEntry>, 
  is_keysend: <bool>, 
  payment_addr: <bytes>, 
}; 
lightning.addInvoice(request, function(err, response) {
  console.log(response);
});
// Console output:
//  { 
//      "r_hash": <bytes>,
//      "payment_request": <string>,
//      "add_index": <uint64>,
//      "payment_addr": <bytes>,
//  }
```

### gRPC Request: [Invoice]()

Parameter | Type | Description
--------- | ---- | ----------- 
property_id | int64  | assets id
value       | double | amount of transfer
memo        | string | 
cltv_expiry | string | expiry time.
private     | bool   | true to private channel or false to open channel.

### gRPC Response: [AddInvoiceResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 
payment_request | string | 


## ParseInvoice

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


## SendPayment

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



# Lightning

## ConnectPeer
## DisconnectPeer
## OpenChannel
## FundChannel
