# API Access

## Overview

Category| URL Path | Description 
--------- | --------- | -----------
Common |/v1/common/* | Common interface, including currency, currency pair, timestamp, etc 
Market Data |/market/*| Market data interface, including trading, depth, quotation, etc 
Account |/v1/account/*  /v1/subuser/* | Account interface, including account information, sub-account ,etc 
Order |/v1/order/* | Order interface, including order creation, cancellation, query, etc 
Margin|/v1/margin/* | Margin interface, including debit, payment, query, etc 
Cross Margin| /v1/cross-margin/* | Cross margin interface, including debit, payment, query, etc 

- Above is a general category, it doesn't cover all API, you can refer to detailed API document according to your requirement.

## Frequency-limited Rule

- Each API Key is limited to 10 times per second
- If API Key is empty in request, then each IP is limited to 10 times per second

For example

- Order interface is limited by API Key: no more than 10 times within 1 sec
- Market data interface is limited by IP: no more than 10 times within 1 sec

## Request Format

The API is restful and there are two method: GET and POST.
- GET request: All parameters are included in URL
- POST request: All parameters are formated as JSON and put int the request body

## Response Format

The response is JSON format。There are four fields in the top level: `status`, `ch`, `ts` and `data`. The first three fields indicate the general status, the business data is is under `data` field.

Below is an example of response:

```json
{
  "status": "ok",
  "ch": "market.btcusdt.kline.1day",
  "ts": 1499223904680,
  "data": // per API response data in nested JSON object
}
```

Field| Data Type | Description 
--------- | --------- | -----------
status    | string    | Status of API response 
ch        | string    | The data stream. It may be empty as some API doesn't have data stream 
ts        | int       | The UTC timestamp when API respond, the unit is millisecond 
data      | object    | The body data in response 

## Error Message

### Market Data  API Error Message

| Error Message | Description |
|-----|-----|
| bad-request | The request is wrong |
| invalid-parameter | The parameter is invalid |
| invalid-command | The command is invalid |
code 的具体解释, 参考对应的 `err-msg`.

### Order API Error Message

| Error Message | Description |
|-----|-----|
| base-symbol-error | Trade pair doesn't exist |
| base-currency-error | Currency doesn't exist |
| base-date-error | The date format is wrong |
| account for id `12,345` and user id `6,543,210` does not exist| The`account-id` is wrong, please use GET `/v1/account/accounts` to get account |
| account-frozen-balance-insufficient-error | Can not froze due to insufficient balance |
| account-transfer-balance-insufficient-error | Can not transfer due to insufficient balance |
| bad-argument | The arugment is wrong |
| api-signature-not-valid | The API signature is wrong |
| gateway-internal-error | System is too busy |
| ad-ethereum-addresss| The Ethereum address is required |
| order-accountbalance-error| Insufficient balance in account |
| order-limitorder-price-error|The limited order price exceeds limitation |
|order-limitorder-amount-error|The limited order amount exceeds limitation |
|order-orderprice-precision-error|The limited order price exceeds precision limitation |
|order-orderamount-precision-error|The limited order amount exceeds precision limitation|
|order-marketorder-amount-error|The order amount exceeds limitation|
|order-queryorder-invalid|Can not query the order|
|order-orderstate-error|The order status is wrong|
|order-datelimit-error|The query exceeds date limitation|
|order-update-error|The order fail to update|

## Suggestions

1. To get market data: Use WebSocket to subscribe the real time data and cache the data for further usage
2. To get latest trade price: Use `/market/trade` or WebSocket to subscribe `market.$symbol.trade.detail`.
3. To get successful transaction: Use WebSocket to subscribe `orders.$symbol.update`, it has better performance and time-ordered.
4. To change account assents: Use WebSocket to subscribe accounts topic, and regularly call API to get latest data.
