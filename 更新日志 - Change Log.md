# 更新日志

|  生效时间（北京时间 UTC+8) | 接口 | 新增 / 修改 | 摘要 |
|-----|-----|-----|-----|
|2019.11.08 19:45| "GET /v1/order/orders/{order-id}/matchresult" & "GET /v1/order/matchresults"|新增|新增返回字段trade-id|
|2019.10.18 19:00| GET /v1/account/history   |新增|新增账户流水查询节点|
|2019.10.12 11:00| POST /v1/dw/withdraw/api/create   |优化|设置ERC20为USDT的默认链|
|2019.10.11 10:00| 支持全仓杠杆资金划转、借贷、还贷、查询借贷订单、查询账户余额等相关节点  |新增|新增全仓杠杆相关节点|
|2019.10.09 20:00| “GET /market/trade”，“GET /market/history/trade”，“market.$symbol.trade.detail”|优化|新增返回字段trade id|
|2019.09.25 20:00| GET /v2/account/withdraw/quota  |新增|新增提币额度查询节点|
|2019.09.23 15:00| POST /v1/order/orders/{order-id}/submitcancel & POST /v1/order/orders/batchcancel  |优化|优化错误码返回|
|2019.09.20 10:00| GET /v2/reference/currencies  |新增|新增币链参考信息节点|
|2019.09.19 16:00| websocket订阅主题“/market/trade.$symbol.bbo”  |新增|新增买一卖一逐笔推送|
|2019.09.18 20:00| GET /v1/subuser/aggregate-balance, GET /v1/account/accounts/{sub-uid}, GET /v1/margin/loan-orders, GET /v1/margin/accounts/balance  |新增|支持子用户逐仓杠杆交易|
|2019.09.16 15:00| GET /v2/account/deposit/address  |新增|新增APIv2节点 - 充币地址查询|
|2019.09.11 17:00| GET v1/stable-coin/quote，POST v1/stable-coin/exchange  |新增|新增稳定币兑换节点|
|2019.09.11 16:00| 删除部分代码示例  |删除|删除部分代码示例|
|2019.09.10 10:00| 除节点"POST /v1/order/orders/submitCancelClientOrder" & "GET /v1/order/openOrders"以外，去除了其它节点中订单状态取值submitting以及cancelling|修改|除节点"POST /v1/order/orders/submitCancelClientOrder" & "GET /v1/order/openOrders"以外，去除了其它节点中订单状态取值submitting以及cancelling|
|2019.09.09 11:00| POST /v1/order/orders/submitCancelClientOrder  |修改|修改返回数据描述|
|2019.09.09 10:00| GET /v1/order/orders; GET /v1/order/matchresults  |修改|修改请求字段start-date与end-date的默认值及取值范围的描述|
|2019.09.02 18:00| POST /v1/order/orders/batchCancelOpenOrders|优化|更改请求字段"symbol"的描述
|2019.09.02 16:00| 删除稳定币兑换相关节点。
|2019.08.29 21:00| 下单、查询、订阅接口|新增|支持止盈止损订单类型。
|2019.08.21 18:00| "GET /v1/order/openOrders"|优化|修改请求字段列表。
|2019.08.05 18:00| "orders.$symbol.update"|新增|新增字段"client-order-id"和"order-type"。
|2019.08.02 18:00| "orders.$symbol.update"|优化|修改对字段"unfilled-amount"的描述。
|2019.07.23 21:00| "GET /v1/order/orders/{order-id}/matchresult" & "GET /v1/order/matchresults"|新增|新增手续费抵扣详情字段。
|2019.07.23 20:00| GET /v1/fee/fee-rate/get|新增|新增费率查询接口。
|2019.07.22 12:00| GET /v1/order/orders/{order-id}/matchresults; GET /v1/order/matchresults|新增|新增成交角色"role"字段以标识每笔成交角色是"taker"还是"maker"。
|2019.07.11 20:00| POST /v1/order/orders/place; POST /v1/order/orders/submitCancelClientOrder; GET /v1/order/orders/getClientOrder|优化/新增|下单/撤单/查询可基于client order ID。
|2019.07.08 12:00| Websocket 订单资产推送接口|优化|优化Websocket 订单资产推送接口[心跳和限频](#5ea2e0cde2-3)。
|2019.06.14 16:00| POST /v1/dw/withdraw/api/create|优化|支持快速提币
|2019.06.17 16:00| GET /v1/stable_coin/exchange_rate; POST /v1/stable_coin/exchange |新增|新增接口支持用户随时获取最新的稳定币兑换汇率信息，并对稳定币执行兑入或兑出。
|2019.06.12 16:00| GET /v1/common/symbols|优化|对交易对基础信息接口返回的内容进行优化，该优化向后兼容|
|2019.06.06 18:00| GET /v1/query/deposit-withdraw|优化|对充提记录查询接口的请求参数进行优化，该优化向后兼容|
|2019.06.05 20:00| 所有需要验签的接口|优化|访问验签接口时，API Key需要有适当的权限，现有的API Key都默认有全部权限。权限分为3类：读取，交易和提币。每个接口相应的权限类别均已更新在各接口说明中|
|2019.06.10 00:00| - GET /v1/order/orders;- GET /v1/order/matchresults  |修改|查询窗口调整为48小时，可查询整体时间范围不变|
|2019.05.15 10:00| - POST /v1/futures/transfer |新增|提供币币与合约账户间的资金划转|
|2019.04.29 19:00| - GET /v1/order/history |新增|新增最近48小时内历史订单查询节点。新节点的上线后，现有订单查询节点“GET /v1/order/orders”仍将被保留。然而，新节点“GET /v1/order/history”被赋予更高服务等级。极端情况下，当服务荷载超过系统既定阈值时，节点“GET /v1/order/orders”的服务可能会不可用，而新节点“GET /v1/order/history”仍将继续提供服务。另外，火币正在计划支持另一个新节点专门用于用户48小时外的历史订单查询。此新节点上线的同时，现有节点“GET /v1/order/orders”将被弃用。火币将及时告知用户这一变更，一旦变更时间确定。|
|2019.04.17 10:00| - GET /v1/order/orders |修改|文档优化，增加Start-date限制说明|
| 2019.04.16 10:00 | - GET /v1/order/openOrders | 修改 | 文档错误，参数account-id和symbol都是必填参数 |
| 2019.01.17 07:00 | - Websocket accounts           | 修改 | - 增加订阅参数 model；<br>- 订阅返回的内容中不再推送交易子账户冻结余额的变化。 |
| 2018.07.10 11:00 | - GET `/market/history/kline`  | 修改 | - `size` 取值范围由 [1-1000] 修改为 [1-2000]。|
| 2018.07.06 16:00 | - POST `/v1/order/orders/place`| 修改 | - 添加 `buy-limit-maker`，`sell-limit-maker` 两种下单类型支持；<br>- 新增获取某个帐号下指定交易对或者所有交易对的所有尚未成交订单接口: `/v1/order/openOrders`。
| 2018.07.06 16:00 | - GET `/v1/order/openOrders`<br>- POST `/v1/order/orders/batchCancelOpenOrders` | 新增 | - 新增获取某个帐号下指定交易对或者所有交易对的所有尚未成交订单接口；<br>- 新增批量取消某个帐号下指定的订单列表中所有订单接口。 |
| 2018.07.02 16:00 | - ETF 相关接口 | 新增 | - 本次接口变更主要是支持 HB10 ETF 的换入和换出。 |
| 2018.06.20 16:00 | - GET `/market/tickers` | 新增 | - 新增 Tickers 接口，Tickers 为当前所有交易对行情数据。 |
