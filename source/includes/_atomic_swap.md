# Contracts

## atomicSwap

### Simple Type -80 Protocol

Type -80 Protocol notifies the counterparty an atomic swap is created. The background and process of atomic swap can be [found here in chapter 5](https://github.com/omnilaboratory/OmniBOLT-spec/blob/master/OmniBOLT-05-Atomic-Swap-among-Channels.md#omnibolt-5-atomic-swap-protocol-among-channels) of the OmniBOLT specification, 

### Websocket Request: Message Type -80

> Request:

```json
{
    "type":-80,
    "data":{
        "channel_id_from":"2e90c8e98074c9d711ef33c9b8ae9ff4437640f032e60ecf5626a4ca9b432b02",
        "channel_id_to":"35b10b403eaf04ed9efc15d756105c4cd9c921f1d871411eda54d499409ec8e8",
        "recipient_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0 ",
        "property_sent":121,
        "amount":20,
        "exchange_rate":1.2,
        "property_received":123,
        "transaction_id":"238822cdb245dd8a392a5d0479b5488e38b21ce4192e77fa03f408796037d88e",
        "time_locker":11
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id_from    | ------- |   data    | the channel where the HTLC 1 for swap(pay token A for token B) is created.
channel_id_to      | ------- |   data    | the channel that need to create the corresponding HTLC 2 to pay token B.
recipient_peer_id        | ------- |   body    | the counterparty who accepts the swap, i.e the onwer of token B.
property_sent      | ------- |   data  | Token A.
amount             | ------- |   data  | Amount of token A.
exchange_rate      | ------- |   data  | exchange rate, which is price(token A)/price(token B).
property_received  | ------- |   data  | Token B.
transaction_id     | ------- |   data  | The transaction ID of the commitment tranaction of this HTLC 1.
time_locker        | ------- |   data  | The time locker of HTLC 1.


### Websocket Response: Message Type -80

> OBD Responses:

```json
{
    "type":-80,
    "status":true,
    "from":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
    "to":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
    "result":{
        "amount":20,
        "channel_id_from":"2e90c8e98074c9d711ef33c9b8ae9ff4437640f032e60ecf5626a4ca9b432b02",
        "channel_id_to":"35b10b403eaf04ed9efc15d756105c4cd9c921f1d871411eda54d499409ec8e8",
        "exchange_rate":1.2,
        "property_received":0,
        "property_sent":121,
        "recipient_peer_id":"39e8b1f3e7aec51a368d70eac6d47195099e55c6963d38bcd729b22190dcdae0",
        "time_locker":11,
        "transaction_id":"238822cdb245dd8a392a5d0479b5488e38b21ce4192e77fa03f408796037d88e"
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
status    | ------- |   body    | true or false if the message is succesfully delivered.
from      | ------- |   body    | peer ID who creates the HTLC 1 for swap.
to      | ------- |   body    | peer ID who accept the swap or reject.
channel_id_from    | ------- |   data    | the channel where the HTLC 1 for swap(pay token A for token B) is created.
channel_id_to      | ------- |   data    | the channel that need to create the corresponding HTLC 2 to pay token B.
recipient_peer_id        | ------- |   data    | the counterparty who accepts the swap, i.e the onwer of token B.
property_sent      | ------- |   data  | Token A.
amount             | ------- |   data  | Amount of token A.
exchange_rate      | ------- |   data  | exchange rate, which is price(token A)/price(token B).
property_received  | ------- |   data  | Token B.
transaction_id     | ------- |   data  | The transaction ID of the commitment tranaction of this HTLC 1.
time_locker        | ------- |   data  | The time locker HTLC 1. 


## acceptSwap

### Simple Type -81 Protocol

Type -81 Protocol accepts or rejects a swap.

### Websocket Request: Message Type -81

> Request:

```json
{
    "type":-81,
    "data":{
        "channel_id_from":"35b10b403eaf04ed9efc15d756105c4cd9c921f1d871411eda54d499409ec8e8",
        "channel_id_to":"2e90c8e98074c9d711ef33c9b8ae9ff4437640f032e60ecf5626a4ca9b432b02",
        "recipient_peer_id":"1f1dbb3518c1fb12f263d065c1d18576d13f88dff55bfc25ef52afaa2c97a5d2",
        "property_sent":123,
        "amount":20,
        "exchange_rate":1.2,
        "property_received":121,
        "transaction_id":"f63385baf4607a59b87af4b85db7a6fc7ac3a785986e4ebd7fc008610335dfcc",
        "target_transaction_id":"238822cdb245dd8a392a5d0479b5488e38b21ce4192e77fa03f408796037d88e",
        "time_locker":11
    }
}
```

Parameter | default | placement | Description
--------- | ------- | --------- | ------------
channel_id_from    | ------- |   data    | the channel that creates the corresponding HTLC 2 to pay token B.
channel_id_to      | ------- |   data    | the channel where the HTLC 1 for swap(pay token A for token B) is created.
recipient_peer_id        | ------- |   body    | the onwer of token A.
property_sent      | ------- |   data  | Token B.
amount             | ------- |   data  | Amount of token B.
exchange_rate      | ------- |   data  | exchange rate, which is price(token A)/price(token B).
property_received  | ------- |   data  | Token A.
transaction_id     | ------- |   data  | The transaction ID of the commitment tranaction of this HTLC 2.
target_transaction_id | ------- |   data  | The transaction ID of the commitment tranaction of this HTLC 1.
time_locker        | ------- |   data  | The time locker of HTLC 2.


### Websocket Response: Message Type -81

