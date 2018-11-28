### websocket market data
```
// websocket server address
wss://stream-testnet.bybit.com/realtime
```
 
### Rate limit
 
A single api_key can establish 20 connections simultaneously, attempt to establish additional connection after 20 connections will be rejected.
 
### Authentication
 
For public topics, no authentication is required. As for private topics, authentication is required.
 
Currently, there are two ways to authenticate
 
1. Apply for authentication upon establishing connection request
2. Apply for authentication after establishing connection through auth request
 
api_key can be applied at <a href="https://testnet.bybit.com/user/api-management`">`https://testnet.bybit.com/user/api-management`</a>
 
```js
// First way to authenticate
var api_key = "";
var secret = "";
// http time where request become invalid, prevent replay attack防止重放攻击
// unit:millisecond
var expires = time.now()+1000;
 
// Signature
var signature = hex(HMAC_SHA256(secret, 'GET/realtime' + expires));
 
// 参数列表 Parameter
var param = "api_key={api_key}&expires={expires}&signature={signature}";
 
// 建立连接 Websocket/Establish connection
var ws = new WebSocket("wsurl?param");
 
// --------------------------------------------------------------------------
 
// Second way to authenticate
var ws = new WebSocket("wsurl")
// signature same as the first way to authenticate
ws.send('{"op":"auth","args":["{api_key}",expires,"{signature}"]}');
```
 
 
### How to subscribe to topic
 
After establishing connection, subscribe topic through sending json request. The specific format is as follows连接建立后，通过发送json格式的订阅指令来进行订阅topic,具体格式如下
```js
ws.send('{"op":"subscribe","args":["topic","topic.filter"]}');
 
// Same type of filter have multiple hours 同一个类型的filter有多个时，for 以'|' segmentation分割
// If subscribe to BTCUSD one minute and three minute kline
ws.send('{"op":"subscribe","args":["kline.BTCUSD.1m|3m"]}');
 
// To subscribe to data of the same type of filter, please use 订阅同一个类型filter的所有数据时请使用'*'
// If subscribe to all product interval kline 如订阅所有产品的所有interval kline
ws.send('{"op":"subscribe","args":["kline.*.*"]}')
 
 
// Result of subscribed topic 订阅topic结果
// Every subscription has a response, response format is as follows 每一个订阅指令都会有一个响应,响应格式如下
{
   "success":true, // Whether subscription is successful订阅是否成功
   "ret_msg":"",   // Successful subscription = zero, unsuccessful subscription shows error message 订阅成功时为空，失败时为具体错误信息
   "request":{     // Request to subscribe 请求订阅的指令
       "op":"subscribe",
       "args":[
           "kline.BTCUSD.1m"
       ]
   }
}
 
```
 
## Currently supported topic
 
### Public topic
* [orderBook25](#orderBook25) `// 25orderBook`
* [kline](#kline) `// K line`
* [trade](#trade) `// real time trade 实时交易`
* [insurance](#insurance) `// daily insurance fund update每日保险基金更新`
 
### Private topic
* [position](#position) `// positions change 仓位变化`
* [execution](#execution) `//active order execution message 委托单成交信息`
* [order](#order) `// active order update委托单的更新`
 
<hr>
 
### <span id="orderBook25">subscribe 25orderBook</span>
```js
// Send subscription request
ws.send('{"op": "subscribe", "args": [orderBook25.BTCUSD]}');
 
// format of message sent 推送的消息格式
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
 
### <span id="kline">k line</span>
 
* currently supported interval目前支持的interval
* 1m 3m 5m 15m 30m
* 1h 2h 3h 4h 6h
* 1d 3d
* 1w 2w
* 1M
```js
ws.send('{"op":"subscribe","args":["kline.BTCUSD.1m"]}');
 
// format of message sent 推送的消息格式
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
 
### <span id="trade"> real time trade message 实时交易信息</span>
 
```js
ws.send('{"op":"subscribe","args":["trade"]}')
 
// format of message sent 推送的消息格式
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
 
### <span id="insurance">daily insurance fund update每日保险基金更新</span>
 
```js
ws.send('{"op":"subscribe","args":["insurance"]}')
 
// format of message sent 推送的消息格式
{
    "topic":"insurance.BTC",
    "action":"update",
    "data":{
       "currency":"BTC",
       "timestamp":"2018-10-24T12:00:00.000Z",
       "walletBalance":140224705439 // unit: ? 单位:聪
    }
}
```
 
<hr>
 
### <span id="position">position change message仓位变化消息</position>
 
```js
ws.send('{"op":"subscribe","args":["position"]}')
 
// format of message sent 推送的消息格式
{
   "topic":"position:BTCUSD",
   "action":"update",
   "data":
   {
       "symbol":"BTCUSD",                  // symbol 产品
       "side":"Sell",                      // direction
       "size":11,                          // quantity
       "entry_price":6907.291588174717,    // entry price
       "liq_price":7100.234,               // liquidation price
       "bust_price":7088.1234,             // bankruptcy price
       "take_profit":0,                    // take profit price
       "stop_loss":0,                      // stop loss price
       "trailing_stop":0,                  // trailing stop points
       "position_value":0.00159252,        // positional value
       "leverage":1,                       // leverage
       "position_status":"Normal",         // status of position(Normal:normal Liq:in the process of liquidation Adl:in the process of ADL)
       "auto_add_margin":0,                // Auto margin replenishment enabled (0:no 1:yes)
       "position_seq":14                   // position sequence number 仓位版本号
   }
}
```
 
 
<hr>
 
### <span id="execution">execution message 成交信息</span>
```js
ws.send('{"op":"subscribe","args":["execution"]}')
 
// format of message sent 推送的消息格式
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
 
### <span id="order">active order update委托更新</span>
 
```js
ws.send('{"op":"subscribe","args":["order"]}')
 
// format of message sent 推送的消息格式
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
 
