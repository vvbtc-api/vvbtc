

### 请求说明：

1.访问地址：

2.签名认证 :目前关于apikey申请和修改，请在“个人中心 - API管理”页面进行相关操作。其中AccessKey为API 访问		        密钥，SecretKey为用户对请求进行签名的密钥（仅申请时可见）。 

3.签名方法 :每个API接口的所有请求参数均参与签名，参数值为空，不需要参与签名。此外，参与签名的参数还有随机字符串（nonceStr）、时间戳（timestamp）、API访问秘钥（accessKey）;将所有参数按照参数名ASCII码顺序进行排序，通过`APISercret`使用`HmacSHA256`协议进行加密，得到签名参数`sign`

```java
	// Java版本签名方法
	public static String sha256_HMAC(SortedMap<String, String> paramMap, String secret) {
        String params = "";

        String firstKey = paramMap.firstKey();
        for (Map.Entry<String, String> entry : paramMap.entrySet()) {
            // 如果是第一个不加&
            String key = entry.getKey();
            String value = entry.getValue();
            if (!firstKey.equals(key)) {
                params += "&";

            }
            params += key + "=" + value;
        }

        return sha256_HMAC(params, secret);
    }

    /**
     * sha256_HMAC 加密
     */
    public static String sha256_HMAC(String param, String secret) {

        String hash = "";

        try {
            Mac sha256_HMAC = Mac.getInstance("HmacSHA256");

            SecretKeySpec secret_key = new SecretKeySpec(secret.getBytes(), "HmacSHA256");

            sha256_HMAC.init(secret_key);

            byte[] bytes = sha256_HMAC.doFinal(param.getBytes());
            hash = byteArrayToHexString(bytes);
        } catch (Exception e) {
            System.out.println("Error HmacSHA256 ===========" + e.getMessage());
        }
        return hash;
    }

    /**
     * 将加密后的字节数组转成字符串
     */
    private static String byteArrayToHexString(byte[] b) {
        StringBuilder hs = new StringBuilder();
        String stmp;
        for (int n = 0; b != null && n < b.length; n++) {
            stmp = Integer.toHexString(b[n] & 0XFF);
            if (stmp.length() == 1)
                hs.append('0');
            hs.append(stmp);
        }
        return hs.toString().toLowerCase();
    }
```



**简要描述：** 

- 币种列表

**请求URL：** 
- ` /v1/common/currencys `

**请求方式：**
- GET 

**参数：** 

​	无


 **返回示例**
``` 
 {
    "code": "0",
    "msg": "ok",
    "data": [
        "BTC",
        "ETH",
        "LTC",
        "ETC",
        "EOS",
        "HSR",
        "QTUM",
        "BOT",
        "USDT",
        "CNY",
        "BBC",
        "BBQ",
        "BTCC",
        "VSET",
        "NED"
    ]
}
```
 **返回参数说明** 

| 参数名 | 类型   | 说明                               |
| :----- | :----- | ---------------------------------- |
| code   | string | 状态码<br> 0 成功 <br> -1 失败     |
| msg    | string | 错误信息                           |
| data   | array  | BTC,ETH,LTC...均为系统支持币种名称 |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 系统所有交易对

**请求URL：** 
- ` /v1/common/symbols `

**请求方式：**
- GET 

**参数：** 

​	无


 **返回示例**
``` 
{
  "code": 0,
  "msg": "成功",
  "data": [
    {
      "pairId": 1,
      "symbol": "BTCUSDT",
      "sizePrecision": 8,
      "pricePrecision": 2
    },
    {
      "pairId": 2,
      "symbol": "ETHUSDT",
      "sizePrecision": 7,
      "pricePrecision": 3
    },
    {
      "pairId": 3,
      "symbol": "ETCUSDT",
      "sizePrecision": 6,
      "pricePrecision": 4
    }
  ]
}
```
 **返回参数说明** 

| 参数名         | 类型   | 说明                                              |
| :------------- | :----- | ------------------------------------------------- |
| code           | string | 状态码<br> 0 成功 <br> -1 失败                    |
| msg            | string | 错误信息                                          |
| pairId         | int    | 交易对ID                                          |
| symbol         | string | BBCUSDT,BBQUSDT,BOTETH...均为系统支持的交易对名称 |
| sizePrecision  | int    | 数量小数位精度                                    |
| pricePrecision | int    | 价格小数位精度                                    |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述





**简要描述：** 

- 单个交易对的最新行情

**请求URL：** 
- ` /v1/common/getQuote `

**请求方式：**
- POST 

**参数：** 

| 参数名 | 必选 | 类型   | 说明                       |
| :----- | :--- | :----- | -------------------------- |
| symbol | 是   | string | 交易对的名称 例如：BTCUSDT |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data": {
        "pairId": 1,
        "fromTime": 1532929801000,
        "toTime": 1533016200000,
        "open": 8183.17,
        "high": 8200,
        "low": 7865.94,
        "close": 8131.99,
        "volume": 5941.848153,
        "quoteVolume": 48133069.17140592,
        "tradeNum": 5703
    }
}
```
 **返回参数说明** 

| 参数名      | 类型       | 说明                           |
| :---------- | :--------- | ------------------------------ |
| code        | string     | 状态码<br> 0 成功 <br> -1 失败 |
| msg         | string     | 错误信息                       |
| pairId      | int        | 交易对ID                       |
| fromTime    | long       | 起始时间                       |
| toTime      | long       | 截止时间                       |
| open        | bigDecimal | 开盘价                         |
| high        | bigDecimal | 最高价                         |
| low         | bigDecimal | 最低价                         |
| close       | bigDecimal | 收盘价                         |
| volume      | bigDecimal | 成交量                         |
| quoteVolume | bigDecimal | 成交额                         |
| tradeNum    | long       | 成交数                         |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 多个交易对最新行情

**请求URL：** 
- ` /v1/common/getQuotes `

**请求方式：**
- POST 

**参数：** 

| 参数名  | 必选 | 类型   | 说明                                                         |
| :------ | :--- | :----- | ------------------------------------------------------------ |
| symbols | 是   | string | 交易对的名称 例如：BTCUSDT,ETHBTC,LTCBTC 注：每个交易对之间用`,`隔开 |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data": {
        "1": {
            "pairId": 1,
            "fromTime": 1532929801000,
            "toTime": 1533016200000,
            "open": 8183.17,
            "high": 8200,
            "low": 7865.94,
            "close": 8131.99,
            "volume": 5941.848153,
            "quoteVolume": 48133069.17140592,
            "tradeNum": 5703
        },
        "9": {
            "pairId": 9,
            "fromTime": 1532374021000,
            "toTime": 1532460420000,
            "open": 0.07190201,
            "high": 0.07190201,
            "low": 0.057098,
            "close": 0.057725,
            "volume": 8708.08697673,
            "quoteVolume": 505.64043215,
            "tradeNum": 2011
        },
        "11": {
            "pairId": 11,
            "fromTime": 1532929801000,
            "toTime": 1533016200000,
            "open": 0.010263,
            "high": 0.010284,
            "low": 0.00999,
            "close": 0.010041,
            "volume": 37697.913226,
            "quoteVolume": 382.77739298,
            "tradeNum": 5690
        }
    }
}
```
 **返回参数说明** 

| 参数名      | 类型       | 说明                           |
| :---------- | :--------- | ------------------------------ |
| code        | string     | 状态码<br> 0 成功 <br> -1 失败 |
| msg         | string     | 错误信息                       |
| pairId      | int        | 交易对ID                       |
| fromTime    | long       | 起始时间                       |
| toTime      | long       | 截止时间                       |
| open        | bigDecimal | 日K线 开盘价                   |
| high        | bigDecimal | 日K线 最高价                   |
| low         | bigDecimal | 日K线 最低价                   |
| close       | bigDecimal | 日K线 收盘价                   |
| volume      | bigDecimal | 成交量                         |
| quoteVolume | bigDecimal | 成交额                         |
| tradeNum    | long       | 成交数                         |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述





**简要描述：** 

- 单个交易对最新深度

**请求URL：** 
- ` /v1/market/depth `

**请求方式：**
- POST 

**参数：** 

| 参数名 | 必选 | 类型   | 说明                       |
| :----- | :--- | :----- | -------------------------- |
| symbol | 是   | string | 交易对的名称 例如：BTCUSDT |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data": {
        "pairId": 15,
        "bids": [
            [
                0.011448,
                5.485
            ],
            [
                0.011443,
                3.80395
            ],
            [
                0.011434,
                2.2
            ],
            [
                0.011384,
                57.55
            ],
            [
                0.011383,
                75
            ],
            [
                0.011382,
                58.913
            ],
            [
                0.011381,
                85.4185
            ],
            [
                0.011379,
                27
            ],
            [
                0.011319,
                293.3
            ],
            [
                0.011318,
                21.38945
            ]
        ],
        "asks": [
            [
                0.011487,
                4.515
            ],
            [
                0.011492,
                16.4316
            ],
            [
                0.0115,
                90
            ],
            [
                0.011502,
                45
            ],
            [
                0.011548,
                21.17745
            ],
            [
                0.011597,
                0.76035
            ],
            [
                0.011632,
                0.00045
            ],
            [
                0.011656,
                2.24085
            ],
            [
                0.011697,
                1.3615
            ],
            [
                0.011712,
                1.42045
            ]
        ]
    }
}
```
 **返回参数说明** 

| 参数名 | 类型   | 说明                                             |
| :----- | :----- | ------------------------------------------------ |
| code   | string | 状态码<br> 0 成功 <br> -1 失败                   |
| msg    | string | 错误信息                                         |
| pairId | int    | 交易对ID                                         |
| bids   | array  | 买盘[price(成交价), amount(成交量)], 按price降序 |
| asks   | array  | 卖盘[price(成交价), amount(成交量)], 按price升序 |


 **备注** 

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 单个交易对最新成交

**请求URL：** 
- ` /v1/market/trade `

**请求方式：**
- POST 

**参数：** 

| 参数名 | 必选 | 类型   | 说明                       |
| :----- | :--- | :----- | -------------------------- |
| symbol | 是   | string | 交易对的名称 例如：BTCUSDT |

 **返回示例**
``` java
{
    "code": "0",
    "msg": "ok",
    "data": {
        "pairId": 15,
        "execs": [
            {
                "price": 0.012424,
                "size": 0.160776,
                "takerSide": "B",
                "time": 1530915770000
            },
            {
                "price": 0.012356,
                "size": 0.115969,
                "takerSide": "S",
                "time": 1530915749000
            },
            {
                "price": 0.012436,
                "size": 0.181267,
                "takerSide": "B",
                "time": 1530915725000
            }
		]
	}
}    
```
 **返回参数说明** 

| 参数名    | 类型       | 说明                           |
| :-------- | :--------- | ------------------------------ |
| code      | string     | 状态码<br> 0 成功 <br> -1 失败 |
| msg       | string     | 错误信息                       |
| pairId    | int        | 交易对ID                       |
| execs     | array      | 成交记录                       |
| price     | bigDecimal | 成交价                         |
| size      | bigDecimal | 成交数量                       |
| takerSide | string     | 主动成交方向                   |
| time      | long       | 成交时间                       |


 **备注** 

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 单个交易对的K线

**请求URL：** 
- ` /v1/symbol/kline `

**请求方式：**
- POST 

**参数：** 

| 参数名       | 必选 | 类型   | 说明                                                         |
| :----------- | :--- | :----- | ------------------------------------------------------------ |
| symbol       | 是   | string | 交易对的名称 例如：BTCUSDT                                   |
| tradeBarType | 是   | string | K线类型 MN1 1分钟，MN5 5分钟，MN15 15 分钟，MN30 30分钟，H1 1小时，H2 2小时，H4 4小时，H6 6小时，H12 12 小时，D1 1天，W1 1周，M1 1月 |
| limit        | 是   | long   | 获取数量                                                     |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data": [
        {
            "barId": 74299,
            "pairId": 15,
            "barType": "MN15",
            "id": 201807070615,
            "close": 0.012424,
            "high": 0.01246,
            "low": 0.012351,
            "open": 0.012415,
            "volume": 56.672738,
            "tradeNum": 24,
            "quoteVolume": 0.70125715,
            "takerBuyQuoteVolume": 36.23527248,
            "takerBuyAssetVolume": 36.233275,
            "createdDate": 1530915312000,
            "updatedDate": 1530915770000
        },
        {
            "barId": 74006,
            "pairId": 15,
            "barType": "MN15",
            "id": 201807070600,
            "close": 0.012415,
            "high": 0.01253,
            "low": 0.012415,
            "open": 0.012473,
            "volume": 17.499743,
            "tradeNum": 53,
            "quoteVolume": 0.2178197,
            "takerBuyQuoteVolume": 9.54962216,
            "takerBuyAssetVolume": 9.54902,
            "createdDate": 1530914400000,
            "updatedDate": 1530915298000
        }
        ...
    ]
}
```
 **返回参数说明** 

| 参数名      | 类型       | 说明                           |
| :---------- | :--------- | ------------------------------ |
| code        | string     | 状态码<br> 0 成功 <br> -1 失败 |
| msg         | string     | 错误信息                       |
| pairId      | int        | 交易对ID                       |
| barId       | long       | K线ID                          |
| barType     | string     | K线类型                        |
| id          | long       | 成交数量                       |
| close       | bigDecimal | 收盘价                         |
| high        | bigDecimal | 最高价                         |
| low         | bigDecimal | 最低价                         |
| open        | bigDecimal | 开盘价                         |
| volume      | bigDecimal | 成交量                         |
| tradeNum    | bigDecimal | 成交数                         |
| quoteVolume | bigDecimal | 成交额                         |

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 查询用户账户余额

**请求URL：** 
- ` /v1/account/balance `

**请求方式：**
- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                                      |
| :-------- | :--- | :----- | --------------------------------------------------------- |
| nonceStr  | 是   | string | 随机字符串                                                |
| timestamp | 是   | string | 时间戳                                                    |
| accessKey | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                 |
| sign      | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名） |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data": {
        "id": "1",
        "state": "working",
        "list": [
            {
                "balance": 100000951.70759641,
                "currency": "BTC",
                "type": "trade"
            },
            {
                "balance": 0,
                "currency": "BTC",
                "type": "frozen"
            },
            {
                "balance": 99999866.4135881,
                "currency": "ETH",
                "type": "trade"
            }
            ...
        ]
    }
}
```
 **返回参数说明** 

| 参数名   | 类型       | 说明                              |
| :------- | :--------- | --------------------------------- |
| code     | string     | 状态码<br> 0 成功 <br> -1 失败    |
| msg      | string     | 错误信息                          |
| id       | string     | 用户ID                            |
| state    | string     | working：正常 lock：账户被锁定    |
| balance  | bigDecimal | 余额                              |
| currency | long       | 币种                              |
| type     | string     | trade: 交易余额，frozen: 冻结余额 |



 **备注** 

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 用户当前委托

**请求URL：** 
- ` /v1/order/orders `

**请求方式：**
- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                                      |
| :-------- | :--- | :----- | --------------------------------------------------------- |
| symbol    | 是   | string | 交易对的名称 例如：BTCUSDT                                |
| nonceStr  | 是   | string | 随机字符串                                                |
| timestamp | 是   | string | 时间戳                                                    |
| accessKey | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                 |
| sign      | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名） |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data":  {
            "orderId": 116,
            "userId": 68,
            "pairId": 1,
            "actionType": "B",
            "orderStatus": "F",
            "orderType": "L",
            "price": 6550.3292,
            "size": 0.1,
            "commission": 0.0002,
            "filledSize": 0.1,
            "lastFilledPrice": 6550.29,
            "createDate": 1530846206000
        }
}
```
 **返回参数说明** 

| 参数名          | 类型       | 说明                                                         |
| :-------------- | :--------- | ------------------------------------------------------------ |
| code            | string     | 状态码<br> 0 成功 <br> -1 失败                               |
| msg             | string     | 错误信息                                                     |
| orderId         | long       | 订单ID                                                       |
| userId          | long       | 用户ID                                                       |
| pairId          | long       | 交易对ID                                                     |
| actionType      | string     | 买卖方向 B - Buy S - Sell                                    |
| orderStatus     | string     | 订单状态  S - 已提交 P - 部分成交 F - 全部成交 C - 已撤销 CP - 部分成交已撤销 E - 失败  EP - 部分成交剩余失败 D - 仅用于市价单：没有成交，已经结束  DP - 仅用于市价单：部分成交，已经结束 |
| orderType       | string     | 订单的类型 限价单(L) 市价单(M) L   止盈止损单(SL)            |
| price           | bigDecimal | 下单价格                                                     |
| size            | bigDecimal | 下单量                                                       |
| filledSize      | bigDecimal | 已成交数量                                                   |
| lastFilledPrice | bigDecimal | 最后成交价格                                                 |
| commission      | bigDecimal | 累计产生交易手续费                                           |
| createdDate     | long       | 下单时间                                                     |
 **备注** 

- 更多返回错误代码请看首页的错误代码描述









**简要描述：** 

- 用户历史委托

**请求URL：** 
- ` /v1/order/history/orders `

**请求方式：**
- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                                      |
| :-------- | :--- | :----- | --------------------------------------------------------- |
| symbol    | 是   | string | 交易对的名称 例如：BTCUSDT                                |
| nonceStr  | 是   | string | 随机字符串                                                |
| timestamp | 是   | string | 时间戳                                                    |
| accessKey | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                 |
| pageNum   | 是   | int    | 页码                                                      |
| pageSize  | 是   | int    | 每页显示条数                                              |
| sign      | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名） |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "ok",
    "data": [
        {
            "orderId": 116,
            "userId": 68,
            "pairId": 1,
            "actionType": "B",
            "orderStatus": "F",
            "orderType": "L",
            "price": 6550.3292,
            "size": 0.1,
            "commission": 0.0002,
            "filledSize": 0.1,
            "lastFilledPrice": 6550.29,
            "createDate": 1530846206000
        },
        {
            "orderId": 118,
            "userId": 68,
            "pairId": 1,
            "actionType": "B",
            "orderStatus": "DP",
            "orderType": "M",
            "price": null,
            "size": 0.1,
            "commission": 0.00010534,
            "filledSize": 0.0526682,
            "lastFilledPrice": 6549.98,
            "createDate": 1530846325000
        }
    ]
}
```
 **返回参数说明** 

| 参数名          | 类型       | 说明                                                         |
| :-------------- | :--------- | ------------------------------------------------------------ |
| code            | string     | 状态码<br> 0 成功 <br> -1 失败                               |
| msg             | string     | 错误信息                                                     |
| orderId         | long       | 订单ID                                                       |
| userId          | long       | 用户ID                                                       |
| pairId          | long       | 交易对ID                                                     |
| actionType      | string     | 买卖方向 B - Buy S - Sell                                    |
| orderStatus     | string     | 订单状态  S - 已提交 P - 部分成交 F - 全部成交 C - 已撤销 CP - 部分成交已撤销 E - 失败  EP - 部分成交剩余失败 D - 仅用于市价单：没有成交，已经结束  DP - 仅用于市价单：部分成交，已经结束 |
| orderType       | string     | 订单的类型 限价单(L) 市价单(M) L   止盈止损单(SL)            |
| price           | bigDecimal | 下单价格                                                     |
| size            | bigDecimal | 下单量                                                       |
| filledSize      | bigDecimal | 已成交数量                                                   |
| lastFilledPrice | bigDecimal | 最后成交价格                                                 |
| commission      | bigDecimal | 累计产生交易手续费                                           |
| createdDate     | long       | 下单时间                                                     |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述





**简要描述：** 

- 用户历史成交

**请求URL：** 
- ` /v1/order/matchresults`
**请求方式：**
- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                                         |
| :-------- | :--- | :----- | ------------------------------------------------------------ |
| symbol    | 是   | string | 交易对的名称 例如：BTCUSDT                                   |
| fromDate  | 是   | string | 起始日期 查询开始日期, 日期格式yyyy-mm-dd                    |
| toDate    | 是   | string | 结束日期 查询结束日期, 日期格式yyyy-mm-dd                    |
| from      | 是   | string | 查询起始ID （订单成交记录ID）                                |
| direct    | 是   | int    | 查询方向 （默认值next，成交记录ID由大到小排序） prev 向前  next 向后 |
| size      | 是   | int    | 查询记录大小 默认值 100 取值范围 <=100                       |
| nonceStr  | 是   | string | 随机字符串                                                   |
| timestamp | 是   | string | 时间戳                                                       |
| accessKey | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                    |
| sign      | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名）    |

``` 
{
    "code": "0",
    "msg": "ok",
    "data": [
        {
            "symbol": "BTCUSDT",
            "match-id": 250019,
            "filled-amount": 0.1,
            "price": 6550.29,
            "filled-fees": 0.0002,
            "created-at": 1530846206000,
            "id": 125,
            "order-id": 116
        },
        {
            "symbol": "BTCUSDT",
            "match-id": 250111,
            "filled-amount": 0.0049,
            "price": 6549.19,
            "filled-fees": 0.0000098,
            "created-at": 1530846325000,
            "id": 127,
            "order-id": 118
        }
	]
}	
```
 **返回参数说明** 

| 参数名        | 类型       | 说明                           |
| :------------ | :--------- | ------------------------------ |
| code          | string     | 状态码<br> 0 成功 <br> -1 失败 |
| msg           | string     | 错误信息                       |
| symbol        | string     | 交易对                         |
| match-id      | long       | 撮合ID                         |
| filled-amount | bigDecimal | 成交数量                       |
| price         | bigDecimal | 成交价格                       |
| filled-fees   | bigDecimal | 成交手续费                     |
| created-at    | bigDecimal | 成交时间                       |
| id            | long       | 订单成交记录ID                 |
| order-id      | long       | 订单 ID                        |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述









**简要描述：** 

- 用户下单

**请求URL：** 
- ` /v1/order/placeOrder `

**请求方式：**
- POST 

**参数：** 

| 参数名       | 必选 | 类型   | 说明                                                      |
| :----------- | :--- | :----- | --------------------------------------------------------- |
| symbol       | 是   | string | 交易对的名称 例如：BTCUSDT                                |
| action       | 是   | string | 订单的买卖向操作 `B` 买 `S` 卖                            |
| orderType    | 是   | string | 订单类型  L 限价单  M 市价单 SL 止盈止损单                |
| price        | 是   | string | 价格                                                      |
| triggerPrice | 是   | string | 止盈止损单触发价                                          |
| size         | 是   | int    | 数量                                                      |
| amount       | 是   | string | 市价买入,买入总额                                         |
| nonceStr     | 是   | string | 随机字符串                                                |
| timestamp    | 是   | string | 时间戳                                                    |
| accessKey    | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                 |
| sign         | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名） |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "下单成功",
    "data": {
        "orderId": 1130
    }
}
```
 **返回参数说明** 

| 参数名  | 类型   | 说明                           |
| :------ | :----- | ------------------------------ |
| code    | string | 状态码<br> 0 成功 <br> -1 失败 |
| msg     | string | 错误信息                       |
| orderId | long   | 订单ID                         |


 **备注** 

- 更多返回错误代码请看首页的错误代码描述











**简要描述：** 

- 用户撤单

**请求URL：** 
- ` /v1/order/cancelOrder `

**请求方式：**
- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                                      |
| :-------- | :--- | :----- | --------------------------------------------------------- |
| orderId   | 是   | string | 交易ID                                                    |
| nonceStr  | 是   | string | 随机字符串                                                |
| timestamp | 是   | string | 时间戳                                                    |
| accessKey | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                 |
| sign      | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名） |

 **返回示例**
``` 
{
    "code": "0",
    "msg": "撤单成功",
}
```
 **返回参数说明** 

| 参数名 | 类型   | 说明                           |
| :----- | :----- | ------------------------------ |
| code   | string | 状态码<br> 0 成功 <br> -1 失败 |
| msg    | string | 错误信息                       |


 **备注** 

- 更多返回错误代码请看首页的错误代码描述







**简要描述：** 

- 查询订单详情

**请求URL：** 

- ` /v1/order/getOrderDetail `

**请求方式：**

- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                                      |
| :-------- | :--- | :----- | --------------------------------------------------------- |
| orderId   | 是   | string | 交易ID                                                    |
| nonceStr  | 是   | string | 随机字符串                                                |
| timestamp | 是   | string | 时间戳                                                    |
| accessKey | 是   | string | VVBTC网站个人中心 APIKey管理中创建的KeyID                 |
| sign      | 是   | string | 所有参数使用ApiSecret加密获得的签名（sign本身不参与签名） |

 **返回示例**

```
{
    "code": "0",
    "msg": "成功",
    "data": {
        "actionType": "B",
        "active": false,
        "commission": 0.002,
        "createdDate": 1533980153000,
        "filledSize": 1,
        "lastFilledPrice": 7061.170001,
        "orderId": 231734,
        "orderStatus": "F",
        "orderType": "L",
        "pairId": 1,
        "price": 7061.170001,
        "size": 1,
        "symbol": "BTCUSDT",
        "userId": 68
    }
}
```

 **返回参数说明** 

| 参数名          | 类型    | 说明                                                         |
| :-------------- | :------ | ------------------------------------------------------------ |
| actionType      | string  | 订单买卖类型, 卖出(S)、买入(B)                               |
| active          | boolean | 是否仍然活跃, 活跃单(true)-挂单中的订单、非活跃单(false)-已成交或已撤销的订单 |
| commission      | double  | 订单累计产生交易手续费                                       |
| createdDate     | long    | 挂单时间（GMT时间）                                          |
| filledSize      | double  | 订单已成交数量                                               |
| lastFilledPrice | double  | 订单最后成交价格                                             |
| orderId         | long    | 订单ID                                                       |
| orderStatus     | string  | 订单状态, 已提交(S)、部份成交(P)、成交(F)、已撤销(C)、部成已撤(CP)、错误(E)、部成错误(EP)、市价单无成交(D)、市价单部份成交(DP) |
| orderType       | string  | 订单的类型 限价单(L) 市价单(M) L   止盈止损单(SL)            |
| price           | double  | 订单价格                                                     |
| size            | double  | 订单数量                                                     |
| symbol          | string  | 订单交易对SYMBOL                                             |
| triggerPrice    | double  | 止盈止损单触发价                                             |
| triggerStatus   | boolean | true 已触发 false  未触发                                    |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述