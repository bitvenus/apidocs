# Bitvenus API Documentation

## 接口列表

|              现货行情接口               | 是否需要登录/验签 |
| --------------------------------------- | ----------------- |
| /openapi/quote/v1/summaryEndpoint       | N                 |
| /openapi/quote/v1/ticker                | N                 |
| /openapi/quote/v1/orderbook/market_pair | N                 |
| /openapi/quote/v1/trades/market_pair    | N                 |
| 合约行情接口                            |                   |
| /openapi/quote/v1/contractsDish         | N                 |
| /openapi/quote/v1/contractsTicker       | N                 |

## 接口明细

```
现货交易对列表
PATH:   /openapi/quote/v1/summaryEndpoint
METHOD: GET
```

返回数据格式 List

|         字段名称         |   类型   |      备注      |     |
| ------------------------ | -------- | -------------- | --- |
| trading_pairs            | string   | 现货交易对id   |     |
| base_currency            | string   | 基础token      |     |
| quote_currency           | 卖一价格 | 计价token      |     |
| last_price               | decimal  | 最新价格       |     |
| lowest_ask               | decimal  | 卖一价格       |     |
| highest_bid              | decimal  | 买一价格       |     |
| base_volume              | decimal  | 交易量         |     |
| quote_volume             | decimal  | 交易额         |     |
| price_change_percent_24h | double   | 24小时涨跌幅   |     |
| highest_price_24h        | decimal  | 24小时最高价格 |     |
| lowest_price_24h         | decimal  | 24小时最低价格 |     |


```
最新成交
PATH:   /openapi/quote/v1/ticker
METHOD: GET
```

返回数据格式 List<Map<String,Data>>, map key 为 现货交易对，例如 BTC_USDT

|   字段名称   |  类型   |   备注   |     |
| ------------ | ------- | -------- | --- |
| last_price   | decimal | 最新价格 |     |
| base_volume  | decimal | 交易量   |     |
| quote_volume | decimal | 交易额   |     |

```
现货 盘口深度信息
PATH:   /openapi/quote/v1/orderbook/market_pair
METHOD: GET
```

请求参数：

| 字段名称    |类型    | 备注                                          | 示例                 |
| ----------- | ------ | --------------------------------------------- | -------------------- |
| market_pair | string | 现货交易对                                    | BTC-USDT             |
| depth       | int    | 最大深度                                      | 0 表示所有深度都返回 |
| level       | int    | 当且仅单传1时返回一档买卖盘，否则返回全部盘口 | 0,1                  |

返回该合约的盘口信息

| 字段名称  |  类型   |   备注    | 
| --------- | ------- | --------- | 
| ticker_id | string  | 合约id    | 
| timestamp | long    | 基础token | 
| bids      | []      | 买盘      | 
| --   [0]  | decimal | 价格      | 
| --   [1]  | decimal | 合约张数  | 
| asks      | []      | 买盘      | 
| --   [0]  | decimal | 价格      | 
| --   [1]  | decimal | 合约张数  | 


```
现货 - 返回指定交易最近完成的交易数据
PATH:   /openapi/quote/v1/trades/market_pair
METHOD: GET
```

请求参数：
|  字段名称   |  类型  |                     备注                      |   示例   |
| ----------- | ------ | --------------------------------------------- | -------- |
| market_pair | string | 现货交易对                                    | BTC-USDT |
| level       | int    | 当且仅单传1时返回一档买卖盘，否则返回全部盘口 | 0,1      |

返回该最新成交的信息

|   字段名称   |  类型   |       备注       |
| ------------ | ------- | ---------------- |
| trade_id     | long    | 合约id           |
| price        | decimal | 成交价           |
| base_volume  | decimal | 成交量           |
| quote_volume | decimal | 成交额           |
| timestamp    | long    | 成交时间，时间戳 |
| type         | string  | Sell, Buy        |


```
合约交易列表
PATH:   /openapi/quote/v1/contractsDish
METHOD: GET
```

返回数据格式 List

|   字段名称    |  类型   |   备注    | 
| ------------- | ------- | --------- | 
| ticker_id     | string  | 合约id    | 
| timestamp     | long    | 基础token | 
| bids          | {}      | 买盘      | 
| --   price    | decimal | 价格      | 
| --   quantity | decimal | 合约张数  | 
| asks          | {}      | 买盘      | 
| --  price     | decimal | 价格      | 
| --   quantity | decimal | 合约张数  | 


```
合约最近成交
PATH:   /openapi/quote/v1/contractsTicker
METHOD: GET
```
返回数据格式 List

|          字段名称           |         备注         | 
| --------------------------- | -------------------- | 
| ticker_id                   | 合约id               | 
| base_currency               | 基础token            | 
| quote_currency              | 计价token            | 
| last_price                  | 最新价格             | 
| base_volume                 | 基础货币计价的成绩量 | 
| quote_volume                | 计价货币计价的成交额 | 
| high                        | 24小时最高价         | 
| low                         | 24小时最低价         | 
| product_type                | --                   | 
| open_interest               | 开仓价值             | 
| open_interest_usd           | 开仓价值 usd 额度    | 
| index_price                 | 指数价格             | 
| next_funding_rate_timestamp | 下一次资金费收取时间 | 
| funding_rate                | 当前资金费率         | 
| next_funding_rate           | 预测资金费率         | 
| contract_type               | --                   | 
| contract_price              | --                   | 
| contract_price_currency     | --                   | 
| usd_volume                  | --                   | 