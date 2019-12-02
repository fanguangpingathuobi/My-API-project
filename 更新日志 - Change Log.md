# Change Log

| Release Time (Beijing Time UTC +8) | API | New / Update | Description |
|-----|-----|-----|-----|
|2019.11.08 19:45| "GET /v1/order/orders/{order-id}/matchresult" & "GET /v1/order/matchresults"|New|Added response field `trade-id`|
|2019.10.18 19:00| GET /v1/account/history   |New|Added account history querying|
|2019.10.12 11:00| POST /v1/dw/withdraw/api/create   |Update|Adjusted ERC20 as default chain for USDT|
|2019.10.11 10:00|   |New|Added cross margin trading|
|2019.10.09 20:00| “GET /market/trade”，“GET /market/history/trade”，“market.$symbol.trade.detail”|Update|Added new response field "trade id"|
|2019.09.25 20:00| GET /v2/account/withdraw/quota  |New|Added withdraw quota querying|
|2019.09.23 15:00| POST /v1/order/orders/{order-id}/submitcancel & POST /v1/order/orders/batchcancel  |Update|Optimized error message|
|2019.09.20 10:00| GET /v2/reference/currencies  |New|Added querying reference information of currency and chains|
|2019.09.19 16:00| WebSocket topic“/market/trade.$symbol.bbo”  |New|Added  best bid/offer update in tick by tick mode|
|2019.09.18 20:00| GET /v1/subuser/aggregate-balance, GET /v1/account/accounts/{sub-uid}, GET /v1/margin/loan-orders, GET /v1/margin/accounts/balance  |New|Added sub account isolated margin trading|
|2019.09.16 15:00| GET /v2/account/deposit/address  |New|Added deposit address querying|
|2019.09.11 17:00| GET v1/stable-coin/quote，POST v1/stable-coin/exchange  |New|Added stable coin exchange|
|2019.09.11 16:00|   |Delete|Removed part of code demo.|
|2019.09.10 10:00| Order related API Except "POST /v1/order/orders/submitCancelClientOrder" & "GET /v1/order/openOrders" |Update|Removed order state values `submitting` and `cancelling`|
|2019.09.09 11:00| POST /v1/order/orders/submitCancelClientOrder  |Update|Revised response message detail|
|2019.09.09 10:00| GET /v1/order/orders; GET /v1/order/matchresults  |Update|Revised description of default value and value range for "start-date" and "end-date"|
|2019.09.02 18:00| POST /v1/order/orders/batchCancelOpenOrders|Update|Revised description of request field `symbol` |
|2019.09.02 16:00|"Stable Coin Exchange" related ||Deleted "Stable Coin Exchange" relavant API|
|2019.08.29 21:00| Order related|New|Add stop-limit order classification|
|2019.08.21 18:00| "GET /v1/order/openOrders"|Update|Corrected query parameters|
|2019.08.05 18:00| "orders.$symbol.update"|New|Added new fileds "client-order-id" and "order-type"。|
|2019.08.02 18:00| "orders.$symbol.update"|Update|Revised the description of field "unfilled-amount"|
|2019.07.23 21:00| "GET /v1/order/orders/{order-id}/matchresult" & "GET /v1/order/matchresults"|New|Added transaction fee deduction details|
|2019.07.23 20:00| GET /v1/fee/fee-rate/get|New|Added transaction fee rate |
|2019.07.22 12:00| GET /v1/order/orders/{order-id}/matchresults; GET /v1/order/matchresults|New|Added new fields `role` to indicate the order was "taker" or "maker"|
|2019.07.11 20:00| POST /v1/order/orders/place; POST /v1/order/orders/submitCancelClientOrder; GET /v1/order/orders/getClientOrder|Update<br />New|Support client order ID。|
|2019.07.08 12:00| Websocket Asset and order topics|Update|Enhanced Websocket heartbeat and rate limit|
|2019.06.14 16:00| POST /v1/dw/withdraw/api/create|Update|support 'fast withdraw' |
|2019.06.17 16:00| GET /v1/stable_coin/exchange_rate; POST /v1/stable_coin/exchange |New| Support user query Stable Coin exchange rate, and perform exchange |
|2019.06.12 16:00| GET /v1/common/symbols|Update|Add more reference information of a symbol|
|2019.06.06 18:00| GET /v1/query/deposit-withdraw|Update|Ehanced the request parameters|
|2019.06.05 20:00| All APIs that need authentication|Update|Set up 3 permission for API Key: Read, Trade and Withdraw|
|2019.06.10 00:00| GET /v1/order/orders;- GET /v1/order/matchresults  |Update|Adjusted query window as 48 hours|
|2019.05.15 10:00| POST /v1/futures/transfer |New|Allow a user to tranfer fund between spot account and future contract account.|
|2019.04.29 19:00| GET /v1/order/history |New|Support historical order querying within 48 hours. With the launching of this new endpoint, the existing REST endpoint “v1/order/orders” will be kept in service. However, the new endpoint “/v1/order/history” will have better service level than the “/v1/order/orders”, especially when the service loading exceeds the threshold of our system, which means in some extremely cases, “v1/order/orders” would become unavailable, but “/v1/order/history” would be kept alive. Meanwhile, Huobi is planning to have a delegated data service to support users’ demands on long-term history data. Once this new service become available, the “v1/order/orders” will be deprecated. We will keep you informed promptly once the timeline determined.|
|2019.04.17 10:00| GET /v1/order/orders |Update|Add clarification on the value range for start-date in documents|
| 2019.04.16 10:00 | GET /v1/order/openOrders |Update| Correct the documents error. Both account-id and symbol are required |
| 2019.01.17 07:00 | Websocket accounts           |Update| - Add subscription parameter model<br>- Subscription does not return frozen balance of sub-account anymore |
| 2018.07.10 11:00 | GET `/market/history/kline`  |Update| The size parameter value range changes from [1-1000] to [1-2000]|
| 2018.07.06 16:00 | POST `/v1/order/orders/place` |Update| Added buy-limit-maker and sell-limit-maker order types|
| 2018.07.06 16:00 | GET `/v1/order/openOrders`<br>POST `/v1/order/orders/batchCancelOpenOrders` | New | |
| 2018.07.02 16:00 | ETF related | New | Support transfer in/out of HB10.|
| 2018.06.20 16:00 | GET `/market/tickers` | New | |