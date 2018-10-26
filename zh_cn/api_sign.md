## 机构api签名
### 获取你的 api_key 和 密钥
<a href="https://testnet.bybit.com/user/api-management">https://testnet.bybit.com/user/api-management</a>

### 频率限制

* 接口访问类限制
  * 对于订单类接口，如创建委托单、查询委托单等，每个账户每分钟可调用800次
  * 对于持仓类接口，如修改杠杆、查询仓位信息等，每个账户每分钟可调用600次

* 业务类限制
  * 未完全成交的活动委托单最多可同时有200个
  * 条件单同一方向最多可同时有5个

### Timing security

##### ·A SIGNED endpoint also requires a parameter, timestamp, to be sent which should be the millisecond timestamp of when the request was created and sent.
##### ·An additional parameter, recvWindow, may be sent to specify the number of milliseconds after timestamp the request is valid for. If recvWindow is not sent, it defaults to 5000.
##### ·The logic is as follows:
```php
if (timestamp < (serverTime + 1000) && (serverTime - timestamp) <= recvWindow) {
  // process request
} else {
  // reject request
}
```
##### ·Serious trading is about timing. Networks can be unstable and unreliable, which can lead to requests taking varying amounts of time to reach the servers. With recvWindow, you can specify that the request must be processed within a certain number of milliseconds or be rejected by the server.

##### ·It recommended to use a small recvWindow of 5000 or less!

### 示例
##### 请求方式：POST
##### 示例接口：order/create
##### 示例语言：PHP
##### 业务参数
字段名 | 字段释义 | 字段值 | 字段类型
:-: | :-: | :-: | :-:
side | 下单方向 | Buy | string
symbol | 产品 | BTCUSD | string
exec_type | 价格类型 | Limit | string
qty | 委托数量 | 1 | int
leverage | 杠杆 | 3 | int
type | 订单类型 | Activity | string
price | 委托价格 | 5991 | int
time_in_force | 执行策略 | GoodTillCancel | string

##### 公共参数
字段名 | 字段释义 | 示例值 | 字段类型
:-: | :-: | :-: | :-:
sign | 签名信息 | 8FTJmO1kCVYU7Yl0SXXOz4faXdzIMyNpInftul3Civc= | string
secret| 密钥 | SaoxTnvUFkMPFg0XjzT5sqQljqB5JNd2633L | string
api_key | api_key | vVZHyVknmOHG6buKpt | string
timestamp | 请求时间戳(毫秒级) | 1540191759000 | int

#### 第一步 ： 将业务参数和timestamp按键值大小进行排序
```php
$params = [
   "side"            => "Buy",
   "symbol"          => "BTCUSD",
   "exec_type"       => "Limit",
   "qty"             => 1,
   "leverage"        => 3,
   "type"            => "Activity",
   "price"           => 5991,
   "time_in_force"   => "GoodTillCancel"，
   "timestamp"       => 1540191759000
];

ksort($params);
```

#### 第二步 ： 将排序后的参数以http GET方式拼接，根据密钥进行 HMAC sha256 加签，然后将得到的结果进行 base64 编码

```php
$signString = '';

foreach ($request_data as $key => $value) {
   if ($key != "" && $value != "") {
       $signString .= "$key=$value&";
   }
}

$sign = base64_encode(hash_hmac('sha256', $signString, $secret, true));
```

#### 第三步: 将前面信息及api_key附加到参数中
```php
$params['sign'] = $sign
$params['api_key'] = "Your API_KEY";
```

#### 第四步 ： 发送 https 请求

### 返回结果示例

字段名 | 字段释义 | 示例值 |
:-: | :-: | :-: | :-:
ret_code | 返回码(0：成功、-1：失败) | 0 
ret_msg | 返回消息 | ok 
ext_code | 补充错误码 | null 
result | 业务数据，对应接口有对应格式 | 276837 
token | 登陆token | sadfsa
time_now | UTC时间戳 | 1540191759.210858
