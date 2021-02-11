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
limit | the amount of trades to fetch | 100

**NOTE:** `limit` can be left empty and/or removed completely from the query, which will return the last 100 trades.

Example: `GET https://stakecube.io/api/v2/exchange/spot/trades?market=SCC_BTC`
