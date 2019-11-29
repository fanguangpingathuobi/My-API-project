# Reference Data

## Get all Supported Trading Symbol

This endpoint returns all Huobi's supported trading symbol.

```shell
curl "https://api.huobi.pro/v1/common/symbols"
```

### HTTP Request

`GET /v1/common/symbols`

### Request Parameters

No parameter is needed for this endpoint.

> Responds:

```json
  "data": [
   {"base-currency":"etc",
    "quote-currency":"usdt",
    "price-precision":6,
    "amount-precision":4,
    "symbol-partition":"default",
    "symbol":"etcusdt",
    "state":"online",
    "value-precision":8,
    "min-order-amt":0.001,
    "max-order-amt":10000,
    "min-order-value":0.0001
    },
    {
    "base-currency":"ltc",
    "quote-currency":"usdt",
    "price-precision":6,
    "amount-precision":4,
    "symbol-partition":"main",
    "symbol":"ltcusdt",
    "state":"online",
    "value-precision":8,
    "min-order-amt":0.001,
    "max-order-amt":10000,
    "min-order-value":100,
    "leverage-ratio":4
    }
  ]
```

### Response Content

Field           | Data Type | Description
---------       | --------- | -----------
base-currency   | string    | Base currency in a trading symbol
quote-currency  | string    | Quote currency in a trading symbol
price-precision | integer   | Quote currency precision when quote price(decimal places)
amount-precision| integer   | Base currency precision when quote amount(decimal places)
symbol-partition| string    | Trading section, possible values: [main，innovation]
symbol          | string    | 
state           | string    | The status of the symbol；Allowable values: [online，offline,suspend]. "online" - Listed, available for trading, "offline" - de-listed, not available for trading， "suspend"-suspended for trading
value-precision | integer   | Precision of value in quote currency (value = price * amount)
min-order-amt   | long      | Minimum order amount (order amount is the ‘amount’ defined in ‘v1/order/orders/place’ when it’s a limit order or sell-market order)
max-order-amt   | long      | Max order amount
min-order-value | long      | Minimum order value (order value refers to ‘amount’ * ‘price’ defined in ‘v1/order/orders/place’ when it’s a limit order or ‘amount’ when it’s a buy-market order)
leverage-ratio  | int       | The applicable leverage ratio

## 获取所有币种

此接口返回所有火币全球站支持的币种。


```shell
curl "https://api.huobi.pro/v1/common/currencys"
```

## Get all Supported Currencies

This endpoint returns all Huobi's supported trading currencies.

```shell
curl "https://api.huobi.pro/v1/common/currencys"
```

### HTTP Request

`GET /v1/common/currencys`

### Request Parameters

No parameter is needed for this endpoint.

> Response:

```json
  "data": [
    "usdt",
    "eth",
    "etc"
  ]
```

### Response Content

<aside class="notice">The returned "data" field contains a list of string with each string represents a suppported currency.</aside>


## APIv2 - Currency & Chains

API user could query static reference information for each currency, as well as its corresponding chain(s). (Public Endpoint)

### HTTP Request

`GET https://api.huobi.pro/v2/reference/currencies`

```shell
curl "https://api.huobi.pro/v2/reference/currencies?currency=usdt"
```

### Request Parameters

| Field Name       | Mandatory | Data Type     | Description     |Value Range |
| ---------- | ---- | ------ | ------ | ---- |
| currency | false | string | Currency   |  btc, ltc, bch, eth, etc ...(available currencies in Huobi Global) |
| authorizedUser | false | boolean | Authorized user   |  true or false (if not filled, default value is true) |

> Response:

```json
{
    "code":200,
    "data":[
        {
            "chains":[
                {
                    "chain":"trc20usdt",
                    "depositStatus":"allowed",
                    "maxTransactFeeWithdraw":"1.00000000",
                    "maxWithdrawAmt":"280000.00000000",
                    "minDepositAmt":"100",
                    "minTransactFeeWithdraw":"0.10000000",
                    "minWithdrawAmt":"0.01",
                    "numOfConfirmations":999,
                    "numOfFastConfirmations":999,
                    "withdrawFeeType":"circulated",
                    "withdrawPrecision":5,
                    "withdrawQuotaPerDay":"280000.00000000",
                    "withdrawQuotaPerYear":"2800000.00000000",
                    "withdrawQuotaTotal":"2800000.00000000",
                    "withdrawStatus":"allowed"
                },
                {
                    "chain":"usdt",
                    "depositStatus":"allowed",
                    "maxWithdrawAmt":"19000.00000000",
                    "minDepositAmt":"0.0001",
                    "minWithdrawAmt":"2",
                    "numOfConfirmations":30,
                    "numOfFastConfirmations":15,
                    "transactFeeRateWithdraw":"0.00100000",
                    "withdrawFeeType":"ratio",
                    "withdrawPrecision":7,
                    "withdrawQuotaPerDay":"90000.00000000",
                    "withdrawQuotaPerYear":"111000.00000000",
                    "withdrawQuotaTotal":"1110000.00000000",
                    "withdrawStatus":"allowed"
                },
                {
                    "chain":"usdterc20",
                    "depositStatus":"allowed",
                    "maxWithdrawAmt":"18000.00000000",
                    "minDepositAmt":"100",
                    "minWithdrawAmt":"1",
                    "numOfConfirmations":999,
                    "numOfFastConfirmations":999,
                    "transactFeeWithdraw":"0.10000000",
                    "withdrawFeeType":"fixed",
                    "withdrawPrecision":6,
                    "withdrawQuotaPerDay":"180000.00000000",
                    "withdrawQuotaPerYear":"200000.00000000",
                    "withdrawQuotaTotal":"300000.00000000",
                    "withdrawStatus":"allowed"
                }
            ],
            "currency":"usdt",
            "instStatus":"normal"
        }
        ]
}

```

### Response Content


| Field Name | Mandatory  | Data Type | Description   | Value Range |
| ---- | ----- | ---- | ---- | ---- |
| code| true | int | Status code |      |
| message| false | string | Error message (if any) |      |
| data| true | object |  |      |
|   { currency | true | string | Currency |      |
|      { chains| true | object |  |      |
|        chain| true | string | Chain name |      |
|        numOfConfirmations| true | int | Number of confirmations required for deposit success (trading & withdrawal allowed once reached) |      |
|        numOfFastConfirmations| true | int | Number of confirmations required for quick success (trading allowed but withdrawal disallowed once reached) |      |
|        minDepositAmt| true | string | Minimal deposit amount in each request |      |
|        depositStatus| true | string | Deposit status | allowed,prohibited     |
|        minWithdrawAmt| true | string | Minimal withdraw amount in each request |      |
|        maxWithdrawAmt| true | string | Maximum withdraw amount in each request |      |
|        withdrawQuotaPerDay| true | string | Maximum withdraw amount in a day |      |
|        withdrawQuotaPerYear| true | string | Maximum withdraw amount in a year |      |
|        withdrawQuotaTotal| true | string |Maximum withdraw amount in total |      |
|        withdrawPrecision| true | int |Withdraw amount precision |      |
|        withdrawFeeType| true | string |Type of withdraw fee (only one type can be applied to each currency)| fixed,circulated,ratio     |
|        transactFeeWithdraw| false | string |Withdraw fee in each request (only applicable to withdrawFeeType = fixed) |      |
|        minTransactFeeWithdraw| false | string |Minimal withdraw fee in each request (only applicable to withdrawFeeType = circulated) |      |
|        maxTransactFeeWithdraw| false | string |Maximum withdraw fee in each request (only applicable to withdrawFeeType = circulated or ratio) |      |
|        transactFeeRateWithdraw| false | string |Withdraw fee in each request (only applicable to withdrawFeeType = ratio) |      |
|        withdrawStatus}| true | string | Withdraw status | allowed,prohibited     |
|      instStatus }| true | string | Instrument status | normal,delisted     |

### Status Code

| Status Code | Error Message  | Scenario | 
| ---- | ----- | ---- |
| 200| success | Request successful |
| 500| error |  System error |
| 2002| invalid field value in "field name" | Invalid field value |

## Get Current System Time

This endpoint returns the current system time in milliseconds adjusted to Beijing time zone.

```shell
curl "https://api.huobi.pro/v1/common/timestamp"
```

### HTTP Request

`GET /v1/common/timestamp`

### Request Parameters

No parameter is needed for this endpoint.

> Response:

```json
  "data": 1494900087029
```

### Response Content

The returned "Data" field contains an integer represents the timestamp in milliseconds adjusted to Beijing time.

# Market Data

## Get Klines(Candles)

This endpoint retrieves all klines in a specific range.

### HTTP Request

`GET https://api.huobi.pro/market/history/kline`

```shell
curl "https://api.huobi.pro/market/history/kline?period=1day&size=200&symbol=btcusdt"
```

### Query Parameters

Parameter | Data Type | Required | Default | Description                 | Value Range
--------- | --------- | -------- | ------- | -----------                 | -----------
symbol    | string    | true     | NA      | The trading symbol to query | All trading symbol supported, e.g. btcusdt, bccbtc
period    | string    | true     | NA      | The period of each candle   | 1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year
size      | integer   | false    | 150     | The number of data returns  | [1, 2000]

<aside class="notice">当前 REST API 不支持自定义时间区间，如需要历史固定时间范围的数据，请参考 Websocket API 中的 K 线接口。</aside>

<aside class="notice">To query hb10, put "hb10" at symbol position.</aside>

<aside class="notice">K线周期以新加坡时间为基准开始计算，例如日K线的起始周期为新加坡时间0时-新加坡时间次日0时。</aside>

> The above command returns JSON structured like this:

```json
"data": [
  {
    "id": 1499184000,
    "amount": 37593.0266,
    "count": 0,
    "open": 1935.2000,
    "close": 1879.0000,
    "low": 1856.0000,
    "high": 1940.0000,
    "vol": 71031537.97866500
  }
]
```

### Response Content

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | The UNIX timestamp in seconds as response id
amount    | float     | The aggregated trading volume in USDT
count     | integer   | The number of completed trades
open      | float     | The opening price
close     | float     | The closing price
low       | float     | The low price
high      | float     | The high price
vol       | float     | The trading volume in base currency

## Get Latest Aggregated Ticker

This endpoint retrieves the latest ticker with some important 24h aggregated market data.

### HTTP Request

`GET https://api.huobi.pro/market/detail/merged`

```shell
curl "https://api.huobi.pro/market/detail/merged?symbol=ethusdt"
```

### Request Parameters

Parameter | Data Type | Required | Default | Description                  | Value Range
--------- | --------- | -------- | ------- | -----------                  | --------
symbol    | string    | true     | NA      | The trading symbol to query  | 取值参考/v1/common/symbols

> The above command returns JSON structured like this:

```json
"data": {
  "id":1499225271,
  "ts":1499225271000,
  "close":1885.0000,
  "open":1960.0000,
  "high":1985.0000,
  "low":1856.0000,
  "amount":81486.2926,
  "count":42122,
  "vol":157052744.85708200,
  "ask":[1885.0000,21.8804],
  "bid":[1884.0000,1.6702]
}
```

### Response Content

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | The UNIX timestamp in seconds as response id
amount    | float     | The aggregated trading volume in USDT
count     | integer   | The number of completed trades
open      | float     | The opening price of last 24 hours
close     | float     | The last price of last 24 hours
low       | float     | The low price of last 24 hours
high      | float     | The high price of last 24 hours
vol       | float     | The trading volume in base currency of last 24 hours
bid       | object    | The current best bid in format [price, quote volume]
ask       | object    | The current best ask in format [price, quote volume]

## Get Latest Tickers for All Pairs

This endpoint retrieves the latest tickers for all supported pairs.

<aside class="notice">The returned data object can contain large amount of tickers.</aside>

### HTTP Request

`GET https://api.huobi.pro/market/tickers`

```shell
curl "https://api.huobi.pro/market/tickers"
```

### Request Parameters

No parameters are needed for this endpoint.

> The above command returns JSON structured like this:

```json
"data": [  
    {  
        "open":0.044297,
        "close":0.042178,
        "low":0.040110,
        "high":0.045255,
        "amount":12880.8510,  
        "count":12838,
        "vol":563.0388715740,
        "symbol":"ethbtc"
    },
    {  
        "open":0.008545,
        "close":0.008656,
        "low":0.008088,
        "high":0.009388,
        "amount":88056.1860,
        "count":16077,
        "vol":771.7975953754,
        "symbol":"ltcbtc"
    }
]
```

### Response Content

Response content is an array of object, each object has below fields.

Field     | Data Type | Description
--------- | --------- | -----------
amount    | float     | The aggregated trading volume in USDT of last 24 hours
count     | integer   | The number of completed trades of last 24 hours
open      | float     | The opening price of last 24 hours
close     | float     | The last price of last 24 hours
low       | float     | The low price of last 24 hours
high      | float     | The high price of last 24 hours
vol       | float     | The trading volume in base currency of last 24 hours
symbol    | string    | The trading symbol of this object, e.g. btcusdt, bccbtc

## Get Market Depth

This endpoint retrieves the current order book of a specific pair.

### HTTP Request

`GET https://api.huobi.pro/market/depth`

```shell
curl "https://api.huobi.pro/market/depth?symbol=btcusdt&type=step1"
```

### Request Parameters

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | The trading symbol to query                       | 取值参考/v1/common/symbols
depth     | integer   | false    | 20                    | The number of market depth to return on each side | 5, 10, 20
type      | string    | true     | step0                 | Market depth aggregation level, details below     | step0, step1, step2, step3, step4, step5

<aside class="notice">when type is set to "step0", the default value of "depth" is 150 instead of 20.</aside>

**"type" Details**

Value     | Description
--------- | ---------
step0     | No market depth aggregation
step1     | Aggregation level = precision*10
step2     | Aggregation level = precision*100
step3     | Aggregation level = precision*1000
step4     | Aggregation level = precision*10000
step5     | Aggregation level = precision*100000

> The above command returns JSON structured like this:

```json
"tick": {
    "version": 31615842081,
    "ts": 1489464585407,
    "bids": [
      [7964, 0.0678],
      [7963, 0.9162],
      [7961, 0.1],
      [7960, 12.8898],
      [7958, 1.2],
      [7955, 2.1009],
      [7954, 0.4708],
      [7953, 0.0564],
      [7951, 2.8031],
      [7950, 13.7785],
      [7949, 0.125],
      [7948, 4],
      [7942, 0.4337],
      [7940, 6.1612],
      [7936, 0.02],
      [7935, 1.3575],
      [7933, 2.002],
      [7932, 1.3449],
      [7930, 10.2974],
      [7929, 3.2226]
    ],
    "asks": [
      [7979, 0.0736],
      [7980, 1.0292],
      [7981, 5.5652],
      [7986, 0.2416],
      [7990, 1.9970],
      [7995, 0.88],
      [7996, 0.0212],
      [8000, 9.2609],
      [8002, 0.02],
      [8008, 1],
      [8010, 0.8735],
      [8011, 2.36],
      [8012, 0.02],
      [8014, 0.1067],
      [8015, 12.9118],
      [8016, 2.5206],
      [8017, 0.0166],
      [8018, 1.3218],
      [8019, 0.01],
      [8020, 13.6584]
    ]
  }
```

### Response Content

<aside class="notice">The returned data object is under 'tick' object instead of 'data' object in the top level JSON</aside>

Field     | Data Type | Description
--------- | --------- | -----------
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Beijing time
version   | integer   | Internal data
bids      | object    | The current all bids in format [price, quote volume]
asks      | object    | The current all asks in format [price, quote volume]


## Get the Last Trade

This endpoint retrieves the latest trade with its price, volume, and direction.

### HTTP Request

`GET https://api.huobi.pro/market/trade`

```shell
curl "https://api.huobi.pro/market/trade?symbol=ethusdt"
```

### Request Parameters

Parameter | Data Type | Required | Default Value         | Description                                       | Value Range
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | The trading symbol to query                       | 取值参考/v1/common/symbols

> The above command returns JSON structured like this:

```json
"tick": {
    "id": 600848670,
    "ts": 1489464451000,
    "data": [
      {
        "id": 600848670,
        "trade-id": 102043494568,
        "price": 7962.62,
        "amount": 0.0122,
        "direction": "buy",
        "ts": 1489464451000
      }
    ]
}
```

### Response Content

<aside class="notice">The returned data object is under 'tick' object instead of 'data' object in the top level JSON</aside>

Parameter | Data Type | Description
--------- | --------- | -----------
id        | integer   | The unique trade id of this trade (to be obsoleted)
trade-id|integer| The unique trade id (NEW)
amount    | float     | The trading volume in base currency
price     | float     | The trading price in quote currency
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Beijing time
direction | string    | The direction of the taker trade: 'buy' or 'sell'

## Get the Most Recent Trades

This endpoint retrieves the most recent trades with their price, volume, and direction.

### HTTP Request

`GET https://api.huobi.pro/market/history/trade`

```shell
curl "https://api.huobi.pro/market/history/trade?symbol=ethusdt&size=2"
```

### Request Parameters

Parameter | Data Type | Required | Default Value    | Description                   | Value Range
--------- | --------- | -------- | -------------    | ----------                    | -----------
symbol    | string    | true     | NA               | The trading symbol to query   | All supported trading symbols, e.g. btcusdt, bccbtc
size      | integer   | false    | 1                | The number of data returns    | [1, 2000]

> The above command returns JSON structured like this:

```json
"data": [  
   {  
      "id":31618787514,
      "ts":1544390317905,
      "data":[  
         {  
            "amount":9.000000000000000000,
            "ts":1544390317905,
            "id":3161878751418918529341,
            "trade-id": 102043495672,
            "price":94.690000000000000000,
            "direction":"sell"
         },
         {  
            "amount":73.771000000000000000,
            "ts":1544390317905,
            "id":3161878751418918532514,
            "trade-id": 102043495673,
            "price":94.660000000000000000,
            "direction":"sell"
         }
      ]
   },
   {  
      "id":31618776989,
      "ts":1544390311353,
      "data":[  
         {  
            "amount":1.000000000000000000,
            "ts":1544390311353,
            "id":3161877698918918522622,
            "trade-id": 102043495674,
            "price":94.710000000000000000,
            "direction":"buy"
         }
      ]
 ]
}
```

### Response Content

<aside class="notice">The returned data object is an array represents one recent timestamp; each timestamp object again is an array represents all trades occurred at this timestamp.</aside>

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | The unique trade id of this trade (to be obsoleted)
trade-id|integer| The unique trade id (NEW)
amount    | float     | The trading volume in base currency
price     | float     | The trading price in quote currency
ts        | integer   | The UNIX timestamp in milliseconds adjusted to Beijing time
direction | string    | The direction of the taker trade: 'buy' or 'sell'

## Get the Last 24h Market Summary

This endpoint retrieves the summary of trading in the market for the last 24 hours.

### HTTP Request

`GET https://api.huobi.pro/market/detail/`

```shell
curl "https://api.huobi.pro/market/detail?symbol=ethusdt"
```

### Request Parameters

Parameter | Data Type | Required | Default Value    | Description                   | Value Range
--------- | --------- | -------- | -------------    | ----------                    | -----------
symbol    | string    | true     | NA               | The trading symbol to query   | 取值参考/v1/common/symbols

> The above command returns JSON structured like this:

```json
"tick": {  
   "amount":613071.438479561,
   "open":86.21,
   "close":94.35,
   "high":98.7,
   "id":31619471534,
   "count":138909,
   "low":84.63,
   "version":31619471534,
   "vol":5.6617373443873316E7
}
```

### Response Content

<aside class="notice">The returned data object is under 'tick' object instead of 'data' object in the top level JSON</aside>

Field     | Data Type | Description
--------- | --------- | -----------
id        | integer   | The UNIX timestamp in seconds as response id
amount    | float     | The aggregated trading volume in USDT
count     | integer   | The number of completed trades
open      | float     | The opening price of last 24 hours
close     | float     | The last price of last 24 hours
low       | float     | The low price of last 24 hours
high      | float     | The high price of last 24 hours
vol       | float     | The trading volume in base currency of last 24 hours
version   | integer   | Internal data


# Account

<aside class="notice">All endpoints in this section require authentication</aside>

## Get all Accounts of the Current User

API Key Permission：Read

This endpoint returns a list of accounts owned by this API user.

### HTTP Request

`GET https://api.huobi.pro/v1/account/accounts`

```shell
curl "https://api.huobi.pro/v1/account/accounts"
```

### Request Parameters

<aside class="notice">No parameter is available for this endpoint</aside>

> The above command returns JSON structured like this:

```json
  "data": [
    {
      "id": 100009,
      "type": "spot",
      "subtype": "",
      "state": "working"
    }
  ]
```

### Response Content

Field               | Data Type | Description              | Value Range
---------           | --------- | -----------              | -----------
id                  | integer   | Unique account id        | NA
state               | string    | Account state            | working, lock
type                | string    | The type of this account | spot, margin, otc, point, super-margin
subtype                | string    | The type of sub account (applicable only for isolated margin accout)| The corresponding trading symbol (currency pair) the isolated margin is based on, e.g. btcusdt

<aside class="notice">Margin/super-margin account will only be created after the first time asset transfer-in.</aside>

## Get Account Balance of a Specific Account

API Key Permission：Read

This endpoint returns the balance of an account specified by account id.

### HTTP Request

`GET https://api.huobi.pro/v1/account/accounts/{account-id}/balance`

'account-id': The specified account id to get balance for, can be found by query '/v1/account/accounts' endpoint.

```shell
curl "https://api.huobi.pro/v1/account/accounts/100009/balance"
```

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>

> The above command returns JSON structured like this:

```json
"data": {
    "id": 100009,
    "type": "spot",
    "state": "working",
    "list": [
      {
        "currency": "usdt",
        "type": "trade",
        "balance": "500009195917.4362872650"
      },
      {
        "currency": "usdt",
        "type": "frozen",
        "balance": "328048.1199920000"
      },
     {
        "currency": "etc",
        "type": "trade",
        "balance": "499999894616.1302471000"
      }
    ],
  }
}
```

### Response Content

Field               | Data Type | Description              | Value Range
---------           | --------- | -----------              | -----------
id                  | integer   | Unique account id        | NA
state               | string    | Account state            | working, lock
type                | string    | The type of this account | spot, margin, otc, point, super-margin
list                | object    | The balance details of each currency

**Per list item content**

Field               | Data Type | Description                           | Value Range
---------           | --------- | -----------                           | -----------
currency            | string    | The currency of this balance          | NA
type                | string    | The balance type                      | trade, frozen
balance             | string    | The balance in the main currency unit | NA


## Get Account History

API Key Permission：Read

This endpoint returns the amount changes of specified user's account.

### HTTP Request

`GET https://api.huobi.pro/v1/account/history`

```shell
curl "https://api.huobi.pro/v1/account/history?account-id=5260185"
```

### Request Parameters

Parameter  | Required | Data Type | Description | Default Value                                  | Value Range
---------  | --------- | -------- | ------- | -----------                                   | ----------
account-id     | true  | string | 账户编号,取值参考/v1/account/accounts      |     |  |
currency      | false | string | 币种,取值参考/v1/common/currencys   |       |  |
transact-types | false | string | Amount change types (multiple selection allowed)  | all     |trade,etf, transact-fee, deduction, transfer, credit, liquidation, interest, deposit-withdraw, withdraw-fee, exchange, other-types |
start-time   | false | long | Far point of time of the query window (unix time in millisecond). Searching based on transact-time. The maximum size of the query window is 1 hour. The query window can be shifted within 30 days. | ((end-time) – 1hour)     | [((end-time) – 1hour), (end-time)]   |
end-time     | false  | long | Near point of time of the query window (unix time in millisecond). Searching based on transact-time. The maximum size of the query window is 1 hour. The query window can be shifted within 30 days.  |  current-time    |[(current-time) – 29days,(current-time)]|
sort     | false  | string | Sorting order  |  asc    |asc or desc|
size     | false  | int | Maximum number of items in each response  |   100   |[1,500]|

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "data": [
        {
            "account-id": 5260185,
            "currency": "btc",
            "transact-amt": "0.002393000000000000",
            "transact-type": "transfer",
            "record-id": 89373333576,
            "avail-balance": "0.002393000000000000",
            "acct-balance": "0.002393000000000000",
            "transact-time": 1571393524526
        },
        {
            "account-id": 5260185,
            "currency": "btc",
            "transact-amt": "-0.002393000000000000",
            "transact-type": "transfer",
            "record-id": 89373382631,
            "avail-balance": "0E-18",
            "acct-balance": "0E-18",
            "transact-time": 1571393578496
        }
    ]
}
```

### Response Content

Field               | Data Type | Description              | Value Range
---------           | --------- | -----------              | -----------
status                 | string   | Status code        | 
data               | object    |             | 
{ account-id  | long   | Account ID|
currency               | string    | Currency|
transact-amt                 | string   | Amount change (positive value if income, negative value if outcome)        | 
transact-type                 | string   | Amount change types        | 
avail-balance                 | string   | Available balance        | 
acct-balance                | string   | Account balance        | 
transact-time                 | long   | Transaction time (database time)      | 
record-id }                 | string   | Unique record ID in the database      | 



## Transfer Fund Between Spot Account and Future Contract Account

API Key Permission：Trade

This endpoint allows a user to transfer fund between spot account and futrue contract account. 

The Rate Limit for this endpoint is 10 requests per minute.

### HTTP Request

`POST /v1/futures/transfer`

```json
  {"currency":  "btc",
  "amount": 0.01,
  "type": "pro-to-futures"
  }
 
```
### Request Parameters

Parameter  | Data Type | Required | Description|Values
---------  | --------- | -------- | ------- | -----------
currency|TRUE|String|currency name|币种,取值参考/v1/common/currencys. 
amount|TRUE|Decimal|amount of fund to transfer|
type|TRUE|String|type of the transfer|“futures-to-pro” or “pro-to-futures”

> Response:

```json
  {"data":  123456,
  "status": "ok"
  }
 
```
### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
data                | Long   | Transfer id
status              |string| Request status. "ok" or "error"
err-code            |string| error code. Please refer to the err-code list below for details
err-msg             |string| error message. Please refer to the err-code and err-msg list below for details

### error code
err-code              | err-msg | Comments
---------           | --------- | -----------
base-msg||Other errors, please refer to err-msg list below for details. 
base-currency-error|The currency is invalid|
frequent-invoke|the operation is too frequent. Please try again later|Rate limit is 10/min
banned-by-blacklist|Blacklist restriction|
dw-insufficient-balance|Insufficient balance. You can only transfer {0} at most.|Insufficient balance of spot account
dw-account-transfer-unavailable|account transfer unavailable|This API endpoint is not available.
dw-account-transfer-error|account transfer error|
dw-account-transfer-failed|Failed to transfer. Please try again later.|
dw-account-transfer-failed-account-abnormality|Account abnormality, failed to transfer。Please try again later.|

### error message for 'base-msg' err-code
err-code              | err-msg | Comments
---------           | --------- | -----------
base-msg|Unable to transfer in currently. Please contact customer service.|
base-msg|Unable to transfer out currently. Please contact customer service.|
base-msg|Abnormal contracts status. Can’t transfer.|
base-msg|Sub-account doesn't own the permissions to transfer in. Please contact customer service.|
base-msg|Sub-account doesn't own the permissions to transfer out. Please contact customer service.|
base-msg|The sub-account does not have transfer permissions. Please login main account to authorize.|
base-msg|Insufficient amount available.|Insufficient amount of Future Contract Account
base-msg|The single transfer-out amount must be no less than {0}{1}.|
base-msg|The single transfer-out amount must be no more than {0}{1}.|
base-msg|The single transfer-in amount must be no less than {0}{1}.|
base-msg|The single transfer-in amount must be no more than {0}{1}.|
base-msg|Your accumulative transfer-out amount is over the daily maximum, {0}{1}. You can't transfer out for the time being.|
base-msg|Your accumulative transfer-in amount is over the daily maximum, {0}{1}. You can't transfer in for the time being.|
base-msg|Your accumulative net transfer-out amount is over the daily maximum, {0}{1}. You can't transfer out for the time being.|
base-msg|Your accumulative net transfer-in amount is over the daily maximum, {0}{1}. You can't transfer in for the time being.|
base-msg|The platform's accumulative transfer-out amount is over the daily maximum. You can't transfer out for the time being.|
base-msg|The platform's accumulative transfer-in amount is over the daily maximum. You can't transfer in for the time being.|
base-msg|The platform's accumulative net transfer-out amount is over the daily maximum. You can't transfer out for the time being.|
base-msg|The platform's accumulative net transfer-in amount is over the daily maximum. You can't transfer in for the time being.|
base-msg|Transfer failed. Please try again later or contact customer service.|
base-msg|Abnormal service, transfer failed. Please try again later.|
base-msg|You don’t have access permission as you have not opened contracts trading.|
base-msg|This contract type doesn't exist.|There is no corresponding Future Contract for the currency defined in the request.


## Transfer Asset between Parent and Sub Account

API Key Permission：Trade

This endpoint allows user to transfer asset between parent and sub account.

### HTTP Request

`POST https://api.huobi.pro/v1/subuser/transfer`

```shell
curl -X POST "https://api.huobi.pro/v1/subuser/transfer" -H "Content-Type: application/json" -d '{"sub-uid": 12345, "currency": "btc", "amount": 123.5, "type": "master-transfer-in"}'
```

### Request Parameters

Parameter  | Data Type | Required | Description                                       | Value Range
---------  | --------- | -------- | -----------                                       | -----------
sub-uid    | integer   | true     | The target sub account uid to transfer to or from | NA
currency   | string    | true     | The crypto currency to transfer                   | NA
amount     | decimal   | true     | The amount of asset to transfer                   | NA
type       | string    | true     | The type of transfer                              | master-transfer-in, master-transfer-out, master-point-transfer-in, master-point-transfer-out

> The above command returns JSON structured like this:

```json
  "data": 12345
```

### Response Content

<aside class="notice">The return data contains a single value instead of an object</aside>

Field               | Data Type | Description
---------           | --------- | -----------
data                | integer   | Unique transfer id


## Get the Aggregated Balance of all Sub-accounts of the Current User

API Key Permission：Read

This endpoint returns the balances of all the sub-account aggregated.

### HTTP Request

`GET https://api.huobi.pro/v1/subuser/aggregate-balance`

```shell
curl "https://api.huobi.pro/v1/subuser/aggregate-balance"
```

> The above command returns JSON structured like this:

```json
  "data": [
      {
        "currency": "eos",
        "type": "spot",
        "balance": "1954559.809500000000000000"
      },
      {
        "currency": "btc",
        "type": "spot",
        "balance": "0.000000000000000000"
      },
      {
        "currency": "usdt",
        "type": "spot",
        "balance": "2925209.411300000000000000"
      }
   ]
```

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>

### Response Content

<aside class="notice">The returned "data" object is a list of aggregated balances</aside>

Field               | Data Type | Description
---------           | --------- | -----------
currency            | string    | The currency of this balance
type|string|account type (spot, margin, point)
balance             | string    | The total balance in the main currency unit including all balance and frozen banlance

## Get Account Balance of a Sub-Account

API Key Permission：Read

This endpoint returns the balance of a sub-account specified by sub-uid.

### HTTP Request

`GET https://api.huobi.pro/v1/account/accounts/{sub-uid}`

'sub-uid': The specified sub user id to get balance for.

```shell
curl "https://api.huobi.pro/v1/account/accounts/10758899"
```

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>

> The above command returns JSON structured like this:

```json
"data": [
  {
    "id": 9910049,
    "type": "spot",
    "list": [
              {
        "currency": "btc",
          "type": "trade",
          "balance": "1.00"
      },
      {
        "currency": "eth",
        "type": "trade",
        "balance": "1934.00"
      }
      ]
  },
  {
    "id": 9910050,
    "type": "point",
    "list": []
  }
]
```

### Response Content

<aside class="notice">The returned "data" object is a list of accounts under this sub-account</aside>

Field               | Data Type | Description                           | Value Range
---------           | --------- | -----------                           | -----------
id                  | integer   | Unique account id                     | NA
type                | string    | The type of this account              | spot, margin, otc, point
list                | object    | The balance details of each currency  | NA

**Per list item content**

Field               | Data Type | Description                           | Value Range
---------           | --------- | -----------                           | -----------
currency            | string    | The currency of this balance          | NA
type                | string    | The balance type                      | trade, frozen
balance             | string    | The balance in the main currency unit | NA


# Wallet (Deposit and Withdraw)

<aside class="notice">All endpoints in this section require authentication</aside>


## APIv2 - Query Deposit Address

API user could query deposit address of corresponding chain, for a specific crypto currency (except IOTA)

API Key Permission：Read

<aside class="notice"> The endpoint does not support deposit address querying for currency "IOTA" at this moment </aside>

### HTTP Request

`GET https://api.huobi.pro/v2/account/deposit/address`

```shell
curl "https://api.huobi.pro/v2/account/deposit/address?currency=btc"
```

### Request Parameters

Field Name  | Data Type | Mandatory | Default Value | Description
---------  | --------- | -------- | ------- | -----------
currency   | string    | true     | N/A      | Crypto currency

> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "data": [
        {
            "currency": "btc",
            "address": "1PSRjPg53cX7hMRYAXGJnL8mqHtzmQgPUs",
            "addressTag": "",
            "chain": "btc"
        }
    ]
}
```

### Response Content

Field Name            | Data Type | Description
---------           | --------- | -----------
code                | int   | Status code
message                | string   | Error message (if any)
data                | object  | 
  { currency|string|Crypto currency
    address|string|Deposit address
    addressTag|string|Deposit address tag
    chain }|string|Block chain name

### Status Code

| Status Code | Error Message  | Scenario | 
| ---- | ----- | ---- |
| 200| success | Request successful |
| 500| error | System error |
| 1002| unauthorized | Unauthorized |
| 1003| invalid signature | Signature failure |
| 2002| invalid field value in "field name" | Invalid field value |
| 2003| missing mandatory field "field name" | Mandatory field missing |


## APIv2 - Query Withdraw Quota

API user could query withdraw quota for currencies

API Key Permission：Read

### HTTP Request

`GET https://api.huobi.pro/v2/account/withdraw/quota`

```shell
curl "https://api.huobi.pro/v2/account/withdraw/quota?currency=btc"
```

### Request Parameters

Field Name  | Data Type | Mandatory | Default Value | Description
---------  | --------- | -------- | ------- | -----------
currency   | string    | true     | N/A      | 取值参考/v1/common/currencys

> The above command returns JSON structured like this:

```json
{
    "code": 200,
    "data": 
        {
            "currency": "btc",
            "chains": [
                {
                    "chain": "btc",
                    "maxWithdrawAmt": "200.00000000",
                    "withdrawQuotaPerDay": "200.00000000",
                    "remainWithdrawQuotaPerDay": "200.000000000000000000",
                    "withdrawQuotaPerYear": "700000.00000000",
                    "remainWithdrawQuotaPerYear": "700000.000000000000000000",
                    "withdrawQuotaTotal": "7000000.00000000",
                    "remainWithdrawQuotaTotal": "7000000.000000000000000000"
                }
        }
    ]
}
```

### Response Content

Field Name            | Data Type | Description
---------           | --------- | -----------
code                | int   | Status code
message                | string   | Error message (if any)
data                | object  | 
  currency|string|Crypto currency
    chains|object|
    { chain |string|Block chain name
      maxWithdrawAmt |  string | Maximum withdraw amount in each request |      |
      withdrawQuotaPerDay |  string | Maximum withdraw amount in a day |      |
      remainWithdrawQuotaPerDay |  string | Remaining withdraw quota in the day |      |
      withdrawQuotaPerYear |  string | Maximum withdraw amount in a year |      |
      remainWithdrawQuotaPerYear |  string | Remaining withdraw quota in the year |      |
      withdrawQuotaTotal |  string | Maximum withdraw amount in total |      |
      remainWithdrawQuotaTotal }|  string | Remaining withdraw quota in total |      |

### Status Code

| Status Code | Error Message  | Scenario | 
| ---- | ----- | ---- |
| 200| success | Request successful |
| 500| error | System error |
| 1002| unauthorized | Unauthorized |
| 1003| invalid signature | Signature failure |
| 2002| invalid field value in "field name" | Invalid field value |


## Create a Withdraw Request

API Key Permission：Withdraw

This endpoint creates a withdraw request from your spot trading account to an external address.

<aside class="notice">If user has chosen fast withdraw preferred in  <a href='https://www.hbg.com/en-us/user_center/uc_setting/'>Settings </a>, the withdraw requests submitted via this endpoint would choose 'fast withdraw' as preferred channel. </aside>

<aside class="notice">Only support the existed addresses in your  <a href='https://www.hbg.com/en-us/withdraw_address/'>withdraw address list </a> </aside>

### HTTP Request

`POST https://api.huobi.pro/v1/dw/withdraw/api/create`

```shell
curl -X POST -H "Content-Type: application/json" "https://api.huobi.pro/v1/dw/withdraw/api/create" -d
'{
  "address": "0xde709f2102306220921060314715629080e2fb77",
  "amount": "0.05",
  "currency": "eth",
  "fee": "0.01"
}'
```

### Request Parameters

Parameter  | Data Type | Required | Default | Description
---------  | --------- | -------- | ------- | -----------
address    | string    | true     | NA      | The desination address of this withdraw
currency   | string    | true     | NA      | 取值参考/v1/common/currencys
amount     | string    | true     | NA      | The amount of currency to withdraw
fee        | string    | true    | NA      | The fee to pay with this withdraw
chain      | string    | false    | NA      | set as "usdt" to withdraw USDT to OMNI, set as "trc20usdt" to withdraw USDT to TRX
addr-tag   | string    | false    | NA      | A tag specified for this address

> The above command returns JSON structured like this:

```json
{  
  "data": 1000
}
```

### Response Content

<aside class="notice">The return data contains a single value instead of an object</aside>

Field               | Data Type | Description
---------           | --------- | -----------
data                | integer   | Transfer id

<aside class="notice">All new transfer id will be incremental to the previous ids. This allows search by transfer id sequences</aside>


## Cancel a Withdraw Request

API Key Permission：Withdraw

This endpoint cancels a previously created withdraw request by its transfer id.

### HTTP Request

`POST https://api.huobi.pro/v1/dw/withdraw-virtual/{withdraw-id}/cancel`

```shell
curl -X POST "https://api.huobi.pro/v1/dw/withdraw-virtual/1000/cancel"
```

'withdraw-id': the id returned when previously created a withdraw request

### Request Parameters

<aside class="notice">No parameter is needed for this endpoint</aside>

> The above command returns JSON structured like this:

```json
  "data": 700
```

### Response Content

<aside class="notice">The return data contains a single value instead of an object</aside>

Parameter           | Data Type | Description
---------           | --------- | -----------
data                | integer   | Withdraw cancel id


## Search for Existed Withdraws and Deposits

API Key Permission：Read

This endpoint searches for all existed withdraws and deposits and return their latest status.

### HTTP Request

`GET https://api.huobi.pro/v1/query/deposit-withdraw`

```shell
curl "https://api.huobi.pro/v1/query/deposit-withdraw?currency=xrp&type=deposit&from=5&size=12"
```

### Request Parameters

Parameter  | Data Type | Required | Description                     | Value Range | Default Value|
---------  | --------- | -------- | -----------                     | ------------|------------------|
currency   | string    | false     | The crypto currency to withdraw | NA |When currency is not specified, the reponse would include the records of ALL currencies. 
type       | string    | true     | Define transfer type to search  | deposit, withdraw| |
from       | string    | false    | The transfer id to begin search | 1 ~ latest record ID| When 'from' is not specified, the default value would be 1 if 'direct' is 'prev' with the response in ascending order, the default value would be the ID of latest record if 'direct' is 'next' with the response in descending order.
size       | string    | false     | The number of items to return   | 1-500 | 100 |
direct     | string    | false     | the order of response | 'prev' (ascending), 'next' (descending)| 'prev' |

> The above command returns JSON structured like this:

```json
{
	"status": "ok",
	"data": [{
		"id": 24383070,
		"type": "deposit",
		"currency": "usdt",
		"chain": "usdterc20",
		"tx-hash": "16382690",
		"amount": 4.000000000000000000,
		"address": "0x138d709030b4e096044d371a27efc5c562889b9b",
		"address-tag": "",
		"fee": 0,
		"state": "safe",
		"created-at": 1571303815800,
		"updated-at": 1571303815826
	}]
}
```

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
id                  | integer   | Transfer id
type                | string    | Define transfer type to search, possible values: [deposit, withdraw]
currency            | string    | The crypto currency to withdraw
tx-hash             | string    | The on-chain transaction hash
chain             | string    | Block chain name
amount              | float   | The number of crypto asset transfered in its minimum unit
address             | string    | The deposit or withdraw source address
address-tag         | string    | The user defined address tag
fee                 | float   | Withdraw fee
state               | string    | The state of this transfer (see below for details)
created-at          | integer   | The timestamp in milliseconds for the transfer creation
updated-at          | integer   | The timestamp in milliseconds for the transfer's latest update

**List of possible withdraw state**

State           | Description
---------       | -----------
submitted       | Withdraw request submitted successfully
reexamine       | Under examination for withdraw validation
canceled        | Withdraw canceled by user
pass            | Withdraw validation passed
reject          | Withdraw validation rejected
pre-transfer    | Withdraw is about to be released
wallet-transfer | On-chain transfer initiated
wallet-reject   | Transfer rejected by chain
confirmed       | On-chain transfer completed with one confirmation
confirm-error   | On-chain transfer faied to get confirmation
repealed        | Withdraw terminated by system

**List of possible deposit state**

State           | Description
---------       | -----------
unknown         | On-chain transfer has not been received
confirming      | On-chain transfer waits for first confirmation
confirmed       | On-chain transfer confirmed for at least one block
safe            | Multiple on-chain confirmation happened
orphan          | Confirmed but currently in an orphan branch


# 现货 / 杠杆交易

<aside class="notice">访问交易相关的接口需要进行签名认证。</aside>

<aside class="warning">逐仓杠杆交易时，“account-id” 参数需设置为 “margin” 的 account-id， “source”参数需设置为 “margin-api”；全仓杠杆交易时，“account-id” 参数需设置为 “super-margin” 的 account-id， “source”参数需设置为 “super-margin-api”。</aside>

## 下单

API Key 权限：交易

发送一个新订单到火币以进行撮合。

### HTTP 请求

- POST ` /v1/order/orders/place`

```json
{
  "account-id": "100009",
  "amount": "10.1",
  "price": "100.1",
  "source": "api",
  "symbol": "ethusdt",
  "type": "buy-limit",
  "client-order-id": "a0001"
}
```

### 请求参数

参数名称 | 数据类型 | 是否必需 | 默认值 | 描述
---------  | --------- | -------- | ------- | -----------
account-id | string    | true     | NA      | 账户 ID，可用 <a href="https://huobiapi.github.io/docs/spot/v1/cn/#bd9157656f">/v1/account/accounts</a>获取。现货交易使用 ‘spot’ 账户的 account-id；逐仓杠杆交易，请使用 ‘margin’ 账户的 account-id；全仓杠杆交易，请使用 ‘super-margin’ 账户的 account-id
symbol     | string    | true     | NA      | 交易对, <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
type       | string    | true     | NA      | 订单类型，包括buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker（说明见下文）, buy-stop-limit, sell-stop-limit
amount     | string    | true     | NA      | 订单交易量（市价买单此字段为订单交易额）
price      | string    | false    | NA      | limit order的交易价格
source     | string    | false    | api     | 现货交易填写“api”，逐仓杠杆交易填写“margin-api”，全仓杠杆交易填写“super-margin-api”
client-order-id| string    | false    | NA     | 用户自编订单号（最大长度64个字符，须在24小时内保持唯一性）
stop-price|string|false|NA|止盈止损订单触发价格
operator|string|false|NA|止盈止损订单触发价运算符 gte – greater than and equal (>=), lte – less than and equal (<=)


**buy-limit-maker**

当“下单价格”>=“市场最低卖出价”，订单提交后，系统将拒绝接受此订单；

当“下单价格”<“市场最低卖出价”，提交成功后，此订单将被系统接受。

**sell-limit-maker**

当“下单价格”<=“市场最高买入价”，订单提交后，系统将拒绝接受此订单；

当“下单价格”>“市场最高买入价”，提交成功后，此订单将被系统接受。

> Response:

```json
{  
  "data": "59378"
}
```

### 响应数据

返回的主数据对象是一个对应下单单号的字符串。

如client order ID（在24小时内）被复用，节点返回先前订单的client order ID。

## 撤销订单

API Key 权限：交易

此接口发送一个撤销订单的请求。

<aside class="warning">此接口只提交取消请求，实际取消结果需要通过订单状态，撮合状态等接口来确认。</aside>


### HTTP 请求

- POST ` /v1/order/orders/{order-id}/submitcancel`


### 请求参数

| 参数名称     | 是否必须 | 类型     | 描述           | 默认值  | 取值范围 |
| -------- | ---- | ------ | ------------ | ---- | ---- |
| order-id | true | string | 订单ID，填在path中 |      |      |


> Response:

```json
{  
  "data": "59378"
}
```

### 响应数据

返回的主数据对象是一个对应下单单号的字符串。

### 错误码

> Response:

```json
{
  "status": "error",
  "err-code": "order-orderstate-error",
  "err-msg": "订单状态错误",
  "order-state":-1 // 当前订单状态
}
```

返回字段列表中，order-state的可能取值包括 -

order-state           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
5| partial-canceled
6| filled
7| canceled
10| cancelling

## 撤销订单（基于client order ID）

API Key 权限：交易

此接口发送一个撤销订单的请求。

<aside class="warning">此接口只提交取消请求，实际取消结果需要通过订单状态，撮合状态等接口来确认。</aside>


### HTTP 请求

- POST ` /v1/order/orders/submitCancelClientOrder`

```json
{
  "client-order-id": "a0001"
}
```


### 请求参数

| 参数名称     | 是否必须 | 类型     | 描述           | 默认值  | 取值范围 |
| -------- | ---- | ------ | ------------ | ---- | ---- |
| client-order-id | true | string | 用户自编订单号 |      |      |


> Response:

```json
{  
  "data": "10"
}
```
### 响应数据

字段名称          | 数据类型 | 描述
---------           | --------- | -----------
data                  | integer   | 撤单状态码

Status Code           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
0| client-order-id not found
5| partial-canceled
6| filled
7| canceled
10| cancelling


## 查询当前未成交订单

API Key 权限：读取

查询已提交但是仍未完全成交或未被撤销的订单。

```json
{
   "account-id": "100009",
   "symbol": "ethusdt",
   "side": "buy"
}
```

### HTTP 请求

- GET `/v1/order/openOrders`


### 请求参数

参数名称 | 数据类型 | 是否必需 | 默认值 | 描述
---------  | --------- | -------- | ------- | -----------
account-id | string    | true    | NA      | 账户 ID，可用 <a href="https://huobiapi.github.io/docs/spot/v1/cn/#bd9157656f">/v1/account/accounts</a>获取。现货交易使用‘spot’账户的 account-id；逐仓杠杆交易，请使用 ‘margin’ 账户的 account-id；全仓杠杆交易，请使用 ‘super-margin’ 账户的 account-id
symbol     | string    | ture    | NA      | 交易对,<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
side       | string    | false    | both    | 指定只返回某一个方向的订单，可能的值有: buy, sell. 默认两个方向都返回。
from       | string | false | |查询起始 ID
direct     | string | false (如字段'from'已设定，此字段'direct'为必填) | |查询方向 (prev - 以起始ID升序检索；next - 以起始ID降序检索)
size       | int       | false    | 100      | 返回订单的数量，最大值500。

> Response:

```json
{  
  "data": [
    {
      "id": 5454937,
      "symbol": "ethusdt",
      "account-id": 30925,
      "amount": "1.000000000000000000",
      "price": "0.453000000000000000",
      "created-at": 1530604762277,
      "type": "sell-limit",
      "filled-amount": "0.0",
      "filled-cash-amount": "0.0",
      "filled-fees": "0.0",
      "source": "web",
      "state": "submitted"
    }
  ]
}
```

### 响应数据

字段名称          | 数据类型 | 描述
---------           | --------- | -----------
id                  | integer   | 订单id
symbol              | string    | 交易对, 例如btcusdt, ethbtc
price               | string    | limit order的交易价格
created-at          | int       | 订单创建的调整为北京时间的时间戳，单位毫秒
type                | string    | 订单类型
filled-amount       | string    | 订单中已成交部分的数量
filled-cash-amount  | string    | 订单中已成交部分的总价格
filled-fees         | string    | 已交交易手续费总额
source              | string    | 现货交易填写“api”
state               | string    | 订单状态，包括submitted, partial-filled, cancelling, created
stop-price|string|止盈止损订单触发价格
operator|string|止盈止损订单触发价运算符

## 批量撤销订单（open orders）

API Key 权限：交易

此接口发送批量撤销订单的请求。

<aside class="warning">此接口只提交取消请求，实际取消结果需要通过订单状态，撮合状态等接口来确认。</aside>

### HTTP 请求

- POST ` /v1/order/orders/batchCancelOpenOrders`


### 请求参数

| 参数名称     | 是否必须 | 类型     | 描述           | 默认值  | 取值范围 |
| -------- | ---- | ------ | ------------ | ---- | ---- |
| account-id | true  | string | 账户ID     |     |      |
| symbol     | false | string | 交易代码列表（最多10 个symbols，多个交易代码间以逗号分隔）     |  all    |     |
| side | false | string | 主动交易方向 |      |   “buy”或“sell”，缺省将返回所有符合条件尚未成交订单   |
| size | false | int | 所需返回记录数  |  100 |   [0,100]   |


> Response:

```json
{
  "status": "ok",
  "data": {
    "success-count": 2,
    "failed-count": 0,
    "next-id": 5454600
  }
}
```


### 响应数据


| 参数名称 | 是否必须 | 数据类型   | 描述    | 取值范围 |
| ---- | ---- | ------ | ----- | ---- |
| success-count | true | int | 成功取消的订单数 |     |
| failed-count | true | int | 取消失败的订单数 |     |
| next-id | true | long | 下一个符合取消条件的订单号 |    |

## 批量撤销订单

API Key 权限：交易

此接口同时为多个订单（基于id）发送取消请求。

### HTTP 请求

- POST ` /v1/order/orders/batchcancel`

```json
{
  "order-ids": [
    "1", "2", "3"
  ]
}
```

### 请求参数

| 参数名称  | 是否必须 | 类型   | 描述   | 默认值  | 取值范围 |
| ---- | ---- | ---- | ----  | ---- | ---- |
| order-ids | true | list | 撤销订单ID列表 |  |单次不超过50个订单id|


> Response:

```json
{  
  "data": {
    "success": [
      "1",
      "3"
    ],
    "failed": [
      {
        "err-msg": "记录无效",
        "order-id": "2",
        "err-code": "base-record-invalid"
        "order-state":-1 // 当前订单状态
      }
    ]
  }
}
```

### 响应数据

| 字段名称 | 数据类型 | 描述
| ---- | ----- | ---- |
| data | map | 撤单结果

### 错误码

> Response:

```json
{
  "status": "ok",
  "data": {
    "success": ["123","456"],
    "failed": [
      {
        "err-msg": "订单状态错误",
        "order-id": "12345678",
        "err-code": "order-orderstate-error",
        "order-state":-1 // 当前订单状态
      }
    ]
  }
}
```

返回字段列表中，order-state的可能取值包括 -

order-state           |  Description
---------       | -----------
-1| order was already closed in the long past (order state = canceled, partial-canceled, filled, partial-filled)
5| partial-canceled
6| filled
7| canceled
10| cancelling

## 查询订单详情

API Key 权限：读取

此接口返回指定订单的最新状态和详情。

### HTTP 请求

- GET `/v1/order/orders/{order-id}`


### 请求参数

| 参数名称     | 是否必须 | 类型  | 描述   | 默认值  | 取值范围 |
| -------- | ---- | ------ | -----  | ---- | ---- |
| order-id | true | string | 订单ID，填在path中 |      |      |


> Response:

```json
{  
  "data": 
  {
    "id": 59378,
    "symbol": "ethusdt",
    "account-id": 100009,
    "amount": "10.1000000000",
    "price": "100.1000000000",
    "created-at": 1494901162595,
    "type": "buy-limit",
    "field-amount": "10.1000000000",
    "field-cash-amount": "1011.0100000000",
    "field-fees": "0.0202000000",
    "finished-at": 1494901400468,
    "user-id": 1000,
    "source": "api",
    "state": "filled",
    "canceled-at": 0
  }
}
```

### 响应数据

| 字段名称     | 是否必须  | 数据类型   | 描述   | 取值范围     |
| ----------------- | ----- | ------ | -------  | ----  |
| account-id        | true  | long   | 账户 ID    |       |
| amount            | true  | string | 订单数量              |    |
| canceled-at       | false | long   | 订单撤销时间    |     |
| created-at        | true  | long   | 订单创建时间    |   |
| field-amount      | true  | string | 已成交数量    |     |
| field-cash-amount | true  | string | 已成交总金额     |      |
| field-fees        | true  | string | 已成交手续费（买入为币，卖出为钱） |     |
| finished-at       | false | long   | 订单变为终结态的时间，不是成交时间，包含“已撤单”状态    |     |
| id                | true  | long   | 订单ID    |     |
| price             | true  | string | 订单价格       |     |
| source            | true  | string | 订单来源   | api |
| state             | true  | string | 订单状态   | submitted 已提交, partial-filled 部分成交, partial-canceled 部分成交撤销, filled 完全成交, canceled 已撤销， created |
| symbol            | true  | string | 交易对   | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 订单类型   | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit  |
| stop-price              | false  | string | 止盈止损订单触发价格   | |
| operator              | false  | string | 止盈止损订单触发价运算符   | gte,lte |


## 查询订单详情（基于client order ID）

API Key 权限：读取

此接口返回指定订单的最新状态和详情。

### HTTP 请求

- GET `/v1/order/orders/getClientOrder`

```json
{
  "clientOrderId": "a0001"
}
```

### 请求参数

| 参数名称     | 是否必须 | 类型  | 描述   | 默认值  | 取值范围 |
| -------- | ---- | ------ | -----  | ---- | ---- |
| clientOrderId | true | string | 用户自编订单号 |      |      |

> Response:

```json
{  
  "data": 
  {
    "id": 59378,
    "symbol": "ethusdt",
    "account-id": 100009,
    "amount": "10.1000000000",
    "price": "100.1000000000",
    "created-at": 1494901162595,
    "type": "buy-limit",
    "field-amount": "10.1000000000",
    "field-cash-amount": "1011.0100000000",
    "field-fees": "0.0202000000",
    "finished-at": 1494901400468,
    "user-id": 1000,
    "source": "api",
    "state": "filled",
    "canceled-at": 0
  }
}
```

### 响应数据

| 字段名称     | 是否必须  | 数据类型   | 描述   | 取值范围     |
| ----------------- | ----- | ------ | -------  | ----  |
| account-id        | true  | long   | 账户 ID    |       |
| amount            | true  | string | 订单数量              |    |
| canceled-at       | false | long   | 订单撤销时间    |     |
| created-at        | true  | long   | 订单创建时间    |   |
| field-amount      | true  | string | 已成交数量    |     |
| field-cash-amount | true  | string | 已成交总金额     |      |
| field-fees        | true  | string | 已成交手续费（买入为币，卖出为钱） |     |
| finished-at       | false | long   | 订单变为终结态的时间，不是成交时间，包含“已撤单”状态    |     |
| id                | true  | long   | 订单ID    |     |
| price             | true  | string | 订单价格       |     |
| source            | true  | string | 订单来源   | api |
| state             | true  | string | 订单状态   | submitted 已提交, partial-filled 部分成交, partial-canceled 部分成交撤销, filled 完全成交, canceled 已撤销，created |
| symbol            | true  | string | 交易对   | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 订单类型   | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| stop-price              | false  | string | 止盈止损订单触发价格   | |
| operator              | false  | string | 止盈止损订单触发价运算符   | gte,lte |

如client order ID不存在，返回如下错误信息 
{
    "status": "error",
    "err-code": "base-record-invalid",
    "err-msg": "record invalid",
    "data": null
}

## 成交明细

API Key 权限：读取

此接口返回指定订单的成交明细。

### HTTP 请求

- GET `/v1/order/orders/{order-id}/matchresults`



### 请求参数

| 参数名称  | 是否必须 | 类型  | 描述  | 默认值  | 取值范围 |
| -------- | ---- | ------ | -----  | ---- | ---- |
| order-id | true | string | 订单ID，填在path中 |      |      |


> Response:

```json
{  
  "data": [
    {
      "id": 29553,
      "order-id": 59378,
      "match-id": 59335,
      "trade-id": 100282808529,
      "symbol": "ethusdt",
      "type": "buy-limit",
      "source": "api",
      "price": "100.1000000000",
      "filled-amount": "9.1155000000",
      "filled-fees": "0.0182310000",
      "created-at": 1494901400435,
      "role": "maker",
      "filled-points": "0.0",
      "fee-deduct-currency": ""
    }
    ...
  ]
}
```

### 响应数据

<aside class="notice">返回的主数据对象为一个对象数组，其中每一个元件代表一个交易结果。</aside>

| 字段名称    | 是否必须 | 数据类型   | 描述   | 取值范围     |
| ------------- | ---- | ------ | -------- | -------- |
| created-at    | true | long   | 成交时间     |    |
| filled-amount | true | string | 成交数量     |    |
| filled-fees   | true | string | 成交手续费，如果为空或为0，代表使用了其他币种进行了抵扣，可根据filled-points和fee-deduct-currency字段判断    |     |
| id            | true | long   | 订单成交记录ID |     |
| match-id      | true | long   | 撮合ID，订单在撮合中执行的顺序ID     |     |
| order-id      | true | long   | 订单ID，成交所属订单的ID    |      |
| trade-id      | false | integer   | Unique trade ID (NEW)唯一成交编号，成交时产生的唯一编号ID    |     |
| price         | true | string | 成交价格  |    |
| source        | true | string | 订单来源  | api      |
| symbol        | true | string | 交易对   | btcusdt, ethbtc, rcneth ...  |
| type          | true | string | 订单类型   | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| role      | true | string   | 成交角色    |maker,taker      |
| filled-points      | true | string   | 抵扣数量（可为ht或hbpoint）    |     |
| fee-deduct-currency      | true | string   | 抵扣类型    |如果为空，代表扣除的手续费是原币；如果为"ht"，代表抵扣手续费的是HT；如果为"hbpoint"，代表抵扣手续费的是点卡     |

## 搜索历史订单

API Key 权限：读取

此接口基于搜索条件查询历史订单。

### HTTP 请求

- GET `/v1/order/orders`

```json
{
   "account-id": "100009",
   "amount": "10.1",
   "price": "100.1",
   "source": "api",
   "symbol": "ethusdt",
   "type": "buy-limit"
}
```


### 请求参数

| 参数名称   | 是否必须  | 类型     | 描述   | 默认值  | 取值范围   |
| ---------- | ----- | ------ | ------  | ---- | ----  |
| symbol     | true  | string | 交易对      |      |<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>  |
| types      | false | string | 查询的订单类型组合，使用','分割  |      | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| start-date | false | string | 查询开始日期, 日期格式yyyy-mm-dd。 以订单生成时间进行查询 | -1d 查询结束日期的前1天 | 取值范围 [((end-date) – 1), (end-date)] ，查询窗口最大为2天，窗口平移范围为最近180天，已完全撤销的历史订单的查询窗口平移范围只有最近7天(state="canceled") |
| end-date   | false | string | 查询结束日期, 日期格式yyyy-mm-dd。 以订单生成时间进行查询 | today     | 取值范围 [(today-179), today] ，查询窗口最大为2天，窗口平移范围为最近180天，已完全撤销的历史订单的查询窗口平移范围只有最近7天(state="canceled")   |
| states     | true  | string | 查询的订单状态组合，使用','分割  |      | submitted 已提交, partial-filled 部分成交, partial-canceled 部分成交撤销, filled 完全成交, canceled 已撤销，created|
| from       | false | string | 查询起始 ID   |      |    |
| direct     | false | string | 查询方向   |      | prev 向前，时间（或 ID）正序；next 向后，时间（或 ID）倒序）    |
| size       | false | string | 查询记录大小      | 100     |  [1, 100]       |



> Response:

```json
{  
  "data": [
    {
      "id": 59378,
      "symbol": "ethusdt",
      "account-id": 100009,
      "amount": "10.1000000000",
      "price": "100.1000000000",
      "created-at": 1494901162595,
      "type": "buy-limit",
      "field-amount": "10.1000000000",
      "field-cash-amount": "1011.0100000000",
      "field-fees": "0.0202000000",
      "finished-at": 1494901400468,
      "user-id": 1000,
      "source": "api",
      "state": "filled",
      "canceled-at": 0
    }
    ...
  ]
}
```

### 响应数据

| 参数名称    | 是否必须  | 数据类型   | 描述   | 取值范围   |
| ----------------- | ----- | ------ | ----------------- | ----  |
| account-id        | true  | long   | 账户 ID    |     |
| amount            | true  | string | 订单数量    |   |
| canceled-at       | false | long   | 接到撤单申请的时间   |    |
| created-at        | true  | long   | 订单创建时间   |    |
| field-amount      | true  | string | 已成交数量   |    |
| field-cash-amount | true  | string | 已成交总金额    |    |
| field-fees        | true  | string | 已成交手续费（买入为基础币，卖出为计价币） |       |
| finished-at       | false | long   | 最后成交时间    |   |
| id                | true  | long   | 订单ID    |    |
| price             | true  | string | 订单价格  |    |
| source            | true  | string | 订单来源   | api  |
| state             | true  | string | 订单状态    | submitted 已提交, partial-filled 部分成交, partial-canceled 部分成交撤销, filled 完全成交, canceled 已撤销，created |
| symbol            | true  | string | 交易对    | btcusdt, ethbtc, rcneth ... |
| type              | true  | string | 订单类型  | submit-cancel：已提交撤单申请  ,buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| stop-price              | false  | string | 止盈止损订单触发价格   | |
| operator              | false  | string | 止盈止损订单触发价运算符   | gte,lte |

### start-date, end-date相关错误码

|错误码|对应错误场景|
|------------|----------------------------------------------|
|invalid_interval| start date小于end date; 或者 start date 与end date之间的时间间隔大于2天|
|invalid_start_date|start date是一个180天之前的日期；或者start date是一个未来的日期|
|invalid_end_date|end date 是一个180天之前的日期；或者end date是一个未来的日期| 


## 搜索最近48小时内历史订单

API Key 权限：读取

此接口基于搜索条件查询最近48小时内历史订单。

### HTTP 请求

- GET `/v1/order/history`

```json
{
   "symbol": "btcusdt",
   "start-time": "1556417645419",
   "end-time": "1556533539282",
   "direct": "prev",
   "size": "10"
}
```


### 请求参数

| 参数名称   | 是否必须  | 类型     | 描述   | 默认值  | 取值范围   |
| ---------- | ----- | ------ | ------  | ---- | ----  |
| symbol     | false  | string | 交易对      |all      |<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>  |
| start-time      | false | long | 查询起始时间（含）  |48小时前的时刻      |UTC time in millisecond |
| end-time | false | long | 查询结束时间（含） | 查询时刻     |UTC time in millisecond |
| direct   | false | string | 订单查询方向（注：仅在检索出的总条目数量超出size字段限定时起作用；如果检索出的总条目数量在size 字段限定内，direct 字段不起作用。） | next     |prev, next   |
| size     | false  | int | 每次返回条目数量  |100      | [10,1000] |



> Response:

```json
{
    "status": "ok",
    "data": [
        {
            "id": 31215214553,
            "symbol": "btcusdt",
            "account-id": 4717043,
            "amount": "1.000000000000000000",
            "price": "1.000000000000000000",
            "created-at": 1556533539282,
            "type": "buy-limit",
            "field-amount": "0.0",
            "field-cash-amount": "0.0",
            "field-fees": "0.0",
            "finished-at": 1556533568953,
            "source": "web",
            "state": "canceled",
            "canceled-at": 1556533568911
        }
    ]
}
```

### 响应数据

| 参数名称    | 是否必须  | 数据类型   | 描述   | 取值范围   |
| ----------------- | ----- | ------ | ----------------- | ----  |
| {account-id        | true  | long   | 账户 ID    |     |
| amount            | true  | string | 订单数量    |   |
| canceled-at       | false | long   | 接到撤单申请的时间   |    |
| created-at        | true  | long   | 订单创建时间   |    |
| field-amount      | true  | string | 已成交数量   |    |
| field-cash-amount | true  | string | 已成交总金额    |    |
| field-fees        | true  | string | 已成交手续费（买入为基础币，卖出为计价币） |       |
| finished-at       | false | long   | 最后成交时间    |   |
| id                | true  | long   | 订单ID    |    |
| price             | true  | string | 订单价格  |    |
| source            | true  | string | 订单来源   | api  |
| state             | true  | string | 订单状态    | partial-canceled 部分成交撤销, filled 完全成交, canceled 已撤销 |
| symbol            | true  | string | 交易对    | btcusdt, ethbtc, rcneth ... |
| stop-price              | false  | string | 止盈止损订单触发价格   | |
| operator              | false  | string | 止盈止损订单触发价运算符   | gte,lte |
| type}              | true  | string | 订单类型  | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单, buy-limit-maker, sell-limit-maker, buy-limit-maker, sell-limit-maker |
| next-time            | false  | long |下一查询起始时间（当请求字段”direct”为”prev”时有效）, 下一查询结束时间（当请求字段”direct”为”next”时有效）。注：仅在检索出的总条目数量超出size字段限定时，此返回字段存在。 |UTC time in millisecond   |


## 当前和历史成交

API Key 权限：读取

此接口基于搜索条件查询当前和历史成交记录。

### HTTP 请求

- GET `/v1/order/matchresults`


### 请求参数

| 参数名称   | 是否必须  | 类型  | 描述   | 默认值  | 取值范围    |
| ---------- | ----- | ------ | ------ | ---- | ----------- |
| symbol     | true  | string | 交易对   | NA |  <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>  |
| types      | false | string | 查询的订单类型组合，使用','分割   |  all    | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单, buy-limit-maker, sell-limit-maker, buy-stop-limit, sell-stop-limit |
| start-date | false | string | 查询开始日期, 日期格式yyyy-mm-dd | -1d 查询结束日期的前1天     | 取值范围 [((end-date) – 1), (end-date)] ，查询窗口最大为2天，窗口平移范围为最近61天 |
| end-date   | false | string | 查询结束日期, 日期格式yyyy-mm-dd |   today   | 取值范围 [(today-60), today] ，查询窗口最大为2天，窗口平移范围为最近61天  |
| from       | false | string | 查询起始 ID    |   订单成交记录ID（最大值）   |     |
| direct     | false | string | 查询方向    |   默认 next， 成交记录 ID 由大到小排序   | prev 向前，时间（或 ID）正序；next 向后，时间（或 ID）倒序）   |
| size       | false | string | 查询记录大小    |   100   | [1，100]  |

> Response:

```json
{  
  "data": [
    {
      "id": 29553,
      "order-id": 59378,
      "match-id": 59335,
      "symbol": "ethusdt",
      "type": "buy-limit",
      "source": "api",
      "price": "100.1000000000",
      "filled-amount": "9.1155000000",
      "filled-fees": "0.0182310000",
      "created-at": 1494901400435,
      "trade-id": 100282808529,
      "role": taker,
      "filled-points": "0.0",
      "fee-deduct-currency": ""
    }
    ...
  ]
}
```

### 响应数据

<aside class="notice">返回的主数据对象为一个对象数组，其中每一个元件代表一个交易结果。</aside>

| 参数名称   | 是否必须 | 数据类型   | 描述   | 取值范围   |
| ------------- | ---- | ------ | -------- | ------- |
| created-at    | true | long   | 成交时间     |    |
| filled-amount | true | string | 成交数量     |    |
| filled-fees   | true | string | 成交手续费    |    |
| id            | true | long   | 订单成交记录 ID |    |
| match-id      | true | long   | 撮合 ID     |    |
| order-id      | true | long   | 订单 ID    |    |
| trade-id      | false | integer   | Unique trade ID (NEW)唯一成交编号    |      |
| price         | true | string | 成交价格     |    |
| source        | true | string | 订单来源     | api   |
| symbol        | true | string | 交易对      | btcusdt, ethbtc, rcneth ...  |
| type          | true | string | 订单类型     | buy-market：市价买, sell-market：市价卖, buy-limit：限价买, sell-limit：限价卖, buy-ioc：IOC买单, sell-ioc：IOC卖单， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit |
| role      | true | string   | 成交角色    |maker,taker      |
| filled-points      | true | string   | 抵扣数量（可为ht或hbpoint）    |     |
| fee-deduct-currency      | true | string   | 抵扣类型    |ht,hbpoint     |

### start-date, end-date相关错误码 

|错误码|对应错误场景|
|------------|----------------------------------------------|
|invalid_interval| start date小于end date; 或者 start date 与end date之间的时间间隔大于2天|
|invalid_start_date|start date是一个61天之前的日期；或者start date是一个未来的日期|
|invalid_end_date|end date 是一个61天之前的日期；或者end date是一个未来的日期| 


## 获取用户当前手续费率

Api用户查询交易对费率，一次限制最多查10个交易对，子用户的费率和母用户保持一致

API Key 权限：读取

```shell
curl "https://api.huobi.pro/v1/fee/fee-rate/get?symbols=btcusdt,ethusdt,ltcusdt"
```

### HTTP 请求

- GET `/v1/fee/fee-rate/get`

### 请求参数

参数       | 数据类型 | 是否必须 | 默认值 | 描述 | 取值范围
--------- | --------- | -------- | ------- | ------ | ------
symbols    | string    | true     | NA      | 交易对，可多填，逗号分隔 | 如"btcusdt,ethusdt",<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>

> Response:

```json
{
  "status": "ok",
  "data": [
     {
        "symbol": "btcusdt",
        "maker-fee":"0.0001",
        "taker-fee":"0.0002"
     },
     {
        "symbol": "ethusdt",
        "maker-fee":"0.002",
        "taker-fee":"0.002"
    },
     {
        "symbol": "ltcusdt",
        "maker-fee":"0.0015",
        "taker-fee":"0.0018"
    }
  ]
}
```
### 响应数据

字段名称      | 数据类型 | 是否强制| 描述
--------- | --------- | -----------| -----------
status        | string  |Y | 响应状态
err-code    | string   |N  | 响应码
err-msg     | string |N   | 响应信息
data|list|Y|交易对费率列表

### List
属性|	数据类型|	说明
--------- | --------- | ------
symbol|	string|	交易对
maker-fee|	string|	挂单手续费率
taker-fee|	string|	吃单手续费率

### 响应码
响应码|	说明|	类型|	备注
--------- | --------- | ------ | ------
base-symbol-error|	无效的交易对|	string|	-
base-too-many-symbol|	最大支持 10 个交易对|	string|	-

# 借贷（逐仓杠杆）

<aside class="notice">访问借贷相关的接口需要进行签名认证。</aside>

<aside class="notice">目前逐仓杠杆交易仅支持部分以 USDT，HSUD， 和 BTC 为报价币种的交易对。</aside>

## 资产划转

API Key 权限：交易

此接口用于现货账户与逐仓杠杆账户的资产互转。

从现货账户划转至逐仓杠杆账户 `transfer-in`，从逐仓杠杆账户划转至现货账户 `transfer-out`

### HTTP 请求

- POST ` /v1/dw/transfer-in/margin`

- POST ` /v1/dw/transfer-out/margin`

```json
{
  "symbol": "ethusdt",
  "currency": "eth",
  "amount": "1.0"
}
```


### 请求参数

参数名称 | 数据类型 | 是否必需 | 默认值 | 描述
---------  | --------- | -------- | ------- | -----------
symbol     | string    | true     | NA      | 交易对,<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
currency   | string    | true     | NA      | 币种，<a href="https://huobiapi.github.io/docs/spot/v1/cn/#7393cc8596">取值参考</a>
amount     | string    | true     | NA      | 划转数量


> Response:

```json
{  
  "data": 1000
}
```

### 响应数据


参数名称 | 数据类型 | 描述
------ | ------- | -----
data   | integer | Transfer id


## 申请借贷

API Key 权限：交易

此接口用于申请借贷.

### HTTP 请求

- POST ` /v1/margin/orders`

```json
{
  "symbol": "ethusdt",
  "currency": "eth",
  "amount": "1.0"
}
```


### 请求参数

参数名称 | 数据类型 | 是否必需 | 默认值 | 描述
---------  | --------- | -------- | ------- | -----------
symbol     | string    | true     | NA      | 交易对, <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
currency   | string    | true     | NA      | 币种，<a href="https://huobiapi.github.io/docs/spot/v1/cn/#7393cc8596">取值参考</a>
amount     | string    | true     | NA      | 借贷数量

> Response:

```json
{  
  "data": 1000
}
```


### 响应数据

字段名称| 数据类型 | 描述
-------| ------  | ----
data   | integer | Margin order id



## 归还借贷

API Key 权限：交易

此接口用于归还借贷.

### HTTP 请求

- POST ` /v1/margin/orders/{order-id}/repay`

```json
{
  "amount": "1.0"
}
```


### 请求参数

参数名称 | 数据类型 | 是否必需 | 描述
---------  | --------- | -------- | -----------
order-id   | string    | true     | 借贷订单 ID，写在 url path 中
amount     | string    | true     | 归还币种数量


> Response:

```json
{  
  "data": 1000
}
```

### 响应数据


参数名称     | 数据类型 | 描述
-------  | ------- | -----------
data     | integer | Margin order id


## 查询借贷订单

API Key 权限：读取

此接口基于指定搜索条件返回借贷订单。

### HTTP 请求

- GET ` /v1/margin/loan-orders`

### 请求参数

| 参数名称       | 是否必须  | 类型     | 描述    | 默认值  | 取值范围   |
| ----- | ----- | ------ |  -------  | ---- |  ----  |
| symbol | true | string | 交易对  |  |  |
| start-date | false | string | 查询开始日期, 日期格式yyyy-mm-dd  |     |    |
| end-date | false | string | 查询结束日期, 日期格式yyyy-mm-dd  |    |    |
| states | false | string | 状态 |     |   |
| from   | false | string | 查询起始 ID  |    |     |
| direct | false | string | 查询方向     |    | prev 向前，时间（或 ID）正序；next 向后，时间（或 ID）倒序） |
| size   | false | string | 查询记录大小  |    |     |
| sub-uid   | false | int | 子用户编号（母用户查询子用户借贷订单时，此字段必填）  |如不填，缺省查询当前用户借贷订单    |     |

> Response:

```json
{  
  "data": [
    {
      "loan-balance": "0.100000000000000000",
      "interest-balance": "0.000200000000000000",
      "interest-rate": "0.002000000000000000",
      "loan-amount": "0.100000000000000000",
      "accrued-at": 1511169724531,
      "interest-amount": "0.000200000000000000",
      "symbol": "ethbtc",
      "currency": "btc",
      "id": 394,
      "state": "accrual",
      "account-id": 17747,
      "user-id": 119913,
      "created-at": 1511169724531
    }
  ]
}
```

### 响应数据


| 字段名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
|-----|-----|-----|-----|------|
|   id  |  true  |  long  |  订单号 | |
|   user-id  |  true  |  long  | 用户ID | |
|   account-id  |  true  |  long  |  账户ID | |
|   symbol  |  true  |  string  |  交易对 | |
|   currency  |  true  |  string  |  币种 | |
| loan-amount | true |string | 借贷本金总额 | |
| loan-balance | true | string | 未还本金 | |
| interest-rate | true | string | 利率 | |
| interest-amount | true | string | 利息总额 | |
| interest-balance | true | string | 未还利息 | |
| created-at | true | long | 借贷发起时间 | |
| accrued-at | true | long | 最近一次计息时间 | |
| state | true | string | 订单状态 |created 未放款，accrual 已放款，cleared 已还清，invalid 异常|


## 借贷账户详情

API Key 权限：读取

此接口返回借贷账户详情。

### HTTP 请求

- GET `/v1/margin/accounts/balance`

```json
{
   "symbol": "ethusdt"
}
```

### 请求参数

| 参数名称 | 是否必须 | 类型 | 描述 | 默认值 | 取值范围 |
|---------|---------|-----|-----|-------|--------|
| symbol | false | string | 交易对，作为get参数,<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>  |  |  |
| sub-uid | false | int | 子用户编号（母用户查询子用户借贷详情时，此字段必填）  | 如不填，缺省查询当前用户借贷详情 |  |

> Response:

```json
{  
  "data": [
    {
      "id": 18264,
      "type": "margin",
      "state": "working",
      "symbol": "btcusdt",
      "fl-price": "0",
      "fl-type": "safe",
      "risk-rate": "475.952571086994250554",
      "list": [
          {
              "currency": "btc",
              "type": "trade",
              "balance": "1168.533000000000000000"
          },
          {
              "currency": "btc",
              "type": "frozen",
              "balance": "0.000000000000000000"
          },
          {
              "currency": "btc",
              "type": "loan",
              "balance": "-2.433000000000000000"
          },
          {
              "currency": "btc",
              "type": "interest",
              "balance": "-0.000533000000000000"
          },
          {
              "currency": "btc",
              "type": "transfer-out-available",//可转btc
              "balance": "1163.872174670000000000"
          },
          {
              "currency": "btc",
              "type": "loan-available",//可借btc
              "balance": "8161.876538350676000000"
          }
      ]
    }
  ]
}
```

### 响应数据

| 字段名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
|-----|-----|-----|-----|------|
| symbol  |  true  |  string  |  交易对 | |
| state  |  true  |  string  |  账户状态 | working,fl-sys,fl-mgt,fl-end |
| risk-rate | true | string | 风险率 | |
| fl-price | true | string | 爆仓价 | |
| list | true | array | 借贷账户详情列表 | |

# 借贷（全仓杠杆）

<aside class="notice">访问借贷相关的接口需要进行签名认证。</aside>

<aside class="notice">目前全仓杠杆交易仅支持部分以 USDT 和 BTC 为报价币种的交易对。</aside>

## 资产划转

API Key 权限：交易

此接口用于现货账户与全仓杠杆账户的资产互转。

从现货账户划转至全仓杠杆账户 `transfer-in`，从全仓杠杆账户划转至现货账户 `transfer-out`

### HTTP 请求

- POST ` /v1/cross-margin/transfer-in`
- POST ` /v1/cross-margin/transfer-out`

```json
{
  "currency": "eth",
  "amount": "1.0"
}
```


### 请求参数

参数名称 | 数据类型 | 是否必需 | 默认值 | 描述
---------  | --------- | -------- | ------- | -----------
currency   | string    | true     | NA      | 币种
amount     | string    | true     | NA      | 划转数量


> Response:

```json
{  
  "status": "ok",
  "data": 1000
}
```

### 响应数据


参数名称 | 数据类型 | 描述
------ | ------- | -----
data   | integer | Transfer id

## 查询借贷利率

API Key 权限：读取

此接口返回用户级别的借贷利率。

### HTTP 请求

- GET ` /v1/cross-margin/interest-rate`

### 请求参数

Null

> Response:

```json
{
    "status": "ok",
    "data": [
        {
            "currency": "bch",
            "interest-rate": "0.001"
        },
        {
            "currency": "btc",
            "interest-rate": "0.001"
        },
        {
            "currency": "eos",
            "interest-rate": "0.001"
        },
        {
            "currency": "eth",
            "interest-rate": "0.001"
        },
        {
            "currency": "ltc",
            "interest-rate": "0.001"
        },
        {
            "currency": "usdt",
            "interest-rate": "0.001"
        },
        {
            "currency": "xrp",
            "interest-rate": "0.001"
        }
    ]
}
```

### 响应数据


参数名称 | 数据类型 | 描述
------ | ------- | -----
currency   | string | 币种，<a href="https://huobiapi.github.io/docs/spot/v1/cn/#7393cc8596">取值参考</a>
interest-rate|string|借贷利率


## 申请借贷

API Key 权限：交易

此接口用于申请借贷.

### HTTP 请求

- POST ` /v1/cross-margin/orders`

```json
{
  "currency": "eth",
  "amount": "1.0"
}
```


### 请求参数

参数名称 | 数据类型 | 是否必需 | 默认值 | 描述
---------  | --------- | -------- | ------- | -----------
currency   | string    | true     | NA      | 币种
amount     | string    | true     | NA      | 借贷数量

> Response:

```json
{  
  "status": "ok",
  "data": 1000
}
```


### 响应数据

字段名称| 数据类型 | 描述
-------| ------  | ----
data   | integer | Margin order id



## 归还借贷

API Key 权限：交易

此接口用于归还借贷.

### HTTP 请求

- POST ` /v1/cross-margin/orders/{order-id}/repay`

```json
{
  "amount": "1.0"
}
```


### 请求参数

参数名称 | 数据类型 | 是否必需 | 描述
---------  | --------- | -------- | -----------
order-id   | string    | true     | 借贷订单 ID，写在 url path 中
amount     | string    | true     | 归还币种数量


> Response:

```json
{  
  "status": "ok",
  "data": null
}
```

### 响应数据


参数名称     | 数据类型 | 描述
-------  | ------- | -----------
data     | null | -


## 查询借贷订单

API Key 权限：读取

此接口基于指定搜索条件返回借贷订单。

### HTTP 请求

- GET ` /v1/cross-margin/loan-orders`

### 请求参数

| 参数名称       | 是否必须  | 类型     | 描述    | 默认值  | 取值范围   |
| ----- | ----- | ------ |  -------  | ---- |  ----  |
| start-date | false | string | 查询开始日期, 日期格式yyyy-mm-dd  |     |    |
| end-date | false | string | 查询结束日期, 日期格式yyyy-mm-dd  |    |    |
| currency | false | string | 币种   |   |
| state | false | string | 状态   |all   |created 未放款，accrual 已放款，cleared 已还清，invalid 异常
| from   | false | string | 查询起始 ID  | 0   |     |
| direct | false | string | 查询方向     |    | prev 向前，时间（或 ID）正序；next 向后，时间（或 ID）倒序） |
| size   | false | string | 查询记录大小  |  10  |[10,100]     |

> Response:

```json
{  
  "status": "ok",
  "data": [
    {
      "loan-balance": "0.100000000000000000",
      "interest-balance": "0.000200000000000000",
      "loan-amount": "0.100000000000000000",
      "accrued-at": 1511169724531,
      "interest-amount": "0.000200000000000000",
      "filled-points" : "0.2",
      "filled-ht" : "0.2",
      "currency": "btc",
      "id": 394,
      "state": "accrual",
      "account-id": 17747,
      "user-id": 119913,
      "created-at": 1511169724531
    }
  ]
}
```

### 响应数据


| 字段名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
|-----|-----|-----|-----|------|
|   id  |  true  |  long  |  订单号 | |
|   user-id  |  true  |  long  | 用户ID | |
|   account-id  |  true  |  long  |  账户ID | |
|   currency  |  true  |  string  |  币种 | |
| loan-amount | true |string | 借贷本金总额 | |
| loan-balance | true | string | 未还本金 | |
| interest-amount | true | string | 利息总额 | |
| interest-balance | true | string | 未还利息 | |
| filled-points | true | string | 点卡抵扣数量 | |
| filled-ht | true | string | HT抵扣数量 | |
| created-at | true | long | 借贷发起时间 | |
| accrued-at | true | long | 最近一次计息时间 | |
| state | true | string | 订单状态 |created 未放款，accrual 已放款，cleared 已还清，invalid 异常|


## 借贷账户详情

API Key 权限：读取

此接口返回借贷账户详情。

### HTTP 请求

- GET `/v1/cross-margin/accounts/balance`

### 请求参数

无

> Response:

```json
{  
  "status": "ok",
  "data": [
    {
      "id": 18264,
      "type": "cross-margin",
      "state": "working",
      "risk-rate": "1000",
      "acct-balance-sum": "12312.123123",
      "debt-balance-sum": "1231.2123123",
      "list": [
          {
              "currency": "btc",
              "type": "trade",
              "balance": "1168.533000000000000000"
          },
          {
              "currency": "btc",
              "type": "frozen",
              "balance": "0.000000000000000000"
          },
          {
              "currency": "btc",
              "type": "loan",
              "balance": "-2.433000000000000000"
          },
          {
              "currency": "btc",
              "type": "interest",
              "balance": "-0.000533000000000000"
          },
          {
              "currency": "btc",
              "type": "transfer-out-available",//可转btc
              "balance": "1163.872174670000000000"
          },
          {
              "currency": "btc",
              "type": "loan-available",//可借btc
              "balance": "8161.876538350676000000"
          }
      ]
    }
  ]
}
```

### 响应数据

| 字段名称 | 是否必须 | 数据类型 | 描述 | 取值范围 |
|-----|-----|-----|-----|------|
| id | true | integer | Account ID 账户编号 | |
| type | true | integer | 账户类型 (margin or cross-margin) |cross-margin |
| state  |  true  |  string  |  账户状态 | working,fl-sys,fl-end,fl-negative |
| risk-rate | true | string | 风险率 | |
| acct-balance-sum | true | string | 总持有usdt折合 | |
| debt-balance-sum | true | string | 总负债usdt折合 | |
| list | true | array | 借贷账户详情列表 | |
| { currency | true | string | 币种| |
|   type | true | string | 账户类型| trade,frozen,loan,interest,transfer-out-available,loan-available|
|   balance } | true | string | 余额（注：当type= transfer-out-available时，如果balance=-1，意味着该币种余额可全部转出）| |

# Websocket行情数据

## 简介

### 接入URL

**Global站行情请求地址**

**`wss://api.huobi.pro/ws`**  

**`wss://api-aws.huobi.pro/ws`**  

注：api-aws.huobi.pro域名对使用aws云服务的用户做了一定的链路延迟优化。  

请使用中国大陆以外的服务器访问火币 API

### 数据压缩

WebSocket API 返回的所有数据都进行了 GZIP 压缩，需要 client 在收到数据之后解压。

### 心跳消息

当用户的Websocket客户端连接到火币Websocket服务器后，服务器会定期（当前设为5秒）向其发送`ping`消息并包含一整数值如下：

> {"ping": 1492420473027} （行情）

当用户的Websocket客户端接收到此心跳消息后，应返回`pong`消息并包含同一整数值：

> {"pong": 1492420473027} （行情）

<aside class="warning">当Websocket服务器连续两次发送了`ping`消息却没有收到任何一次`pong`消息返回后，服务器将主动断开与此客户端的连接。</aside>


### 订阅主题

成功建立与Websocket服务器的连接后，Websocket客户端发送如下请求以订阅特定主题：

```json
{
  "sub": "market.btcusdt.kline.1min",
  "id": "id1"
}
```

{
  "sub": "topic to sub",
  "id": "id generate by client"
}

成功订阅后，Websocket客户端将收到确认：

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.kline.1min",
  "ts": 1489474081631
}
```

之后, 一旦所订阅的主题有更新，Websocket客户端将收到服务器推送的更新消息（push）：

```json
{
  "ch": "market.btcusdt.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```

### 取消订阅

取消订阅的格式如下：

```json
{
  "unsub": "market.btcusdt.trade.detail",
  "id": "id4"
}
```

{
  "unsub": "topic to unsub",
  "id": "id generate by client"
}

取消订阅成功确认：

```json
{
  "id": "id4",
  "status": "ok",
  "unsubbed": "market.btcusdt.trade.detail",
  "ts": 1494326028889
}
```

### 请求数据

Websocket服务器同时支持一次性请求数据（pull）。

请求数据的格式如下：

```json
{
  "req": "market.ethbtc.kline.1min",
  "id": "id10"
}
```

{
  "req": "topic to req",
  "id": "id generate by client"
}

一次性返回的数据：

```json
{
  "status": "ok",
  "rep": "market.btcusdt.kline.1min",
  "data": [
    {
      "amount": 1.6206,
      "count":  3,
      "id":     1494465840,
      "open":   9887.00,
      "close":  9885.00,
      "low":    9885.00,
      "high":   9887.00,
      "vol":    16021.632026
    },
    {
      "amount": 2.2124,
      "count":  6,
      "id":     1494465900,
      "open":   9885.00,
      "close":  9880.00,
      "low":    9880.00,
      "high":   9885.00,
      "vol":    21859.023500
    }
  ]
}
```

## K线数据

### 主题订阅

一旦K线数据产生，Websocket服务器将通过此订阅主题接口推送至客户端：

`market.$symbol$.kline.$period$`

> 订阅请求

```json
{
  "sub": "market.ethbtc.kline.1min",
  "id": "id1"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 描述                      | 取值范围
--------- | --------- | -------- | -----------                      | -----------
symbol    | string    | true     | 交易代码     | <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
period     | string    | true     | K线周期   | 1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.ethbtc.kline.1min",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.ethbtc.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```

### 数据更新字段列表

字段     | 数据类型 | 描述
--------- | --------- | -----------
id        | integer   | unix时间，同时作为K线ID
amount    | float     | 成交量
count     | integer   | 成交笔数
open      | float     | 开盘价
close     | float     | 收盘价（当K线为最晚的一根时，是最新成交价）
low       | float     | 最低价
high      | float     | 最高价
vol       | float     | 成交额, 即 sum(每一笔成交价 * 该笔的成交量)

<aside class="notice">当symbol被设为“hb10”或“huobi10”时，amount，count，vol均为零值。</aside>

### 数据请求

用请求方式一次性获取K线数据需要额外提供以下参数：
（每次最多返回300条）

```json
{
  "req": "market.$symbol.kline.$period",
  "id": "id generated by client",
  "from": "from time in epoch seconds",
  "to": "to time in epoch seconds"
}
```

参数 | 数据类型 | 是否必需 | 缺省值                          | 描述      | 取值范围
--------- | --------- | -------- | -------------                          | -----------      | -----------
from      | integer   | false    | 1501174800(2017-07-28T00:00:00+08:00)  | 起始时间 (epoch time in second)   | [1501174800, 2556115200]
to        | integer   | false    | 2556115200(2050-01-01T00:00:00+08:00)  | 结束时间 (epoch time in second)      | [1501174800, 2556115200] or ($from, 2556115200] if "from" is set

## 市场深度行情数据

此主题发送最新市场深度快照。快照频率为每秒1次。

### 主题订阅

`market.$symbol.depth.$type`

> Subscribe request

```json
{
  "sub": "market.btcusdt.depth.step0",
  "id": "id1"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                       | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | 交易代码                   | <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
type      | string    | true     | step0                 | 合并深度类型     | step0, step1, step2, step3, step4, step5

**"type" 合并深度类型**

Value     | Description
--------- | ---------
step0     | 不合并深度
step1     | Aggregation level = precision*10
step2     | Aggregation level = precision*100
step3     | Aggregation level = precision*1000
step4     | Aggregation level = precision*10000
step5     | Aggregation level = precision*100000

当type值为‘step0’时，默认深度为150档;
当type值为‘step1’,‘step2’,‘step3’,‘step4’,‘step5’时，默认深度为20档。

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.depth.step0",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.htusdt.depth.step0",
  "ts": 1572362902027,
  "tick": {
    "bids": [
      [3.7721, 344.86],// [price, quote volume]
      [3.7709, 46.66] 
    ],
    "asks": [
      [3.7745, 15.44],
      [3.7746, 70.52]
    ],
    "version": 100434317651,
    "ts": 1572362902012
  }
}
```

### 数据更新字段列表

<aside class="notice">在'tick'object下方呈现买盘卖盘深度列表</aside>

字段     | 数据类型 | 描述
--------- | --------- | -----------
bids      | object    | 当前的所有买单 [price, quote volume]
asks      | object    | 当前的所有卖单 [price, quote volume]
version   | integer   | 内部字段
ts        | integer   | 北京时间的时间戳，单位毫秒

<aside class="notice">当symbol被设为"hb10"时，amount, count, vol均为零值 </aside>

### 数据请求

支持数据请求方式一次性获取市场深度数据：

```json
{
  "req": "market.btcusdt.depth.step0",
  "id": "id10"
}
```

## 买一卖一逐笔行情

当买一价、买一量、卖一价、卖一量，其中任一数据发生变化时，此主题推送逐笔更新。

### 主题订阅

`market.$symbol.bbo`

> Subscribe request

```json
{
  "sub": "market.btcusdt.bbo",
  "id": "id1"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                       | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | 交易代码                   |<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.bbo",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.btcusdt.bbo",
  "ts": 1489474082831,
  "tick": {
    "symbol": "btcusdt",
    "quoteTime": "1489474082811",
    "bid": "10008.31",
    "bidSize": "0.01",
    "ask": "10009.54",
    "askSize": "0.3"
  }
}
```

### 数据更新字段列表

字段     | 数据类型 | 描述
--------- | --------- | -----------
symbol     | string    | 交易代码
quoteTime      | long    | 盘口更新时间
bid      | float    | 买一价
bidSize      | float    | 买一量
ask      | float    | 卖一价
askSize      | float    | 卖一量


## 成交明细

### 主题订阅

此主题提供市场最新成交逐笔明细。

`market.$symbol.trade.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.trade.detail",
  "id": "id1"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                       | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | 交易代码                     | <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.trade.detail",
  "ts": 1489474081631
}
```

> Update example

```json
{
  "ch": "market.btcusdt.trade.detail",
  "ts": 1489474082831,
  "tick": {
        "id": 14650745135,
        "ts": 1533265950234,
        "data": [
            {
                "amount": 0.0099,
                "ts": 1533265950234,
                "id": 146507451359183894799,
                "tradeId": 102043494568,
                "price": 401.74,
                "direction": "buy"
            }
            // more Trade Detail data here
        ]
  }
}
```

### 数据更新字段列表

字段      | 数据类型 | 描述
--------- | --------- | -----------
id        | integer   | 唯一成交ID（将被废弃）
tradeId|integer|唯一成交ID（NEW）
amount    | float     | 成交量
price     | float     | 成交价
ts        | integer   | 成交时间 (UNIX epoch time in millisecond)
direction | string    | 成交主动方 (taker的订单方向) : 'buy' or 'sell'

### 数据请求

支持数据请求方式一次性获取成交明细数据（仅能获取最多最近300个成交记录）：

```json
{
  "req": "market.btcusdt.trade.detail",
  "id": "id11"
}
```

## 市场概要

### 主题订阅

此主题提供24小时内最新市场概要快照。快照频率不超过每秒10次。

`market.$symbol.detail`

> Subscribe request

```json
{
  "sub": "market.btcusdt.detail",
  "id": "id1"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                       | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | 交易代码                     |<a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>

> Response

```json
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.detail",
  "ts": 1489474081631
}
```

> Update example

```json
  "tick": {
    "amount": 12224.2922,
    "open":   9790.52,
    "close":  10195.00,
    "high":   10300.00,
    "ts":     1494496390000,
    "id":     1494496390,
    "count":  15195,
    "low":    9657.00,
    "vol":    121906001.754751
  }
```

### 数据更新字段列表

字段     | 数据类型 | 描述
--------- | --------- | -----------
id        | integer   | unix时间，同时作为消息ID
ts        | integer   | unix系统时间
amount    | float     | 24小时成交量
count     | integer   | 24小时成交笔数
open      | float     | 24小时开盘价
close     | float     | 最新价
low       | float     | 24小时最低价
high      | float     | 24小时最高价
vol       | float     | 24小时成交额

### 数据请求

支持数据请求方式一次性获取市场概要数据：

```json
{
  "req": "market.btcusdt.detail",
  "id": "id11"
}
```

# Websocket资产及订单

## 简介

### 接入URL

**Websocket资产及订单**

**`wss://api.huobi.pro/ws/v1`**  

**`wss://api-aws.huobi.pro/ws/v1`**   

注：api-aws.huobi.pro域名对使用aws云服务的用户做了一定的链路延迟优化。  

请使用中国大陆以外的服务器访问火币 API。

### 数据压缩

WebSocket API 返回的所有数据都进行了 GZIP 压缩，需要 client 在收到数据之后解压。

### 心跳消息

 **2019/07/08之后**

当用户的Websocket客户端连接到火币Websocket服务器后，服务器会定期（当前设为20秒）向其发送`ping`消息并包含一整数值如下：

> {
    "op":"ping",
    "ts":1492420473027
} (资产订单推送)

当用户的Websocket客户端接收到此心跳消息后，应返回`pong`消息并包含同一整数值：

> {
    "op":"pong",
    "ts":1492420473027
} （资产订单推送）

<aside class="warning">当Websocket服务器连续三次发送了`ping`消息却没有收到任何一次`pong`消息返回后，服务器将主动断开与此客户端的连接。</aside>

**2019/07/08之前**

当用户的Websocket客户端连接到火币Websocket服务器后，服务器会定期（设为30秒）向其发送`ping`消息并包含一整数值如下：

> {
    "op":"ping",
    "ts":1492420473027
}

当用户的Websocket客户端接收到此心跳消息后，应返回`pong`消息并包含同一整数值：

> {
    "op":"pong",
    "ts":1492420473027
}

<aside class="warning">当Websocket服务器连续两次发送了`ping`消息却没有收到任何一次`pong`消息返回后，服务器将主动断开与此客户端的连接。</aside>

### 订阅主题

成功建立与Websocket服务器的连接后，Websocket客户端发送如下请求以订阅特定主题：

```json
{
  "op": "operation type, 'sub' for subscription, 'unsub' for unsubscription",
  "topic": "topic to sub",
  "cid": "id generate by client"
}
```

成功订阅后，Websocket客户端将收到确认：

```json
{
  "op": "operation type, refer to the operation which triggers this response",
  "cid": "id1",
  "error-code": 0, // 0 for no error
  "topic": "topic to sub if the op is sub",
  "ts": 1489474081631
}
```

之后, 一旦所订阅的主题有更新，Websocket客户端将收到服务器推送的更新消息（push）：

```json
{
  "op": "notify",
  "topic": "topic of this notify",
  "ts": 1489474082831,
  "data": {
    // data of specific topic update
  }
}
```

### 取消订阅

取消订阅的格式如下：

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "client generated id"
}
```

取消订阅成功确认：

```json
{
  "op": "unsub",
  "topic": "accounts",
  "cid": "id generated by client",
  "err-code": 0,
  "ts": 1489474081631
}
```

### 请求数据

Websocket服务器同时支持一次性请求数据（pull）。

当与Websocket服务器成功建立连接后，以下三个主题可供用户请求：

* accounts.list
* orders.list
* orders.detail

具体请求方式请见后文。

### 限频

**数据请求（sub）限频规则**

限频规则基于API key而不是连接。当sub数量超出限值时，Websocket客户端将收到"too many request"错误码。具体规则如下：

单个连接每秒最多50次sub和50次unsub。
单个连接sub总量限制100个，sub总量达到限额后不允许再sub，但每次unsub可以减少sub总量的计数。比如：30个sub后unsub 1个，此时sub总量count为29，还有71个sub总量可用。 

**数据请求（req）限频规则**

限频规则基于API key而不是连接。当请求频率超出限值时，Websocket客户端将收到"too many request"错误码。以下为各主题当前限频设定：

* accounts.list: once every 2 seconds
* orders.list AND orders.detail: once every 5 seconds

### 鉴权

资产及订单主题鉴权请求数据格式如下：

```json
{
  "op": "auth",
  "AccessKeyId": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx",
  "SignatureMethod": "HmacSHA256",
  "SignatureVersion": "2",
  "Timestamp": "2017-05-11T15:19:30",
  "Signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=",
}
```

**鉴权请求数据格式说明**

  filed              |type   |  instruction|
  ------------------ |----   |  -----------------------------------------------------
  op                 |string | 必填；操作名称，鉴权固定值为 auth；
  cid                |string | 选填；Client 请求唯一 ID
  AccessKeyId        |string | 必填；API 访问密钥, 您申请的 APIKEY 中的 AccessKey
  SignatureMethod    |string | 必填；签名方法, 用户计算签名的基于哈希的协议，此处使用 HmacSHA256
  SignatureVersion   |string | 必填；签名协议的版本，此处使用 2
  Timestamp          |string | 必填；时间戳, 您发出请求的时间 (UTC 时区) (UTC 时区) (UTC 时区) 。在查询请求中包含此值有助于防止第三方截取您的请求。如：2017-05-11T16:22:06。再次强调是 (UTC 时区)
  Signature          |string |必填；签名, 计算得出的值，用于确保签名有效和未被篡改

> **注：**
> - 参考[https://huobiapi.github.io/docs/spot/v1/cn/#c64cd15fdc] 生成有效签名
> - 签名计算中请求方法固定值为`GET`

## 订阅账户更新

API Key 权限：读取

订阅账户资产变动更新。

### 主题订阅

`accounts`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "accounts",
  "model": "0"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                       | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
model     | string    | false    | 0                     | 是否包含已冻结余额                 | 1 to include frozen balance, 0 to not

<aside class="notice">如果同时订阅可用和总余额，需要为 0 和 1 各开启一条websocket连接</aside>

> Response

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "accounts"
}
```

> Update example

```json
{
  "op": "notify",
  "ts": 1522856623232,
  "topic": "accounts",
  "data": {
    "event": "order.place",
    "list": [
      {
        "account-id": 419013,
        "currency": "usdt",
        "type": "trade",
        "balance": "500009195917.4362872650"
      }
    ]
  }
}

```

### 数据更新字段列表

字段     | 数据类型 | 描述
--------- | --------- | -----------
event     | string    | 资产变化通知相关事件说明，比如订单创建(order.place) 、订单成交(order.match)、订单成交退款（order.refund)、订单撤销(order.cancel) 、点卡抵扣交易手续费（order.fee-refund)、杠杆账户划转（margin.transfer)、借贷本金（margin.loan)、借贷计息（margin.interest)、归还借贷本金利息(margin.repay)、其他资产变化(other)
account-id| integer   | 账户 id
currency  | string    | 币种
type      | string    | 账户类型, 交易子账户（trade),借贷子账户（loan），利息子账户（interest)
balance   | string    | 账户余额 (当订阅model=0时，该余额为可用余额；当订阅model=1时，该余额为总余额）

## 订阅订单更新

API Key 权限：读取

订阅账户下的订单更新。

### 主题订阅

`orders.$symbol`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.htusdt",
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                    | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | 交易代码                       | <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>, 支持通配符"*".

> Response

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "orders.htusdt"
}
```

> Update example

```json
{
  "op": "notify",
  "topic": "orders.htusdt",
  "ts": 1522856623232,
  "data": {
    "seq-id": 94984,
    "order-id": 2039498445,
    "symbol": "htusdt",
    "account-id": 100077,
    "order-amount": "5000.000000000000000000",
    "order-price": "1.662100000000000000",
    "created-at": 1522858623622,
    "order-type": "buy-limit",
    "order-source": "api",
    "order-state": "filled",
    "role": "taker|maker",
    "price": "1.662100000000000000",
    "filled-amount": "5000.000000000000000000",
    "unfilled-amount": "0.000000000000000000",
    "filled-cash-amount": "8301.357280000000000000",
    "filled-fees": "8.000000000000000000"
  }
}
```

### 数据更新字段列表

字段               | 数据类型 | 描述
---------           | --------- | -----------
seq-id              | integer   | 流水号(不连续)
order-id            | integer   | 订单 id
symbol              | string    | 交易对
account-id          | string    | 账户 id
order-amount        | string    | 订单数量
order-price         | string    | 订单价格
created-at          | int       | 订单创建时间 (UNIX epoch time in millisecond)
order-type          | string    | 订单类型, 有效取值: buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker,buy-stop-limit,sell-stop-limit
order-source        | string    | 订单来源, 有效取值: sys, web, api, app
order-state         | string    | 订单状态, 有效取值: submitted, partial-filled, filled, canceled, partial-canceled
role                | string    | 成交角色: taker or maker
price               | string    | 成交价格
filled-amount       | string    | 单次成交数量
unfilled-amount  | string    | 单次未成交数量
filled-cash-amount         | string    | 单次成交金额
filled-fees     | string    | 单次成交手续费（买入为币，卖出为钱）

## 订阅订单更新 (NEW)

API Key 权限：读取

相比现有用户订单更新推送主题“orders.$symbol”， 新增主题“orders.$symbol.update”拥有更低的数据延迟以及更准确的消息顺序。建议API用户订阅此新主题接收订单更新推送，以替代现有订阅主题 “orders.$symbol”。（现有订阅主题 “orders.$symbol”仍将在Websocket API服务中被保留直至另行通知。）

### 主题订阅

`orders.$symbol.update`

> Subscribe request

```json
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.htusdt.update"
}
```

### 参数

参数 | 数据类型 | 是否必需 | 缺省值         | 描述                                       | 取值范围
--------- | --------- | -------- | -------------         | -----------                                       | -----------
symbol    | string    | true     | NA                    | 交易代码                       | <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>, 支持通配符"*".



> Response

```json
{
  "op": "sub",
  "ts": 1489474081631,
  "topic": "orders.htusdt.update",
  "err-code": 0,
  "cid": "40sG903yz80oDFWr"  
}
```

> Update example

```json
{
  "op": "notify",
  "ts": 1522856623232,
  "topic": "orders.htusdt.update",  
  "data": {
    "unfilled-amount": "0.000000000000000000",
    "filled-amount": "5000.000000000000000000",
    "price": "1.662100000000000000",
    "order-id": 2039498445,
    "symbol": "htusdt",
    "match-id": 94984,
    "filled-cash-amount": "8301.357280000000000000",
    "role": "taker|maker",
    "order-state": "filled",
    "client-order-id": "a0001",
    "order-type": "buy-limit"
  }
}
```

### 数据更新字段列表

Field               | Data Type | Description
---------           | --------- | -----------
match-id              | integer   | 最近撮合编号（当order-state = submitted, canceled, partial-canceled时，match-id 为消息序列号；当order-state = filled, partial-filled 时，match-id 为最近撮合编号。）
order-id            | integer   | 订单编号
symbol              | string    | 交易代码
order-state         | string    | 订单状态, 有效取值: submitted, partial-filled, filled, canceled, partial-canceled
role                | string    | 最近成交角色（当order-state = submitted, canceled, partial-canceled时，role 为缺省值taker；当order-state = filled, partial-filled 时，role 取值为taker 或maker。）
price               | string    | 最新价（当order-state = submitted 时，price 为订单价格；当order-state = canceled, partial-canceled 时，price 为零；当order-state = filled, partial-filled 时，price 为最近成交价。）
filled-amount       | string    | 最近成交数量
filled-cash-amount  | string    | 最近成交数额
unfilled-amount     | string    | 最近未成交数量（当order-state = submitted 时，unfilled-amount 为原始订单量；当order-state = canceled OR partial-canceled 时，unfilled-amount 为未成交数量；当order-state = filled 时，如果 order-type = buy-market，unfilled-amount 可能为一极小值；如果order-type <> buy-market 时，unfilled-amount 为零；当order-state = partial-filled AND role = taker 时，unfilled-amount 为未成交数量；当order-state = partial-filled AND role = maker 时，unfilled-amount 为未成交数量。）
client-order-id|string|用户自编订单号
order-type|string|订单类型，包括buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc, buy-limit-maker, sell-limit-maker,buy-stop-limit,sell-stop-limit


## 请求用户资产数据

API Key 权限：读取

查询当前用户的所有账户余额数据。

### 数据请求

`accounts.list`

> Query request

```json
{
  "op": "req",
  "cid": "40sG903yz80oDFWr",
  "topic": "accounts.list",
}
```
成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来查询账户数据：

参数  | 数据类型   |  描述|
----------| --------| -------------------------------------------------------|
op         |string  | 必填；操作名称，固定值为 req|
cid        |string  | 选填；Client 请求唯一 ID|
topic      |string   |必填；固定值为accounts.list|

### 返回

> Successful

```json
    {
      "op": "req",
      "topic": "accounts.list",
      "cid": "40sG903yz80oDFWr",
      "err-code": 0,
      "ts": 1489474082831,
      "data": [
        {
          "id": 419013,
          "type": "spot",
          "state": "working",
          "list": [
            {
              "currency": "usdt",
              "type": "trade",
              "balance": "500009195917.4362872650"
            },
            {
              "currency": "usdt",
              "type": "frozen",
              "balance": "9786.6783000000"
            }
          ]
        },
        {
          "id": 35535,
          "type": "point",
          "state": "working",
          "list": [
            {
              "currency": "eth",
              "type": "trade",
              "balance": "499999894616.1302471000"
            },
            {
              "currency": "eth",
              "type": "frozen",
              "balance": "9786.6783000000"
            }
          ]
        }
      ]
    }
```

> Failed

```json
    {
      "op": "req",
      "topic": "foo.bar",
      "cid": "40sG903yz80oDFWr",
      "err-code": 12001, //Response codes,0  represent success;others value  is error,the list of Response codes is in appendix
      "err-msg": "detail of error message",
      "ts": 1489474081631
    }
```

字段                |数据类型 |    描述|
-------------------- |--------| ------------------------------------|
{ id                   |long    | 账户ID|
type              |string   |账户类型|
state           |string     |账户状态|
list               |string   |账户列表|
{currency                |string   |子账户币种|
type           |string     |子账户类型|
balance }}           |string     |子账户余额|

## 请求当前及历史订单

API Key 权限：读取

根据设定条件查询当前委托、历史委托。

### 数据请求

`order.list`

> Query request

```json
{
  "op": "req",
  "topic": "orders.list",
  "cid": "40sG903yz80oDFWr",
  "symbol": "htusdt",
  "states": "submitted,partial-filled"
}
```

### 参数

参数  | 数据类型 | 是否必需 | 缺省值 | 描述                                   | 取值范围
---------  | --------- | -------- | ------- | -----------                                   | ----------
account-id | int       | true     | NA      | 账户 id                        | NA
symbol     | string    | true     | NA      | 交易对                | <a href="https://huobiapi.github.io/docs/spot/v1/cn/#0e505d18dc">取值参考</a>
types      | string    | false    | NA      | 查询的订单类型组合，使用','分割   | buy-market, sell-market, buy-limit, sell-limit, buy-ioc, sell-ioc， buy-limit-maker, sell-limit-maker, buy-stop-limit，sell-stop-limit
states     | string    | true    | NA      | 查询的订单状态组合，使用','分割  | submitted, partial-filled, partial-canceled, filled, canceled，created
start-date | string    | false    | -61d    | 查询开始日期, 日期格式yyyy-mm-dd      | NA
end-date   | string    | false    | today   | 查询结束日期, 日期格式yyyy-mm-dd        | NA
from       | string    | false    | NA      | 查询起始 ID                 | NA
direct     | string    | false    | next    | 查询方向          | next, prev
size       | string       | false    | 100     | 查询记录大小               | [1, 100]

### 数据更新字段列表
  
> Successful

```json
{
  "op": "req",
  "topic": "orders.list",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1522856623232,
  "data": [
    {
      "id": 2039498445,
      "symbol": "htusdt",
      "account-id": 100077,
      "amount": "5000.000000000000000000",
      "price": "1.662100000000000000",
      "created-at": 1522858623622,
      "type": "buy-limit",
      "filled-amount": "5000.000000000000000000",
      "filled-cash-amount": "8301.357280000000000000",
      "filled-fees": "8.000000000000000000",
      "finished-at": 1522858624796,
      "source": "api",
      "state": "filled",
      "canceled-at": 0
    }
  ]
}
```

字段                 |数据类型 |    描述|
-------------------- |--------| ------------------------------------|
id                   |long    | 订单ID|
symbol               |string   |交易对|
account-id           |long     |账户ID|
amount               |string   |订单数量|
price                |string   |订单价格|
created-at           |long     |订单创建时间|
type                 |string   |订单类型，请参考订单类型说明|
filled-amount        |string   |已成交数量|
filled-cash-amount   |string   |已成交总金额|
filled-fees          |string   |已成交手续费|
finished-at          |string   |最后成交时间|
source               |string   |订单来源，请参考订单来源说明|
state                |string   |订单状态，请参考订单状态说明|
cancel-at            |long     |撤单时间|
stop-price              | string  | 止盈止损订单触发价格   | 
operator              | string  |  止盈止损订单触发价运算符   |


## 以订单编号请求订单

API Key 权限：读取

以订单编号请求订单数据

### 数据请求

`orders.detail`

> Query request

```json
{
  "op": "req",
  "topic": "orders.detail",
  "order-id": "2039498445",
  "cid": "40sG903yz80oDFWr"
}
```

### 参数

参数  | 是否必需 |  数据类型    | 描述       |                                      缺省值  | 取值范围|
----------| ----------| --------| ------------------------------------------------ |--------| ----------|
op         |true       |string   |操作名称，固定值为 req    |||                                
cid        |true       |string   |Client 请求唯一 ID        |||                                
topic      |false      |string   |固定值为 orders.detail  |||          
order-id   |true       |string   |订单ID    |||                                                


### 返回
  
> Successful

```json
{
  "op": "req",
  "topic": "orders.detail",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1522856623232,
  "data": {
    "id": 2039498445,
    "symbol": "htusdt",
    "account-id": 100077,
    "amount": "5000.000000000000000000",
    "price": "1.662100000000000000",
    "created-at": 1522858623622,
    "type": "buy-limit",
    "filled-amount": "5000.000000000000000000",
    "filled-cash-amount": "8301.357280000000000000",
    "filled-fees": "8.000000000000000000",
    "finished-at": 1522858624796,
    "source": "api",
    "state": "filled",
    "canceled-at": 0
  }
}
```
字段                 |数据类型 |    描述|
-------------------- |--------| ------------------------------------|
id                   |long    | 订单ID|
symbol               |string   |交易对|
account-id           |long     |账户ID|
amount               |string   |订单数量|
price                |string   |订单价格|
created-at           |long     |订单创建时间|
type                 |string   |订单类型，请参考订单类型说明|
filled-amount        |string   |已成交数量|
filled-cash-amount   |string   |已成交总金额|
filled-fees          |string   |已成交手续费|
finished-at          |string   |最后成交时间|
source               |string   |订单来源，请参考订单来源说明|
state                |string   |订单状态，请参考订单状态说明|
cancel-at            |long     |撤单时间|
stop-price              | string  | 止盈止损订单触发价格   | 
operator              | string  |  止盈止损订单触发价运算符   |

# 稳定币兑换

## 稳定币兑换价格查询

GET v1/stable-coin/quote
API Key 权限：读取

### 请求参数

| 参数名称       | 是否必须 | 类型     | 描述     |取值范围 |
| ---------- | ---- | ------ | ------ | ---- |
| currency | true | string | 与HUSD兑换的稳定币币种   |  USDT/PAX/USDC/TUSD |
| amount     | true | string | 与HUSD兑换的稳定币币种数量   |amount必须为整数      |
| type     | true | string | 兑换方向  |buy兑入/sell兑出     |

### 响应数据

| 参数名称 | 是否必须  | 数据类型 | 描述   | 取值范围 |
| ---- | ----- | ---- | ---- | ---- |
| currency | true | string | 与HUSD兑换的稳定币币种   |  USDT/PAX/USDC/TUSD |
| amount     | true | string | 与HUSD兑换的稳定币币种数量   |因兑换账户额度等因素影响，返回的amount可能会比请求的amount小      |
| type     | true | string | 兑换方向  |buy兑入/sell兑出     |
| exchange-amount     | true | string | 匹配的HUSD数量  |type=buy时，exchange-amount为用户所需支付的husd数量；type=sell时，exchange-amount为用户可获得的husd数量     |
| quote-id     | true | string | 该次稳定币报价唯一ID  |     |
| expiration     | true | string | 确认兑换有效期  |时间（一般为接口请求时间向后延伸10秒）     |

### 错误码

| 响应码 | 说明  | 
| ---- | ----- | 
| invalid-currency | 币种无效 | 
| invalid-amount | 币种数量小于最低值（10万）或大于当前可兑换额度 |
| invalid-type | type不为sell或buy | 
| quote-failure | 后端其他错误引起的后端其他错误引起的价格查询失败 | 

# Stable Coin Exchange

## Get Exchange Rate

API Key Permission：Read

### HTTP Request

`GET https://api.huobi.pro/v1/stable-coin/quote`

### Request Parameters

Parameter  | Data Type | Required | Default | Description
---------  | --------- | -------- | ------- | -----------
currency    | string    | true     | NA      | Stable coin name (USDT/PAX/USDC/TUSD)
amount     | string    | true     | NA      | Amount of stable coin to exchange (the value must be an intger.)
type        | string    | true    | NA      | Type of the exchange (buy/sell)

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
currency    | string    | Stable coin name (USDT/PAX/USDC/TUSD)
amount     | string    | Amount of stable coin to exchange (Due to factors such as the amount of the exchange account, the amount returned may be smaller than the amount requested.)
type        | string   | Type of the exchange (buy/sell)
exchangeAmount       | string   | Amount of HUSD to exchange in or out
quoteId       | string   | Stable currency quoteID
expiration|string|Term of validity

### Error Code

Error Code               | Description
---------           | --------- 
invalid-currency    | invalid currency    
invalid-amount     | amount<100，000 or amount>the max     
invalid-type        | type not 'buy' or 'sell'
quote-failure|other errors

## Exchange Stable Coin

API Key Permission：Trade

### HTTP Request

`POST https://api.huobi.pro/v1/stable-coin/exchange`

### Request Parameters

Parameter  | Data Type | Required | Default | Description
---------  | --------- | -------- | ------- | -----------
quote-id    | string    | true     | NA      | stable currency quoteID

### Response Content

Field               | Data Type | Description
---------           | --------- | -----------
transact-id    | long    | Exchange record id
currency    | string    | Stable coin name (USDT/PAX/USDC/TUSD)
amount     | string   |  Amount of stable coin to exchange
type        | string   | Type of the exchange (buy/sell)
exchange-amount       | string   | Amount of HUSD to exchange in or out
time       | long   | Timestampe

### Error Code

Error Code               | Description
---------           | --------- 
invalid-quote-id    | Paramemter ‘quote-id’ is invalid    
insufficient-balance     | insufficient balance to buy or sell stable coins     
insufficient-quota        | the quota is exceeded
exchange-failure|other errors
Base-user-request-exceed-limit|Operation is too frequent

# ETF（HB10）

## 基本信息

用户可以通过该接口取得关于 ETF 换入换出的 基本信息，包括一次换入最小量，一次换入最大量，一 次换出最小量，一次换出最大量，换入费率，换出费率，最新 ETF 换入换出状态，以及 ETF 的成分结构。


### HTTP 请求

- GET `/etf/swap/config`

### 请求参数

参数|是否必填|数据类型|长度|说明|取值范围|
-----|-----|-----|------|-------|------|
etf_name| true | string |- | etf基金名称 | hb10|

> Response:

```json
{
  "code": 200,
  "data": {
    "purchase_min_amount": 10000,
    "purchase_max_amount": 100000,
    "redemption_min_amount": 10000,
    "redemption_max_amount": 10000,
    "purchase_fee_rate": 0.001,
    "redemption_fee_rate": 0.002,
    "etf_status":1,
    "unit_price":
    [
      {
        "currency": "eth",
        "amount": 19.9
      },
      {
        "currency": "btc",
        "amount": 9.9
      }
    ]
  },
  "message": null,
  "success": true
}
```

### 响应数据

参数|是否必填 | 数据类型 | 长度 | 说明 | 取值范围 |
-----------|------------|-----------|------------|----------|--|
purchase_min_amount | true| int | - | 最小单次换入数量|      |
purchase_max_amount  | False| int | - | 最大单次换入数量 |      |
redemption_min_amount  | true| int | - | 最小单次换出数量 |      |
redemption_max_amount  | False| int | - | 最大单次换出数量 |      |
purchase_fee_rate  | true| double | (5,4)  | 换入费率 |      |
redemption_fee_rate  | true| double | (5,4) | 换出费率 |      |
etf_status  | true| int | - | 换入换出状态 | 状态： 正常 – 1;  由调仓引起的换入换出暂停 - 2; 其他原因引起的换入换出暂停 - 3; 换入暂停 - 4; 换出暂停 – 5  |
unit_price  | true| Array | - | ETF成分信息，包含成分币代码和对应的数量 | 调仓会引起成分信息发生变化  |

- unit_price

参数|是否必填|数据类型|长度|说明|
-----|-----|-----|------|-------|
currency| true | string |- | 成分币币种 |
amount| true | double |- | 成分币数量 |


## 换入换出

API Key 权限：交易

用户可以通过该接口取得关于 ETF 换入（swap/in）换出（swap/out）的 基本信息，包括一次换入最小量，一次换入最大量，一次换出最小量，一次换出最大量，换入费率，换出费率，最新 ETF 换入换出状态，以及 ETF 的成分结构。


### HTTP 请求

- POST ` /etf/swap/in `

- POST ` /etf/swap/out`

### 请求参数

参数|是否必填 | 数据类型 | 长度 | 说明 | 取值范围 |
-----------|------------|-----------|------------|----------|--|
etf_name  | true| string | - | etf基金名称|    hb10  |
amount  | true| int | - | 换入数量  (POST /etf/swap/in) 或 换出数量 (POST /etf/swap/out) | 换入换出数量的范围请参照接口GET /etf/swap/config 提供的相应范围 |


> Response:

```json
{
    "code": 200,
    "data": null,
    "message": null,
    "success": true
}
```


### 响应数据


参数|是否必填 | 数据类型 | 长度 | 说明 | 取值范围 |
-----------|------------|-----------|------------|----------|--|
code | true| int | - | 结果返回码|   请参照返回码解释表 |
data | true|   | - |  |     |
message | true|   | - |  |     |
success | true| Boolean | - | 请求是否成功|  true or false |

* 返回码解释表

返回码|说明|
--|--|
200|正常|
10404|基金代码不正确或不存在|
13403|账户余额不足|
13404|基金调整中，不能换入换出|
13405|因配置项问题基金不可换入换出|
|13406|非API调用，请求被拒绝
|13410|API签名错误
|13500|系统错误
|13601|调仓期：暂停换入换出
|13603|其他原因，暂停换入和换出
|13604|暂停换入
|13605|暂停换出
|13606|换入或换出的基金份额超过规定范围

## 操作记录

API Key 权限：读取

用户可以通过该接口取得关于 ETF 换入换出操 作的明细记录。最多返回 100 条记录。


### HTTP 请求

- GET `/etf/swap/list `

### 请求参数

参数|是否必填 | 数据类型 | 长度 | 说明 | 取值范围 |
-----------|------------|-----------|------------|----------|--|
etf_name | true| string | - | etf基金名称|   hb10 |
offset | true|  int | - | 开始位置 | >=0. 比如，当offset=0, 开始位置就 是最新的这一条记录。 |
limit | true|  int  | - |最大返回记录条数|  [1, 100]  |

> Response:

```json
{
  "code": 200,
  "data": [
    {
      "id": 112222,
      "gmt_created": 1528855872323,
      "currency": "hb10",
      "amount": 11.5,
      "type": 1,
      "status": 2,
      "detail": 
      {
        "used_ currency_list": 
        [
          {
            "currency": "btc",
            "amount": 0.666
          },
          {
            "currency": "eth",
            "amount": 0.666
          }
        ],
        "rate": 0.002,
        "fee": 100.11,
        "point_card_amount":1.0,
        "obtain_ currency_list": 
        [
          {
            "currency": "hb10",
            "amount": 1000
          }
        ]
      }
    },
    {
      "id": 112223,
      "gmt_created": 1528855872323,
      "currency": "hb10",
      "amount": 11.5,
      "type": 2,
      "status": 1,
      "detail": 
      {
        "used_ currency_list": 
        [
          {
            "currency": "btc",
            "amount": 0.666
          },
          {
            "currency": "eth",
            "amount": 0.666
          }
        ],
        "rate": 0.002,
        "fee": 100.11,
        "point_card_amount":1.0,
        "obtain_ currency_list":
        [
          {
            "currency": "hb10",
            "amount": 1000
          }
        ]
      }
    }
  ],
  "message": null,
  "success": true
}
```

### 响应数据


参数|是否必填 | 数据类型 | 长度 | 说明 | 取值范围 |
---|------- |------   |---- |-----|--------|
id | true| long | - |操作ID |     |
gmt_created | true| long | - |操作时间（毫秒） |     |
currency | true| string | - |基金名称 |     |
amount | true| double | - |基金数量 |     |
type | true| int | - |操作类型 |    换入-1；换出-2 |
status | true| int | - |操作结果状态 |     成功-2|
detail | true| Detail[] | - |详情 |     |

Detail

参数|是否必填|数据类型|长度|说明|
-----|-----|-----|------|-------|
used_ currency_list| ture| Currency[]| -| 换出的资产列表。如果是换入，该参数包括的是用于换入的成分币详情。如果是换出，该参数则是用于换出的基金详情。|
rate|ture| double| -|费率|
fee|ture| double| -|实际收取的手续费|
point_card_amount| ture| double|-|用点卡折扣的手续费|
obtain_ currency_list| ture| Currency[]| -|换回的资产列表。如果是换入，该参数包括的是用 于换出的基金详情。如果是换出，该参数则是用于 换入的成分币详情。 |

Currency

参数|是否必填|数据类型|长度|说明|
-----|-----|-----|------|-------|
currency| true | string |- | 成分币名称或基金名称 |
amount| true | double |- | 数量 |

<br>
<br>
<br>
<br>
<br>
