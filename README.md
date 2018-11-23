# EOS WebSocket
## ⚠️DEPRECATED ⚠️

> Library is now being maintained at https://github.com/dfuse-io/eosws-js

> Go to https://www.dfuse.io and click on `"Get a Free API Key"`

## EOSWS Servers

**Mainnet**

-   `wss://eosws.mainnet.eoscanada.com/v1/stream?token=[API TOKEN]`

**Kylin**

-   `wss://eosws.kylin.eoscanada.com/v1/stream?token=[API TOKEN]`

## Install

**npm**

```bash
$ npm install --save eosws
```

## Quickstart

### Get Actions

```ts
import WebSocket from "ws";
import { get_actions, parse_actions } from "eosws";

const API_TOKEN = "eyJ...IBg";
const origin = "https://example.com";
const ws = new WebSocket(`wss://<SERVER>/v1/stream?token=${API_TOKEN}`, {origin});

ws.onopen = () => {
    ws.send(get_actions("eosio.token", "transfer"));
};

ws.onmessage = (message) => {
    const actions = parse_actions(message.data);

    if (actions) {
        const { from, to, quantity, memo } = actions.data.trace.act.data;
        console.log(from , to, quantity, memo);
    }
};
```

### Get Table Rows

```ts
import { get_table_rows, parse_table_rows } from "eosws";

ws.onopen = () => {
    ws.send(get_table_rows("eosio", "eosio", "voters"));
};

ws.onmessage = (message) => {
    const table = parse_table_rows<Voters>(message.data, voters_req_id);

    if (table) {
        const {owner, producers, last_vote_weight} = table.data.row;
        console.log(owner, producers, last_vote_weight);
    }
};
```

## Related Javascript

-   [x] WebSockets (<https://github.com/websockets/ws>)
-   [ ] Socket.io (<https://github.com/socketio/socket.io>)

## Related Video

-   Push Irreversible Transaction (<https://youtu.be/dO-Le3TTim0?t=34m6s>)

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [get_actions](#get_actions)
    -   [Parameters](#parameters)
    -   [Examples](#examples)
-   [get_table_rows](#get_table_rows)
    -   [Parameters](#parameters-1)
    -   [Examples](#examples-1)
-   [unlisten](#unlisten)
    -   [Parameters](#parameters-2)
    -   [Examples](#examples-2)
-   [generateReqId](#generatereqid)
    -   [Examples](#examples-3)
-   [parse_actions](#parse_actions)
    -   [Parameters](#parameters-3)
    -   [Examples](#examples-4)
-   [parse_table_rows](#parse_table_rows)
    -   [Parameters](#parameters-4)
    -   [Examples](#examples-5)
-   [parse_ping](#parse_ping)
    -   [Parameters](#parameters-5)
    -   [Examples](#examples-6)

### get_actions

Get Actions

#### Parameters

-   `account` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Account
-   `action_name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Action Name
-   `receiver` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Receiver
-   `options`   (optional, default `{}`)

#### Examples

```javascript
ws.send(get_actions("eosio.token", "transfer"));
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Message for `ws.send`

### get_table_rows

Get Table Deltas

#### Parameters

-   `code` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Code
-   `scope` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Scope
-   `table_name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Table Name
-   `options` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Optional parameters (optional, default `{}`)
    -   `options.req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Request ID
    -   `options.start_block` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Start at block number
    -   `options.fetch` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** Fetch initial request

#### Examples

```javascript
ws.send(get_table_rows("eosio", "eosio", "global"));
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Message for `ws.send`

### unlisten

Unlisten to WebSocket based on request id

#### Parameters

-   `req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Request ID

#### Examples

```javascript
ws.send(unlisten("req123"));
```

### generateReqId

Generate Req ID

#### Examples

```javascript
generateReqId() // => req123
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Request ID

### parse_actions

Parse Actions from `get_actions` from WebSocket `onmessage` listener

#### Parameters

-   `data` **WebSocketData** WebSocket Data from message event
-   `req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Request ID

#### Examples

```javascript
const actions = parse_actions<any>(message);
```

Returns **ActionTrace** Action Trace

### parse_table_rows

Parse Table Deltas from `get_table_rows` from WebSocket `onmessage` listener

#### Parameters

-   `data` **WebSocketData** WebSocket Data from message event
-   `req_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Request ID

#### Examples

```javascript
const table_deltas = parse_table_rows<any>(message);
```

Returns **ActionTrace** Action Trace

### parse_ping

Parse Ping from WebSocket `onmessage` listener

#### Parameters

-   `data` **WebSocketData** WebSocket Data from message event

#### Examples

```javascript
const ping = parse_ping(message);
```

Returns **Ping** Ping
