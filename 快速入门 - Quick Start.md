# Quick Start

## Preparation

Before you use API, you need to login the website to create API Key with proper permissions.

You can manage your API Key <a href='https://www.hbg.com/zh-cn/apikey/'>here</a>.

Every account can create at most 5 API Key, each of them has three permissions:

- Read permission: It is used to query the data, such as order query, trade query.
- Trade permission: It is used to create order, cancel order and transfer, etc.
- Withdraw permission: It is used to create withdraw order, cancel withdraw order, etc.

Please remember below information after creation:

- `Access Key`  It is used in API request
  
- `Secret Key`  It is used to generate the signature (only visible once after creation)

<aside class="notice">
The API Key can bind one or more IP addresses, we strongly suggest you bind IP address for security purpose. The API Key without IP binding will be expired after 90 days.
</aside>
<aside class="warning">
<red><b>Warning</b></red>: These two keys are important to your account safety, please don't share <b>both</b> of them together to anyone else. If you find your API Key is disposed, please remove it immediately.
</aside> 

## SDK and Demo

**SDK (Suggested)**

[Java](https://github.com/huobiapi/huobi_Java)

[Python3](https://github.com/huobiapi/huobi_Python)

[C++](https://github.com/huobiapi/huobi_Cpp)

**其它代码示例**

https://github.com/huobiapi?tab=repositories

## Interface Type

There are two types of interfac, you can choose the proper one according to your scenario and preferences.

### REST API

REST (Representational State Transfer) is one of the most popular communication mechanism under HTTP, each URL represents a type of resource.

It is suggested to use Rest API for one-off operation, like trading and withdraw.

### WebSocket API

WebSocket is a new protocol in HTML5. It is full-duplex between client and server. The connection can be established by a single handshake, and then server can push the notification to client actively.

It is suggest to use WebSocket API to get data updae, like market data and order update.

**Authentication**

Both API has two levels of authentication:

Public API: It is for basic information and market data. It doesn't need authentication.

Private API: It is for account related operation like trading and account management. Each private API must be authenticated with API Key.

## Access URLs
You can compare the network latency between two domain <u>api.huobi.pro</u> and <u>api-aws.huobi.pro</u>, and then choose the better one for you.

In general, the domain <u>api-aws.huobi.pro</u> is optimized for AWS client, the latency will be lower.

**REST API**

**`https://api.huobi.pro`**  

**`https://api-aws.huobi.pro`**  

**Websocket Feed (market)**

**`wss://api.huobi.pro/ws`**  

**`wss://api-aws.huobi.pro/ws`**  

**Websocket Feed (account and order)**

**`wss://api.huobi.pro/ws/v1`**  

**`wss://api-aws.huobi.pro/ws/v1`**     

<aside class="notice">
Please initiate API calls with non-China IP.
</aside>
<aside class="notice">
It is not recommended to use proxy to access Huobi API because it will introduce high latency and low stability.
</aside>
<aside class="notice">
It is recommended to access Huobi API from AWS Japan for better stability. If your server is in China mainland, it may be not stable.
</aside> 

## Authentication

### Overview

The API request may be tampered during internet, therefore all private API must be signed by your API Key (Secrete Key).

Each API Key has permission property, please check the API permission, and make sure your API key has proper permission.

A valid request consists of below parts:

- API Path: for example <u>api.huobi.pro/v1/order/orders</u>
- API Access Key: The 'Access Key' in your API Key
- Signature Method: The Hash method that is used to sign, it uses **HmacSHA256**
- Signature Versio: The version for the signature protocol, it uses **2**
- Timestamp: The UTC time when the request is sent, e.g. 2017-05-11T16:22:06. It is useful to prevent the request to be intercepted by third-party.
- Parameters: Each API Method has a group of parameters, you can refer to detailed document for each of them. 
  - For GET request, all the parameters must be signed.
  - For POST request, the parameters needn't be signed and they should be put in request body.
- Signature: The value after signed, it is guarantee the signature is valid and the request is not be tempered.

### Signature Method

The signature may be different if the request text is different, thereore the request should be normalized before signing. Below signing steps take the order query as an example:

This is a full URL to query one order:

`https://api.huobi.pro/v1/order/orders?`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`&SignatureMethod=HmacSHA256`

`&SignatureVersion=2`

`&Timestamp=2017-05-11T15:19:30`

`&order-id=1234567890`

#### 1. The request Method (GET or POST), append line break “\n”


`GET\n`

#### 2. The host with lower case, append line break “\n”

`
api.huobi.pro\n
`

#### 3. The path, append line break “\n”

`
/v1/order/orders\n
`

#### 4. The parameters are URI encoded, and ordered by ASCII

For example below is the original parameters:

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`order-id=1234567890`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

<aside class="notice">
Use UTF-8 encoding and URI encoded, the hex must be upper case. For example, The semicolon ':' should be encoded as '%3A', The space should be encoded as '%20'.
</aside>
<aside class="notice">
The 'timestamp' should be formated as 'YYYY-MM-DDThh:mm:ss' and URI encoded.
</aside>

Then above parameter should be ordered like below:


`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx`

`SignatureMethod=HmacSHA256`

`SignatureVersion=2`

`Timestamp=2017-05-11T15%3A19%3A30`

`order-id=1234567890`

#### 5. Use char  “&” to concatenate all parameters


`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890`

#### 6. Assemble the pre-signed text

`GET\n`

`api.huobi.pro\n`

`/v1/order/orders\n`

`AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890`


#### 7. Use the pre-signed text and your Secret Key to generate a signature

1. Use the pre-signed text in above step and your API Secret Key to generate hash code by HmacSHA256 hash function.
2. Encode the hash code with base-64 to generate the signature.

`4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=`

#### 8. Put the signature into request URL

1. Put all the prameter in the URL

2. Append the signature in the URL, with parameter name “Signature”.

Finall, the request sent to API should be:

`https://api.huobi.pro/v1/order/orders?AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D`

## Sub Account

Sub account can be used to isolate the assets and trade, the assets can be transferred between parent and sub account. Sub account can only trade within the sub account. The assets can not be transferred between sub accounts, only parent account has the transfer permission.  

Sub account has independent login password and API Key, they are managed under parent account in website.

Each parent account can create **200** sub accounts, each sub account can create at most **5** API Key, each API key can have either of two permissions: **read** and **trade**.

The sub account API Key can also bind IP address, the expiry policy is the same with parent account.

You can access <a href='https://account.hbg.com/en-us/subaccount/management'>here</a> to create and manage sub account.

The sub account can access all public API (including basic information and market data), below are the private APIs that sub account can access:

API|Description
----------------------|---------------------
[POST /v1/order/orders/place](#fd6ce2a756)	|Create and execute an order
[POST /v1/order/orders/{order-id}/submitcancel](#4e53c0fccd)	|Cancel an order
[POST /v1/order/orders/batchcancel](#ad00632ed5)	|Cancel multiple orders
[POST /v1/order/orders/batchCancelOpenOrders](#open-orders)	|Cancel the open orders
[GET /v1/order/orders/{order-id}](#92d59b6aad)	|Query a specific order
[GET /v1/order/orders](#d72a5b49e7)	|Query orders with criteria
[GET /v1/order/openOrders](#95f2078356)	|Query open orders
[GET /v1/order/matchresults](#0fa6055598)	|Query the order matching result
[GET /v1/order/orders/{order-id}/matchresults](#56c6c47284)	|Query a specific order matching result
[GET /v1/account/accounts](#bd9157656f)	|Query all accounts in current user
[GET /v1/account/accounts/{account-id}/balance](#870c0ab88b)	|Query the specific account balance
[POST /v1/futures/transfer](#e227a2a3e8)	|Transfer with future account
[POST /v1/dw/transfer-in/margin](#0d3c2e7382)|Transfer from spot to margin account
[POST /v1/dw/transfer-out/margin](#0d3c2e7382)|Transfer from margin to spot account
[POST /v1/margin/orders](#48cca1ce88)|Request margin loan
[POST /v1/margin/orders/{order-id}/repay](#48aa7c8199)|Repay the debit for specific order
[GET /v1/margin/loan-orders](#e52396720a)|Query history loan orders
[GET /v1/margin/accounts/balance](#6e79ba8e80)|Query margin account balance

<aside class="notice">
All other APIs couldn't be accessed by sub account, otherwise the API will return “error-code 403”。
</aside>

## Glossary

### Trading symbols

The trading symbols are consist of base currency and quote currency. Take the symbol `BTC/USDT` as an example, `BTC` is the base currency, and `USDT` is the quote currency.  

### Account

The `account-id` defines the Identity for different business type, it can be retrieved from API `/v1/account/accounts` , where the `account-type` is the business types.
The types include:

  1. spot: Spot account
  2. otc: OTC account
  3. margin: Isolated margin account, the detailed currency type is defined in `subType`
  4. super-margin / cross-margin:  Cross-margin account
  5. point: Point card account
  6. minepool: Minepool account
  7. etf: ETF account

### Identity

The frequently used Identities are listed below:

- order-id: The unique identity for order.
- client-order-id: The identity that defined by client. This id is included in order creation request, and will be returned as order-id. This id is valid within 24 hours.
- match-id : The identity for order matching.
- trade-id : The unique identity for the trade.

### Order Type
The order type is consist of trade direction and order classification. Take `buy-market` as an example, the trade direction is `buy`, the operation type is `market`.  

Trade direction includes:
 - buy
 - sell  

Order classification includes:
 - limit: Both of the price and amount should be specifided in order creation request.
 - market : The price is not required in order creation request, you only need to specify either money or amount. The matching and trade will happen automatically according to the request.
 - limit-maker: The price is specified in order creation request as market maker. It will not be matched in the matching queue.
 - ioc: ioc stands for "immediately or cancel", it means the order will be canceled if it couldn't be matched. If the order is partially traded, the remaining part will be canceled.
 - stop-limit: The order will be put into matching queue only when the price reaches the trigger price.

### Order Status

- submitted: The order is submitted, and already in the matching queue, waiting for deal.

- partial-filled: The order is already in the matching queue and partially traded, and is waiting for further matching and trade.

- filled: The order is already traded and not in the matching queue any more.

- partial-canceled: The order is not in the matching queue any more. The status is transferred from 'partial-filled', the order is partially trade, but remaining is canceled.

- canceled: The order is not in the matching queue any more, and completely canceled. There is no trade assoicated with this order.

- canceling: The order is under canceling, but haven't been removed from matching queue yet.


You can refer to <a href='https://www.huobi.com/en-us/guide/'>火币成长学院</a> to get detailed information
