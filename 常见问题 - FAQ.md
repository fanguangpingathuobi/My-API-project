# Frequently Asked Questions

## API Announcements
Huobi will publish API announcement in advance for any API change, please subscribe our announcements so that you can get latest update. 

How to subscribe: Login to API Announcements page, click "Follow" button in the top right of the page, then choose the type you want to follow. After you subscribe, the button will changed to "Following". If you don't have any account, you need to register first. 

You can click <a href='https://huobiglobal.zendesk.com/hc/en-us/sections/360000070201-API-Announcements'>Here </a>  to subscribe the announcements.

## Access and Authentication

### Q1：How many API Keys one user can apply?
A:  Every main account can create 5 API Keys, and each API Key has 3 permissions: **read**, **transact** and **withdraw**.
Each main account could create 200 sub-account, and each sub-account could create 5 API Keys, each API key has 2 permissions: **read** and **transact**.
1) Read permission: It is used to query data, for example, **order query**, **deal query**. 
2) Transaction permission: it is used to **place order**, **cancel order** and **transfer**.
3) Withdraw permission: it is used to **schedule withdraw**, **cancel withdraw**.

### Q2：Why APIs are always disconnected or timeout?
A：Please follow below suggestions:
1) It is unstable if the client machine is in China mainland, it is suggested to invoke API from AWS Japan
2) It is suggested to invoke API in domain <u>api.huobi.pro</u> or <u>api-was.huobi.pro</u>.

### Q3：Why the WebSocket is often disconnected?
A：Please check below situations:
1) The client didn't respond 'Pong'. It is requird to respond 'Pong' after receive 'Ping' from server.
2) The server didn't receive 'Pong' successfully due to network issue.
3) The connection is broken due to network issue.
4) It is suggested to implement WebSocket re-connect mechanism. If the connection is broken after Ping/Pong heartbeat, the application should re-connect automatically.

### Q4：What is the difference between <u>api.huobi.pro</u> 与 <u>api-aws.huobi.pro</u>?
A：The domain <u>api-aws.huobi.pro</u> is optimized for AWS client, the latency will be lower.

### Q5：Why the signature authentication always fail?
A：Please compare  your signature text with below example: 

```
GET\n
api.huobi.pro\n
/v1/account/accounts\n
AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-10-28T07%3A28%3A38
```
Please check whether you follow below rules:
1) The parameter in signature text should be ordered by ASCII, for example below is the original parameters:

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
order-id=1234567890
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
```
They should be ordered like below:
```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
order-id=1234567890
```

2) The signature text should be URI encoded, for example

> The semicolon `:`should be encoded as `%3A`, The space should be encoded as `%20`.
> The timestamp should be formated as `YYYY-MM-DDThh:mm:ss` and after encoded it should be like `2017-05-11T15%3A19%3A30`  

3) The signature should be base64 encoded.
4) The parameter for Get request should be included in signature request.
5) The Timestamp should be UTC time and the format should be YYYY-MM-DDTHH:mm:ss.
6) The time difference between your timestamp and standard should be less than 1 minute.
7) The message body doesn't need URI encoded if you are using WebSocket for authentication.
8) The host in signature text should be the same as the host in your API request.
9) The hidden text in API Key and Secret Key may have impact on the signature.

- Right now the official SDK supports 3 language: Java, Python3 and C++, you can choose the one that suitable for you.
<a href='https://github.com/HuobiRDCenter'>Download SDK </a> 
<a href='python_signature_demo.md'>Python sigature demo</a>
<a href='java_signature_demo.md'>JAVA signature demo</a>
<a href='cpp_signature_demo.md'>C++ signature demo</a>

- You can also refer to the signature guide: https://huobiapi.github.io/docs/spot/v1/cn/#c64cd15fdc

### Q6：Why the API return 'gateway-internal-error'?
A：Please check below situations:
1) Check the `account-id`, it should be returned from `GET /v1/account/accounts`.
2) It may be due to network issue, please try again later.
3) The data format should be correct (standard JSON).
4) The `Content-Type` in POST header should be `application/json` 。

### Q7：Why the API return 'login-required'?
A：Please check below situations:
1) The parameter should include `AccessKeyId`.
2) The `account-id` should be returned from `GET /v1/account/accounts`.
3) The request body in POST should NOT included in signature text.
4) The request parameter in GET should be ordered by ASCII.

## Market Data
### Q1：What is the update frequency?
A：The data is updated **once per second**, for both Rest API and WebSocket.

### Q2：Could the trade volume within 24 hours (/market/detail) decrease?
A：The trade volume from `/market/detail`  is for last 24 hours, it is possible that the volume decrease as time goes on.

### Q3：How to retrieve the last trade price?
A：It is suggest to use Rest API `/market/trade` to retrieve last trade, or use WebSocket to subscribe `market.$symbol.trade.detail` topic, the price will be included in the respond.

### Q4：What is the star time for candlestick chart?
A： The start time for candlesticks chart is based on Singapore time, for example, the duration for daily candlestick chart is from 00:00:00 to 23:59:59 Singapore time.

## Order and Trade
### Q1：What is account-id?
A： The `account-id` defines the Identity for different business type, it can be retrieved from API `/v1/account/accounts` , where the `account-type` is the business types.
The types include:

1. spot: Spot account
2. otc: OTC account
3. margin: Isolated margin account, the detailed currency type is defined in `subType`
4. super-margin / cross-margin:  Cross-margin account
5. point: Point card account
6. minepool: Minepool account
7. etf: ETF account

### Q2：What is client-order-id?
A： The `client-order-id` is one parameter of the place order request, it is string type and max length is 64. This id is generated by client, and is valid within 24 hours.

### Q3：How to get the order size, price and decimal precision?
A： You can call API `/v1/common/symbols` to get the currency pair information, pay attention to the difference between the minimum amount and the minimum price.   
Below are common errors:
order-value-min-error: The order price is less than mininum price
order-orderprice-precision-error : The precision for limited order price is wrong 
order-orderamount-precision-error : The precision for limited order amount is wrong
order-limitorder-price-max-error : The limited order price is higher than the threshold
order-limitorder-price-min-error : The limited order prices is lower than the threshold
order-limitorder-amount-max-error : The limited order amount is larger than the threshold
order-limitorder-amount-min-error : The limited order amount is smaller than the threshold  

### Q4：What is the difference between two WebSocket topic 'orders.\$symbol' and 'orders.\$symbol.update'?
A： Below are the difference:
1. The topic `order.$symbol` is the legacy version, we will not update and support in the future, it is suggested to use topic `orders.$symbol.update`.
2. The notification from new topic `orders.$symbol.update` is ordered by time, which follow the orders matching time, and it has lower latency.
3. In order to avoid duplicate data and for better performance, the orignial order information is not included in topic `orders.$symbol.update`. If you require the original order information, you can save them when you place the order, or query the detailed order information when you receive the notification.

### Q5：After I receive a trade successful notification I place a new order, why I get balance insufficient error?
A：In order to update timely with low latency, the notification is sent after the orders match successfully, therefor your assets may not be cleared. If you want to place a new order succesfully, you can follow below ways:
1. You can subscribe the `accounts` notification to make sure your assets are cleared.
2. After you get the trade notification, use Rest API to query your balance to make sure your balance is sufficient.
3. Leave sufficient balance in your account.

### Q6: What is the difference between 'filled-fees' and 'filled-points' in matching result?
A: There are two types of transaction fee, filled-fees and filled-points, and they won't happen together:
1) filled-fees: When the transaction happens, the fee is charged from your bought currency. For example, if you buy BTC/USDT with `filled-fees` value，then the transaction fee will be BTC.
2) filled-points: When the transaction happens, the fee is charged from HT or point card. The unit of the fee should refer to `fee-deduct-currency`.

### Q7: What is the difference between 'match-id' and 'trade-id' in matching result?
A: The `match-id` is the identity for order matching, the `trade-id` is the identity for the transaction. One `match-id` may be correlated with multiple `trade-id`, or no `trade-id`( the order is cancelled).

## Account Deposit and Withdraw
### Q1：Why the API return error 'api-not-support-temp-addr' when withdraw?
A：In order for safe purpose, the withdraw API only support the address in withdraw address list. Right now the address can be updated in website or mobile App manually, the address can NOT be updated by API.

### Q2：Why the API return error 'Invaild-Address' when withdraw USDT?
A：The USDT has multiple chains, therefore the withdraw order request should contains the chain parameter. Below tables the relationship between the Chain and the chain parameter:

| Chain           | chain parameter |
| --------------- | --------------- |
| ERC20 (default) | `usdterc20`     |
| OMNI            | `usdt`          |
| TRX             | `trc20usdt`     |

If the chain parameter is empty, default target chain is `ERC20`. Or you can explicitly set the chain parameter to `usdterc20`.
If the target chain is `OMNI` or `TRX`, the chain parameter should be `usdt` or `trc20usdt`.
The available chain name can be retreived from API `/v2/reference/currencies`
Refer to API document: https://huobiapi.github.io/docs/spot/v1/cn/#apiv2

### Q3：How to assign parameter 'fee' when create withdraw request?
A：Please refer to the response from API `/v2/reference/currencies`, the response field `withdrawFeeType` is the fee type, and the fee should be  assigned according to different fee type: 
transactFeeWithdraw : The fee per time (only applicable for fixed type, withdrawFeeType=fixed）    	
minTransactFeeWithdraw : The minimum fee per time (only applicable for circulated type, withdrawFeeType=circulated)
maxTransactFeeWithdraw : The maximum fee per time (only applicable for circulated or ratio type)	
transactFeeRateWithdraw :  The fee ratio per time (only applicable for ratio type，withdrawFeeType=ratio)   Refer to API document: https://huobiapi.github.io/docs/spot/v1/cn/#apiv2   

### Q4：How to query my withdraw quota?
A：Please check the response fields from API `/v2/account/withdraw/quota`, they contain the quota for once, current day, current time, total and remaining.
接口文档地址: https://huobiapi.github.io/docs/spot/v1/cn/#apiv2-3  
Note: If you need to withdraw large amount and it reaches the limitation, you can contact our official support (for example, send email to support@huobi.pro).

## API Technical Support
If you have any other questions on API, you can contact us by below ways:
1. Join official QQ group (火币网API交流群(8), 595882031), please tell your UID and programming language in your join request.
2. Send email to api_service@huobi.com
In order to better understand your question and respond you quickly, please use below template in your email:

```
1. UID:
2. AccessKey:
3. Full URL request:
4. Request parameters:
5. Request time:
6. Original response:
7. Problem description: (such as steps, field question, frequency)
8. Signature text (mandatory if you have signature authentication issue):
```




Belwo is an example：

```
1. UID：123456
2. AccessKey:rfhxxxxx-950000847-boooooo3-432c0
3. Full URL request: https://api.huobi.pro/v1/account/accounts?&SignatureVersion=2&SignatureMethod=HmacSHA256&Timestamp=2019-11-06T03%3A25%3A39&AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&Signature=HhJwApXKpaLPewiYLczwfLkoTPnFPHgyF61iq0iTFF8%3D
4. Request parameters: N/A
5. Request time: 2019-11-06 11:26:14
6. Original response：{"status":"error","err-code":"api-signature-not-valid","err-msg":"Signature not valid: Incorrect Access key [Access key错误]","data":null}
7. Problem description: API returns error
8. Signature text:
GET\n
api.huobi.pro\n
/v1/account/accounts\n
AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-11-06T03%3A26%3A13
```



Note：It is safe to share your Access Key, which is to prove your identity, and it will not affect your account safety. Remember do **not** share your `Secret Key` to any one. If you expose your `Secret Key` by accident, please [remove](https://www.hbg.com/zh-cn/apikey/) the related API Key immediately.

----------------

