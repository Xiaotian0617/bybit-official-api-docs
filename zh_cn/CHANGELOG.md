<hr>

## 2018-11-09

### Web Socket
- 身份认证的expires参数单位`由秒改为毫秒`
- insurance topic: walletBalance字段名由`walletBalance`改为`wallet_balance`
- execution topic: data字段类型由`object`改为`array`
- order topic: data字段类型由`object`改为`array`
- 订阅公共类topic不再需要先进行身份验证
- 增加auth指令，用于身份验证

<hr>

## 2018-11-07

### Rest API

- api_key参与加签
- 取消对sign的base64 encode
- 增加recv_window参数说明
- 增加示例加签sign，方便快速验证sign是否一致

<hr>
