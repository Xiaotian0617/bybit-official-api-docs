

### 委托单

* [创建委托单](#ordercreatepost)

* [我的委托](#ordermylistget)

* [撤单](#ordercancelpost)

### 持仓

* [用户杠杆](#userleverageget)

* [修改用户杠杆](#userleveragesavepost)

* [我的仓位](#positionlistget)

* [更新保证金](#positionchange-position-marginpost)



-----------
## <span id="ordercreatepost">创建委托单 </span>
#### 接口功能

> 创建委托单。

#### URL

> [https://api-testnet.bybit.com/order/create](https://api-testnet.bybit.com/order/create)

#### HTTP请求方式

> post

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |
|side |true |string |方向 (Buy Sell )    |
|symbol |true |string |产品 (BTCUSD ETHUSD )    |
|order_type |true |string |价格类型 (Limit Market )    |
|qty |true |string |数量 |
|leverage |true |string |杠杆,1-100倍范围 |
|type |true |string |订单类型 (Activity Conditions )    |
|price |false |string |委托价格 |
|trigger_price |false |string |条件触发价格 |
|time_in_force |true |string |执行策略 (GoodTillCancel ImmediateOrCancel FillOrKill )    |
|take_profit |false |number |止盈价格 |
|stop_loss |false |number |止损价格 |


#### 返回示例

```js

    {
        'ret_code':0   错误码 - 正确,
        'ret_msg':'ok' 错误消息,
        'ext_code':''  ,
        'result':,     定单id
        'token':'',    登陆token
        'time_now':'1539778407.210858',    UTC时间戳
    }

```

-----------
## <span id="ordermylistget">我的委托 </span>
#### 接口功能

> 获取我的委托单列表。

#### URL

> [https://api-testnet.bybit.com/order/my/list](https://api-testnet.bybit.com/order/my/list)

#### HTTP请求方式

> get

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |
|export |false |string |导出我的委托单列表 (true false )    |
|symbol |false |string |产品 (BTCUSD ETHUSD )    |
|type |false |string |类型 (activity conditions deal )    |
|sort |false |string |排序 (created_at cum_exec_qty qty last_exec_price price cum_exec_value cum_exec_fee closing_profit_loss )    |
|order |false |string |顺序 (desc asc )    |
|page |false |string |页码 |
|limit |false |string |一页数量 |


#### 返回示例

```js

{
    'ret_code':0,
    'ret_msg':'ok',
    'ext_code':'',
    'result':{
        'data':[
            {
                'id':3,
                'clOrdID':'',               uuid类型订单唯一id
                'stop_order_id':0,
                'order_ext_id':1,
                'user_id':,                 用户id
                'symbol':'BTCUSD',          产品类型
                'side':'Buy',               购买方向
                'order_type':'Limit',       订单类型
                'price':6000,               委托价格
                'qty':10,                   委托数量
                'time_in_force':'GoodTillCancel',       执行策略
                'create_type':'CreateByUser',
                'cancel_type':'',
                'order_status':'New',       委托状态
                'last_exec_time':'0.000000',最近一次成交时间
                'last_exec_price':0,        最近一次成交价格
                'leaves_qty':10,            剩余委托数量
                'cum_exec_qty':0,           累计成交数量
                'cum_exec_value':0,         累计成交的名义价值
                'cum_exec_fee':0,           累计已成交手续费
                'reject_reason':'NoError',  被拒单的原因
                'cross_seq':-1,
                'created_at':'2018-10-15T04:12:19.000Z',
                'updated_at':'2018-10-15T04:12:19.000Z',
                'order_value':0.00166667,   委托价值，保留8位小数，四舍五入
                'take_profit_stop_loss':
                {
                    'id':1,
                    'user_id':122481,
                    'ext_status':'Active',
                    'ext':'{\'TakeProfit\':\'7000\',\'StopLoss\':\'5000\',\'MarketPrice\':\'6554\',\'Side\':\'Sell\'}'
                }
            }
        ],
        'current_page':1,
        'total':1
    },
    'token':'DkOivKP1GYJtiuFCzgciQDsel2n8V6A4zjI4jJXZ',
    'time_now':'1539781050.462841'
}

```

-----------
## <span id="ordercancelpost">撤单 </span>
#### 接口功能

> 

#### URL

> [https://api-testnet.bybit.com/order/cancel](https://api-testnet.bybit.com/order/cancel)

#### HTTP请求方式

> post

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |
|order_id |true |string |委托单id |
|type |true |string |订单类型 (Activity Conditions )    |


#### 返回示例

```js

{
    'ret_code':0   错误码 - 正确,
    'ret_msg':'ok' 错误消息,
    'ext_code':''  ,
    'result':,     订单id
    'token':'',    登陆token
    'time_now':'1539778407.210858',    UTC时间戳
}

```

-----------
## <span id="userleverageget">用户杠杆 </span>
#### 接口功能

> 获取用户杠杆。

#### URL

> [https://api-testnet.bybit.com/user/leverage](https://api-testnet.bybit.com/user/leverage)

#### HTTP请求方式

> get

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |


#### 返回示例

```js

{
    'ret_code':0   返回码(0：成功、-1：失败)
    'ret_msg':'ok' 错误消息,
    'ext_code':''  补充错误码,
    'result': {
        'BTCUSD': {
            'leverage': 100
        },
        'ETHUSD': {
            'leverage': 1
        }
    'token':'',    登陆token
    'time_now':'1539778407.210858',    UTC时间戳
}
 
```

-----------
## <span id="userleveragesavepost">修改用户杠杆 </span>
#### 接口功能

> 修改用户杠杆。

#### URL

> [https://api-testnet.bybit.com/user/leverage/save](https://api-testnet.bybit.com/user/leverage/save)

#### HTTP请求方式

> post

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |
|symbol |true |string |产品类型 (BTCUSD ETHUSD )    |
|leverage |true |string |杠杆 |


#### 返回示例

```js

{
    'ret_code':0   返回码(0：成功、-1：失败)
    'ret_msg':'ok' 错误消息,
    'ext_code':''  补充错误码,
    'result': null,根据 ret_code 判断是否请求成功，result返回恒为null
    'token':'',    登陆token
    'time_now':'1539778407.210858',    UTC时间戳
}
 
```

-----------
## <span id="positionlistget">我的仓位 </span>
#### 接口功能

> 获取我的仓位列表。

#### URL

> [https://api-testnet.bybit.com/position/list](https://api-testnet.bybit.com/position/list)

#### HTTP请求方式

> get

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |


#### 返回示例

```js

{
    'ret_code': 0,    返回码(0：成功、-1：失败)
    'ret_msg': 'ok',  错误消息
    'ext_code': '',   补充错误码
    'result': [
        {
            'id': 1,                仓位id
            'user_id': 1,           用户id
            'risk_id': 1,           风险限额id
            'symbol': 'BTCUSD',     产品类型  BTCUSD,ETHUSD
            'side': 'None',         仓位方向  None,buy,sell
            'size': 0,              仓位数量
            'position_value': 0,    仓位价值
            'entry_price': 0,       仓位开仓价
            'leverage': 1,          用户杠杆
            'auto_add_margin': 0,   自动追加保证金开关
            'position_margin': 0,   仓位保证金
            'liq_price': 999999,    强平价格
            'bust_price': 999999,   破产价格
            'occ_closing_fee': 0,   平仓手续费
            'take_profit': 0,       止盈价格
            'stop_loss': 0,         止损价格
            'trailing_stop': 0,     追踪止损点数
            'position_status': 'Normal',   仓位状态  Normal(正常),Liq(强平中),Adl(减仓中)
            'deleverage_indicator': 1,
            'oc_calc_data': '{\'blq\':\'0\',\'bmp\':\'0\',\'slq\':\'0\',\'smp\':\'0\'}',
            'order_margin': 0,      委托预占用保证金
            'wallet_balance': 0,    账户余额
            'realised_pnl': 0,      今日已结盈亏
            'cum_realised_pnl': 0,  累计已结盈亏
            'cum_commission': 0,    累计佣金
            'cross_seq': 0,         撮合返回版本号
            'position_seq': 2,      仓位版本号
            'created_at': '2018-10-18T07:15:51.000Z',
            'updated_at': '2018-10-20T13:43:21.000Z'
        }
    ],
    'token': 'uzNt4DJ4dmQEgBLFqUqzvulRbW0XQNUrqUXZF3bw',   登陆token
    'time_now': '1540043097.995523'      UTC时间戳
}
 *    
```

-----------
## <span id="positionchange-position-marginpost">更新保证金 </span>
#### 接口功能

> 更新保证金

#### URL

> [https://api-testnet.bybit.com/position/change-position-margin](https://api-testnet.bybit.com/position/change-position-margin)

#### HTTP请求方式

> post

#### 请求参数

|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |
|symbol |true |string |产品 (BTCUSD ETHUSD )    |
|margin |true |string |保证金 |


#### 返回示例

```js

{
    'ret_code':0   返回码(0：成功、-1：失败)
    'ret_msg':'ok' 错误消息,
    'ext_code':'', 补充错误码
    'result': null, 根据 ret_code 判断是否请求成功，result返回恒为null
    'token':'',    登陆token
    'time_now':'1539778407.210858',    UTC时间戳
}

```




