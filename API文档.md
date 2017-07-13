## API文档

* [签名和验签](#%E7%AD%BE%E5%90%8D%E5%92%8C%E9%AA%8C%E7%AD%BE)
    * [请求数据签名：](#%E8%AF%B7%E6%B1%82%E6%95%B0%E6%8D%AE%E7%AD%BE%E5%90%8D)
    * [响应数据验签](#%E5%93%8D%E5%BA%94%E6%95%B0%E6%8D%AE%E9%AA%8C%E7%AD%BE)
    * [注意事项](#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)
* [支付](#%E6%94%AF%E4%BB%98)
    * [发起支付（下单）](#%E5%8F%91%E8%B5%B7%E6%94%AF%E4%BB%98%E4%B8%8B%E5%8D%95)
    * [查询支付](#%E6%9F%A5%E8%AF%A2%E6%94%AF%E4%BB%98)
* [退款](#%E9%80%80%E6%AC%BE)
    * [发起退款](#%E5%8F%91%E8%B5%B7%E9%80%80%E6%AC%BE)
    * [查询退款](#%E6%9F%A5%E8%AF%A2%E9%80%80%E6%AC%BE)
* [下载对账单](#%E4%B8%8B%E8%BD%BD%E5%AF%B9%E8%B4%A6%E5%8D%95)
	* [发起下载](#%E5%8F%91%E8%B5%B7%E4%B8%8B%E8%BD%BD)
	* [返回结果](#%E8%BF%94%E5%9B%9E%E7%BB%93%E6%9E%9C)
* [支付渠道编码](#%E6%94%AF%E4%BB%98%E6%B8%A0%E9%81%93%E7%BC%96%E7%A0%81)
* [支付渠道附加参数](#%E6%94%AF%E4%BB%98%E6%B8%A0%E9%81%93%E9%99%84%E5%8A%A0%E5%8F%82%E6%95%B0)
    * [支付宝移动支付](#%E6%94%AF%E4%BB%98%E5%AE%9D%E7%A7%BB%E5%8A%A8%E6%94%AF%E4%BB%98)
    * [微信移动支付](#%E5%BE%AE%E4%BF%A1%E7%A7%BB%E5%8A%A8%E6%94%AF%E4%BB%98)
    * [Apple Pay](#apple-pay)
    * [微信公众号](#%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7)
    * [支付宝即时到账](#%E6%94%AF%E4%BB%98%E5%AE%9D%E5%8D%B3%E6%97%B6%E5%88%B0%E8%B4%A6)
    * [拉卡拉 PC 网关支付](#%E6%8B%89%E5%8D%A1%E6%8B%89-pc-%E7%BD%91%E5%85%B3%E6%94%AF%E4%BB%98)
    * [拉卡拉 PC 快捷支付](#%E6%8B%89%E5%8D%A1%E6%8B%89-pc-%E5%BF%AB%E6%8D%B7%E6%94%AF%E4%BB%98)
    * [拉卡拉移动SDK支付](#%E6%8B%89%E5%8D%A1%E6%8B%89%E7%A7%BB%E5%8A%A8sdk%E6%94%AF%E4%BB%98)
    * [拉卡拉 H5 支付](#%E6%8B%89%E5%8D%A1%E6%8B%89-h5-%E6%94%AF%E4%BB%98)
* [关于下载对账单的说明](#%E5%85%B3%E4%BA%8E%E4%B8%8B%E8%BD%BD%E5%AF%B9%E8%B4%A6%E5%8D%95%E7%9A%84%E8%AF%B4%E6%98%8E)
* [关于退款的说明](#%E5%85%B3%E4%BA%8E%E9%80%80%E6%AC%BE%E7%9A%84%E8%AF%B4%E6%98%8E)
* [响应错误码](#%E5%93%8D%E5%BA%94%E9%94%99%E8%AF%AF%E7%A0%81)

## 签名和验签

1. https传输协议

2. **采用双向RSA签名**

   商户在发送请求时，需要对使用商户的私钥对请求数据进行签名。在接收到返回值时，需要对返回数据中的签名进行校验。

3. 公钥交换

   * 商户在Paymax网站设置商户公钥，Paymax使用该公钥验签商户请求
   * Paymax公钥可以在Paymax网站进行获取，用于验签Paymax的返回数据



#### 请求数据签名：

参与签名的字段及顺序：

| 顺序   | 参数            | 获取位置        | 描述                             | 示例                                       |
| ---- | ------------- | ----------- | :----------------------------- | ---------------------------------------- |
| 1    | method        | http_method | 请求方法，小写(post/get)              | post                                     |
| 2    | uri           | url         | 统一资源标识符[API方法]                 | /v1/charges                              |
| 3    | query_string  | url         | 查询字符串                          | a=1&b=2&c=3                              |
| 4    | nonce         | header      | 一次性随机数                         | 049e73d6e0744a7491cda2a0a537b90d         |
| 5    | timestamp     | header      | 时间戳                            | 1466399895704                            |
| 6    | Authorization | header      | 商户secret key,Paymax 提供给商户的唯一标识 | 5b97b3138041437587646b37f52dc7f7         |
| 7    | request_data  | body        | 请求数据                           | {"amount":1,"app":"app_49b0f1dd741646d2b277524de2785836","body":"Your Body","description":"description","subject":"Your Subject","channel":"alipay_app","client_ip":"127.0.0.1","order_no":"d0a4877e-39fa-4704-a3e1-ca484a7f1363","metadata":{"metadata_key1":"metadata_value1","metadata_key2":"metadata_value2"},"currency":"CNY"} |



签名步骤：

1. 签名前以 换行符（"\n"） 拼接各个字段：

   ```
   method+"\n"+uri+"\n"+query_string+"\n"+nonce+"\n"+timestamp+"\n"+Authorization+"\n"+request_data
   ```

   **不同的语言，换行符可能有所不同**

   组装成要签名的数据，例如：

   ```
   post
   /v1/charges
   a=1&b=2&c=3
   7650d33c9b6f4e8a8025465061937376
   1466404370089
   5b97b3138041437587646b37f52dc7f7
   {"amount":1,"app":"app_49b0f1dd741646d2b277524de2785836","body":"Your Body","description":"description","subject":"Your Subject","channel":"alipay_app","client_ip":"127.0.0.1","order_no":"a2c37ff4-e253-4d69-803f-2b097a9c995f","metadata":{"metadata_key1":"metadata_value1","metadata_key2":"metadata_value2"},"currency":"CNY"}
   ```


1. 以UTF-8编码将待签名字符串转换成字节数组，然后使用**商户私钥**对其签名，签名算法为SHA1WithRSA，将签名后的结果以Base64编码转码后存放到header中，key为'sign'。

   假如签名结果为：

   ```
   BkBa8OLkU2KzRIrWA4swP5WCSouVwXHFAUM6NlJlgDGbMQYKKvVveE30pXppPcesRLPPlpTctFdCt+nY4czX79efg19FDEizq94d+HAsE/3dtzUMiiFyxOWFH50FGO+gJZDAEpYQRCdJlOs6D380ta+y0wRfUhlfgX1+RXtcDnA=
   ```

   则最后的请求Header是：

   ```http
   POST /v1/charges HTTP/1.1
   Host: https://www.paymax.cc/merchant-api
   ContentType: application/json;charset=utf-8
   Authorization: 5b97b3138041437587646b37f52dc7f7
   nonce: 049e73d6e0744a7491cda2a0a537b90d
   timestamp: 1466399895704
   sign: BkBa8OLkU2KzRIrWA4swP5WCSouVwXHFAUM6NlJlgDGbMQYKKvVveE30pXppPcesRLPPlpTctFdCt+nY4czX79efg19FDEizq94d+HAsE/3dtzUMiiFyxOWFH50FGO+gJZDAEpYQRCdJlOs6D380ta+y0wRfUhlfgX1+RXtcDnA=

   {"amount":1,"app":"app_49b0f1dd741646d2b277524de2785836","body":"Your Body","description":"description","subject":"Your Subject","channel":"alipay_app","client_ip":"127.0.0.1","order_no":"a2c37ff4-e253-4d69-803f-2b097a9c995f","metadata":{"metadata_key1":"metadata_value1","metadata_key2":"metadata_value2"},"currency":"CNY"}
   ```



#### 响应数据验签

参与验签的字段及顺序：

| 顺序   | 参数            | 获取位置   | 描述                             | 示例                               |
| ---- | ------------- | ------ | :----------------------------- | -------------------------------- |
| 1    | nonce         | header | 一次性随机数                         | 691aefb20d874d3f8fb9219331868868 |
| 2    | timestamp     | header | 时间戳                            | 1466399895704                    |
| 3    | Authorization | header | 商户secret key,Paymax 提供给商户的唯一标识 | 5b97b3138041437587646b37f52dc7f7 |
| 4    | response_data | body   | 响应数据(json格式)                   | {"amount":1,"currency":"CNY"}    |



验签步骤：

返回的response示例：

```http

ContentType: application/json;charset=utf-8
Authorization: 5b97b3138041437587646b37f52dc7f7
nonce: 1095f1872473413c8c8ce51979f3ca6d
timestamp: 1466404452749
sign: Lp6TovxVq1r+qgai/B7M7ovV8NDsncZ6j6GfFUlR6QGVPtvpqkliS2kgo/mfm6AgFqpVy+edOGZnjlnohDEjQ7QO4W/AzvMb+/S+UZPcvSyY4zamg8ne0+6cwh7mxu5rQvTknYKSwE99fYtTkla2IvWUfn5ch9fW6MSErRQyzRc=


{"amount":1,"currency":"CNY"}
```



1. 验签前以 '\n' 拼接各个字段：

   ```
   nonce+'\n'+timestamp+'\n'+Authorization+'\n'+response_data
   ```

   组装成要验签的数据，例如：

   ```
   1095f1872473413c8c8ce51979f3ca6d
   1466404452749
   5b97b3138041437587646b37f52dc7f7
   {"amount":1,"currency":"CNY"}
   ```

2. 以UTF-8编码将待验签字符串转换成字节数组，然后使用**Paymax公钥**对其签名，签名算法为SHA1WithRSA。

3. 将第二步的签名结果和Header中取到的sign进行比对，相同即为验签通过，可以说明该响应数据来自于Paymax。



#### 注意事项

1. 对数据进行签名或验签时，字段顺序不能变；
2. 对签名后的值要进行Base64Encode后才能往header中存放；
3. 验签时对获取到的签名数据【sign】 要进行 Base64 Decode后再进行验证；
4. 验签时要对 签名数据、商户secret key、请求的时效性 进行验证。



## 支付

支付订单相关的要素通过Charge对象来承载。


**Charge对象结构：**

| 参数              | 类型      | 描述                                       |
| :-------------- | ------- | ---------------------------------------- |
| id              | String  | 支付订单id，系统内唯一，以“ch_”开头，后跟24位随机数           |
| order_no        | String  | 商户订单号，在商户系统内唯一，1-64位数字或字母，不允许特殊字符     |
| amount          | Decimal | 订单总金额，大于0的数字，单位是该币种的货币单位                 |
| subject         | String  | 购买商品的标题，最长32位                            |
| body            | String  | 购买商品的描述信息，最长128个字符                       |
| channel         | String  | 支付渠道编码，唯一标识一个支付渠道，参考[支付渠道编码](#支付渠道编码)    |
| client_ip       | String  | 发起支付的客户端IP                               |
| description     | String  | 订单备注，限制300个字符内                           |
| extra           | Map     | 特定渠道需要的的额外附加参数，参考[支付渠道附加参数](#支付渠道附加参数)   |
| transaction_no  | String  | 支付渠道订单号                                  |
| metadata        | Map     | 用户自定义元数据                                 |
| app             | String  | 应用的appKey                                |
| amount_settle   | Decimal | 已结算金额                                    |
| amount_refunded | Decimal | 已退款金额                                    |
| currency        | String  | 三位ISO货币代码，只支持人民币cny，默认cny                |
| refunded        | Boolean | 是否已经开始退款                                 |
| refunds         | List    | 退款记录                                     |
| time_created    | Long    | 订单创建时间，13位Unix时间戳                        |
| time_paid       | Long    | 订单支付完成时间，13位Unix时间戳                      |
| time_expire     | Long    | 订单失效时间，13位Unix时间戳                        |
| time_settle     | Long    | 订单结算时间，13位Unix时间戳                        |
| credential      | String  | 支付凭据，用于调起支付APP或者跳转支付网关                   |
| livemode        | Boolean | 是否是生产模式                                  |
| status          | String  | 订单状态，只有三种（PROCESSING-处理中，SUCCEED-成功，FAILED-失败） |
| failure_code    | String  | 订单的错误码，详见[响应错误码](#响应错误码)                 |
| failure_msg     | String  | 订单的错误消息的描述，详见[响应错误码](#响应错误码)             |



### 发起支付（下单）

使用HTTP协议调用支付接口发起请求：


```http
POST /v1/charges HTTP/1.1
Host: https://www.paymax.cc/merchant-api
ContentType: application/json;charset=utf-8
Authorization: {填入Secret Key}
nonce: {填入nonce随机字符串}
timestamp: {填入timestamp}
sign: {填入签名结果}
```

请求参数：

| 参数          | 类型      | 必须    | 描述                                       |
| :---------- | ------- | ----- | ---------------------------------------- |
| order_no    | String  | true  | 商户订单号，在商户系统内唯一，1-64位数字或字母，不允许特殊字符        |
| amount      | Decimal | true  | 订单总金额，大于0的数字，单位是该币种的货币单位                 |
| subject     | String  | true  | 购买商品的标题，最长32位                            |
| body        | String  | true  | 购买商品的描述信息，最长128个字符                       |
| channel     | String  | true  | 支付渠道编码，唯一标识一个支付渠道，参考[支付渠道编码](#支付渠道编码)    |
| app         | String  | true  | 应用的appKey                                |
| client_ip   | String  | true  | 发起支付的客户端IP                               |
| description | String  | false | 订单备注，限制300个字符内                           |
| time_expire | Long    | false | 订单失效时间，13位Unix时间戳，默认1小时，微信公众号支付对其的限制为3分钟 |
| currency    | String  | false | 三位ISO货币代码，只支持人民币cny，默认cny                |
| extra       | Map     | false | 特定渠道需要的的额外附加参数，参考[支付渠道附加参数](#支付渠道附加参数)   |
| metadata    | Map     | false | 用户自定义元数据                                 |



返回：

Charge对象


### 查询支付

```http
GET /v1/charges/{CHARGE_ID} HTTP/1.1
Host: https://www.paymax.cc/merchant-api
ContentType: application/json;charset=utf-8
Authorization: {填入Secret Key}
nonce: {填入nonce随机字符串}
timestamp: {填入timestamp}
sign: {填入签名结果}
```

返回：

Charge对象


## 退款

退款订单相关的要素通过Refund对象来承载。各个渠道对于退款的处理有一定的差异，具体请参考[关于退款的说明](#关于退款的说明)。

**Refund对象结构：**

| 参数             | 类型      | 描述                                       |
| :------------- | ------- | ---------------------------------------- |
| id             | String  | 退款订单id，系统内唯一，以“re_”开头，后跟24位随机数           |
| order_no       | String  | 商户订单号，在商户系统内唯一，1-64位数字或字母，不允许特殊字符       |
| charge         | String  | 退款订单对应的支付订单id                            |
| amount         | Decimal | 退款订单总金额，大于0的数字，单位是该币种的货币单位               |
| description    | String  | 退款备注，限制300个字符内                           |
| extra          | Map     | 特定渠道需要的的额外附加参数以及特定渠道的额外返回值，具体请参考[支付渠道附加参数](#支付渠道附加参数)和参考[关于退款的说明](#关于退款的说明) |
| transaction_no | String  | 支付渠道退款订单号                                |
| metadata       | Map     | 用户自定义元数据                                 |
| time_created   | Long    | 订单创建时间，13位Unix时间戳                        |
| time_succeed   | Long    | 订单退款完成时间，13位Unix时间戳                      |
| status         | String  | 订单状态，只有三种（PROCESSING-处理中，SUCCEED-成功，FAILED-失败） |
| failure_code   | String  | 订单的错误码，详见[响应错误码](#响应错误码)                 |
| failure_msg    | String  | 订单的错误消息的描述，详见[响应错误码](#响应错误码)             |


### 发起退款

使用HTTP协议调用退款接口发起请求：

```http
POST /v1/charges/{CHARGE_ID}/refunds HTTP/1.1
Host: https://www.paymax.cc/merchant-api
ContentType: application/json;charset=utf-8
Authorization: {填入Secret Key}
nonce: {填入nonce随机字符串}
timestamp: {填入timestamp}
sign: {填入签名结果}
```

请求参数：

| 参数          | 类型      | 必须    | 描述           |
| :---------- | ------- | ----- | ------------ |
| amount      | Decimal | false | 退款金额，默认为全额退款 |
| metadata    | Map     | false | 用户自定义元数据     |
| description | String  | true  | 退款原因和描述      |

返回：

Refund对象

### 查询退款

```http
GET /v1/charges/{CHARGE_ID}/refunds/{REFUND_ID} HTTP/1.1
Host: https://www.paymax.cc/merchant-api
ContentType: application/json;charset=utf-8
Authorization: {填入Secret Key}
nonce: {填入nonce随机字符串}
timestamp: {填入timestamp}
sign: {填入签名结果}
```

返回：

Refund对象


# 下载对账单
>应用场景

商户可以通过该接口下载历史交易清单。比如掉单、系统错误等导致商户侧和Paymax侧数据不一致，通过对账单核对后可校正支付状态。

>需要注意

1. Paymax在次日10点启动生成前一天的对账单，建议商户11点后再获取；
2. 对账单接口只能下载前三个月至昨天周期内任意一个自然日的账单；
3. 接口请求的签名同其他Paymax接口一致，**接收返回值不需要验签**。


## 发起下载

使用HTTP协议调用对账单下载接口发起请求：


```http
POST /v1/statement/download HTTP/1.1
Host: https://www.paymax.cc/merchant-api
ContentType: application/json;charset=utf-8
Authorization: {填入Secret Key}
nonce: {填入nonce随机字符串}
timestamp: {填入timestamp}
sign: {填入签名结果}
```

	
>请求参数

|参数|类型|必须|描述|
|--------|------|------|---------|------|
|appointDay|String(8)|是|指定日期（只能是某个自然日，格式：yyyyMMdd）范围为最早前3个月，最晚昨天|
|channelCategory|String|是|Paymax支持下载的支付渠道类别：<br>ALIPAY（支付宝）<br>WECHAT（微信）<br>APPLE（苹果）<br>LAKALA（拉卡拉）
|statementType|String|是|Paymax支持下载的对账单交易类别：<br>ALL（全部）<br>SUCCESS（交易）<br>REFUND（退款）<br>WECHAT_CSB（微信公众号 C2B扫码）|

>各支付渠道适用的交易类别（非适用的传值不会查到数据）

|渠道类别|交易类别|
|--------|------|------|
|ALIPAY| SUCCESS |
| WECHAT |ALL（全部，**不包含WECHAT_CSB**）<br>SUCCESS <br> REFUND <br> WECHAT_CSB |
| APPLE |ALL<br>SUCCESS <br> REFUND |
| LAKALA |SUCCESS <br> REFUND |

>请求示例

```
{
	"appointDay": "20161018",
	"channelCategory": "ALIPAY",
	"statementType": "SUCCESS"
}	
```

## 返回结果
**成功时**，数据以文本表格的方式返回，**第一行为表头，从第二行起，为数据记录，各参数以逗号分隔，字段顺序与表头一致。倒数第二行为对账单汇总标题（目前有且仅有三列：总交易单数，总交易金额，总退款金额），最后一行为汇总统计数据**。

示例（根据请求下载的对账单类型不同而不同）：

```
入账时间,支付宝交易号,支付宝流水号,商户订单号,账务类型,收入（元）,支出（元）,账户余额（元）,对方账户,对方名称,商品名称,备注,用户编号,交易场所
2016-08-10 19:04:17.0,2016081021001004170202560748,101260165191171,ch_f4af353ee4b92a0cc1e0d38a,在线支付,0.01,,1.96,pay@alipay.com,张三,subject,,2088002303691178,支付宝
2016-08-10 19:04:59.0,2016081021001004170202560763,101260169699171,ch_207dcfc773d4c0c0c9aef0d1,在线支付,0.05,,2.01,pay@alipay.com,张三,subject,,2088002303691178,支付宝
2016-08-10 19:25:54.0,2016081021001004170202560763,101260305063170,ch_207dcfc773d4c0c0c9aef0d1,交易退款,,-0.01,2.00,pay@alipay.com,张三,subject,协商一致退款,2088002303691178,支付宝
2016-08-10 19:32:39.0,2016081021001004170201056565,101260347916171,ch_a4ad373eae123a9a18b22717,在线支付,0.05,,2.05,pay@alipay.com,张三,subject,,2088002303691178,支付宝
总交易单数,总交易金额（元）,总退款金额（元）
5,0.11,0.03 
```


**失败时**，请参照 [响应错误码](#响应错误码)



## 支付渠道编码

| 渠道             | 编码              |
| :------------- | :-------------- |
| 支付宝移动支付        | alipay_app      |
| 微信移动支付         | wechat_app      |
| Apple Pay      | apple_pay_app   |
| 微信公众号          | wechat_wap      |
| 微信公众号（C2B扫码）支付 | wechat_csb      |
| 支付宝即时到账        | alipay_web      |
| 拉卡拉 PC 网关支付    | lakala_web      |
| 拉卡拉 PC 快捷支付    | lakala_web_fast |
| 拉卡拉移动 SDK 支付   | lakala_app      |
| 拉卡拉 H5 支付      | lakala_h5       |



## 支付渠道附加参数

#### 支付宝移动支付

无

#### 微信移动支付

无

#### Apple Pay

无

#### 微信公众号

open_id：必填，用户在公众号下的唯一标识

**示例**：

```json
{"open_id":"obc-jswk25IUGp3q8RPTYu083rmk"}
```

#### 支付宝即时到账

* return_url：必填，支付完成后回跳地址 
* show_url：非必填，商品在支付宝支付页面的展示“链接地址”

```json
{
  "show_url":"http://www.baidu.com",
  "return_url":"http://www.baidu.com"
}
```

#### 拉卡拉 PC 网关支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；
* return_url: 必填，支付完成后的回调地址；

```json
{
  "user_id":"123",
  "return_url":"http://www.abc.cn/"
}
```

#### 拉卡拉 PC 快捷支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；
* return_url: 必填，支付完成后的回调地址；

```json
{
  "user_id":"123",
  "return_url":"http://www.abc.cn/"
}
```

#### 拉卡拉移动SDK支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；

```json
{
  "user_id":"123"
}
```

#### 拉卡拉 H5 支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；
* return_url: 必填，支付完成后的回调地址；
* show_url: 必填，支付界面的返回按钮跳转的地址；

```json
{
  "user_id":"123",
  "return_url":"http://www.abc.cn/",
  "show_url":"http://www.abc.cn/charge"
}
```


## 关于退款的说明

各个渠道对于退款的处理有一定的差异，主要包括两种形式：

1. 需要商户手动确认的退款

   支付宝：商户发起支付宝退款申请后，如果申请提交成功，Paymax会返回一个status为PROCESSING的refund对象，会在extra参数中有一个refundUrl参数，值为需要用户手动确认的退款URL地址。

2. 不需要商户手动确认的退款

   除了支付宝之外，其他的渠道进行退款都不需要用户手动确认，Paymax会在退款状态发生改变时发送Webhooks通知。






## 响应错误码

当调用API接口失败时，会返回一个json对象，标明失败的原因和具体描述。

例如：

```json
{
  "failure_code": "SECRET_KEY_IS_BLANK",
  "failure_msg": "SecretKey为空"
}
```

* failure_code是对失败原因的唯一标识，failure_msg是对失败原因的进一步的解释和描述


* 对于同一种failure_code，可能会有多种failure_msg。



**详细的错误代码表及示例：**


| 错误码(failure_code)          | 描述示例(failure_msg) | 说明                                    |
| -------------------------- | ----------------- | ------------------------------------- |
| SECRET_KEY_IS_BLANK        | SecretKey为空       |                                       |
| SECRET_KEY_IS_INVALID      | 非法的SecretKey      |                                       |
| SIGN_CHECK_FAILED          | 签名校验失败            | 签名校验的原因                               |
| APP_NOT_EXISTED            | 应用不存在             |                                       |
| ORDER_NO_NOT_EXIST         | 订单不存在             |                                       |
| CHANNEL_NOT_AVAILABLE      | 该支付渠道未申请或者未开通     |                                       |
| AMOUNT_NOT_ENOUGH          | 金额不足              |                                       |
| MULTI_REFUND_RECORDS       | 已经存在一笔处理中的退款      | Paymax同时只能存在一笔处理中的退款                  |
| ORDER_NO_DUPLICATE         | 商户订单号已存在          | 商户使用相同的订单号重复下单                        |
| ILLEGAL_ARGUMENT           | 参数错误              | 调用接口的参数错误                             |
| ILLEGAL_CHANNEL_PARAMETERS | 支付渠道参数配置有误        |                                       |
| CHANNEL_CHARGE_FAILED      | 向支付渠道下单失败         | Paymax向支付渠道下单时失败，会在failure_msg中显示失败原因 |
| CHANNEL_FREEZE             | 该支付渠道已经被冻结交易      |                                       |
| CHANNEL_REFUND_FAILED      | 向支付渠道申请退款失败       | Paymax向支付渠道退款时失败，会在failure_msg中显示失败原因 |
|OUT_OF_DOWNLOAD|下载次数超限|对于对账单的下载，Paymax有一些约定的配置，在一定的时间内，调用接口不能超过规定次数|
| SYSTEM_ERROR               | 系统内部错误            | 由于某些原因造成系统处理异常，请联系Paymax处理            |
