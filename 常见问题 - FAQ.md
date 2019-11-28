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
<a href='http://phabricator.huobidev.com/w/ams/dailylog/duqingxiang/api_qaqa/python_signature_demo/'>Python sigature demo</a>
<a href='http://phabricator.huobidev.com/w/ams/dailylog/duqingxiang/api_qaqa/java_signature_demo/'>JAVA signature demo</a>
<a href='http://phabricator.huobidev.com/w/ams/dailylog/duqingxiang/api_qaqa/cpp_signature_demo/'>C++ signature demo</a>

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
### Q1：为什么创建提币时返回api-not-support-temp-addr错误？
A：因安全考虑，API创建提币时仅支持已在提现地址列表中的地址，暂不支持使用API添加地址至提币地址列表中，需在网页端或APP端添加地址后才可在API中进行提币操作。

### Q2：为什么USDT提币时返回Invaild-Address错误？
A：USDT币种为典型的一币多链币种， 创建提币订单时应填写chain参数对应地址类型，若不填写该字段时，使用该币种的默认链作为chain参数，如果提现时未传chain字段（当前默认chain为usdterc20），提现地址输入OMNI地址，则会返回该错误。  
chain参数可使用值请参考/v2/reference/currencies接口。  
接口文档地址：https://huobiapi.github.io/docs/spot/v1/cn/#apiv2  

### Q3：创建提币时fee字段应该怎么填？
A：请参考/v2/reference/currencies接口返回值，返回信息中withdrawFeeType为提币手续费类型，根据类型选择对应字段设置提币手续费。  
提币手续费类型包含：     
transactFeeWithdraw : 单次提币手续费（仅对固定类型有效，withdrawFeeType=fixed）    	
minTransactFeeWithdraw : 最小单次提币手续费（仅对区间类型有效，withdrawFeeType=circulated）   	
maxTransactFeeWithdraw : 最大单次提币手续费（仅对区间类型和有上限的比例类型有效，withdrawFeeType=circulated or ratio）  	
transactFeeRateWithdraw :  单次提币手续费率（仅对比例类型有效，withdrawFeeType=ratio）       	
接口文档地址：https://huobiapi.github.io/docs/spot/v1/cn/#apiv2   

### Q4：如何查看我的提币额度？
A：请参考/v2/account/withdraw/quota接口返回值，返回信息中包含您查询币种的单次、当日、当前、总提币额度以及剩余额度的信息。  
接口文档地址：https://huobiapi.github.io/docs/spot/v1/cn/#apiv2-3  
备注：若您有大额提币需求，且提币数额超出相关限额，可联系官方客服（发送信息至support@huobi.pro邮箱）进行沟通。  

## API Technical Support
若以上内容任未帮助到您，可选择以下任一方式联系我们：  
1. 加入官方QQ群（火币网API交流群(8) 595882031），进群时请注明UID 和编程语言。
2. 发送邮件至api_service@huobi.com    
邮件渠道问题反馈模板  
为了能够更快的了解和调查您反馈的问题，请按照如下模板向我们反馈问题。  

```
1. UID：
2. Api Key（AccessKeyId）：
3. 完整请求URL：
4. 请求参数：
5. 请求时间点：
6. 接口返回原始数据：
7. 问题说明：（例如操作步骤，字段疑问，问题发生频率）
8. 签名前字符串（签名认证错误时必填）：


注意：Api Key仅能证明您的身份，不会影响您账户的安全，切记不要将Secret Key信息反馈出来，若您不小心将Secret Key暴露，请尽快删除该Api Key，若因此造成您的账户损失我们概不负责。

```
Example：

```
1. UID：123456
2. Api Key（AccessKeyId）：rfhxxxxx-950000847-boooooo3-432c0
3. 完整请求URL： https://api.huobi.pro/v1/account/accounts?&SignatureVersion=2&SignatureMethod=HmacSHA256&Timestamp=2019-11-06T03%3A25%3A39&AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&Signature=HhJwApXKpaLPewiYLczwfLkoTPnFPHgyF61iq0iTFF8%3D
4. 请求参数：无
5. 请求时间点：2019-11-06 11:26:14
6. 接口返回原始数据：{"status":"error","err-code":"api-signature-not-valid","err-msg":"Signature not valid: Incorrect Access key [Access key错误]","data":null}
7. 问题说明：调用接口时发生了错误
8. 签名前字符串（签名认证错误时必填）：
GET\n
api.huobi.pro\n
/v1/account/accounts\n
AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-11-06T03%3A26%3A13

注意：Api Key仅能证明您的身份，不会影响您账户的安全，切记不要将Secret Key信息反馈出来，若您不小心将Secret Key暴露，请尽快删除该Api Key，若因此造成您的账户损失我们概不负责。

```

----------------

