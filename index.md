### 简介

----

####  API简介

欢迎使用BitTok API！

此文档是BitTok API的唯一官方文档，BitTok API提供的功能和服务会在此持续更新，请大家及时关注。



#### 联系我们

使用过程中如有问题或者建议，请联系我们：

- 通过官网的“帮助中心”或者发送邮件至service@bittok.io  support@bittok.io 联系客服。



如您遇到API错误，请按照如下模板向我们反馈问题。

` 1. 问题描述`

`2. 完整的URL请求`

` 3. 完整的JSON格式请求参数（如果有）`

`4. 完整的JSON格式返回结果`

`5. 问题出现时间和频率（如何时开始出现，是否可以重现）`

` 6. 签名前字符串（如果是签名认证错误）`



注意：Access Key仅能证明您的身份，不会影响您账户的安全。切记**不**要将Secret Key信息分享给任何人，若您不小心将Secret Key暴露，请尽快删除其对应的API Key，以免造成您的账户损失。



### 快速入门

------

#### 接入准备

如需使用API ，请先登录网页端，完成API key的申请和权限配置，再据此文档详情进行开发和交易。

您可以点击 [这里](http://ts03.dev.jmafan.com/) 创建 API Key。

创建成功后请务必记住以下信息：

-  `Access Key` API 访问密钥

- `Secret Key` 签名认证加密所使用的密钥

  

#### 接口类型

BitTok为用户提供两种接口，您可根据自己的使用场景和偏好来选择适合的方式进行查询行情、交易或提币。



#### REST API

##### REST

REST，即Representational State Transfer的缩写，是目前较为流行的基于HTTP的一种通信机制，每一个URL代表一种资源。

交易或资产提币等一次性操作，建议开发者使用REST API进行操作。

##### WebSocket API

WebSocket是HTML5一种新的协议（Protocol）。它实现了客户端与服务器全双工通信，通过一次简单的握手就可以建立客户端和服务器连接，服务器可以根据业务规则主动推送信息给客户端。

市场行情和买卖深度等信息，建议开发者使用WebSocket API进行获取。



#### 接入URLs

REST API

`https://api.bittok.io`



#### 签名认证

##### 签名说明

API 请求在通过 internet 传输的过程中极有可能被篡改，接口均必须使用您的 API Key 做签名认证，以校验参数或参数值在传输途中是否发生了更改。

一个合法的请求由以下几部分组成：

- 方法请求地址：即访问服务器地址 api.bittok.io。

- API 访问Id（AccessKeyId）：您申请的 API Key 中的 Access Key。

- 签名方法（SignatureMethod）：用户计算签名的基于哈希的协议，此处使用 HmacSHA256。

- 签名版本（SignatureVersion）：签名协议的版本，此处使用2。

- 时间戳（Timestamp）：您发出请求的时间 (UTC 时间) 。如：2017-05-11T16:22:06。在查询请求中包含此值有助于防止第三方截取您的请求。

- 必选和可选参数：每个方法都有一组用于定义 API 调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。

  - 对于 GET 请求，每个方法自带的参数都需要进行签名运算。

  - 对于 POST 请求，每个方法自带的参数不进行签名认证，并且需要放在 body 中。

- 签名：签名计算得出的值，用于确保签名有效和未被篡改。

##### 签名步骤

规范要计算签名的请求 因为使用 HMAC 进行签名计算时，使用不同内容计算得到的结果会完全不同。所以在进行签名计算前，请先对请求进行规范化处理。下面以查询某订单详情请求为例进行说明：

查询某订单详情时完整的请求URL

```
https://api.bittok.io/v1/order/orders?
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T15:19:30
&order-id=1234567890
```

**1. 请求方法（GET 或 POST，WebSocket用GET），后面添加换行符 “\n”**

例如： `GET\n`

**2. 添加小写的访问域名，后面添加换行符 “\n”**

例如： `api.bittok.io\n`

**3. 访问方法的路径，后面添加换行符 “\n”**

例如查询订单：
`/v1/order/orders\n`

**4. 对参数进行URL编码，并且按照ASCII码顺序进行排序**

例如，下面是请求参数的原始顺序，且进行URL编码后

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
order-id=1234567890
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
```

- ` UTF-8 编码，且进行了 URL 编码，十六进制字符必须大写，如 “:” 会被编码为 “%3A” ，空格被编码为 “%20”。`

- ` 时间戳（Timestamp）需要以YYYY-MM-DDThh:mm:ss格式添加并且进行 URL 编码。`

经过排序之后

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
order-id=1234567890
```

**5. 按照以上顺序，将各参数使用字符 “&” 连接**

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```

**6. 组成最终的要进行签名计算的字符串如下**

```
GET\n
api.huobi.pro\n
/v1/order/orders\n
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```

**7. 用上一步里生成的 “请求字符串” 和你的密钥 (Secret Key) 生成一个数字签名**

1. 将上一步得到的请求字符串和 API 私钥作为两个参数，调用HmacSHA256哈希函数来获得哈希值。
2. 将此哈希值用base-64编码，得到的值作为此次接口调用的数字签名。

```
4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=
```

**8. 将生成的数字签名加入到请求里**

对于Rest接口：

1. 把所有必须的认证参数添加到接口调用的路径参数里
2. 把数字签名在URL编码后加入到路径参数里，参数名为“Signature”。

最终，发送到服务器的 API 请求应该为

```
https://api.bittok.io/v1/order/orders?AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D
```



### 现货交易

----

#### 简介

现货交易接口提供了下单、撤单、订单查询、成交查询、手续费率查询等功能。



#### 下单

交易 限频值；100次/2s

发送一个新订单到BitTok以进行撮合。

#### HTTP 请求

- POST   /v1/order/orders/place

#### 请求参数

