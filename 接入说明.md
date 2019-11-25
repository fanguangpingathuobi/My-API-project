# 接入说明

## 接口概览

接口分类| 分类链接 | 概述
--------- | --------- | -----------
基础类 |/v1/common/* | 基础类接口，包括币种、币种对、时间戳等接口
行情类 |/market/*| 公共行情类接口，包括成交、深度、行情等
账户类 |/v1/account/*  /v1/subuser/* | 账户类接口，包括账户信息，子用户等
订单类 |/v1/order/* | 订单类接口，包括下单、撤单、订单查询、成交查询等
逐仓杠杆类|/v1/margin/* | 逐仓杠杆类接口，包括借贷、还款、查询等
全仓杠杆类接口| /v1/cross-margin/* | 全仓杠杆类接口，包括借贷、还款、查询等

- 该分类为大类整理，部分接口未遵循此规则，请根据需求查看有关接口文档

## 限频规则

- 行情类接口调用根据IP进行限频：10秒100次
- 资产订单类接口调用根据Api Key维度进行限频：10秒100次
- 限频规则按照时间均匀分布，10秒100次 等于 100毫秒1次

<aside class="notice">
单个 API Key 维度限制。行情 API 访问无需签名。
</aside>

## 请求格式

所有的API请求都以GET或者POST形式发出。对于GET请求，所有的参数都在路径参数里；对于POST请求，所有参数则以JSON格式发送在请求主体（body）里。

## 返回格式

所有的接口返回都是JSON格式。在JSON最上层有几个表示请求状态和属性的字段："status", "ch", 和 "ts". 实际的接口返回内容在"data"字段里.

### 返回内容格式

> Responds:

```json
{
  "status": "ok",
  "ch": "market.btcusdt.kline.1day",
  "ts": 1499223904680,
  "data": // per API response data in nested JSON object
}
```

参数名称| 数据类型 | 描述
--------- | --------- | -----------
status    | string    | API接口返回状态
ch        | string    | 接口数据对应的数据流。部分接口没有对应数据流因此不返回此字段
ts        | int       | 接口返回的调整为北京时间的时间戳，单位毫秒
data      | object    | 接口返回数据主体

## 错误信息

### 行情 API 错误信息

| 错误码  |  描述 |
|-----|-----|
| bad-request | 错误请求 |
| invalid-parameter | 参数错误 |
| invalid-command | 指令错误 |
code 的具体解释, 参考对应的 `err-msg`.

### 交易 API 错误信息

| 错误码  |  描述 |
|-----|-----|
| base-symbol-error |  交易对不存在 |
| base-currency-error |  币种不存在 |
| base-date-error | 错误的日期格式 |
| account for id `12,345` and user id `6,543,210` does not exist| `account-id` 错误，请使用GET `/v1/account/accounts` 接口查询 | 
| account-frozen-balance-insufficient-error | 余额不足 |
| account-transfer-balance-insufficient-error | 余额不足无法冻结 |
| bad-argument | 无效参数 |
| api-signature-not-valid | API签名错误 |
| gateway-internal-error | 系统繁忙，请稍后再试|
| ad-ethereum-addresss| 请输入有效的以太坊地址|
| order-accountbalance-error| 账户余额不足|
| order-limitorder-price-error|限价单下单价格超出限制 |
|order-limitorder-amount-error|限价单下单数量超出限制 |
|order-orderprice-precision-error|下单价格超出精度限制 |
|order-orderamount-precision-error|下单数量超过精度限制|
|order-marketorder-amount-error|下单数量超出限制|
|order-queryorder-invalid|查询不到此条订单|
|order-orderstate-error|订单状态错误|
|order-datelimit-error|查询超出时间限制|
|order-update-error|订单更新出错|

## 使用建议

1.行情类数据信息，建议使用WebSocket方式实时接收数据，并对接收数据进行缓存操作

2.获取最新成交价，推荐使用/market/trade接口或使用WebSocket订阅market.$symbol.trade.detail，获取实时成交价格

3.订单成交推送建议使用orders.$symbol.update主题，该主题具有严格的时序性以及更低的时延。

4.下单资产信息使用WebSocket定订阅accounts主题，并结合Rest接口定时请求资产数据进行更新。
