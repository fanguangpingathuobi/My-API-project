# Frequently Asked Questions

## API Announcements
Huobi will publish API announcement in advance for any API change, please subscribe our announcements so that you can get latest update. 

You can click <a href='https://huobiglobal.zendesk.com/hc/en-us/sections/360000070201-API-Announcements'>Here </a>  to subscribe the announcements.

How to subscribe: Login to API Announcements page, click "Follow" button in the top right of the page, then choose the type you want to follow. After you subscribe, the button will changed to "Following". If you don't have any account, you need to register first. 

## Access and Authentication

### Q1：How many API Keys one user can apply?
A:  Every parent user can create 5 API Keys, and each API Key can be granted with either of 3 permissions: **read**, **trade** and **withdraw**.
Each parent user could create up to 200 sub users, and each sub user could create 5 API Keys, each API key can be granted with either of 2 permissions: **read** and **transact**.
1) Read permission: It is used to query data, for example, **query orders**, **query trades**. 
2) Trade permission: it is used to **place order**, **cancel order** and **transfer**.
3) Withdraw permission: it is used to **withdraw**, **cancel withdraw**.

### Q2：Why APIs are always disconnected or timeout?
A：Please follow below suggestions:
1) It is unstable if the client's server locates in China mainland, it is suggested to invoke API from a server at AWS Japan.
2) It is suggested to invoke API only to host <u>api.huobi.pro</u> or <u>api-was.huobi.pro</u>.

### Q3：Why the WebSocket is often disconnected?
A：Please check below possible reasons:
1) The client didn't respond 'Pong'. It is requird to respond 'Pong' after receive 'Ping' from server.
2) The server didn't receive 'Pong' successfully due to network issue.
3) The connection is broken due to network issue.
4) It is suggested to implement WebSocket re-connect mechanism. If Ping/Pong works well but the connection is broken, the application should be able to re-connect automatically.

### Q4：What is the difference between <u>api.huobi.pro</u> 与 <u>api-aws.huobi.pro</u>?
A：The host <u>api-aws.huobi.pro</u> is optimized for AWS client, the latency is lower.

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

- The semicolon `:`should be encoded as `%3A`, The space should be encoded as `%20`.
- The timestamp should be formated as `YYYY-MM-DDThh:mm:ss` and after encoded it should be like `2017-05-11T15%3A19%3A30`  

3) The signature should be base64 encoded.
4) The parameter for Get request should be included in signature request.
5) The Timestamp should be UTC time and the format should be YYYY-MM-DDTHH:mm:ss.
6) The time difference between your timestamp and standard should be less than 1 minute.
7) The message body doesn't need URI encoded if you are using WebSocket for authentication.
8) The host in signature text should be the same as the host in your API request.
9) The hidden text in API Key and Secret Key may have impact on the signature.

- Right now the official SDK supports 3 language: Java, Python3 and C++, you can choose the one that suitable for you.
<a href='https://github.com/HuobiRDCenter'>Download SDK </a> 
<a href='python_signature_demo.md'>Python sigature demo</a>
<a href='java_signature_demo.md'>JAVA signature demo</a>
<a href='cpp_signature_demo.md'>C++ signature demo</a>

- You can also refer to the signature guide: https://huobiapi.github.io/docs/spot/v1/cn/#c64cd15fdc

### Q6：Why the API return 'gateway-internal-error'?
A：Please check below possible reasons:
- Check the `account-id`, it could be returned from `GET /v1/account/accounts`.
- It may be due to network issue, please try again later.
- The data format should be correct (standard JSON).
- The `Content-Type` in POST header should be `application/json` .

### Q7：Why the API return 'login-required'?
A：Please check below possible reasons:
- The parameter should include `AccessKeyId`.
- Check the `account-id` it could be returned from `GET /v1/account/accounts`.
- The request body in POST request should NOT be included in signature text.
- The request parameter in GET request should be ordered by ASCII.

## Market Data

### Q1：What is the update frequency of market depth?
A：The data is updated **once per second**. But, the BBO (Best Bid/Offer) feed upon Websocket subscription to `market.$symbol.bbo` is updating in tick by tick mode.

### Q2：Could the total volume of Last 24h Market Summary (GET /v1/market/detail) decrease?
A：Yes, it is possible that the accumulated volume and the accumulated value counted for current 24h window is smaller than previous.

### Q3：How to retrieve the last trade price in market?
A：It is suggested to request to `GET /v1/market/trade` to get last market price, or to subscribe WebSocket topic `market.$symbol.trade.detail` for getting the same.

### Q4：Which timezone the start time of candlesticks falls into?
A： The start time for candlesticks is based on Singapore time (GMT+8), for example, the duration for daily candlesticks is from 00:00:00 to 23:59:59 Singapore time.

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
A： The `client-order-id` is an optional request parameter while placing order. It's string type which maximum length is 64. The client order id is generated by client, and is only valid within 24 hours.

### Q3：How to get the order size, price and decimal precision?
A： You can call API `GET /v1/common/symbols` to get the currency pair information, pay attention to the difference between the minimum amount and the minimum price.   
Below are common errors:
- order-value-min-error: The order price is less than mininum price
- order-orderprice-precision-error : The precision for limited order price is wrong 
- order-orderamount-precision-error : The precision for limited order amount is wrong
- order-limitorder-price-max-error : The limited order price is higher than the threshold
- order-limitorder-price-min-error : The limited order price is lower than the threshold
- order-limitorder-amount-max-error : The limited order amount is larger than the threshold
- order-limitorder-amount-min-error : The limited order amount is smaller than the threshold  

### Q4：What is the difference between two WebSocket topic 'orders.\$symbol' and 'orders.\$symbol.update'?
A： Below are the difference:
1. The topic `order.$symbol` is the legacy version, which will be no longer supported in the near future. It is strongly recommended to subscribe topic `orders.$symbol.update` instead for getting order updates.
2. The update message sequence of `orders.$symbol.update` strictly follows transaction time, with lower latency.
3. In order to reduce latency, the topic `orders.$symbol.update` doesn't include original order details and transaction fee etc. If you require the original order information or transaction fee details, you may query to corresponding REST API endpoint.

### Q5：Why I got insufficient balance error while placing an order just after a successful order matching?
A：The time order matching update being sent down, the clearing service of that order may be still in progress at backend. Suggest to follow either of below to ensure a successful order submission:
1. Subscribe to Websocket topic `accounts` for getting account balance moves to ensure the completion of asset clearing.
2. Check account balance from REST endpoint to ensure sufficient available balance for the next order submission.
3. Leave sufficient balance in your account.

### Q6: What is the difference between 'filled-fees' and 'filled-points' in match result?
A: Transaction fee can be paid from either of below.
1) filled-fees: Filled-fee is also called transaction fee. It's charged from your income currency from the transaction. For example, if your purchase order of BTC/USDT got matched，the transaction fee will be based on BTC.
2) filled-points: If user enabled transaction fee deduction, the fee should be charged from either HT or Point. User could refer to field `fee-deduct-currency` to get the exact deduction type of the transaction.


### Q7: What is the difference between 'match-id' and 'trade-id' in matching result?
A: The `trade-id` is the unique identifier for each trade. If a taker's order got matched with 3 maker's orders at the same time, it generates 3 trade IDs but only one match ID.

### Q8: Why the order submission could be rejected even though the order price is set as same as current best bid (or best ask)?
A: For some extreme illiquid trading symbols, the best quote price at particular time might be far away from last trade price. But the price limit is actually based on last trade price which could possibly exclude best quote price from valid range for any new order.

## Deposit and Withdraw

### Q1：Why the API returns error 'api-not-support-temp-addr' when withdrawing?
A：User has to include the address into the pre-defined address table on Huobi official website before withdrawing through API.

### Q2：Why the API returns error 'Invaild-Address' when withdraw USDT?
A：USDT locates on multiple chains, therefore the withdraw order should clearly specify which chain the withdrawal goes to. See the table below:

| Chain           | Field Value |
| --------------- | --------------- |
| ERC20 (default) | `usdterc20`     |
| OMNI            | `usdt`          |
| TRX             | `trc20usdt`     |

If leaving the field empty, default target chain is `ERC20`. Or you can explicitly set the chain to `usdterc20`.
If the target chain is `OMNI` or `TRX`, the field value should be `usdt` or `trc20usdt`.
The full chain name list for all currencies can be retrieved from endpoint `GET /v2/reference/currencies`.

### Q3：How to specify 'fee' when creating a withdraw request?

A：Please refer to the response from endpoint `GET /v2/reference/currencies`, where the field `withdrawFeeType` defining different fee types below: 
- transactFeeWithdraw : The withdraw fee per request (only applicable when withdrawFeeType=fixed).    	
- minTransactFeeWithdraw : The minimum withdraw fee per request (only applicable when withdrawFeeType=circulated).
- maxTransactFeeWithdraw : The maximum withdraw fee per request (only applicable when withdrawFeeType=circulated or ratio).
- transactFeeRateWithdraw : The withdraw fee rate per request (only applicable when withdrawFeeType=ratio).

### Q4：How to query my withdraw quota?
A：Please refer to the response from endpoint `GET /v2/account/withdraw/quota`, where quota per request, daily quota, annual quota, overall quota are available.

Note: If you need to withdraw large amount which breaking the limitation, you can contact our official support (support@huobi.pro) for assistance.

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

