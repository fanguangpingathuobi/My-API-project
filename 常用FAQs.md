# 常用FAQs

## API信息通知
关于API新增、更新、下线等信息火币会提前发布公告进行通知，建议您关注和订阅我们的公告，及时获取相关信息。  

公告订阅流程：点击“API公告”右上角“关注”按钮，用账号登录成功后，再次点击“关注”按钮，并选择需要关注的内容类型，按钮变为“正在关注”，即表示订阅成功。若无账号，点击弹窗左下角“注册”按钮，进行注册。  

您可以点击 <a href='https://huobiglobal.zendesk.com/hc/zh-cn/sections/360000070201-API-%E5%85%AC%E5%91%8A'>这里 </a> 订阅公告。

## 接入、验签相关

### Q1：一个用户可以申请多少个Api Key？
A:  每个母账号可创建5组Api Key，每个Api Key可对应设置读取、交易、提币三种权限。  
    每个母账号还可创建200个子账号，每个子账号可创建5组Api Key，每个Api Key可对应设置读取、交易两种权限。  
1) 读取权限：读取权限用于对数据的查询接口，例如：订单查询、成交查询等。
2) 交易权限：交易权限用于下单、撤单、划转类接口。
3) 提币权限：提币权限用于数字货币创建提币定点、取消提币订单操作。

### Q2：为什么经常出现断线、超时的情况？
A：请检查是否属于以下情况：
1) 客户端服务器如在中国大陆境内，连接的稳定性很难保证，建议使用日本AWS云服务器进行访问。
2) 域名建议使用api.huobi.pro或api-aws.huobi.pro，其他不建议使用。

### Q3：为什么WebSocket总是断开连接？
A：常见原因有：
1) 未回复Pong，WebSocket连接需在接收到服务端发送的Ping信息后回复Pong，保证连接的稳定。
2) 网络原因造成客户端发送了Pong消息，但服务端并未接收到。
3) 网络原因造成连接断开。
4) 建议用户做好WebSocket连接断连重连机制，在确保心跳（Ping/Pong）消息正确回复后若连接意外断开，程序能够自动进行重新连接。

### Q4：api.huobi.pro 与 api-aws.huobi.pro有什么区别？
A：api-aws.huobi.pro域名对使用aws云服务的用户做了链路延迟优化，请求时延更低。

### Q5：为什么签名认证总返回失败？
A：请对比使用Secret Key签名前的字符串与以下字符串的区别
```
GET\n
api.huobi.pro\n
/v1/account/accounts\n
AccessKeyId=rfhxxxxx-950000847-boooooo3-432c0&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2019-10-28T07%3A28%3A38
```
对比时请注意一下几点：
1) 签名参数按照ASCII码排序
{F1254350}
2) 签名串需进行URI编码
{F1254364}
3) 签名需进行 base64 编码
4) Get请求参数需在签名串中
5) 时间为UTC时间转换为YYYY-MM-DDTHH:mm:ss
6) 检查本机时间与标准时间是否存在偏差（偏差应小于1分钟）
7) WebSocket发送验签认证消息时，消息体不需要URI编码
8) 签名时所带Host应与请求接口时Host相同
9) Api Key 与 Secret Key中是否存在隐藏特殊字符，影响签名

- 当前官方已支持Java、Python3、C++三种语言的SDK， 可根据语言选择使用或进行参考。  
<a href='https://github.com/HuobiRDCenter'>SDK下载地址 </a>   
<a href='http://phabricator.huobidev.com/w/ams/dailylog/duqingxiang/api_qaqa/python_signature_demo/'>Python签名&demo </a>   
<a href='http://phabricator.huobidev.com/w/ams/dailylog/duqingxiang/api_qaqa/java_signature_demo/'>JAVA签名&demo </a>  
<a href='http://phabricator.huobidev.com/w/ams/dailylog/duqingxiang/api_qaqa/cpp_signature_demo/'>CPP签名&demo </a>  

- 也可参考签名指导文档：https://huobiapi.github.io/docs/spot/v1/cn/#c64cd15fdc

### Q6：调用接口返回gateway-internal-error错误是什么原因？
A：请检查是否属于以下情况：
1) account-id 是否正确，是否由 GET /v1/account/accounts 接口返回的数据。
2) 可能为网络原因，请稍后进行重试。
3) 发送数据格式是否正确(需要标准JSON格式)。
4) POST请求头header是否声明为`Content-Type:application/json` 。

### Q7：调用接口返回 login-required错误是什么原因？
A：请检查是否属于以下情况：
1) 是否将AccessKeyId参数带入URL中。
2) account-id 是否正确，是由 GET /v1/account/accounts 接口返回的数据。
3) POST 请求是否把业务参数也计算进签名。
4) GET 请求是否将参数按照 ASCII 码表顺序排序。

## 行情相关
### Q1：当前盘口数据多久更新一次？
A：当前盘口数据数据一秒更新一次，无论是RESTful查询或是Websocket推送都是一秒一次。

### Q2：24小时行情接口(/market/detail)数据接口的成交量会出现负增长吗？
A：/market/detail接口中的成交量、成交金额为24小时滚动数据（平移窗口大小24小时），有可能会出现后一个窗口内的累计成交量、累计成交额小于前一窗口的情况。

### Q3：如何获取最新成交价格？
A：推荐使用/market/trade接口请求最新成交，或使用WebSocket订阅market.$symbol.trade.detail主题，根据返回数据中的price获取。

### Q4：K线是按照什么时间开始计算的？
A： K线周期以新加坡时间为基准开始计算，例如日K线的起始周期为新加坡时间0时-新加坡时间次日0时。

## 交易相关
### Q1：account-id是什么？
A： account-id对应用户不同业务账户的ID，可通过/v1/account/accounts接口获取，并根据account-type区分具体账户。
账户类型包括：
1. spot 现货账户
2. otc OTC账户
3. margin 逐仓杠杆账户，该账户类型以subType区分具体币种对账户
4. super-margin（或cross-margin） 全仓杠杆账户
5. point 点卡账户
6. minepool 矿池账户
7. etf ETF账户

### Q2：client-order-id是什么？
A： client-order-id作为下单请求标识的一个参数，类型为字符串，长度为64。 此id为用户自己生成，24小时内有效。

### Q3：如何获取下单数量、金额、小数限制、精度的信息？
A： 可使用/v1/common/symbols获取相关币对信息， 下单时注意最小下单数量和最小下单金额的区别。   
常见返回错误如下：  
order-value-min-error: 下单金额小于最小交易额  
order-orderprice-precision-error : 限价单价格精度错误  
order-orderamount-precision-error : 下单数量精度错误  
order-limitorder-price-max-error : 限价单价格高于限价阈值  
order-limitorder-price-min-error : 限价单价格低于限价阈值  
order-limitorder-amount-max-error : 限价单数量高于限价阈值  
order-limitorder-amount-min-error : 限价单数量低于限价阈值  

### Q4：WebSocket 订单更新推送主题orders.$symbol 和 orders.$symbo.update的区别？
A： 区别如下：
1. order.$symbol 主题作为老的推送主题，会在一段时间后停止主题的维护和使用， 推荐使用order.$symbol.update主题，
2. 新主题orders.$symbol.update具有严格的时序性，保证数据严格按照撮合成交顺序进行推送，且具有更快的时效性以及更低的时延。
3. 为减少重复数据推送量以及更快的速度，在orders.$symbol.update推送中并未携带原始订单数量，价格信息，若需要此信息，建议可在下单时在本地维护订单信息，或在接收到推送消息后，使用rest接口进行查询。

### Q5： 为什么收到订单推送的消息后进行下单，返回余额不足？
A：为保证订单的及时送达以及低延时， 订单推送的结果是在撮合后直接推送，此时订单可能并未完成资产的清算，建议使用以下方式保证资金可以正确下单：
1. 结合资产推送主题``accounts``同步接收资产变更的消息，确保资金已经完成清算。
2. 收到订单推送消息时，使用rest接口调用账户余额，验证账户资金是否足够。
3. 账户中保留相对充足的资金余额。

### Q6: 撮合结果里的filled-fees和filled-points有什么区别？
A: 撮合成交中的成交手续费分为普通手续费以及抵扣手续费两种类型，两种类型不会同时存在。
1) 普通手续费表示，在成交时，未开启HT抵扣、点卡抵扣，使用原币进行手续费扣除。例如：在BTCUSDT币种对下购买BTC，filled-fees字段不为空，表示扣除了普通手续费，单位是BTC。
2) 抵扣手续费表示，在成交时，开启了HT抵扣或点卡抵扣，使用HT或点卡进行手续费的抵扣。例如BTCUSDT币种对下购买BTC，HT\点卡充足时，filled-fees为空，filled-points不为空，表示扣除了HT或点卡作为手续费，扣除单位需参考fee-deduct-currency字段

### Q7: 撮合结果中match-id和trade-id有什么区别？
A: match-id表示订单在撮合中的顺序号，trade-id表示成交时的序号， 一个match-id可能有多个trade-id（成交时），也可能没有trade-id(创建订单、撤销订单)

## 账户充提相关
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

## API技术支持
若以上内容任未帮助到您，可选择以下任一方式联系我们：  
1. 加入官方QQ群（火币网API交流群(8) 595882031），进群时请注明UID 和编程语言。
2. 发送邮件至api_service@huobi.com    
邮件渠道问题反馈模板  
为了能够更快的了解和调查您反馈的问题，请按照如下模板向我们反馈问题。  

```
1. UID：
2. Api Key（AccessKeyId）：
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
2. Api Key（AccessKeyId）：rfhxxxxx-950000847-boooooo3-432c0
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

