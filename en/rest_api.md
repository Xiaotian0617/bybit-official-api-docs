### 活动委托单 Active Order
 
* [Create active order](#open-apiordercreatepost)
 
* [Check active order查询活动委托](#open-apiorderlistget)
 
* [Cancel active order](#open-apiordercancelpost)
 
### 条件委托单 Conditional Order
 
* [Create conditional order](#open-apistop-ordercreatepost)
 
* [Check conditional order查询条件委托](#open-apistop-orderlistget)
 
* [Cancel conditional order](#open-apistop-ordercancelpost)
 
### 持仓 Positions
 
* [User leverage](#userleverageget)
 
* [Change leverage](#userleveragesavepost)
 
* [My position](#positionlistget)
 
* [Change margin](#positionchange-position-marginpost)
 
 
-----------
## <span id="open-apiordercreatepost"> Create active order </span>
#### Port function 接口功能
 
> all active orders have to fill in &#39;side&#39;, &#39;symbol&#39;, &#39;order_type&#39;, &#39;qty&#39;, &#39;price&#39;, &#39;time_in_force&#39;parameter，other parameters can be chosen unless otherwise specified.其它参数除非有特殊说明，否则都是可选的。
Market price active order: a traditional market price order, will be filled at the best available price. 一个传统的市场价格订单,会以当前的最优价格为您成交订单。
 
Limit price active order: you can set an order price for your order, when last traded price reaches the order price, the system will fill your order. 您可以为您的订单设置一个执行价格，当市场价格达到您的设置价格时，系统会为您成交订单。
 
Take profit Stop loss: you may only set a take profit/stop loss conditional order upon opening a position，when submitting an active order while holding a position, the take profit and stop loss set will no longer be valid. 您仅能在开仓时设置止盈止损条件单，一旦持有仓位后提交活动委托时关联的止盈止损则不再有效。
 
Order quantity委托数量: indicates the quantity of perpetual contracts you buy or sell, currently Bybit only support order quantity in an integer.  表示您要购买/卖出的永续合约数，对于委托数量目前Bybit只允许提交正整数。
 
Order price委托价格: indicates the price of perpetual contracts you hope to buy or sell, currently Bybit only support price increment of every 0.5. 表示您期望购买/卖出永续合约的价格，对于委托价格目前Bybit只允许提交以0.5为增幅的正数。
 
Customize conditional order ID: you may customize order ID for active order, we will link the order ID to the system, and return the system unique order ID to you after the active order is created successfully, you may use this order ID to cancel active order, at the same time, you are required to create a customized order ID that is unique with a maximum length of no more than 36 fields.自定义条件单ID: 您可以自定义活动委托订单ID，我们会为您关联到系统的订单ID，并把系统的唯一订单ID在活动委托创建成功后一并返回给您，您可以使用该订单ID去取消活动委托，同时要求您传递的自定义订单ID最大长度不超过36个字段且唯一。
 
 
Please note: active orders created limit is 200.请注意: 创建活动委托单上限为200个。
 
 
 
 
#### URL
 
> [https://api-testnet.bybit.com/open-api/order/create](https://api-testnet.bybit.com/open-api/order/create)
 
#### HTTP request way 请求方式
 
> post
 
#### request parameters
 
|parameter|required|type类型|comments说明|
|:----- |:-------|:-----|----- |
|side |true |string |direction方向, valid option有效选项:Buy, Sell (Buy Sell )    |
|symbol |true |string |contract type产品类型, valid option 有效选项:BTCUSD, ETHUSD (BTCUSD ETHUSD )    |
|order_type |true |string |active order price type委托单价格类型, valid option有效选项:Limit, Market (Limit Market )    |
|qty |true |integer |order quantity委托数量, maximum quantity at 1 million单比最大1百万 |
|price |true |integer |order price, when no position held, order price has to be 10% more and less than 1 million. If there is position held, order price has to be better than liquidation price and the minimum unit of order price  increase or decrease is 0.5.  委托价格, 在没有仓位时，出价需大于市价10%且小于1百万。如有仓位时则需优于强平价. 单笔价格增减最小单位为0.5 |
|time_in_force |true |string |Time in force, valid option:GoodTillCancel, ImmediateOrCancel, FillOrKill (GoodTillCancel ImmediateOrCancel FillOrKill )    |
|order_link_id |false |string |customized order ID, maximum length at 36 fields, and order ID under the same business has to be unique机构自定义订单ID, 最大长度36位，且同一机构下自定义ID不可重复 |
 
 
#### Return example返回示例
 
```js
 
   {
       'ret_code':0   Error code - yes 错误码 - 正确,
       'ret_msg':'ok' Error message错误消息,
       'ext_code':''  ,
       'result':{
           'order_id': 'string',              UUID unique order ID UUID类型订单唯一ID
           'user_id': 0,                      User ID
           'symbol': 'string',                Symbol 产品类型
           'side': 'string',                  Direction 购买方向
           'order_type': 'string',            Order type订单类型
           'price': 0,                        Order price委托价格
           'qty': 0,                          Order quantity委托数量
           'time_in_force': 'string',         Time in force执行策略
           'order_status': 'string',          Order status委托状态: Created: create order创建订单;Rejected: order rejected 订单被拒绝;New: order pending 订单待成交;PartiallyFilled: order filled partially 订单部分成交;Filled: order filled fully 订单全部成交,Cancelled: order cancelled 订单被取消
           'last_exec_time': 0.000000,        Last execution time最近一次成交时间
           'last_exec_price': 0,              Last execution price 最近一次成交价格
           'leaves_qty': 0,                   Remaining order quantity剩余委托数量
           'cum_exec_qty': 0,                 Accumulated execution quantity 累计成交数量
           'cum_exec_value': 0,               Accumulated execution value累计成交的名义价值
           'cum_exec_fee': 0,                 Accumulated execution fee累计已成交手续费
           'reject_reason': 'string',         Reason for rejection 被拒单的原因
           'order_link_id': 'string',         Business customized order ID机构自定义订单ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    UTC timestamp
   }
 
```
 
-----------
## <span id="open-apiorderlistget">Check active order </span>
#### Port function接口功能
 
> Request for active order list 获取我的活动委托单列表。
 
#### URL
 
> [https://api-testnet.bybit.com/open-api/order/list](https://api-testnet.bybit.com/open-api/order/list)
 
#### HTTP请求方式
 
> get
 
#### 请求参数 Request parameters
 
|参数parameters|required必选|type类型|comments说明|
|:----- |:-------|:-----|----- |
|order_id |false |string |order ID |
|order_link_id |false |string |customized order ID 机构自定义订单ID |
|symbol |false |string |Symbol产品类型 (BTCUSD ETHUSD )    |
|sort |false |string |Sort, default sort by created time 排序字段，默认按创建时间排序 (created_at cum_exec_qty qty last_exec_price price cum_exec_value cum_exec_fee )    |
|order |false |string |Ascending descending, default as descending升序降序， 默认降序 (desc asc )    |
|page |false |integer |page, default as first page data 页码，默认取第一页数据 |
|limit |false |integer |one page limit, default as showing 20 data一页数量，一页默认展示20条数据 |
 
 
#### Return example返回示例
 
```js
 
   {
       'ret_code':0,
       'ret_msg':'ok',
       'ext_code':'',
       'result':{
           'data':[
               {
                   'order_id': 'string',       UUID unique order ID类型订单唯一ID
                   'user_id': 0,               User ID 用户ID
                   'symbol': 'string',         Symbol 产品类型
                   'side': 'string',           Direction 购买方向
                   'order_type': 'string',     Order type 订单类型
                   'price': 0,                 Order price 委托价格
                   'qty': 0,                   Order quantity委托数量
                   'time_in_force': 'string',  Time in force执行策略
                   'order_status': 'string',   Order Status委托状态: Created: create order创建订单;Rejected: order rejected 订单被拒绝;New: order pending 订单待成交;PartiallyFilled: order filled partially 订单部分成交;Filled: order filled fully 订单全部成交,Cancelled: order cancelled 订单被取消
                   'last_exec_time': 0.000000 , Last execution time 最近一次成交时间
                   'last_exec_price': 0,       Last execution price最近一次成交价格
                   'leaves_qty': 0,            Remaining order quantity剩余委托数量
                   'cum_exec_qty': 0,          Accumulated execution quantity 累计成交数量
                   'cum_exec_value': 0,        Accumulated execution value累计成交的名义价值
                   'cum_exec_fee': 0,          Accumulated execution fee累计已成交手续费
                   'reject_reason': 'string',  Reason for rejection被拒单的原因
                   'order_link_id': 'string',  Business customized order ID机构自定义订单ID
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
## <span id="open-apiordercancelpost">撤销活动委托单 Cancel active order</span>
#### 接口功能 port function
 
> all cancelled active orders have to fill in 所有撤销活动委托都必须填写 &#39;order_id&#39; will return the 36 fields unique order ID to you after the active order is created successfully 在您创建活动委托成功时会为您返回36位唯一的订单ID。
 
You may cancel active order that are unfilled and partially filled. Fully filled order cannot be cancelled. 您可以撤销未成交、部分成交的活动委托单。但全部成交的活动委托不可取消。
 
#### URL
 
> [https://api-testnet.bybit.com/open-api/order/cancel](https://api-testnet.bybit.com/open-api/order/cancel)
 
#### HTTP请求方式 request way
 
> post
 
#### 请求参数 request parameters
 
|参数parameters|required必选|type类型|comments说明|
|:----- |:-------|:-----|----- |
|order_id |true |string |active order ID, data derived from unique order ID return from active order created活动委托单ID, 数据来自创建活动委托单返回的订单唯一ID |
 
 
#### 返回示例 return example
 
```js
 
   {
       'ret_code':0   Error code - yes 错误码 - 正确,
       'ret_msg':'ok' Error message错误消息,
       'ext_code':''  ,
       'result': {
           'order_id': 'string',              UUID unique order ID类型订单唯一ID
           'user_id': 0,                      User ID
           'symbol': 'string',                Symbol 产品类型
           'side': 'string',                  Direction 购买方向
           'order_type': 'string',            Order type订单类型
           'price': 0,                        Order price委托价格
           'qty': 0,                          Order quantity委托数量
           'time_in_force': 'string',         Time in force执行策略
           'order_status': 'string',          Order status委托状态: Created: create order创建订单;Rejected: order rejected 订单被拒绝;New: order pending 订单待成交;PartiallyFilled: order filled partially 订单部分成交;Filled: order filled fully 订单全部成交,Cancelled: order cancelled 订单被取消
           'last_exec_time': 0.000000,        Last execution time最近一次成交时间
           'last_exec_price': 0,              Last execution price 最近一次成交价格
           'leaves_qty': 0,                   Remaining order quantity剩余委托数量
           'cum_exec_qty': 0,                 Accumulated execution quantity 累计成交数量
           'cum_exec_value': 0,               Accumulated execution value累计成交的名义价值
           'cum_exec_fee': 0,                 Accumulated execution fee累计已成交手续费
           'reject_reason': 'string',         Reason for rejection 被拒单的原因
           'order_link_id': 'string',         Business customized order ID机构自定义订单ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    UTC timestamp
   }
 
```
 
-----------
## <span id="open-apistop-ordercreatepost"> Create conditional order 创建条件委托单 </span>
#### 接口功能 port function
 
> all active orders have to fill in 所有活动委托都必须填写 &#39;side&#39;, &#39;symbol&#39;, &#39;order_type&#39;, &#39;qty&#39;, &#39;price&#39;, &#39;stop_px&#39;, &#39;time_in_force&#39; parameter，other parameters can be chosen unless otherwise specified. 参数，其它参数除非有特殊说明，否则都是可选的。
 
Market price active order: a traditional market price order, will be filled at the best available price. 一个传统的市场价格订单,会以当前的最优价格为您成交订单。
 
Limit price active order: you can set an order price for your order, when last traded price reaches the order price, the system will fill your order. 您可以为您的订单设置一个执行价格，当市场价格达到您的设置价格时，系统会为您成交订单。
 
Take profit Stop loss: you may only set a take profit/stop loss conditional order upon opening a position，when submitting an active order while holding a position, the take profit and stop loss set will no longer be valid. 您仅能在开仓时设置止盈止损条件单，一旦持有仓位后提交活动委托时关联的止盈止损则不再有效。
 
Order quantity委托数量: indicates the quantity of perpetual contracts you buy or sell, currently Bybit only support order quantity in an integer.  表示您要购买/卖出的永续合约数，对于委托数量目前Bybit只允许提交正整数。
 
Order price委托价格: indicates the price of perpetual contracts you hope to buy or sell, currently Bybit only support price increment of every 0.5. 表示您期望购买/卖出永续合约的价格，对于委托价格目前Bybit只允许提交以0.5为增幅的正数。
 
Conditional order trigger price: you may set a trigger price for your conditional order,conditional order does not enter the order book, only when last traded price reaches trigger price will the conditional order enter the trading system. When last traded price hits trigger price: 1) your limit conditional order enters order book, waiting to be executed; 2) your marker conditional order will be executed immediately at the best available market price.件委托触发价格: 您可以为您的条件委托单设置一个触发价格，条件委托单不进入委托表（Order Book)，只有触发条件成立如市场价格到达触发价格时，条件委托单才会进入交易系统。当市场价格到达触发价格：1）您的限价条件委托单进入Order Book，等待被执行；2）您的市价条件委托单将按照市场最优价格立即被执行。
 
Customize conditional order ID: you may customize order ID for active order, we will link the order ID to the system, and return the system unique order ID to you after the active order is created successfully, you may use this order ID to cancel active order, at the same time, you are required to create a customized order ID that is unique with a maximum length of no more than 36 fields.自定义条件单ID: 您可以自定义活动委托订单ID，我们会为您关联到系统的订单ID，并把系统的唯一订单ID在活动委托创建成功后一并返回给您，您可以使用该订单ID去取消活动委托，同时要求您传递的自定义订单ID最大长度不超过36个字段且唯一。
 
Please note: Setting take profit or stop loss order is not supported when opening conditional entry position, if setting take profit and stop loss conditional order is required, please use active order port. Only a maximum of 10 conditional orders are allowed (not inclusive of take profit and stop loss order).
请注意: 开仓设置条件单是是不允许创建止盈止损条件单，如需开仓设置止盈止损条件单请使用创建活动委托接口。只允许最多创建10个条件委托单(不包含止盈止损)
 
 
#### URL
 
> [https://api-testnet.bybit.com/open-api/stop-order/create](https://api-testnet.bybit.com/open-api/stop-order/create)
 
#### HTTP请求方式 request way
 
> post
 
#### 请求参数 request parameter
 
|参数parameters|required必选|type类型|comments说明|
|:----- |:-------|:-----|----- |
|side |true |string |direction方向, valid option有效选项:Buy, Sell (Buy Sell )    |
|symbol |true |string |contract type产品类型, valid option 有效选项:BTCUSD, ETHUSD (BTCUSD ETHUSD )    |
|order_type |true |string |active order price type委托单价格类型, valid有效选项:Limit, Market (Limit Market )    |
|qty |true |integer |order quantity委托数量, maximum quantity at 1 million单比最大1百万 |
|price |true |integer |order price, when no position held, order price has to be 10% more and less than 1 million. If there is position held, order price has to be better than liquidation price and the minimum unit of order price  increase or decrease is 0.5.  委托价格, 在没有仓位时，出价需大于市价10%且小于1百万。如有仓位时则需优于强平价. 单笔价格增减最小单位为0.5 |
|time_in_force |true |string |Time in force, valid option:GoodTillCancel, ImmediateOrCancel, FillOrKill (GoodTillCancel ImmediateOrCancel FillOrKill )    |
|order_link_id |false |string |customized order ID, maximum length at 36 fields, and order ID under the same business has to be unique机构自定义订单ID, 最大长度36位，且同一机构下自定义ID不可重复 |
 
 
#### 返回示例 return example
 
```js
 
   {
       'ret_code':0   Error code - yes 错误码 - 正确,
       'ret_msg':'ok' Error message 错误消息,
       'ext_code':''  ,
       'result':{
            'order_id': 'string',             UUID unique order ID类型订单唯一ID
           'user_id': 0,                      User ID
           'symbol': 'string',                Symbol 产品类型
           'side': 'string',                  Direction 购买方向
           'order_type': 'string',            Order type订单类型
           'price': 0,                        Order price委托价格
           'qty': 0,                          Order quantity委托数量
           'time_in_force': 'string',         Time in force执行策略
           'order_status': 'string',          Order status委托状态: Created: create order创建订单;Rejected: order rejected 订单被拒绝;New: order pending 订单待成交;PartiallyFilled: order filled partially 订单部分成交;Filled: order filled fully 订单全部成交,Cancelled: order cancelled 订单被取消
           'last_exec_time': 0.000000,        Last execution time最近一次成交时间
           'last_exec_price': 0,              Last execution price 最近一次成交价格
           'leaves_qty': 0,                   Remaining order quantity剩余委托数量
           'cum_exec_qty': 0,                 Accumulated execution quantity 累计成交数量
           'cum_exec_value': 0,               Accumulated execution value累计成交的名义价值
           'cum_exec_fee': 0,                 Accumulated execution fee累计已成交手续费
           'reject_reason': 'string',         Reason for rejection 被拒单的原因
           'order_link_id': 'string',         Business customized order ID机构自定义订单ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       },
       'time_now':'1539778407.210858',    UTC timestamp
 
   }
 
```
 
-----------
## <span id="open-apistop-orderlistget">查询条件委托 check conditional order </span>
#### 接口功能 port function
 
> obtain my conditional order list获取我的条件委托单列表。
 
#### URL
 
> [https://api-testnet.bybit.com/open-api/stop-order/list](https://api-testnet.bybit.com/open-api/stop-order/list)
 
#### HTTP请求方式 request way
 
> get
 
#### 请求参数 request parameter
 
|参数parameter|required必选|type类型|comments说明|
|:----- |:-------|:-----|----- |
|stop_order_id |false |string |Order ID of conditional order条件委托订单ID |
|order_link_id |false |string |Business customized order ID机构自定义订单ID |
|symbol |false |string |Symbol产品类型 (BTCUSD ETHUSD )    |
|sort |false |string |Sort, default sort by created time 排序字段，默认按创建时间排序 (created_at)    |
|order |false |string |Ascending descending, default as descending升序降序， 默认降序 (desc asc )    |
|page |false |integer |page, default as first page data 页码，默认取第一页数据 |
|limit |false |integer |one page limit, default as showing 20 data一页数量，一页默认展示20条数据 |
 
 
#### 返回示例 return example
 
```js
 
   {
        'ret_code':0   Error code - yes 错误码 - 正确,
       'ret_msg':'ok' Error message 错误消息,
       'ext_code':''  ,
       'result':{
            'order_id': 'string',             UUID unique order ID类型订单唯一ID
           'user_id': 0,                     User ID
 
                   'stop_order_status': 'string',  Conditional order status: untriggered 条件单状态: Untriggered: Pending for last traded price to trigger conditional order: triggered 等待市价触发条件单; Triggered: last traded price has triggered conditional order 市价已触发条件单; Cancelled: Cancelled 取消; Active: Conditional order has been triggered and order is executed successfully 条件单触发成功且下单成功; Rejected: Conditional order has been triggered but order is executed unsuccessfully 条件触发成功但下单失败
                   'symbol': 'string',             Symbol 产品类型
                   'side': 'string',               Direction购买方向
                   'order_type': 'string',         Price type 价格类型
                   'price': 0,                     Order price委托价格
                   'qty': 0,                       Order quantity委托数量
                   'time_in_force': 'string',      Time in force执行策略
                   'stop_order_type': 'string',    Order type订单类型
                   'base_price': 0,                Base price下单时市价
                   'stop_px': 0,                   Trigger price触发价格
                   'order_link_id': 'string',      Business customized order ID 机构自定义订单ID
                   'created_at':'2018-10-15T04:12:19.000Z',
                   'updated_at':'2018-10-15T04:12:19.000Z',
               }
           ],
           'current_page':1,
           'total':1
       },
       'time_now':'1539781050.462841'
   }
 
```
 
-----------
## <span id="open-apistop-ordercancelpost">撤消条件委托单 Cancel conditional order </span>
#### 接口功能 Port function
 
> All cancelled conditional order has to fill in &#39;order_id&#39; will be your 36 digit unique order ID when you successfully created a conditional order 所有撤销条件委托都必须填写 &#39;order_id&#39;，在您创建条件委托成功时会为您返回36位唯一的订单ID。
 
You may cancel every untriggered conditional order. Essentially, after a conditional order is triggered, it will become an active order. So, when a conditional order is triggered, cancellation has to be done through the active order port for all unfilled or partial filled active order. Similarly, order that has been fully filled cannot be cancelled. 您可以撤销所有未被激活的条件委托。本质上所有条件委托在被激活后都是属于活动委托，所以条件委托一旦被激活，您需要通过调用取消活动委托接口来取消所有未成交、部分成交的活动委托单。同样全部成交的活动委托不可取消。
 
#### URL
 
> [https://api-testnet.bybit.com/open-api/stop-order/cancel](https://api-testnet.bybit.com/open-api/stop-order/cancel)
 
#### HTTP请求方式 request way
 
> post
 
#### 请求参数
 
|参数parameter|必选required|类型type | comments说明|
|:----- |:-------|:-----|----- |
|order_id |true |string | Order ID, the data comes from the order returned by the creation of the active order unique ID 委托单ID, 数据来自创建活动委托单返回的订单唯一ID |
 
 
#### 返回示例 return example
 
```js
 
   {
        'ret_code':0   Error code - yes 错误码 - 正确,
       'ret_msg':'ok' Error message 错误消息,
       'ext_code':''  ,
       'result':{
            'order_id': 'string',             UUID unique order ID类型订单唯一ID
           'user_id': 0,                      User ID
           'stop_order_status': 'string',  untriggered 条件单状态: Untriggered: Pending for last traded price to trigger conditional order: triggered 等待市价触发条件单; Triggered: last traded price has triggered conditional order 市价已触发条件单; Cancelled: Cancelled 取消; Active: Conditional order has been triggered and order is executed successfully 条件单触发成功且下单成功; Rejected: Conditional order has been triggered but order is executed unsuccessfully 条件触发成功但下单失败                                                                      
           'symbol': 'string',                Symbol 产品类型
'side': 'string',                  Direction 购买方向
           'order_type': 'string',            Order type订单类型
           'price': 0,                        Order price委托价格
           'qty': 0,                          Order quantity委托数量
           'time_in_force': 'string',         Time in force执行策略
           'stop_order_type': 'string',       Order type订单类型
           'base_price': 0,                 Base price 下单时市价
           'stop_px': 0,                     触发价格
           'order_link_id': 'string',         Business customized order ID 机构自定义订单ID
           'created_at':'2018-10-15T04:12:19.000Z',
           'updated_at':'2018-10-15T04:12:19.000Z',
       }
       'time_now':'1539778407.210858',    UTC timestamp
   }
 
```
 
-----------
## <span id="userleverageget">用户杠杆 User leverage</span>
#### 接口功能 port function
 
> 获取用户杠杆。Get user leverage
 
#### URL
 
> [https://api-testnet.bybit.com/user/leverage](https://api-testnet.bybit.com/user/leverage)
 
#### HTTP请求方式 request way
 
> get
 
#### 请求参数 request parameter
 
|参数parameter|必选required|类型type|comments说明|
|:----- |:-------|:-----|----- |
 
 
#### 返回示例 return example
 
```js
 
{
   'ret_code':0   return code (0: successful, -1: failed) 返回码(0：成功、-1：失败)
   'ret_msg':'ok' error message错误消息,
   'ext_code':''  error code 补充错误码,
   'result': {
       'BTCUSD': {
           'leverage': 100
       },
       'ETHUSD': {
           'leverage': 1
       }
   'time_now':'1539778407.210858',    UTC timestamp
}
 
```
 
-----------
## <span id="userleveragesavepost">修改用户杠杆 Change user leverage</span>
#### 接口功能 port function
 
> 修改用户杠杆。Change user leverage
 
#### URL
 
> [https://api-testnet.bybit.com/user/leverage/save](https://api-testnet.bybit.com/user/leverage/save)
 
#### HTTP请求方式 request way
 
> post
 
#### 请求参数 request parameter
 
|参数parameter|必选required|类型type|comments说明|
|:----- |:-------|:-----|----- |
|symbol |true |string |Symbol 产品类型 (BTCUSD ETHUSD )    |
|leverage |true |string |leverage 杠杆 |
 
 
#### 返回示例 return example
 
```js
 
{
   'ret_code':0   return code (0:successful, -1: failed) 返回码(0：成功、-1：失败)
   'ret_msg':'ok' error message错误消息,
   'ext_code':''  error code补充错误码,
   'result': null, depending on ret_code to determine whether request is successful, return result is constant at null 根据 ret_code 判断是否请求成功，result返回恒为null
   'time_now':'1539778407.210858',    UTC timestamp
}
 
```
 
-----------
## <span id="positionlistget">我的仓位 My position</span>
#### 接口功能 port function
 
> 获取我的仓位列表。Get my position list
 
#### URL
 
> [https://api-testnet.bybit.com/position/list](https://api-testnet.bybit.com/position/list)
 
#### HTTP请求方式 request way
 
> get
 
#### 请求参数 request parameter
 
|参数parameter|必选required|类型type|comments说明|
|:----- |:-------|:-----|----- |
 
 
#### 返回示例 return example
 
```js
 
{
   'ret_code': 0,    return code (0:successful, -1:failed)返回码(0：成功、-1：失败)
   'ret_msg': 'ok',  error message错误消息
   'ext_code': '',   error code补充错误码
   'result': [
       {
           'id': 1,                position ID 仓位id
           'user_id': 1,           user ID 用户id
           'risk_id': 1,           risk limit ID 风险限额id
           'symbol': 'BTCUSD',     symbol 产品类型  BTCUSD,ETHUSD
           'side': 'None',         direction none, buy,sell仓位方向  None,buy,sell
           'size': 0,              position size 仓位数量
           'position_value': 0,    position value仓位价值
           'entry_price': 0,       entry price 仓位开仓价
           'leverage': 1,          用户杠杆
           'auto_add_margin': 0,   auto margin replenishment switch自动追加保证金开关
           'position_margin': 0,   position margin仓位保证金
           'liq_price': 999999,    liquidation price强平价格
           'bust_price': 999999,   bankruptcy price破产价格
           'occ_closing_fee': 0,   position closing fee平仓手续费
           'take_profit': 0,       take profit price止盈价格
           'stop_loss': 0,         stop loss price止损价格
           'trailing_stop': 0,     trailing stop point 追踪止损点数
           'position_status': 'Normal',   Status Normal(normal), Liq(Liquidation in process), ADL(ADL in process) 仓位状态  Normal(正常),Liq(强平中),Adl(减仓中)
           'deleverage_indicator': 1,
           'oc_calc_data': '{\'blq\':\'0\',\'bmp\':\'0\',\'slq\':\'0\',\'smp\':\'0\'}',
           'order_margin': 0,      Used margin by order委托预占用保证金
           'wallet_balance': 0,    wallet balance账户余额
           'realised_pnl': 0,      daily realized profit and loss今日已结盈亏
           'cum_realised_pnl': 0,  Total realized profit and loss累计已结盈亏
           'cum_commission': 0,    Total commissions累计佣金
           'cross_seq': 0,         return sequence number撮合返回版本号
           'position_seq': 2,      position sequence number仓位版本号
           'created_at': '2018-10-18T07:15:51.000Z',
           'updated_at': '2018-10-20T13:43:21.000Z'
       }
   ],
   'time_now': '1540043097.995523'      UTC timestamp
}
*    
```
 
-----------
## <span id="positionchange-position-marginpost">更新保证金 Change position margin</span>
#### 接口功能 port function
 
> 更新保证金 update margin
 
#### URL
 
> [https://api-testnet.bybit.com/position/change-position-margin](https://api-testnet.bybit.com/position/change-position-margin)
 
#### HTTP请求方式 request way
 
> post
 
#### 请求参数 request parameter
 
|参数parameter|required必选|type类型|comments说明|
|:----- |:-------|:-----|----- |
|symbol |true |string |symbol 产品 (BTCUSD ETHUSD )    |
|margin |true |string |margin 保证金 |
 
 
#### 返回示例 return example
 
```js
 
{
   'ret_code':0   return code(0:successful, -1:failed)返回码(0：成功、-1：失败)
   'ret_msg':'ok' error message错误消息,
   'ext_code':'', error code 补充错误码
  'result': null, depending on ret_code to determine whether request is successful, return result is constant at null 根据 ret_code 判断是否请求成功，result返回恒为null
   'time_now':'1539778407.210858',    UTC timestamp
}
 
```
 
