# Bitvenus API Documentation

## Api Endpoints for BitVenus Broker

Name | base endpoint
------------ | ------------
rest-api | **[https://www.bitvenus.me/api/](https://www.bitvenus.me/api/)**
web-socket-streams | **[wss://wsapi.bitvenus.me](wss://wsapi.bitvenus.me)**
user-data-stream | **[wss://wsapi.bitvenus.me](wss://wsapi.bitvenus.me)**

## Interface List

|       Spot Market Data Interface      | Is login/authentication required |
| --------------------------------------- | ----------------- |
| /openapi/quote/v1/summaryEndpoint       | N                 |
| /openapi/quote/v1/ticker                | N                 |
| /openapi/quote/v1/orderbook/market_pair | N                 |
| /openapi/quote/v1/trades/market_pair    | N                 |
| Contract market interface                         |                   |
| /openapi/quote/v1/contractsDish         | N                 |
| /openapi/quote/v1/contractsTicker       | N                 |

## Interface Details

```
Spot Trading Pair List
PATH:   /openapi/quote/v1/summaryEndpoint
METHOD: GET
```

Returned Data Format List

|        Field Name        |      Type      |         Note         |
| ------------------------ | -------------- | -------------------- |
| trading_pairs            | string         | Spot Trading Pair ID |
| base_currency            | string         | Base Token           |
| quote_currency           | Best Ask Price | 计价token            |
| last_price               | decimal        | Latest Price         |
| lowest_ask               | decimal        | Best Ask Price       |
| highest_bid              | decimal        | Best Bid Price       |
| base_volume              | decimal        | Trading Volume       |
| quote_volume             | decimal        | Trading Amount       |
| price_change_percent_24h | double         | 24-hour Price Change |
| highest_price_24h        | decimal        | 24-hour High Price   |
| lowest_price_24h         | decimal        | 24-hour Low Price    |


```
Latest Trade
PATH:   /openapi/quote/v1/ticker
METHOD: GET
```

Returned Data Format List<Map<String,Data>>, Map Key for Spot Trading Pair, such as BTC_USDT

|  Field Name  |  Type   |      Note      |
| ------------ | ------- | -------------- |
| last_price   | decimal | Latest Price   |
| base_volume  | decimal | Trading Volume |
| quote_volume | decimal | Trading Amount |

```
Spot Market Depth Information
PATH:   /openapi/quote/v1/orderbook/market_pair
METHOD: GET
```

Request Parameters:

| Field Name  |Type    | Note                                       |Example               |
| ----------- | ------ | --------------------------------------------- | -------------------- |
| market_pair | string | Spot Trading Pair                      | BTC-USDT             |
| depth       | int    | Maximum Depth               | 0 indicates that all depths will be returned |
| level       | int    | When 1 is passed, only one bid and ask level will be returned. Otherwise, all market depth will be returned. | 0,1                  |

Return market depth information for this contract.

| Field Name  |  Type   |   Note    | 
| --------- | ------- | --------- | 
| ticker_id | string  | Contract ID   | 
| timestamp | long    | Base Token | 
| bids      | []      | Bids      | 
| --   [0]  | decimal | Price     | 
| --   [1]  | decimal | Contract Quantity  | 
| asks      | []      | Bids      | 
| --   [0]  | decimal | Price      | 
| --   [1]  | decimal | Contract Quantity   | 


```
Spot - Return the latest completed transaction data for a specified trading pair.
PATH:   /openapi/quote/v1/trades/market_pair
METHOD: GET
```

Request parameters：
|  Field Name  |  Type  |                   Note                    |Example|
| ----------- | ------ | --------------------------------------------- | -------- |
| market_pair | string | Spot trading pair                          | BTC-USDT |
| level       | int    | Return the top bid and ask orders only if the value is 1, otherwise return all the orders for the market.| 0,1      |

Return the latest transaction information.

|   Field Name   | Type   |       Note       |
| ------------ | ------- | ---------------- |
| trade_id     | long    | Contract ID         |
| price        | decimal | 	Trade price          |
| base_volume  | decimal | Trading Volume      |
| quote_volume | decimal | Trading Amount          |
| timestamp    | long    | Transaction time, timestamp |
| type         | string  | Sell, Buy        |


```
Contract transaction list
PATH:   /openapi/quote/v1/contractsDish
METHOD: GET
```

Returned Data Format List

|  Field Name   | Type  |   Note   | 
| ------------- | ------- | --------- | 
| ticker_id     | string  | Contract ID    | 
| timestamp     | long    | Base Token | 
| bids          | {}      | Bids      | 
| --   price    | decimal | Price    | 
| --   quantity | decimal | Contract volume  | 
| asks          | {}      | asks     | 
| --  price     | decimal | Price     | 
| --   quantity | decimal | Contract volume  | 


```
Latest contract transaction
PATH:   /openapi/quote/v1/contractsTicker
METHOD: GET
```
Returned Data Format List

|         Field Name          |                   Note                   |
| --------------------------- | ---------------------------------------- |
| ticker_id                   | Contract ID                              |
| base_currency               | Base Token                               |
| quote_currency              | Quoted Token                             |
| last_price                  | Latest price                             |
| base_volume                 | Trading volume priced in base currency   |
| quote_volume                | Trading volume priced in quoted currency |
| high                        | 24-hour highest price                    |
| low                         | 24-hour lowest price                     |
| product_type                | --                                       |
| open_interest               | Open interest value                      |
| open_interest_usd           | Open interest value in USD               |
| index_price                 | Index price                              |
| next_funding_rate_timestamp | Next funding rate settlement time        |
| funding_rate                | Current funding rate                     |
| next_funding_rate           | Predicted funding rate                   |
| contract_type               | --                                       |
| contract_price              | --                                       |
| contract_price_currency     | --                                       |
| usd_volume                  | --                                       |
