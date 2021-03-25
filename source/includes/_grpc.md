# OBD gRPC API Reference

This is the gRPC API reference documentation for omnibolt daemon. 

The grpc API is offered when your obd runs in exclusive mode, which is that only you are able to connect the obd instance. This is the mode same to lnd. We suggest you to visit the link below for obd architecture:

https://omnilaboratory.github.io/obd/#/Architecture?id=exclusive-mode

In exclusive mode, connection type is http protocol between client and obd currently.

The proto files are here:

https://github.com/omnilaboratory/obd/tree/master/proxy/pb

Code examples invoking obd API use `grpcurl` command line tool：

Github repo is here: https://github.com/fullstorydev/grpcurl/


# HTLC

## AddInvoice

### Unary RPC

AddInvoice attempts to add a new invoice to the invoice database. 

```shell

$ grpcurl -plaintext -d '{"cltv_expiry":<string>, "value":<double>, "property_id":<int64>, "private":<bool>}' localhost:50051 proxy.Htlc/AddInvoice
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

### gRPC Request: [Invoice]

Parameter | Type | Description
--------- | ---- | ----------- 
property_id | int64  | assets id
value       | double | amount of transfer
memo        | string | 
cltv_expiry | string | expiry time.
private     | bool   | true to private channel or false to open channel.

### gRPC Response: [AddInvoiceResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
payment_request | string | 


## ListInvoices

### Unary RPC

ListInvoices returns a list of all the invoices currently stored within the database. Any active debug invoices are ignored. It has full support for paginated responses, allowing users to query for specific invoices through their add_index. This can be done by using either the first_index_offset or last_index_offset fields included in the response as the index_offset of the next request. By default, the first 100 invoices created will be returned. Backwards pagination is also supported through the Reversed flag.

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

### gRPC Request: [ListInvoiceRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
index_offset     | uint64 | The index of an invoice that will be used as either the start or end of a query to determine which invoices should be returned in the response.
num_max_invoices | uint64 | The max number of invoices to return in the response to this query.
reversed         | bool   | If set, the invoices returned will result from seeking backwards from the specified index offset. This can be used to paginate backwards.

### gRPC Response: [ListInvoiceResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
invoices           | Invoice | A list of invoices from the time slice of the time series specified in the request.
last_index_offset  | uint64 | The index of the last item in the set of returned invoices. This can be used to seek further, pagination style.
first_index_offset | uint64 | The index of the last item in the set of returned invoices. This can be used to seek backwards, pagination style.


## ParseInvoice

### Unary RPC

ParseInvoice decodes the `payment_request` and get out all of details from an invoice.

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

### gRPC Request: [ParseInvoiceRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
payment_request | string | 

### gRPC Response: [ParseInvoiceResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
property_id | int64  | assets id
value       | double | amount of transfer
memo        | string | 
cltv_expiry | string | expiry time.
private     | bool   | true to private channel or false to open channel.
h           | string | preimage
recipient_node_peer_id | string | 
recipient_user_peer_id | string | 


## SendPayment

### Unary RPC

SendPayment attempts to route a payment described by the passed PaymentRequest to the final destination. The call returns payment data.

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

### gRPC Request: [SendRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
payment_request | string  | 
invoice_detail  | ParseInvoiceResponse | 

### gRPC Response: [SendResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
payment_hash | string | 
payment_preimage | string | 
amount_to_rsmc | double | 
amount_to_htlc | double | 
amount_to_counterparty | double | 


# Lightning

## ChannelBalance

### Unary RPC

ChannelBalance returns a report on the total funds across all open channels, categorized in local/remote, pending local/remote and unsettled local/remote balances.

```shell

$ grpcurl -plaintext localhost:50051 proxy.Lightning/ChannelBalance
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
}; 
lightning.ChannelBalance(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ChannelBalanceRequest]

This request has no parameters.

### gRPC Response: [ChannelBalanceResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
local_balance  | double  | Sum of channels local balances.
remote_balance  | double  | Sum of channels remote balances.
unsettled_local_balance  | double  | Sum of channels local unsettled balances.
unsettled_remote_balance  | double  | Sum of channels remote unsettled balances.
pending_open_local_balance  | double  | Sum of channels pending local balances.
pending_open_remote_balance  | double  | Sum of channels pending remote balances.


## ConnectPeer

### Unary RPC

ConnectPeer attempts to establish a connection to a remote peer. This is at the networking level, and is used for communication between nodes. This is distinct from establishing a channel with a peer.

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

### gRPC Request: [ConnectPeerRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | address of the peer

### gRPC Response: [ConnectPeerResponse]

This response has no parameters. 


## DisconnectPeer

### Unary RPC

DisconnectPeer attempts to disconnect one peer from another identified. In the case that we currently have a pending or active channel with the target peer, then this action will be not be allowed.

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
addr | string  | address of the peer

### gRPC Response: [DisconnectPeerResponse]()

This response has no parameters.


## FundChannel

### Unary RPC

FundChannel attempts to fund some bitcoin for miner fee and omni assets into a channel.

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

### gRPC Request: [FundChannelRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
template_channel_id | string  | 
btc_amount | double  | bitcoin for miner fee
property_id | int64  | ID of an omni asset
asset_amount | double  | 
recipientInfo | RecipientNodeInfo  | 

### gRPC Response: [FundChannelResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 


## GetTransactions

### Unary RPC

GetTransactions returns a list describing all the known transactions relevant to a specified channel.

```shell

$ grpcurl -plaintext -d '{"channel_id":<string>, "page_size":<int32>, "page_index":<int32>}' localhost:50051 proxy.Lightning/GetTransactions
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
  channel_id: <string>, 
  page_size: <int32>, 
  page_index: <int32>,
}; 
lightning.GetTransactions(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [GetTransactionsRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 
page_size  | int32  | 
page_index | int32  | 

### gRPC Response: [TransactionDetails]

Parameter | Type | Description
--------- | ---- | ----------- 
transactions | Transaction  | The list of transactions relevant to the specified channel.
total_count | int32  | 
page_size   | int32  | 
page_index  | int32  | 


## LatestTransaction

### Unary RPC

LatestTransaction returns the latest transaction from specified channel.

```shell

$ grpcurl -plaintext -d '{"channel_id":<string>}' localhost:50051 proxy.Lightning/LatestTransaction
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
  channel_id: <string>, 
}; 
lightning.LatestTransaction(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [LatestTransactionRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 

### gRPC Response: [Transaction]

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 
amount_a | double  | 
amount_b | double  | 
peer_a | string  | 
peer_b | string  | 
curr_state | int32  | 
tx_hash | string  | 
tx_type | int32  | 
h | string  | 
r | string  | 
amount_htlc | string  | 


## ListChannels

### Unary RPC

ListChannels returns a description of all the open channels that this node is a participant in.

```shell

$ grpcurl -plaintext -d '{"active_only":<bool>, "inactive_only":<bool>, "public_only":<bool>, "private_only":<bool>, "peer":<bytes>, "page_size":<int32>, "page_index":<int32>}' localhost:50051 proxy.Lightning/ListChannels
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
  active_only: <bool>, 
  inactive_only: <bool>, 
  public_only: <bool>, 
  private_only: <bool>, 
  peer: <bytes>, 
  page_size: <int32>, 
  page_index: <int32>, 
}; 
lightning.ListChannels(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ListChannelsRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
active_only | bool  | 
inactive_only | bool  | 
public_only | bool  | 
private_only | bool  | 
peer | bytes  | Filters the response for channels with a target peer's pubkey. If peer is empty, all channels will be returned.
page_size | int32  | 
page_index | int32  | 

### gRPC Response: [ListChannelsResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
channels | Channel  | The list of active channels


## OpenChannel

### Unary RPC

OpenChannel attempts to open a singly funded channel specified in the request to a remote peer.

```shell

$ grpcurl -plaintext -d '{"node_pubkey_string":<string>, "private":<bool>, "recipientInfo":<RecipientNodeInfo>}' localhost:50051 proxy.Lightning/OpenChannel
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
  private: <bool>, 
  recipientInfo: <RecipientNodeInfo>, 
}; 
lightning.OpenChannel(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [OpenChannelRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
node_pubkey_string | string  | The pubkey of the node to open a channel with.
private | bool  | true to private channel or false to open channel.
recipientInfo | RecipientNodeInfo  | 

### gRPC Response: [OpenChannelResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
template_channel_id | string  | 


## PendingChannels

### Unary RPC

PendingChannels returns a list of all the channels that are currently considered "pending". A channel is pending if it has finished the funding workflow and is waiting for confirmations for the funding txn, or is in the process of closure, either initiated cooperatively or non-cooperatively.

```shell

$ grpcurl -plaintext -d '{"page_size":<int32>, "page_index":<int32>}' localhost:50051 proxy.Lightning/PendingChannels
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
  page_size: <int32>, 
  page_index: <int32>, 
}; 
lightning.PendingChannels(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [PendingChannelsRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
page_size | int32  | 
page_index | int32  | 

### gRPC Response: [ListChannelsResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
channels | Channel  | The list of active channels



# Rsmc

## RsmcPayment

### Unary RPC

RsmcPayment attempts to send a payment with RSMC.

```shell

$ grpcurl -plaintext -d '{"channel_id":<string>, "amount":<double>, "recipientInfo":<RecipientNodeInfo>}' localhost:50051 proxy.Rsmc/RsmcPayment
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
const packageDefinition = protoLoader.loadSync('rsmc.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let rsmc = new obdrpc.Rsmc('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  channel_id: <string>, 
  amount: <double>, 
  recipientInfo: <RecipientNodeInfo>, 
}; 
rsmc.RsmcPayment(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [RsmcPaymentRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 
amount | double  | 
recipientInfo | RecipientNodeInfo  | 

### gRPC Response: [RsmcPaymentResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
channel_id | string  | 
amount_a | double  | 
amount_b | double  | 


# Wallet

## ChangePassword

### Unary RPC

ChangePassword change the password as known the `login_token` that used to login by administrator.

```shell

$ grpcurl -plaintext -d '{"current_password":<string>, "new_password":<string>}' localhost:50051 proxy.Wallet/ChangePassword
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  current_password: <string>, 
  new_password: <string>, 
}; 
wallet.ChangePassword(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ChangePasswordRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
current_password | string  | 
new_password | string  | 

### gRPC Response: [ChangePasswordResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
result | string  | 


## EstimateFee

### Unary RPC

EstimateFee asks the chain backend to estimate the fee rate and total fees for a transaction that pays to multiple specified outputs.

```shell

$ grpcurl -plaintext -d '{"conf_target":<int32>}' localhost:50051 proxy.Wallet/EstimateFee
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  conf_target: <int32>, 
}; 
wallet.EstimateFee(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [EstimateFeeRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
conf_target | int32  | The target number of blocks that this transaction should be confirmed by.

### gRPC Response: [EstimateFeeResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
sat_per_kw | int64  | The total fee in satoshis.


## GetInfo

### Unary RPC

GetInfo returns general information concerning the lightning node including it's identity pubkey, alias, the chains it is connected to, and information concerning the number of open+pending channels.

```shell

$ grpcurl -plaintext localhost:50051 proxy.Wallet/GetInfo
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
}; 
wallet.GetInfo(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [GetInfoRequest]

This request has no parameters.

### gRPC Response: [GetInfoResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
user_peerId | string  | 
node_peerId | string  | 
node_address | string  | 
htlc_fee_rate | double  | 
htlc_max_fee | double  | 
chain_node_type | string  | 
is_admin | bool  | 


## GenSeed

### Unary RPC

GenSeed is the first method that should be used to instantiate a new obd instance. This method allows a caller to generate a new aezeed cipher seed given an optional passphrase. 

```shell

$ grpcurl -plaintext -d '{"aezeed_passphrase":<bytes>, "seed_entropy":<bytes>}' localhost:50051 proxy.Wallet/GenSeed
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  aezeed_passphrase: <bytes>, 
  seed_entropy: <bytes>, 
}; 
wallet.GenSeed(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [GenSeedRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
aezeed_passphrase | bytes  | aezeed_passphrase is an optional user provided passphrase that will be used to encrypt the generated aezeed cipher seed.
seed_entropy | bytes  | seed_entropy is an optional 16-bytes generated via CSPRNG. If not specified, then a fresh set of randomness will be used to create the seed.

### gRPC Response: [GenSeedResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
cipher_seed_mnemonic | string  | cipher_seed_mnemonic is a 24-word mnemonic that encodes a prior aezeed cipher seed obtained by the user. This field is optional, as if not provided, then the daemon will generate a new cipher seed for the user. Otherwise, then the daemon will attempt to recover the wallet state linked to this cipher seed.
enciphered_seed | string  | enciphered_seed are the raw aezeed cipher seed bytes. This is the raw cipher text before run through our mnemonic encoding scheme.


## ListPeers

### Unary RPC

ListPeers returns a verbose listing of all currently active peers.

```shell

$ grpcurl -plaintext localhost:50051 proxy.Wallet/ListPeers
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
}; 
wallet.ListPeers(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [ListPeersRequest]

This request has no parameters.

### gRPC Response: [ListPeersResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
peers | Peer  | The list of currently connected peers


## Login

### Unary RPC

Login attempts to login to an obd instance with administrator role.

```shell

$ grpcurl -plaintext -d '{"mnemonic":<string>, "login_token":<string>}' localhost:50051 proxy.Wallet/Login
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
  mnemonic: <string>, 
  login_token: <string>, 
}; 
wallet.Login(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [LoginRequest]

Parameter | Type | Description
--------- | ---- | ----------- 
mnemonic | string  | mnemonic words
login_token | string  | as known password

### gRPC Response: [LoginResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
user_peerId | string  | 
node_peerId | string  | 
node_address | string  | 
htlc_fee_rate | double  | 
htlc_max_fee | double  | 
chain_node_type | string  | 


## Logout

### Unary RPC

Logout attempts to logout from an obd instance.

```shell

$ grpcurl -plaintext localhost:50051 proxy.Wallet/Logout
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
}; 
wallet.Logout(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [LogoutRequest]

This request has no parameters.

### gRPC Response: [LogoutResponse]

This response has no parameters.


## NextAddr

### Unary RPC

NextAddr returns the next unused address within the wallet.

```shell

$ grpcurl -plaintext localhost:50051 proxy.Wallet/NextAddr
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
}; 
wallet.NextAddr(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [AddrRequest]

This request has no parameters.

### gRPC Response: [AddrResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | The address encoded using a bech32 format.


## NewAddress

### Unary RPC

NewAddress creates a new address under control of the local wallet.

```shell

$ grpcurl -plaintext localhost:50051 proxy.Wallet/NewAddress
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
const packageDefinition = protoLoader.loadSync('wallet.proto', loaderOptions);
const obdrpc = grpc.loadPackageDefinition(packageDefinition).proxy;
let wallet = new obdrpc.Wallet('localhost:50051', grpc.credentials.createInsecure());
let request = { 
}; 
wallet.NewAddress(request, function(err, response) {
  console.log(response);
});

```

### gRPC Request: [NewAddressRequest]

This request has no parameters.

### gRPC Response: [NewAddressResponse]

Parameter | Type | Description
--------- | ---- | ----------- 
addr | string  | The address encoded using a bech32 format.

