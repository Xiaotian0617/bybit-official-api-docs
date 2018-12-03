## API Signature 

### Getting Your API Key and Secret Key

// For Testnet

<a href="https://testnet.bybit.com/user/api-management">https://testnet.bybit.com/user/api-management</a>
 
 // For Mainnet

<a href="https://www.bybit.com/app/user/api-management">https://www.bybit.com/app/user/api-management</a>

### Limits
 
* API Request Rate Limits
    * For order related API, such as *'place active order'*, *'get active order'*, the rate limit for each account is 800 requests per minute. 
    * For position related API, such as *'leverage adjustment'*, *'get position'*, the rate limit for each account is 600 requests per minute.
 
* Order Count Limits 
    * Each account can hold up to 200 active orders yet to be filled entirely simultaneously.
    * Each account can hold up to 5 conditional orders in the same side simultaneously.
 
### Authentication
 
When calling API, you need to provide your API Key as an identification for every request. Meanwhile, a signature of the request you are making is asked. You need to sign the request data with the corresponding Secret Key.
 
#### Public Parameters
Name | Description | Type | Required | Default value| Comments
:- | :- | :- | :- | :- | :-
api_key | API_KEY requested from platform | string | yes | -- | For identification.
timestamp | timestamp when initiating request, unit: millisecond | int64 | yes | -- | UTC timestamp, server will verify this parameter when it receives the request. Rule of verification: timestamp < server_time + 1000.
recv_window| valid request timespan, unit: millisecond| int | no | 5000 | An http request will be invalid after this time: timestamp+recv_window. This is to prevent replay attacks. 
sign | signature message |  string | yes | no | The signature message which is generated from a certain algorithm.
 
#### How to Sign
1.Concatenate all the public parameters except 'sign' in the format of http GET, by ascending order of parameters' name. To take *'leverage adjustment'* as example, it has two parameters('symbol' and *leverage*), and the result of concatenation is 
 
``` js
var param_str = 'api_key=B2Rou0PLPpGqcU0Vu2&leverage=100&symbol=BTCUSD&timestamp=1541564432';
```

2.Sign the parameters string.
```js
var secret = 't7T0YlFnYXk0Fx3JswQsDrViLg1Gh3DUU5Mr';
var sign = hex(HMAC_SHA256($secret, $param_str));
// sign = 3e5f312ba7bd63caa468a27906b718f3f21b7af5dce4276bf7077f556a3f232c
```

3.Append the signature at the end of parameters string, and send the http request. Currently, we support the following two types of requesting parameters:
 
```http
POST /user/leverage/save HTTP/1.1
Host: api-testnet.bybit.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 144
 
api_key=B2Rou0PLPpGqcU0Vu2&leverage=100&symbol=BTCUSD&timestamp=1541564432&sign=3e5f312ba7bd63caa468a27906b718f3f21b7af5dce4276bf7077f556a3f232c
 
```
 
```http
POST /user/leverage/save HTTP/1.1
Host: api-testnet.bybit.com
content-type: application/json
Content-Length: 183
 
{
    "api_key":"B2Rou0PLPpGqcU0Vu2",
    "leverage":100, 
    "symbol":"BTCUSD", 
    "timestamp":1541564432, 
    "sign":"3e5f312ba7bd63caa468a27906b718f3f21b7af5dce4276bf7077f556a3f232c" 
}
```
 
### Response Messages
 
Name | Description | Sample value|
:-: | :-: | :-:
ret_code | return code(0：success, otherwise failure) | 0
ret_msg | return message | ok
ext_code | external code error| null
result | refer to each API|
 
