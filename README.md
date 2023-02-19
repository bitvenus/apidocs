# Bitvenus API Documentation
## Introduction

The BitVenus API allows users to access real-time and historical data for spot and contract markets. This document outlines the available endpoints, required parameters, and expected responses for each API call.

All API calls require an API key and secret to be included in the request header. Please see the BitVenus website for instructions on obtaining an API key.

## General API Information

* All endpoints return either a JSON object or array.
* Data is returned in ascending order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
* HTTP 4XX return codes are used for for malformed requests; the issue is on the sender's side.
* HTTP 429 return code is used when breaking a request rate limit.
* HTTP 418 return code is used when an IP has been auto-banned for continuing to send requests after receiving 429 codes.
* HTTP 5XX return codes are used for internal errors; the issue is on broker's side. It is important to NOT treat this as a failure operation; the execution status is UNKNOWN and could have been a success.
* Any endpoint can return an ERROR; the error payload is as follows:
    ```
    {
    "code": -1121,
    "msg": "Invalid symbol."
    }
    ```
* Specific error codes and messages defined in another document.
* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a `query string` or in the `request body` with content type `application/x-www-form-urlencoded`. You may mix parameters between both the `query string` and `request body` if you wish to do so.
* Parameters may be sent in any order.
* If a parameter sent in both the `query string` and `request body`, the `query string` parameter will be used.

### Api Endpoints 

|        Name        |           rest-api           |
| ------------------ | ---------------------------- |
| base endpoint      | https://www.bitvenus.me/api/ |
| web-socket-streams | wss://wsapi.bitvenus.me      |

### Making Requests

Request bodies should have content type application/json and be in valid JSON format.

## Data Interface

|       Spot Market Data Interface        | Is login/authentication required |
| --------------------------------------- | -------------------------------- |
| /openapi/quote/v1/summaryEndpoint       | N                                |
| /openapi/quote/v1/ticker                | N                                |
| /openapi/quote/v1/orderbook/market_pair | N                                |
| /openapi/quote/v1/trades/market_pair    | N                                |
| Contract market interface               |                                  |
| /openapi/quote/v1/contractsDish         | N                                |
| /openapi/quote/v1/contractsTicker       | N                                |
| /openapi/quote/v1/orderbook             | N                                |

For the spot market data interface, there are four endpoints:

* /openapi/quote/v1/summaryEndpoint: Provides a summary of the market, including trading volume and price change percentage. No authentication is required to access this endpoint.
* /openapi/quote/v1/ticker: Provides real-time ticker data for a specific market. No authentication is required to access this endpoint.
* /openapi/quote/v1/orderbook/market_pair: Provides the current order book for a specific market, including buy and sell orders. No authentication is required to access this endpoint.
* /openapi/quote/v1/trades/market_pair: Provides a list of recent trades for a specific market. No authentication is required to access this endpoint.

For the contract market interface, there are two endpoints:

* /openapi/quote/v1/contractsDish: Provides a list of available contract markets, including the contract name and expiration date. No authentication is required to access this endpoint.
* /openapi/quote/v1/contractsTicker: Provides real-time ticker data for a specific contract market. No authentication is required to access this endpoint.
## Interface Details

```
Spot Trading Pair List
PATH:   /openapi/quote/v1/summaryEndpoint
METHOD: GET
```

Returned Data Format: List

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

Return market depth information for this symbol.

| Field Name |  Type   |       Note        |
| ---------- | ------- | ----------------- |
| ticker_id  | string  | Contract ID       |
| timestamp  | long    | Base Token        |
| bids       | []      | Bids              |
| --   [0]   | decimal | Price             |
| --   [1]   | decimal | Contract Quantity |
| asks       | []      | Bids              |
| --   [0]   | decimal | Price             |
| --   [1]   | decimal | Contract Quantity |


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
| trade_id     | long    | Symbol ID         |
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
Returned Data Format: List

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
<<<<<<< HEAD
=======


```
Contract - Return the OrderBook
PATH:   /openapi/quote/v1/orderbook 
METHOD: GET
```

Request parameters：

| Field Name |  Type  |            Note             |                   Example                    |
| ---------- | ------ | --------------------------- | -------------------------------------------- |
| ticker_id  | string | Contract trading  symbol id | BTC-SWAP-USDT                                |
| depth      | int    | Maximum Depth               | 0 indicates that all depths will be returned |

Return the latest transaction information.

| Field Name |  Type   |       Note        |
| ---------- | ------- | ----------------- |
| ticker_id  | string  | Contract ID       |
| timestamp  | long    | Base Token        |
| bids       | []      | Bids              |
| --   [0]   | decimal | Price             |
| --   [1]   | decimal | Contract Quantity |
| asks       | []      | Bids              |
| --   [0]   | decimal | Price             |
| --   [1]   | decimal | Contract Quantity |


## remarks

When using our API, please ensure that you adhere to the following guidelines:

* Carefully read and comply with our terms of service and privacy policy.
* Do not attempt to modify, copy, tamper with, crack, damage, or attempt to obtain any part of the source code or any other related information of our API without authorization.
* Ensure that your application complies with applicable laws, regulations, and industry standards and is not used for any illegal purposes.
* Ensure that your application can effectively handle and process all responses and error codes provided by us when using the API.
* If you have any questions or concerns, please contact us through our support email, and we will be happy to assist you.
* Please Description that we reserve the right to update and change these guidelines at any time. If you violate these guidelines, we may suspend or terminate your permission to use the API and reserve the right to take further legal action.

Thank you for using our API, and we look forward to your success in developing applications!