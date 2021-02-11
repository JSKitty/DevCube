## Setting up

Use `https://stakecube.io/api/v2/` as the base of every endpoint.

For example; `GET https://stakecube.io/api/v2/exchange/spot/markets?base=BTC`

## Endpoints list

## :unlock: **Public APIs (No Auth Required)**

### Arbitrage Info
> Returns a list of all market pairs matching your Maker/Taker coin, for example; `ticker=SCC` would return all markets paired with `SCC`.
- Endpoint: `/exchange/spot/arbitrageInfo`

Parameter | Description | Example
------------ | ------------- | -------------
ticker | a coin's ticker | SCC

Example: `GET https://stakecube.io/api/v2/exchange/spot/arbitrageInfo?ticker=SCC`

---

### Markets
> Returns a list of all markets matching your Base coin, for example; `base=BTC` would return all markets paired against `BTC`, you may also add a `orderBy` parameter which allows you to customize how the list is ordered, by default the list is in alphabetical order, for example; `base=BTC&orderBy=volume` would return the list of markets by descending volume.
- Endpoint: `/exchange/spot/markets`

Parameter | Description | Example
------------ | ------------- | -------------
base | a base-coin's ticker | BTC
orderBy | the list's ordering | `volume` or `change`

Example: `GET https://stakecube.io/api/v2/exchange/spot/markets?ticker=BTC&orderBy=volume`

---

### OHLC Charting Data
> Returns raw OHLC chart data for the specified market and interval, for example; `market=SCC_BTC&interval=1h` would return 1h-per-bar timeframe data on the SCC_BTC market.
- Endpoint: `/exchange/spot/ohlcData`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
interval | the chosen time-period | 1h

**NOTE:** `interval` is limited to the below options:
- 1m
- 5m
- 15m
- 30m
- 1h
- 4h
- 1d
- 1w
- 1mo

Example: `GET https://stakecube.io/api/v2/exchange/spot/ohlcData?market=SCC_BTC&interval=1h`

---

### Orderbook
> Returns orderbook data for a specified market pair and orderbook side, for example; `market=SCC_BTC&side=BUY` would return all `bids` of the `SCC_BTC` market.
- Endpoint: `/exchange/spot/orderbook`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
(optional) side | the chosen orderbook side | `BUY` or `SELL`

**NOTE:** `side` can be left empty and/or removed completely from the query, which will return BOTH sides of the orderbook (Asks + Bids).

Example: `GET https://stakecube.io/api/v2/exchange/spot/orderbook?market=SCC_BTC`

---

### Rate Limits
> Returns rate limit information.
- Endpoint: `/exchange/spot/rateLimits`

Example: `GET https://stakecube.io/api/v2/exchange/spot/rateLimits`

---

### Trades
> Returns the last trades of a specified market pair, for example; 
- Endpoint: `/exchange/spot/trades`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
(optional) limit | the amount of trades to fetch | 100

**NOTE:** `limit` can be left empty and/or removed completely from the query, which will return the last 100 trades.

Example: `GET https://stakecube.io/api/v2/exchange/spot/trades?market=SCC_BTC`

---

## :lock: **Private APIs (Authentication Required)**


For security, our private APIs require two additional `body` fields on the majority of endpoints (with a few exceptions), these fields being:
Field | Description
------------ | -------------
nonce | any integer larger than the last API call's integer (millisecond timestamp)
signature | a HMAC signature of the full `body` contents in URL-encoded format

In addition, your API key should be included as a **header**, under this name: `X-API-KEY`

For new API users, the nonce will start at `0`, you may provide any integer higher than that as the nonce, for example: your nonce is 0, so your next call uses nonce 1, next call uses nonce 2... and repeat. You may also use timestamps for this purpose, but ensure it's a millisecond timestamp, otherwise a second-based timestamp limits you to a call every second maximum.

The HMAC signature is composed of all the URL-encoded parameters of your API call's body, so for example; you're calling `/exchange/spot/order` which contains the `body` parameters of: `"nonce=123&market=SCC_BTC&side=BUY&price=0.00010000&amount=10"`, you would sign ALL of this exact input, digest as HEX, and append the signature as the parameter: `signature=` at the end of your request.

### Account
> Returns general information about your StakeCube account, including wallets, balances, fee-rate in percentage and your account username.
- Endpoint: `/user/account`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
nonce | the incremental integer | UNIX Timestamp
signature | the parameter's HMAC signature | HEX Format

Example: `GET https://stakecube.io/api/v2/user/account?nonce=123&signature=xxx`

---

### My Trades
> Returns a list of your last trades in a selected market, descending by timestamp, you may set a custom limit to the amount of returned trades, for example: `market=SCC_BTC&limit=100` returns your last 100 trades on the SCC_BTC market.
- Endpoint: `/exchange/spot/myTrades`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
(optional) limit | the maximum trades to return | 100
nonce | the incremental integer | UNIX Timestamp
signature | the parameter's HMAC signature | HEX Format

**Note:** `limit` can be left empty and/or removed completely from the query, which will use `100` as the default limit.

Example: `GET https://stakecube.io/api/v2/exchange/spot/myTrades?market=SCC_BTC&limit=100&nonce=123&signature=xxx`

---

### My Order History
> Returns a list of your historical orders of a chosen market and limit, descending by timestamp, for example: `market=SCC_BTC&limit=100` returns your last 100 orders done on the SCC_BTC market.
- Endpoint: `/exchange/spot/myOrderHistory`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
(optional) limit | the maximum trades to return | 100
nonce | the incremental integer | UNIX Timestamp
signature | the parameter's HMAC signature | HEX Format

**Note:** `limit` can be left empty and/or removed completely from the query, which will use `100` as the default limit.

Example: `GET https://stakecube.io/api/v2/exchange/spot/myOrderHistory?market=SCC_BTC&limit=100nonce=123&signature=xxx`

---

### Order
> Creates an exchange limit order on the chosen market, side, price and amount, for example: `market=SCC_BTC&side=BUY&price=0.00010000&amount=10` would buy 10 SCC with BTC at a price of 10k sats per SCC.
- Endpoint: `/exchange/spot/order`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
side | the market side to place the order | `BUY` or `SELL`
price | the price in the Base coin | `0.00010000` (BTC)
amount | the amount in the Market coin | `10` (SCC)
nonce | the incremental integer | UNIX Timestamp
signature | the parameter's HMAC signature | HEX Format

**Note:** The `side` must be in full caps, e.g: `BUY` or `SELL`, not `buy` or `sell`.

Example: `POST https://stakecube.io/api/v2/exchange/spot/order`

POST Body: `market=SCC_BTC&side=BUY&price=0.00010000&amount=10&nonce=123&signature=xxx`

---

### Cancel
> Cancels an order by it's unique ID, for example; You place an order, it's returned ID is 123, you then cancel with the parameters: `orderId=123` - the order is cancelled and removed from it's market.
- Endpoint: `/exchange/spot/cancel`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
orderId | the order's unique ID | 123
nonce | the incremental integer | UNIX Timestamp
signature | the parameter's HMAC signature | HEX Format

Example: `POST https://stakecube.io/api/v2/exchange/spot/cancel`

POST Body: `orderId=123&nonce=123&signature=xxx`

---

### Cancel All
> Cancels all orders in a chosen market pair, e.g: `market=SCC_BTC` will cancel ALL limit orders on the SCC_BTC market pair.
- Endpoint: `/exchange/spot/cancelAll`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
market | the chosen market pair | SCC_BTC
nonce | the incremental integer | UNIX Timestamp
signature | the parameter's HMAC signature | HEX Format

Example: `POST https://stakecube.io/api/v2/exchange/spot/cancelAll`

POST Body: `market=SCC_BTC&nonce=123&signature=xxx`
