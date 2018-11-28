## api signature 
### obtain your api_key and secret key
<a href="https://testnet.bybit.com/user/api-management">https://testnet.bybit.com/user/api-management</a>
 
### Rate limits
 
* API request limit
 * for order related API, such as create active order, query active order etc, every account can request 800 times per minute 
 * for position related API, such as leverage adjustment, query position etc, every account can request 600 times per minute
 
* business related limit 
 * can hold up to 200 active orders that have yet to be filled entirely at the same time
 * can hold up to 5 conditional orders in the same direction at the same time 
 
### authentication
 
when calling API, need to provide API Key as an identification for each request and sign the request data with secret key.
 
#### Public parameters
Name | Description | Type | Required | Default value| Comments
:- | :- | :- | :- | :- | :-
api_key | API_KEY requested from platform | string | yes | -- | for identification
timestamp | timestamp when initiating request, unit: millisecond | int64 | yes | -- | UTC timestamp, server will verify this peter when it receives the request, rule of verification: timestamp < server_time + 1000
recv_window| valid request timespan,unit: millisecond| int | no | 5000 | http request will be invalid after this time: timestamp+recv_window, to prevent replay attack 
sign | signature message |  string | yes | no |
signature message generated from certain algorithm
 
#### how to sign
1. concat all the public parameters except 'sign' in the fomart of http GET, order by parameter's name asc.take 'leverage adjustment ' as example:there are two parameters(symbol and leverage),the result of concatnation is 
 
``` js
var param_str = 'api_key=B2Rou0PLPpGqcU0Vu2&leverage=100&symbol=BTCUSD&timestamp=1541564432';
```
 
2. sign the string
```js
var secret = 't7T0YlFnYXk0Fx3JswQsDrViLg1Gh3DUU5Mr';
var sign = hex(HMAC_SHA256($secret, $param_str));
// sign = 3e5f312ba7bd63caa468a27906b718f3f21b7af5dce4276bf7077f556a3f232c
```
 
3.append sign parameter and send http request. currently we support two ways of requesting parameters as follows
 
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
​"api_key":"B2Rou0PLPpGqcU0Vu2",
​"leverage":100, 
​"symbol":"BTCUSD", 
​"timestamp":1541564432, 
​"sign":"3e5f312ba7bd63caa468a27906b718f3f21b7af5dce4276bf7077f556a3f232c" 
}
```
 
### response
 
Name | Description | Sample value|
:-: | :-: | :-:
ret_code | return code(0：successful, others fail) | 0
ret_msg | return message | ok
ext_code | external code error| null
result | refer to each api|
 