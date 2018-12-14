### Active Order
 
* [Place active order](#open-apiordercreatepost)
 
* [Get active order](#open-apiorderlistget)
 
* [Cancel active order](#open-apiordercancelpost)
 
### Conditional Order
 
* [Place conditional order](#open-apistop-ordercreatepost)
 
* [Get conditional order](#open-apistop-orderlistget)
 
* [Cancel conditional order](#open-apistop-ordercancelpost)
 
### Positions
 
* [User leverage](#userleverageget)
 
* [Change leverage](#userleveragesavepost)
 
* [My position](#positionlistget)
 
* [Change margin](#positionchange-position-marginpost)
 
### Funding
 
* [Funding rate](#lastfundingrate)
 
* [My funding fee](#userlastfundingfee)
 
-----------
## <span id="open-apiordercreatepost"> Place Active Order </span>
#### API Function
 
>Parameters of 'side', 'symbol', 'order_type', 'qty', 'price', 'time_in_force' are required for all active orders. Other parameters are optional unless specified.

>Market price active order: A traditional market price order, will be filled at the best available price. 'price' and 'time_in_force' can set to be "" if and only if you are placing market price order.
 
>Limit price active order: You can set an execution price for your order. Only when last traded price reaches the order price, the system will fill your order. 
 
>Take profit/Stop loss: You may only set a take-profit/stop-loss conditional order upon opening the position. Once you hold a position, the take profit and stop loss information u sent when placing an order will no longer be valid.
 
>Order quantity: This parameter indicates the quantity of perpetual contracts you want to buy or sell, currently Bybit only support order quantity in an integer.
 
>Order price: This parameter indicates the price of perpetual contracts you want to buy or sell, currently Bybit only support price increment of every 0.5.
 
>Customize conditional order ID: You may customize order IDs for active orders. We will link it to the system order ID , and return the unique system order ID to you after the active order is created successfully. You may use this order ID to cancel your active order. The customized order ID is asked to be unique, with a maximum length of 36 characters.
 
>Note: Each account can hold up to 200 active orders yet to be filled entirely simultaneously.
 
#### URL
 
> For Testnet:
> [https://api-testnet.bybit.com/open-api/order/create](https://api-testnet.bybit.com/open-api/order/create)

> For Mainnet:
> [https://api.bybit.com/open-api/order/create](https://api.bybit.com/open-api/order/create)
 
#### HTTP Request Method
 
> post
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|side |true |string |Side. Valid option: Buy, Sell    |
|symbol |true |string |Contract type. Valid option: BTCUSD, ETHUSD    |
|order_type |true |string |Active order type. Valid option: Limit, Market   |
|qty |true |integer |Order quantity. Maximum quantity of 1 million |
|price |true |integer |Order price. If you held no position, order price has to be more than 10% of the market price and less than 1 million. If you has held any position already, your order price has to be better than liquidation price. The minimum unit of order price's increment or decrement is 0.5.  |
|time_in_force |true |string |Time in force, Valid option: GoodTillCancel, ImmediateOrCancel, FillOrKill |
|order_link_id |false |string |Customized order ID, maximum length at 36 characters, and order ID under the same agency has to be unique.|
 
 
#### Response example
 
```js
 
   {
       'ret_code':0   //Error code - True
       'ret_msg':'ok' //Error message
       'ext_code':''  ,
       'result':{
           'order_id': 'string',              //Unique order ID
           'user_id': 0,                      //User ID
           'symbol': 'string',                //Contract type
           'side': 'string',                  //Side
           'order_type': 'string',            //Order type
           'price': 0,                        //Order price
           'qty': 0,                          //Order quantity
           'time_in_force': 'string',         //Time in force
           'order_status': 'string',          //Order status: Created: order created; Rejected: order rejected; New: order pending; PartiallyFilled: order filled partially; Filled: order filled fully, Cancelled: order cancelled 
           'last_exec_time': 0.000000,        //Last execution time
           'last_exec_price': 0,              //Last execution price
           'leaves_qty': 0,                   //Remaining order quantity
           'cum_exec_qty': 0,                 //Accumulated execution quantity
           'cum_exec_value': 0,               //Accumulated execution value
           'cum_exec_fee': 0,                 //Accumulated execution fee
           'reject_reason': 'string',         //Reason for rejection
           'order_link_id': 'string',         //Agency customized order ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    //UTC timestamp
   }
 
```
 
-----------
## <span id="open-apiorderlistget">Get Active Order </span>
#### API Function
 
> Get my active order list
 
#### URL

> For Testnet:
> [https://api-testnet.bybit.com/open-api/order/list](https://api-testnet.bybit.com/open-api/order/list)

> For Mainnet:
> [https://api.bybit.com/open-api/order/list](https://api.bybit.com/open-api/order/list)
 
#### HTTP Request Method
 
> get
 
#### Request parameters
 
|parameters|required|type|comments|
|:----- |:-------|:-----|----- |
|order_id |false |string |Order ID |
|order_link_id |false |string |Customized order ID |
|symbol |false |string | Contract type (BTCUSD, ETHUSD )    |
|sort |false |string |Sorted. Default sort by created time (created_at, cum_exec_qty, qty, last_exec_price, price, cum_exec_value, cum_exec_fee)    |
|order |false |string |Ascending or descending. Default as descending  (desc, asc )   |
|page |false |integer |Page. Default getting first page data |
|limit |false |integer |Limit for data size per page. Default as showing 20 pieces of data per page |
|order_status |false |string | Query your orders for all statuses if 'order_status' is empty. If you want to query orders with specific statuses , you can pass the order_status split by ','.  Available order_status: Created, New, PartiallyFilled, Filled, Cancelled, Rejected 
 
#### Response example
 
```js
 
   {
       'ret_code':0,
       'ret_msg':'ok',
       'ext_code':'',
       'result':{
           'data':[
               {
                   'order_id': 'string',       //Unique order ID
                   'user_id': 0,               //User ID 
                   'symbol': 'string',         //Contract type
                   'side': 'string',           //Side
                   'order_type': 'string',     //Order type 
                   'price': 0,                 //Order price 
                   'qty': 0,                   //Order quantity
                   'time_in_force': 'string',  //Time in force
                   'order_status': 'string',   //Order status: Created: order created; Rejected: order rejected; New: order pending; PartiallyFilled: order filled partially; Filled: order filled fully, Cancelled: order cancelled 
                   'last_exec_time': 0.000000 , //Last execution time
                   'last_exec_price': 0,       //Last execution price
                   'leaves_qty': 0,            //Remaining order quantity
                   'cum_exec_qty': 0,          //Accumulated execution quantity
                   'cum_exec_value': 0,        //Accumulated execution value
                   'cum_exec_fee': 0,          //Accumulated execution fee
                   'reject_reason': 'string',  //Reason for rejection
                   'order_link_id': 'string',  //Agency customized order ID
                   'created_at':'2018-10-15T04:12:19.000Z',
                   'updated_at':'2018-10-15T04:12:19.000Z',
               }
           ],
           'current_page': 1,
           'total': 1
       },
       'time_now':'1539781050.462841'
   }
 
```
 
-----------
## <span id="open-apiordercancelpost">Cancel Active Order</span>
####API Function
 
> 'order_id' is required for cancelling active order. The unique 36 characters order ID was returned to you when the active order was created successfully. 
 
>You may cancel active order that are unfilled and partially filled. Fully filled order cannot be cancelled.
 
#### URL

> For Testnet
> [https://api-testnet.bybit.com/open-api/order/cancel](https://api-testnet.bybit.com/open-api/order/cancel)

> For Mainnet
> [https://api.bybit.com/open-api/order/cancel](https://api.bybit.com/open-api/order/cancel)

 
####  HTTP Request Method
 
> post
 
####  Request Parameters
 
|parameters|required|type|comments|
|:----- |:-------|:-----|----- |
|order_id |true |string |Your active order ID. The unique order ID returned to you when the corresponding active order was created |
 
 
#### Response example
 
```js
 
   {
       'ret_code':0   //Error code - True
       'ret_msg':'ok' //Error message
       'ext_code':''  ,
       'result':{
           'order_id': 'string',              //Unique order ID
           'user_id': 0,                      //User ID
           'symbol': 'string',                //Contract type
           'side': 'string',                  //Side
           'order_type': 'string',            //Order type
           'price': 0,                        //Order price
           'qty': 0,                          //Order quantity
           'time_in_force': 'string',         //Time in force
           'order_status': 'string',          //Order status: Created: order created; Rejected: order rejected; New: order pending; PartiallyFilled: order filled partially; Filled: order filled fully, Cancelled: order cancelled 
           'last_exec_time': 0.000000,        //Last execution time
           'last_exec_price': 0,              //Last execution price
           'leaves_qty': 0,                   //Remaining order quantity
           'cum_exec_qty': 0,                 //Accumulated execution quantity
           'cum_exec_value': 0,               //Accumulated execution value
           'cum_exec_fee': 0,                 //Accumulated execution fee
           'reject_reason': 'string',         //Reason for rejection
           'order_link_id': 'string',         //Agency customized order ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    //UTC timestamp
   }
 
```
 
-----------
## <span id="open-apistop-ordercreatepost"> Place Conditional Order </span>
#### API Function
 
>Parameters of 'side', 'symbol', 'order_type', 'qty', 'price', 'base_price', 'stop_px', 'time_in_force' are required for all active orders. Other parameters are optional unless specified.

>Market price conditional order: A traditional market price order, will be filled at the best available price. 'price' and 'time_in_force' can set to be "" if and only if you are placing market price order.
 
>Limit price conditional order: You can set an execution price for your order. Only when last traded price reaches the order price, the system will fill your order. 
 
>Take profit/Stop loss: You may only set a take-profit/stop-loss conditional order upon opening the position. Once you hold a position, the take profit and stop loss information u sent when placing an order will no longer be valid.
 
>Order quantity: This parameter indicates the quantity of perpetual contracts you want to buy or sell, currently Bybit only support order quantity in an integer.
 
>Order price: This parameter indicates the price of perpetual contracts you want to buy or sell, currently Bybit only support price increment of every 0.5.

>Conditional order trigger price: You may set a trigger price for your conditional order. conditional order will not enter the order book until the last price hits the trigger price. When last price hits trigger price: 1) your limit conditional order will enter order book, and wait to be executed; 2) your market conditional order will be executed immediately at the best available market price.
 
>Customize conditional order ID: You may customize order IDs for active orders. We will link it to the system order ID , and return the unique system order ID to you after the active order is created successfully. You may use this order ID to cancel your active order. The customized order ID is asked to be unique, with a maximum length of 36 characters.
 
>Note: Take profit/Stop loss is not supported in placing conditional orders. One can only use these 2 functions when placing active orders. Moreover, each account can hold up to 10 conditional orders yet to be filled entirely simultaneously.


 
#### URL
> For Testnet
> [https://api-testnet.bybit.com/open-api/stop-order/create](https://api-testnet.bybit.com/open-api/stop-order/create)

> For Mainnet
> [https://api.bybit.com/open-api/stop-order/create](https://api.bybit.com/open-api/stop-order/create)
 
#### HTTP Request Method
 
> post
 
#### Request Parameters
 
 
 |parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|side |true |string |Side. Valid option: Buy, Sell    |
|symbol |true |string |Contract type. Valid option: BTCUSD, ETHUSD    |
|order_type |true |string |Conditional order type. Valid option: Limit, Market |
|qty |true |integer |Order quantity. Maximum quantity of 1 million |
|price| true | integer | Execution price for conditional order|
|base_price |true |integer | Send current market price. It will be used to compare with the value of 'stop_px', to decide whether your conditional order will be triggered by crossing trigger price from upper side or lower side. Mainly used to identify the expected direction of the current conditional order. |
|stop_px | true | integer | Trigger price |
|time_in_force |true |string |Time in force, Valid option: GoodTillCancel, ImmediateOrCancel, FillOrKill |
|order_link_id |false |string |Customized order ID, maximum length at 36 characters, and order ID under the same agency has to be unique.|
 
 
#### Response example
 
```js
 
   {
       'ret_code':0   //Error code - True
       'ret_msg':'ok' //Error message
       'ext_code':''  ,
       'result':{
           'stop_order_id': 'string',         //Unique order ID
           'user_id': 0,                      //User ID
           'stop_order_status': 'string',     //Order status: Untriggered: order waits to be triggered; Triggered: order is triggered; Cancelled: order is cancelled, Active: order is triggered and placed successfully; Rejected: Order is triggered but fail to be placed
           'symbol': 'string',                //Contract type
           'side': 'string',                  //Side
           'order_type': 'string',            //Price type
           'price': 0,                        //Order price
           'qty': 0,                          //Order quantity
           'time_in_force': 'string',         //Time in force
           'stop_order_type': 'string',       //Order type
           'base_price': 0,                   //
           'stop_px': 0,                      //trigger price
           'order_link_id': 'string',         //Agency customized order ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    //UTC timestamp
   }
 
```
 
-----------
## <span id="open-apistop-orderlistget">Get Conditional Order </span>
#### API Function
 
> Get my conditional order list。
 
#### URL
> For Testnet
> [https://api-testnet.bybit.com/open-api/stop-order/list](https://api-testnet.bybit.com/open-api/stop-order/list)

> For Mainnet
> [https://api.bybit.com/open-api/stop-order/list](https://api.bybit.com/open-api/stop-order/list)
 
#### HTTP Request Method
 
> get
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|stop_order_id |false |string |Order ID of conditional order|
|order_link_id |false |string |Agency customized order ID|
|symbol |false |string |Contract type (BTCUSD ETHUSD )    |
|sort |false |string |Sorted. Default sort by created time (created_at)    |
|order |false |string |Ascending or descending. Default as descending  (desc, asc )   |
|page |false |integer |Page. Default getting first page data |
|limit |false |integer |Limit for data size per page. Default as showing 20 pieces of data per page |
 
 
#### Response example
 
```js
 
   {
       'ret_code':0,
       'ret_msg':'ok',
       'ext_code':'',
       'result':{
           'data':[
               {
                   'stop_order_id': 'string',         //Unique order ID
                   'user_id': 0,                      //User ID 
                   'stop_order_status': 'string',     //Order status: Untriggered: Pending order waits to be triggered; Triggered: order is triggered; Cancelled: order is cancelled, Active: order is triggered and placed successfully; Rejected: Order is triggered but fail to be placed
                   'symbol': 'string',                //Contract type
                   'side': 'string',                  //Side
                   'order_type': 'string',            //Price type
                   'price': 0,                        //Order price
                   'qty': 0,                          //Order quantity
                   'time_in_force': 'string',         //Time in force
                   'stop_order_type': 'string',       //Order type
                   'base_price': 0,                   //
                   'stop_px': 0,                      //trigger price
                   'order_link_id': 'string',         //Agency customized order ID
                   'created_at':'2018-10-15T04:12:19.000Z',
                   'updated_at':'2018-10-15T04:12:19.000Z',
               }
           ],
           'current_page': 1,
           'total': 1
       },
       'time_now':'1539781050.462841'
   }
```
 
-----------
## <span id="open-apistop-ordercancelpost">Cancel Conditional Order </span>
#### API Function
 
 
>'stop_order_id' is required for cancelling conditional order. The unique 36 characters order ID was returned to you when the condional order was created successfully. 
 
>You may cancel all untriggered conditional orders. Essentially, after a conditional order is triggered, it will become an active order. So, when a conditional order is triggered, cancellation has to be done through the active order port for all unfilled or partial filled active order. Similarly, order that has been fully filled cannot be cancelled. 
 
#### URL
 
> For Testnet
> [https://api-testnet.bybit.com/open-api/stop-order/cancel](https://api-testnet.bybit.com/open-api/stop-order/cancel)

> For Mainnet
> [https://api.bybit.com/open-api/stop-order/cancel](https://api.bybit.com/open-api/stop-order/cancel)

#### HTTP Request Method
 
> post
 
#### Request Parameters
 
|parameter|required|type | comments|
|:----- |:-------|:-----|----- |
|order_id |true |string | Order ID. The unique order ID returned to you when the corresponding order was created. |
 
 
#### Response example
 
```js
 
   {
       'ret_code':0   //Error code - True
       'ret_msg':'ok' //Error message
       'ext_code':''  ,
       'result':{
           'stop_order_id': 'string',         //Unique order ID
           'user_id': 0,                      //User ID
           'stop_order_status': 'string',     //Order status: Untriggered: order waits to be triggered; Triggered: order is triggered; Cancelled: order is cancelled, Active: order is triggered and placed successfully; Rejected: Order is triggered but fail to be placed
           'symbol': 'string',                //Contract type
           'side': 'string',                  //Side
           'order_type': 'string',            //Price type
           'price': 0,                        //Order price
           'qty': 0,                          //Order quantity
           'time_in_force': 'string',         //Time in force
           'stop_order_type': 'string',       //Order type
           'base_price': 0,                   //
           'stop_px': 0,                      //trigger price
           'order_link_id': 'string',         //Agency customized order ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    //UTC timestamp
   }
 
```
 
-----------
## <span id="userleverageget">User Leverage</span>
#### API Function
 
> Get user leverage
 
#### URL
 
 > For Testnet
> [https://api-testnet.bybit.com/user/leverage](https://api-testnet.bybit.com/user/leverage)

> For Mainnet
> [https://api.bybit.com/user/leverage](https://api.bybit.com/user/leverage)

 
#### HTTP Request Method
 
> get
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
 
 
####  Response example
 
```js
 
{
   'ret_code':0   //return code (0: successful, -1: failed)
   'ret_msg':'ok' //error message,
   'ext_code':''  //error code,
   'result': {
       'BTCUSD': {
           'leverage': 100
       },
       'ETHUSD': {
           'leverage': 1
       }
   'time_now':'1539778407.210858',    //UTC timestamp
}
 
```
 
-----------
## <span id="userleveragesavepost"> Change User Leverage</span>
#### API Function
 
> Change user leverage
 
#### URL

> For Testnet
> [https://api-testnet.bybit.com/user/leverage/save](https://api-testnet.bybit.com/user/leverage/save)

> For Mainnet
> [https://api.bybit.com/user/leverage/save](https://api.bybit.com/user/leverage/save)


#### HTTP Request Method
 
> post
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|symbol |true |string | Contract type (BTCUSD ETHUSD )    |
|leverage |true |string |leverage  |
 
 
####  Response example
 
```js
 
{
   'ret_code':0   //return code (0: successful, -1: failed) 
   'ret_msg':'ok' //error message,
   'ext_code':''  //error code,
   'result': null, //One can decide whether a request is successful depending on ret_code. Will always return 'null'
   'time_now':'1539778407.210858',    //UTC timestamp
}
 
```
 
-----------
## <span id="positionlistget"> My Position</span>
#### API Function
 
> Get my position list
 
#### URL

> For Testnet
> [https://api-testnet.bybit.com/position/list](https://api-testnet.bybit.com/position/list)

> For Mainnet
> [https://api.bybit.com/position/list](https://api.bybit.com/position/list)

#### HTTP Request Method
 
> get
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
 
 
#### Response example
 
```js
 
{
   'ret_code': 0,    //return code (0: successful, -1: failed)
   'ret_msg': 'ok',  //error message
   'ext_code': '',   //error code
   'result': [
       {
           'id': 1,                //position ID
           'user_id': 1,           //user ID 
           'risk_id': 1,           //risk limit ID 
           'symbol': 'BTCUSD',     //Contract type (BTCUSD,ETHUSD)
           'side': 'None',         //position Side  (None, buy, sell)
           'size': 0,              //position size
           'position_value': 0,    //position value
           'entry_price': 0,       //entry price
           'leverage': 1,          //user leverage
           'auto_add_margin': 0,   //auto margin replenishment switch
           'position_margin': 0,   //position margin
           'liq_price': 999999,    //liquidation price
           'bust_price': 999999,   //bankruptcy price
           'occ_closing_fee': 0,   //position closing 
           'take_profit': 0,       //take profit price
           'stop_loss': 0,         //stop loss price
           'trailing_stop': 0,     //trailing stop point 
           'position_status': 'Normal',   //Status Normal(normal), Liq(Liquidation in process), ADL(ADL in process) 
           'deleverage_indicator': 1,
           'oc_calc_data': '{\'blq\':\'0\',\'bmp\':\'0\',\'slq\':\'0\',\'smp\':\'0\'}',
           'order_margin': 0,      //Used margin by order
           'wallet_balance': 0,    //wallet balance
           'realised_pnl': 0,      //daily realized profit and loss
           'cum_realised_pnl': 0,  //Total realized profit and loss
           'cum_commission': 0,    //Total commissions
           'cross_seq': 0,         //
           'position_seq': 2,      //position sequence number
           'created_at': '2018-10-18T07:15:51.000Z',
           'updated_at': '2018-10-20T13:43:21.000Z'
       }
   ],
   'time_now': '1540043097.995523'      UTC timestamp
}
*    
```
 
-----------
## <span id="positionchange-position-marginpost"> Change Position Margin</span>
#### API Function
 
> Update margin
 
#### URL
 
> For Testnet
> [https://api-testnet.bybit.com/position/change-position-margin](https://api-testnet.bybit.com/position/change-position-margin)

> For Mainnet
> [https://api.bybit.com/position/change-position-margin](https://api.bybit.com/position/change-position-margin)


 
#### HTTP Request Method
 
> post
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|symbol |true |string |Contract type (BTCUSD ETHUSD )    |
|margin |true |string |margin  |
 
 
#### Response example
 
```js
 
{
   'ret_code':0   //return code(0: successful, -1: failed)
   'ret_msg':'ok' //error message
   'ext_code':'', //error code
  'result': null,  //One can decide whether a request is successful depending on ret_code. Will always return 'null'
   'time_now':'1539778407.210858',    //UTC timestamp
}
 
```
 
-----------
## <span id="lastfundingrate"> Get the Last Funding Rate</span>
#### API Function
 
> Funding rate is generated every 8 hours at 00:00 UTC, 08:00 UTC and 16:00 UTC. 
> If it's 12:00 UTC now, what you will get is the funding rate generated at 08:00 UTC.
 
#### URL
 
> For Testnet
> [https://api-testnet.bybit.com/open-api/funding/prev-funding-rate](https://api-testnet.bybit.com/open-api/funding/prev-funding-rate)

> For Mainnet
> [https://api.bybit.com/open-api/funding/prev-funding-rate](https://api.bybit.com/open-api/funding/prev-funding-rate)


 
#### HTTP Request Method
 
> get
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|symbol |true |string |Contract type (BTCUSD ETHUSD )    |
 
 
#### Response example
 
```js
 
{
    'ret_code':0   // return code(0：successful、-101：Parameters verification failed)
    'ret_msg':'ok' // error message
    'ext_code':'', // additional error code
    'result': {
        'symbol':'BTCUSD',
        'funding_rate':'0.00375000', // When the Funding Rate is positive, longs pay shorts. When it is negative, shorts pay longs. 
        'funding_rate_timestamp':1539950401 //The time of funding rate generated, UTC timestamp
    },
    'time_now':'1539778407.210858',    UTC timestamp
}
 
```


-----------
## <span id="userlastfundingfee"> Get My Last Funding Fee</span>
#### API Function
 
> Funding settlement occurs every 8 hours at 00:00 UTC, 08:00 UTC and 16:00 UTC. 
> The current intervals's fund fee settlement is based on the previous intervals's fund rate.
> For example, at 16 o'clock, the settlement is based on the fund rate generated at 8 o'clock.
> The fund rate generated at 16 o'clock will be used at 0:00 on the next day.
 
#### URL
 
> For Testnet
> [https://api-testnet.bybit.com/open-api/funding/prev-funding](https://api-testnet.bybit.com/open-api/funding/prev-funding)

> For Mainnet
> [https://api.bybit.com/open-api/funding/prev-funding](https://api.bybit.com/open-api/funding/prev-funding)


 
#### HTTP Request Method
 
> get
 
#### Request Parameters
 
|parameter|required|type|comments|
|:----- |:-------|:-----|----- |
|symbol |true |string |Contract type (BTCUSD ETHUSD )    |
 
 
#### Response example
 
```js
 
{
    'ret_code':0   // return code(0：successful、-101：Parameters verification failed)
    'ret_msg':'ok' // error message
    'ext_code':'', // additional error code
    'result': {
        'symbol':'BTCUSD',
        'side': 'Buy', // Your position side at the time of settlement
        'size': 10,   // Your position size at the time of settlement
        'funding_rate':'0.00375000', // Funding rate for settlement
        'exec_fee': 0.00000116, // Funding fee. When the Funding fee is positive, longs pay shorts. When it is negative, shorts pay longs. 
        'exec_timestamp': 1539950401, // The time of funding settlement occurred, UTC timestamp
    },
    'time_now':'1539778407.210858',    UTC timestamp
    'rate_limit_status':10, // The number of remaining calls for this type of api in the current period(1 min).
}
 
```
