### WebSocket Data
```
// websocket server address

//For Testnet
wss://stream-testnet.bybit.com/realtime

//For Mainnet
wss://stream.bybit.com/realtime
```
 
### Rate Limits
 
One single api_key can establish 20 connections simultaneously. Any additional connection after 20 connections will be rejected.
 
### Authentication
 
For public topics, no authentication is required. As for private topics, authentication is required.
 
Currently, there are two ways to authenticate your identity.
 
1. Apply for authentication when establishing a connection.
2. Apply for authentication after establishing a connection through auth request.
 
 
```js
// First way to authenticate
var api_key = "";
var secret = "";
// A UNIX timestamp after which the request become invalid. This is to prevent replay attacks.
// unit:millisecond
var expires = time.now()+1000;

// Signature
var signature = hex(HMAC_SHA256(secret, 'GET/realtime' + expires));
 
// Parameters string
var param = "api_key={api_key}&expires={expires}&signature={signature}";
 
// Establishing connection
var ws = new WebSocket("wsurl?param");
 
// --------------------------------------------------------------------------
 
// Second way to authenticate
var ws = new WebSocket("wsurl")
// Signature is the same as the first way's
ws.send('{"op":"auth","args":["{api_key}",expires,"{signature}"]}');
```
 
 
### How to Subscribe to a New Topic
 
After establishing the connection, one can subscribe to a new topic by sending a json request. The specific formats are as follows:
```js
ws.send('{"op":"subscribe","args":["topic","topic.filter"]}');
 
// Split the multiple filters by '|' if they belong to the same cluster of topics.
// For example, subscribing to BTCUSD one minute and three minutes kline.
ws.send('{"op":"subscribe","args":["kline.BTCUSD.1m|3m"]}');
 
// Use '*' when subscribing to all data of the same type filter.
// For exmaple, subscribing to all products' interval kline.
ws.send('{"op":"subscribe","args":["kline.*.*"]}')
 
 
// Result of subscribed topic
// Every subscription will have a response. The response is sent in the following format:
{
   "success":true, // Whether subscription is successful
   "ret_msg":"",   // For successful subscription it shows "", otherwise it shows error message
   "request":{     // Request to your subscription
       "op":"subscribe",
       "args":[
           "kline.BTCUSD.1m"
       ]
   }
}
 
```
 
## Currently Supported Topics
 
### Public Topic
* [orderBook25](#orderBook25) `// OrderBook of 25 depth per side`
* [kline](#kline) `// Candlestick chart`
* [trade](#trade) `// Real-time trading information`
* [insurance](#insurance) `// Daily insurance fund update`
* [instrument](#instrument) `// Lastet information for symbol`
 
### Private Topic
* [position](#position) `// Positions of your account`
* [execution](#execution) `// Execution message`
* [order](#order) `// Update for your orders`
 
<hr>
 
### <span id="orderBook25">OrderBook of 25 depth per side</span>
```js
// Send subscription request
ws.send('{"op": "subscribe", "args": ["orderBook25.BTCUSD"]}');
 
// Response content format
{
   "topic":"orderBook25",
   "action":"snapshot",
   "data":{
       "lastUpdateId":709,
       "symbol":"BTCUSD",
       "bids":[],
       "asks":[
           {"price":6400,"quantity":38},
           {"price":6300,"quantity":5},
       ]
   }
}
 
```
 
<hr>
 
### <span id="kline">Candlestick chart</span>
 
* Currently supported interval
* 1m 3m 5m 15m 30m
* 1h 2h 3h 4h 6h
* 1d 3d
* 1w 2w
* 1M
```js
ws.send('{"op":"subscribe","args":["kline.BTCUSD.1m"]}');
 
// Response content format
{
   "topic":"kline.BTCUSD.1m",
   "data":{
       "id":563,
       "symbol":"BTCUSD",
       "open_time":1539918000,
       "open":5900,
       "high":6501,
       "low":6501,
       "close":6501,
       "volume":9,
       "turnover":0.0013844,
       "interval":"1m"
   }
}
```
 
<hr>
 
### <span id="trade"> Real-time trading information </span>
 
```js
ws.send('{"op":"subscribe","args":["trade"]}')
 
// Response content format
{
   "topic":"trade.BTCUSD",
   "data":[
       {
           "timestamp":"2018-10-25T06:21:11.846Z",
           "symbol":"BTCUSD",
           "side":"Buy",
           "size":10670,
           "price":6398.5,
           "tick_direction":"ZeroMinusTick" // [PlusTick,ZeroPlusTick,MinusTick,ZeroMinusTick]
       }
   ]
}
```
 
<hr>
 
### <span id="insurance">Daily insurance fund update</span>
 
```js
ws.send('{"op":"subscribe","args":["insurance"]}')
 
// Response content format
{
    "topic":"insurance.BTC",
    "action":"update",
    "data":{
       "currency":"BTC",
       "timestamp":"2018-10-24T12:00:00.000Z",
       "walletBalance":140224705439 // unit: Satoshi
    }
}
```
 
 
 <hr>
 
### <span id="instrument"> Lastet information for symbol</span>
 
```js
ws.send('{"op":"subscribe","args":["instrument.BTCUSD"]}')

// Response content format
// NOTE: Message belong to "data" class will only be sent to you when it changes. 
// For example, if 'index_price' and 'mark_price' changed and the 'transaction price' didn't change, then only 'symbol', 'index_price' and 'mark_price' will be sent to you, without 'last_price'.
 {
     "topic":"instrument.BTCUSD",
     "data":{
        "symbol": "BTCUSD",
        "mark_price": 5000.5, // mark price
        "index_price": 5000.5, // index price
        "last_price": 5000.5 // latest price
     }
 }
```
 
 

<hr>
 
### <span id="position">Positions of your account</position>
 
```js
ws.send('{"op":"subscribe","args":["position"]}')
 
// Response content format
{
   "topic":"position:BTCUSD",
   "action":"update",
   "data":[
       {
           "symbol":"BTCUSD",                  // the contract for this position
           "side":"Sell",                      // side
           "size":11,                          // the current position amount
           "entry_price":6907.291588174717,    // entry price
           "liq_price":7100.234,               // liquidation price
           "bust_price":7088.1234,             // bankruptcy price
           "take_profit":0,                    // take profit price
           "stop_loss":0,                      // stop loss price
           "trailing_stop":0,                  // trailing stop points
           "position_value":0.00159252,        // positional value
           "leverage":1,                       // leverage
           "position_status":"Normal",         // status of position (Normal:normal Liq:in the process of liquidation Adl:in the process of Auto-Deleveraging)
           "auto_add_margin":0,                // Auto margin replenishment enabled (0:no 1:yes)
           "position_seq":14                   // position version number
       }
   ]
}
```

<hr>
 
### <span id="execution">Execution message</span>
```js
ws.send('{"op":"subscribe","args":["execution"]}')
 
// Response content format
{
    "topic":"execution",
    "action":"new",
    "data":[
        {
           "symbol":"BTCUSD",
           "side":"Buy",
           "order_id":"aaaaaaaa",
           "price":6400.5,
           "qty":100,
           "leaves_qty":50,
           "is_maker":true,
           "trade_time":134234123.121322
       }
    ]
}
```
 
<hr>
 
### <span id="order">Update for your orders</span>
 
```js
ws.send('{"op":"subscribe","args":["order"]}')
 
// Response content format
{
   "topic":"order",
   "data":[
       {
           "order_id":"3ceb73c8-11db-45b1-a407-613dba55e5bf",
           "symbol":"BTCUSD",
           "side":"Sell",
           "order_type":"Limit",
           "price":6418.5,
           "qty":596801,
           "time_in_force":"GoodTillCancel",
           "order_status":"PartiallyFilled",
           "leaves_qty":596800,
           "cum_exec_qty":1,
           "cum_exec_value":0.00015579,
           "cum_exec_fee":0.00000003,
           "timestamp":"2018-10-26T10:35:31.000Z"
       }
   ]
}
```
 
