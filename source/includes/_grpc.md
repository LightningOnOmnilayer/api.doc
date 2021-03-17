# OBD gRPC API Reference

For code examples in shell sector, invoke APIs by `grpcurl` command line tool.

Github repo is here: https://github.com/fullstorydev/grpcurl/


# HTLC

## AddInvoice

### Unary RPC

 AddInvoice attempts to add a new invoice to the invoice database. Any duplicated invoices are rejected, therefore all invoices *must* have a unique payment preimage.

```shell

$ grpcurl -plaintext -d '{"cltv_expiry":<string>, "value":<double>, "property_id":<int64>, "private":<bool>}' localhost:50051 proxy.Htlc/AddInvoice

# --> FOR LOGIN
$ grpcurl -plaintext -d '{"mnemonic":"opera muffin option float guess bracket arrest snake correct business captain brass", "login_token":"n4ek19lf"}' localhost:50051 proxy.Wallet/Login
```

```javascript

const grpc = require('grpc');
const protoLoader = require('@grpc/proto-loader');
const loaderOptions = {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
};
const packageDefinition = protoLoader.loadSync('htlc.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let htlc = new obdrpc.Htlc('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  property_id: <int64>, 
  value: <double>, 
  memo: <string>, 
  cltv_expiry: <string>, 
  private: <bool>, 
}; 
htlc.AddInvoice(request, function(err, response) {
  console.log(response);
});

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

### Unary RPC

 ParseInvoice decode the `payment_request` and get out all of details from an invoice.

```shell

$ grpcurl -plaintext -d '{"payment_request":<string>}' localhost:50051 proxy.Htlc/ParseInvoice

```

```javascript

const grpc = require('grpc');
const protoLoader = require('@grpc/proto-loader');
const loaderOptions = {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
};
const packageDefinition = protoLoader.loadSync('htlc.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let htlc = new obdrpc.Htlc('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  payment_request: <string>, 
}; 
htlc.ParseInvoice(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ParseInvoiceRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
payment_request | string | 

### gRPC Response: [ParseInvoiceResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 
property_id | int64  | assets id
value       | double | amount of transfer
memo        | string | 
cltv_expiry | string | expiry time.
private     | bool   | true to private channel or false to open channel.
h           | string | 
recipient_node_peer_id | string | 
recipient_user_peer_id | string | 


## ListInvoices

### Unary RPC

ListInvoices list all of invoices.

```shell

$ grpcurl -plaintext -d '{"index_offset":<uint64>, "num_max_invoices":<uint64>, "reversed":<bool>}' localhost:50051 proxy.Htlc/ListInvoices

```

```javascript

const grpc = require('grpc');
const protoLoader = require('@grpc/proto-loader');
const loaderOptions = {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
};
const packageDefinition = protoLoader.loadSync('htlc.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let htlc = new obdrpc.Htlc('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  index_offset: <uint64>, 
  num_max_invoices: <uint64>, 
  reversed: <bool>, 
}; 
htlc.ListInvoices(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ListInvoiceRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
index_offset     | uint64 | The index of an invoice that will be used as either the start or end of a query to determine which invoices should be returned in the response.
num_max_invoices | uint64 | The max number of invoices to return in the response to this query.
reversed         | bool   | If set, the invoices returned will result from seeking backwards from the specified index offset. This can be used to paginate backwards.

### gRPC Response: [ListInvoiceResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 
invoices           | Invoice | A list of invoices from the time slice of the time series specified in the request.
last_index_offset  | uint64 | The index of the last item in the set of returned invoices. This can be used to seek further, pagination style.
first_index_offset | uint64 | The index of the last item in the set of returned invoices. This can be used to seek backwards, pagination style.


## SendPayment

### Unary RPC

SendPayment send an invoice.

```shell

$ grpcurl -plaintext -d '{"payment_request":<string>, "invoice_detail":<ParseInvoiceResponse>}' localhost:50051 proxy.Htlc/SendPayment
```

```javascript

const grpc = require('grpc');
const protoLoader = require('@grpc/proto-loader');
const loaderOptions = {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
};
const packageDefinition = protoLoader.loadSync('htlc.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let htlc = new obdrpc.Htlc('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  payment_request: <string>, 
  invoice_detail: <ParseInvoiceResponse>, 
}; 
htlc.SendPayment(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [SendRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
payment_request | string  | 
invoice_detail  | ParseInvoiceResponse | 

### gRPC Response: [SendResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 
payment_hash | string | 
payment_preimage | string | 
amount_to_rsmc | double | 
amount_to_htlc | double | 
amount_to_counterparty | double | 


# Lightning

## ConnectPeer

### Unary RPC

ConnectPeer establish a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/ConnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.ConnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ConnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [ConnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 


## DisconnectPeer

### Unary RPC

DisconnectPeer disconnect a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/DisconnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.DisconnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [DisconnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [DisconnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 


## OpenChannel

### Unary RPC

OpenChannel launch a request to counterparty for create a new channel.

```shell

$ grpcurl -plaintext -d '{"node_pubkey_string":<string>, "node_pubkey_index":<int32>, "private":<bool>, "recipientInfo":<RecipientNodeInfo>}' localhost:50051 proxy.Lightning/OpenChannel
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  node_pubkey_string: <string>, 
  node_pubkey_index: <int32>, 
  private: <bool>, 
  recipientInfo: <RecipientNodeInfo>, 
}; 
lightning.OpenChannel(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [OpenChannelRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
node_pubkey_string | string  | 
node_pubkey_index | int32  | 
private | bool  | 
recipientInfo | RecipientNodeInfo  | 

### gRPC Response: [OpenChannelResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 
template_channel_id | string  | 


## FundChannel

### Unary RPC

FundChannel funding assets into a channel.

```shell

$ grpcurl -plaintext -d '{"template_channel_id":<string>, "btc_amount":<double>, "property_id":<int64>, "asset_amount":<double>, "recipientInfo":<RecipientNodeInfo>}' localhost:50051 proxy.Lightning/FundChannel
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  template_channel_id: <string>, 
  btc_amount: <double>, 
  property_id: <int64>, 
  asset_amount: <double>, 
  recipientInfo: <RecipientNodeInfo>, 
}; 
lightning.FundChannel(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [FundChannelRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
template_channel_id | string  | 
btc_amount | double  | 
property_id | int64  | 
asset_amount | double  | 
recipientInfo | RecipientNodeInfo  | 

### gRPC Response: [FundChannelResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 


## ListChannels

### Unary RPC

ConnectPeer establish a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/ConnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.ConnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ConnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [ConnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 

## PendingChannels

### Unary RPC

ConnectPeer establish a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/ConnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.ConnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ConnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [ConnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 

## LatestTransaction

### Unary RPC

ConnectPeer establish a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/ConnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.ConnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ConnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [ConnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 

## GetTransactions

### Unary RPC

ConnectPeer establish a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/ConnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.ConnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ConnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [ConnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 

## ChannelBalance

### Unary RPC

ConnectPeer establish a connection between two peers.

```shell

$ grpcurl -plaintext -d '{"addr":<string>}' localhost:50051 proxy.Lightning/ConnectPeer
```

```javascript

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
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let lightning = new obdrpc.Lightning('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  addr: <string>, 
}; 
lightning.ConnectPeer(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ConnectPeerRequest]()

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | 

### gRPC Response: [ConnectPeerResponse]()

Parameter | Type | Description
--------- | ---- | ----------- 


