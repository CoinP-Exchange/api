# CoinP OPEN API

## Getting Started

**Welcome to the CoinP documentation center. CoinP provides an
easy-to-use API interface, it allows traders operate in trading markets
by using automated 3rd-party trading application.

### Apply API Key

In a signuped account of **[CoinP]
(**[[https://www.coinp.com]{.underline}](https://www.coinp.com/)**)**,
user can create an API Key in [API Management], and get a set of
randomly generated API Key and Secret after creation. The Key, used by
in your applications' authentication to trading.

**Please do NOT disclose the API Key and Secret Key to protect your
assets. It is recommended that users bind IP addresses for the API. Each
key is bound to a maximum of 4 IPs, separated by commas.**

### Interface call mode description

-   REST API

Provide market query, balance query, currency transaction, order
management functions, users are recommended to use REST API for account
balance query, currency transaction and order management

### server

-   The CoinP server runs in Tokyo. In order to minimize the delay of
    > API access, it is recommended to use a server with smooth
    > communication with Tokyo.

## REST API

### Access URL

- [Main
URL: [[https://market.coinp.com]{.underline}](https://market.coinp.com/)]{.mark}

- Backup
URL: [[https://www.coinp.com]{.underline}](https://www.coinp.com/) ,
when the Main URL is not avaliable, try to use the Backup URL.

### Request interaction

#### Intro

REST API Provide market inquiry, balance inquiry, currency trading,
order management functions

All requests are based on the Https protocol, and the content-type in
the request header needs to be uniformly set to the form format:

- **content-type:application/x-www-form-urlencoded**

#### error code

  -----------------------------------------------------------------------
  **Error     **Description**                      **Resaon**
  code**                                           
  ----------- ------------------------------------ ----------------------
  0           Success                              

  1           Invalid parameter                    

  2           Internal error                       

  3           Service unavailable                  

  4           Method Not Found                     

  5           Service timeout                      

  10          Insufficient amount                  

  11          Number of transactions is too small  

  12          Insufficient depth                   

                                                   

  10005       Record Not Found                     X-SITE-ID, Setting
                                                   Error

  10022       Failed real-name authentication      

  10051       User forbids trading                 

  10056       Less than minimum amount             

  10059       The asset has not been opened for    
              trading yet                          

  10060       This trading pair has not yet opened 
              trading                              

  10062       Inaccurate amount accuracy           
  -----------------------------------------------------------------------

### Attention

-All interface requests (public and private interfaces) must add the
X-SITE-ID field in the header of the Request request. The value of this
field is "127". This field is not required for signature verification.
-If security requires the use of SHA256 and the request URL contains
"/private/", please read README_v2.md

## Market API public interface

### Get Exchange Market Data

Get /api/v1/tickers

Frequency limit: 10 times / s

## example

Request:

GET https://www.coinp.com/api/v1/tickers

Response:

{

"ticker":[

​{

​"buy":"0.378",

​"high":"0.39999995",

​"last":"0.388",

​"low":"0.374101",

​"sell":"0.387",

​"symbol":"BTC_USDT",

​"vol":"3485328.1114718"

​},

​{

​"buy":"1924",

​"high":"1938.84",

​"last":"1924",

​"low":"1864.97",

​"sell":"1926",

​"symbol":"ETH_USDT",

​"vol":"2948.19477569"

​}

],

"timestamp":"1535452275851"

}

### respond

timestamp: time

buy: BID

high: high price

last: last price

low: low price

sell: ASK

vol: volume (24h volume)

## Get Depth Information

Get /api/v1/depth

Frequency limit: 10 times / s

### parameter

  -------------------------------------------------------------------------
  **Parameter**       **required**      **type**      **Description**
  ------------------- ----------------- ------------- ---------------------
  symbol              true              string        BTC_USDT

  size                true              string        1 ~ 100
  -------------------------------------------------------------------------

### example

Request:

GET https://www.coinp.com/api/v1/depth?symbol=BTC_USDT

Response:

{

"asks":[

​["0.387","43189.58824906"]

],

"bids":[

​["0.378","2078.91534391"]

]

}

### respond

asks : ask[price，amount]

bids : bid[price，amount]

### Get Recent Trades

Get /api/v1/trades

Frequency limit: 10 times / s

### parameter

  -------------------------------------------------------------------------
  **Parameter**       **required**      **type**      **Description**
  ------------------- ----------------- ------------- ---------------------
  symbol              true              string        BTC_USDT

  size                true              string        1 ~ 100
  -------------------------------------------------------------------------

### example

Request:

GET https://www.coinp.com/api/v1/trades?symbol=BTC_USDT&size=1

Response:

{

​"amount":"500",

​"price":"0.401",

​"side":"sell",

​"timestamp":"1535507624521"

},

{

​"amount":"442",

​"price":"0.401",

​"side":"sell",

​"timestamp":"1535507612055"

}

]

### Response

amount : amount

price : price

side: buy/sell

timestamp: time

## Get K-line Info

Get /api/v1/kline

Frequency limit: 10 times / s

### parameter

  -----------------------------------------------------------------------
  **Parameter**      **Description**
  ------------------ ----------------------------------------------------
  symbol             BTC_USDT

  type               1min,5min,15min,30min,hour,day,week

  size               1 ~ 100
  -----------------------------------------------------------------------

### example

Request:

GET https://www.coinp.com/api/v1/kline?symbol=BTC_USDT&type=1min&size=10

Response:

[

[

​1535508060000,

​"0.401",

​"0.401",

​"0.401",

​"0.401",

​"0"

],

[

​1535508120000,

​"0.401",

​"0.401",

​"0.401",

​"0.401",

​"0"

],

[

​1535508180000,

​"0.401",

​"0.401",

​"0.401",

​"0.401",

​"0"

]

...

]

### Response

1532655300000: time

2370.16: open

2380: high

2352: low

2367.37: close

17259.83: volume

## Get Pair Info

Get /api/v1/exchangeInfo

Frequency limit: 10 times / s

### example

Request:

GET https://www.coinp.com/api/v1/exchangeInfo

Response:

[

{

​"baseAsset":"BTC",

​"baseAssetPrecision":8,

​"quoteAsset":"USDT",

​"quoteAssetPrecision":8,

​"status":"trading",

​"symbol":"BTC_USDT"

},

{

​"baseAsset":"ETH",

​"baseAssetPrecision":8,

​"quoteAsset":"USDT",

​"quoteAssetPrecision":8,

​"status":"trading",

​"symbol":"ETH_USDT"

}

]

### Response

baseAsset: baseAsset

baseAssetPrecision: baseAssetPrecision

quoteAsset: quoteAsset

quoteAssetPrecision: quoteAssetPrecision

status: status

symbol: pair

# Trading API private interface

-   All private interface requests use the POST method, and the
    > parameters are submitted in the form-data

## Signature Authentication

-   All parameters must be verified by signature, and all parameters
    > must be sorted according to the alphabet according to the
    > parameter name

### example

para：

'market':'BTC_USDT',

'side':1,

'price':'50',

'amount':'0.02'

Parameter string:

amount=0.02&api_key=apiKey&market=BTC_USDT&price=50&side=1

Note: The secretKey must be generated to generate the MD5 signature. Add
the secret_key to the generated string to generate the final string.

Final signature string

:amount=0.02&api_key=apiKey&market=BTC_USDT&price=50&side=1&secret_key=secretKey

MD5 signature：

Use 32bit MD5 encrypted string, the generated encrypted string must be
capitalized

Then use the generated encrypted string as the value of sign parameter

Final HTTP request parameter example:

amount=0.02&api_key=apiKey&market=BTC_USDT&price=50&side=1&sign=MD5SignatureString

## Get User Assets

POST /api/v1/private/user

Frequency limit: 20 times / s

### example

Request:

POST https://www.coinp.com/api/v1/private/user

Response:

{

"code": 0,

"message": "successful",

"result": {

"USDT": {

"available": "10718.74453852",

"freeze": "0.10999996",

"other_freeze": "0",

"recharge_status": 0,

"trade_status": 1,

"withdraw_fee": "0.1",

"withdraw_max": "1000",

"withdraw_min": "0.001",

"withdraw_status": 1

},

"ETH": {

"available": "395.196",

"freeze": "0",

"other_freeze": "0",

"recharge_status": 1,

"trade_status": 1,

"withdraw_fee": "0.01",

"withdraw_max": "100000",

"withdraw_min": "0.001",

"withdraw_status": 1

},

"BTC": {

"available": "46.20370336",

"freeze": "0",

"other_freeze": "0",

"recharge_status": 0,

"trade_status": 1,

"withdraw_fee": "0.1",

"withdraw_max": "100000",

"withdraw_min": "1",

"withdraw_status": 1

}

}

}

### Response

available: available

freeze: freeze

other_freeze: other_freeze（withdraw freeze）

recharge_status: Recharge status. 0 is not rechargeable, 1 is
rechargeable

withdraw_fee: withdraw_fee

withdraw_max: withdraw_max

withdraw_min: withdraw_min

withdraw_status: Withdrawal status 0 means no withdrawal, 1 means
withdrawal

## Limit Trading

POST /api/v1/private/trade/limit

Frequency limit: 20 times / s

### para

  -----------------------------------------------------------------------
  **para**                  **desc**
  ------------------------- ---------------------------------------------
  market                    BTC_USDT

  side                      1 : ASK，2 : BID

  amount                    amount

  price                     price
  -----------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/trade/limit

Response:

{

"code": 0,

"message": "successful",

"result": {

"amount": "1",

"ctime": 1535537926.246487,

"deal_fee": "0",

"deal_money": "0",

"deal_stock": "0",

"id": 32865,

"left": "1",

"maker_fee": "0.001",

"market": "BTC_USDT",

"mtime": 1535537926.246487,

"price": "10",

"side": 2,

"source": "web,1",

"taker_fee": "0.001",

"type": 1,

"user": 670865

}

}

### Response

amount: amount

ctime: creat time

deal_fee: deal fee

deal_money: deal money

deal_stock: deal stock

id: id

left: left

maker_fee: maker fee

market: market

mtime: publish to market time

price: price

side: 1:ASK，2:BID

source:source

taker_fee: taker fee

type: trading type，1:limit，2:market

user: userid

## Market Trading

POST /api/v1/private/trade/market market trading

Frequency limit: 20 times / s

### para

  -----------------------------------------------------------------------
  **para**                  **desc**
  ------------------------- ---------------------------------------------
  market                    BTC_USDT

  side                      1 : ASK，2 : BID

  amount                    amount
  -----------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/trade/market

Response:

{

"code": 0,

"message": "successful",

"result": {

"amount": "1",

"ctime": 1535538409.189721,

"deal_fee": "0.00019607843",

"deal_money": "0.999999993",

"deal_stock": "0.19607843",

"id": 32868,

"left": "7.0000000e-9",

"maker_fee": "0",

"market": "BTC_USDT",

"mtime": 1535538409.189735,

"price": "0",

"side": 2,

"source": "web,1",

"taker_fee": "0.001",

"type": 2,

"user": 670865

}

}

### Response

amount: amount

ctime: creat time

deal_fee: deal fee

deal_money: deal money

deal_stock: deal stock

id: id

left: left

maker_fee: maker fee

market: market

mtime: publish to market time

price: price

side: 1:ASK，2:BID

source:source

taker_fee: taker fee

type: trading type，1:limit，2:market

user: userid

## Cancel an Order

POST /api/v1/private/trade/cancel

Frequency limit: 20 times / s

### para

  -----------------------------------------------------------------------
  **para**                        **desc**
  ------------------------------- ---------------------------------------
  market                          BTC_USDT

  order_id                        order id
  -----------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/trade/cancel

Response:

{

"code": 0,

"message": "successful",

"result": {

"amount": "1",

"ctime": 1535538409.189721,

"deal_fee": "0.00019607843",

"deal_money": "0.999999993",

"deal_stock": "0.19607843",

"id": 32868,

"left": "7.0000000e-9",

"maker_fee": "0",

"market": "BTC_USDT",

"mtime": 1535538409.189735,

"price": "0",

"side": 2,

"source": "web,1",

"taker_fee": "0.001",

"type": 2,

"user": 670865

}

}

### Response

amount: amount

ctime: creat time

deal_fee: deal fee

deal_money: deal money

deal_stock: deal stock

id: id

left: left

maker_fee: maker fee

market: market

mtime: publito market time

price: price

side: 1:ASK，2:BID

source:source

taker_fee: taker fee

type: trading type，1:limit，2:market

user: userid

## Bulk Cancel Order

POST /api/v1/private/trade/cancel_batch The number of orders cancelled
in batches each time does not exceed 10.

Frequency limit: 20 times / S

### para

  ---------------------------------------------------------------------------------------------------
  **para**      **desc**
  ------------- -------------------------------------------------------------------------------------
  orders_json   [{"market":"XX_USDT","order_id":1},{"market":"XX_USDT","order_id":2}]

  sign          sign

  api_key       api_key
  ---------------------------------------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/trade/cancel_batch

Response:

{

"code": 0,

"message": "successful",

"result": [

{

"market": "BTC_USDT",

"order_id": 458815,

"result": true

},

{

"market": "BTC_USDT",

"order_id": 458813,

"result": true

},

{

"market": "BTC_USDT",

"order_id": 458812,

"result": false

}

]

}

### Response

market: market

order_id: order id

result: true:successful，false:fail)

## Query Order Transaction Interface

POST /api/v1/private/order/deals

Frequency limit: 20 times / s

### para

  -----------------------------------------------------------------------
  **para**                             **desc**
  ------------------------------------ ----------------------------------
  order_id                             order id

  offset                               0

  limit                                1 ~ 100
  -----------------------------------------------------------------------

### example

# Request

POST https://www.coinp.com/api/v1/private/order/deals

# Response

{

"code": 0,

"message": "successful",

"result": {

"limit": 20,

"offset": 0,

"records": [

{

​"amount": "1",

​"deal": "19.96",

​"deal_order_id": 32730,

​"fee": "0.001",

​"id": 25503,

​"price": "19.96",

​"role": 2,

​"time": 1535437951.751402,

​"user": 670865

}

]

}

}

### Response

limit: limit

offset: offset

records: records

amount: amount

deal: deal

deal_order_id: deal order id

fee: fee

id: deal id

price: price

role:，1:Maker,2:Taker

time: time

user: user id

## Query Unfilled Orders

POST /api/v1/private/order/pending

Frequency limit: 20 times / s

### para

  -----------------------------------------------------------------------
  **para**                      **desc**
  ----------------------------- -----------------------------------------
  market                        BTC_USDT

  offset                        0

  limit                         1 ~ 100
  -----------------------------------------------------------------------

### example

# Request

POST https://www.coinp.com/api/v1/private/order/pending

# Response

{

"code": 0,

"message": "successful",

"result": {

"limit": 10,

"offset": 0,

"records": [

{

​"amount": "1",

​"ctime": 1535544362.168106,

​"deal_fee": "0",

​"deal_money": "0",

​"deal_stock": "0",

​"id": 32871,

​"left": "1",

​"maker_fee": "0.001",

​"market": "BTC_USDT",

​"mtime": 1535544362.168106,

​"price": "5.1",

​"side": 2,

​"source": "web,1",

​"taker_fee": "0.001",

​"type": 1,

​"user": 670865

}

],

"total": 1

}

}

### Response

amount: amount

ctime: create time

deal_fee: deal fee

deal_money: deal money

deal_stock:deal stock

id: id

left: left

maker_fee: maker fee

market: market

mtime: publish to market time

price: price

side: 1:ASK，2:BID

source:source

taker_fee: taker fee

type: trading time，1:limit，2:market

user: user id

## Query Details Of An Unfilled Order

POST /api/v1/private/order/pending/detail

Frequency limit: 20 times / s

### parme

  -----------------------------------------------------------------------
  **para**                         **desc**
  -------------------------------- --------------------------------------
  market                           BTC_USDT

  Order_id                         order id
  -----------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/order/pending/detail

Response:

{

"code": 0,

"message": "successful",

"result": {

"amount": "10",

"ctime": 1565681852.879657,

"deal_fee": "0",

"deal_money": "0",

"deal_stock": "0",

"id": 1080,

"left": "10",

"maker_fee": "0",

"market": "BTC_USDT",

"mtime": 1565681852.879657,

"price": "1"

"side": 2,

"source": "web,127",

"taker_fee": "0",

"type": 1,

"user": 2

}

}

### Response

amount: amount

ctime: create time

deal_fee: deal fee

deal_money: deal money

deal_stock: deal stock

id: id

left: left

maker_fee: maker fee

market: market

ftime: publish to market time

price: price

side: 1:ASK,2:BID

source: source

taker_fee: taker fee

type: trading type,1:limit,2:market

user: user id

## Querying the Completed Order

POST /api/v1/private/order/finished

Frequency limit: 500 times / s

### para

  -----------------------------------------------------------------------
  **para**                         **desc**
  -------------------------------- --------------------------------------
  market                           BTC_USDT

  start_time                       start time

  end_time                         end time

  offset                           0

  limit                            1 ~ 100

  side                             1:ASK 2:BID
  -----------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/order/finished

Response:

{

"code": 0,

"message": "example",

"result": {

"limit": 2,

"offset": 0,

"records": [

{

​"amount": "1",

​"ctime": 1535538409.189721,

​"deal_fee": "0.00019607843",

​"deal_money": "0.999999993",

​"deal_stock": "0.19607843",

​"ftime": 1535538409.189735,

​"id": 32868,

​"maker_fee": "0",

​"market": "BTC_USDT",

​"price": "0",

​"side": 2,

​"source": "web,1",

​"taker_fee": "0.001",

​"type": 2,

​"user": 670865

},

{

​"amount": "10",

​"ctime": 1535538403.233823,

​"deal_fee": "0.001109999955",

​"deal_money": "1.109999955",

​"deal_stock": "0.21764705",

​"ftime": 1535538409.189735,

​"id": 32867,

​"maker_fee": "0.001",

​"market": "BTC_USDT",

​"price": "5.1",

​"side": 1,

​"source": "web,1",

​"taker_fee": "0.001",

​"type": 1,

​"user": 670865

}

]

}

}

### Response

amount: amount

ctime: creat time

deal_fee: deal fee

deal_money: deal money

deal_stock: deal stock

id: id

left: left

maker_fee: maker fee

market: market

ftime: finish time

price: price

side: 1:ASK，2:BID

source:source

taker_fee: taker fee

type: trading type，1:limit，2:market

user: user id

## Query Details Of An Unfilled Order

POST /api/v1/private/order/finished/detail

Frequency limit: 20 times / s

### para

  -----------------------------------------------------------------------
  **para**                             **desc**
  ------------------------------------ ----------------------------------
  order_id                             order id

  -----------------------------------------------------------------------

### example

Request:

POST https://www.coinp.com/api/v1/private/order/finished/detail

Response:

{

"code": 0,

"message": "successful",

"result": {

"amount": "10",

"ctime": 1565681925.295415,

"deal_fee": "0",

"deal_money": "19.5",

"deal_stock": "10",

"ftime": 1565681925.295421,

"id": 1081,

"maker_fee": "0",

"market": "BTC_USDT",

"price": "2",

"side": 2,

"source": "web,127",

"taker_fee": "0",

"type": 1,

"user": 2

}

}

### Response

amount: amount

ctime: create time

deal_fee: deal fee

deal_money: deal money

deal_stock: deal stock

id: id

left: left

maker_fee: maker fee

market: market

ftime: finish time

price: price

side: 1:ASK，2:BID

source:source

taker_fee: taker fee

type: trading type，1:limit，2:market

user: user id

## WebSocket Market Streams

-   The base endpoint
    > is: **wss://**[[www.coinp.com/ws]{.underline}](http://www.coinp.com/ws)

-   Streams can be subscribed in a single raw stream

-   The format subscribing to wss is unified, including method, params
    > and id, {method: "", params: [], id: 5719}, and the parameter
    > of id should not be same in singal individual socket.

-   client should send ping packet every 3 minutes, the format of the
    > ping packet is
    > {"method":"server.ping","params":[],"id":5160}, the
    > format of the response packet is {"error": null, "result":
    > "pong", "id": 5160}

### Depth Stream

#### Subscribe Depth Stream

Parameters:

​method: depth.subscribe

​params:

​symbol -- BTC_USDT

​depth -- depth length, currently supported are in the lists, [5, 10,
15, 20, 30, 50, 100]

​precision -- price precision, currently supported precisions are in the
lists, [0, 0.1, 0.01, 0.001, 0.0001, 0.00001, 0.000001, 0.0000001,
0.00000001]

Example:

​request:

​
{"method":"depth.subscribe","params":["BTC_USDT",50,"0.01"],"id":2066}

​response:

​{"error": null, "result": {"status": "success"}, "id": 2066}

#### Unsubscribe Depth Stream

Parameters:

​method: depth.unsubscribe

​params: []

Example:

​request:

​{"method":"depth.unsubscribe","params":[],"id":2067}

​response:

​{"error": null, "result": {"status": "success"}, "id": 2067}

#### Depth Push Event

Parameters:

​method: depth.update

​params:

​is_full: true or false

​asks: ask order lists

​bids: bid order lists

​id: null

Example Event:

​{"method": "depth.update", "params": [true, {"asks":
[["21505.92", "0.02"], ["21505.93", "0.09"]...], "bids
": [["21505.21", "0.01"], ["21505.2", "0.22"]...]},
"BTC_USDT"], "id": null}

​{"method": "depth.update", "params": [false, {"bids":
[["21505.93", "0.03"]]}, "BTC_USDT"], "id": null}

### Kline Stream

#### Subscribe Kline Stream

Parameters:

​method: kline.subscribe

​params:

​symbol -- BTC_USDT

​interval -- the time interval of kline in seconds, such as 15 minutes
set to 900, and 1 day set to 86400

Example:

​Request:

​
{"method":"kline.subscribe","params":["BTC_USDT",900],"id":2068}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2068}

#### Unsubscribe Kline Stream

Parameters:

​method: kline.unsubscribe

​params: []

Example:

​Request:

​{"method":"kline.unsubscribe","params":[],"id":2069}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2069}

#### Kline Push Event

Parameters:

​method: kline.update

​params:

​The fields in params are: timestamp, opening price, closing price,
highest price, lowest price, volume, deal, pair name

​id: null

Example Event:

​{"method": "kline.update", "params": [[1660924800, "21444.54",
"21445.84", "21447.87", "21437.21", "21.74", "466178.2626",
"BTC_USDT"]], " id": null}

### Deal Stream

#### Subscribe Deal Stream

Parameters:

​method: deals.subscribe

​params:

​symbol -- BTC_USDT

Example:

​Request:

​
{"method":"deals.subscribe","params":["BTC_USDT"],"id":2070}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2070}

#### Unsubscribe Deal Stream

Parameters:

​method: deals.unsubscribe

​params: []

Example:

​Request:

​{"method":"deals.unsubscribe","params":[],"id":2071}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2071}

#### Deal Push Event

Parameters:

​method: deals.update

​params:

​The fields in params are: pair name, amount, timestamp, transaction ID,
transaction type, price

​id: null

Example Event:

​{"method": "deals.update", "params": ["BTC_USDT",
[{"amount": "0.2", "time": 1660924893.1702139, "id": 848000900,
"type": "buy", "price": "21446.35"}, {"amount": "0.03",
"time": 1660924893.1698799, "id": 848000899, "type": "buy",
"price": "21446.34"}]], "id": null}

​{"method": "deals.update", "params": ["BTC_USDT",
[{"amount": "0.02", "time": 1660924901.0485499, "id":
848002029, "type": "sell", "price": "21445.84"}]], "id":
null}

### Last Price Stream

#### Subscribe Last Price Stream

Parameters:

​method: price.subscribe

​params:

​symbols -- list of the pair, ["BTC_USDT","ETH_USDT"...]

Example:

​Request:

{"method":"price.subscribe","params":["BTC_USDT","ETH_USDT"],"id":2072}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2072}

#### Unsubscribe Last Price Stream

Parameters:

​method: price.unsubscribe

​params: []

Example:

​Request:

​{"method":"price.unsubscribe","params":[],"id":2073}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2073}

#### Last Price Push Event

Parameters:

​method: price.update

​params:

​The fields in params are: pair name, last price

​id: null

Example Event:

​{"method": "price.update", "params": ["BTC_USDT",
"21446.34"], "id": null}

### 24hour Market State Stream

#### Subscribe 24hour Market State Stream

Parameters:

​method: state.subscribe

​params:

​symbols -- list of the pair, ["BTC_USDT","ETH_USDT"...]

Example:

​Request:

{"method":"state.subscribe","params":["BTC_USDT","ETH_USDT"],"id":2074}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2074}

#### Unsubscribe 24hour Market State Stream

Parameters:

​method: state.unsubscribe

​params: []

Example:

​Request:

​{"method":"state.unsubscribe","params":[],"id":2075}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2075}

#### 24hour Market State Push Event

Parameters:

​method: state.update

​params:

​The fields in params are: pair name, period, last price, opening price,
closing price, highest price, lowest price, volume, deal

​id: null

Example Event:

​{"method": "state.update", "params": ["BTC_USDT", {"period":
86400, "last":"23728.7", "open": "23901.05", "close":
"23902.11", "high": "24411.64", "low": "23727.7", "volume":
"25664.43", "deal": "614923080.4154"}], "id": null}

### Today Market State Stream

#### Subscribe Today Market State Stream

Parameters:

​method: today.subscribe

​params:

​symbols -- list of the pair, ["BTC_USDT","ETH_USDT"...]

Example:

​Request:

{"method":"today.subscribe","params":["BTC_USDT","ETH_USDT"],"id":2076}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2076}

#### Unsubscribe Today Market State Stream

Parameters:

​method: today.unsubscribe

​params: []

Example:

​Request:

{"method":"today.unsubscribe","params":[],"id":2077}

​Response:

​{"error": null, "result": {"status": "success"}, "id": 2077}

#### Today Market State Push Event

Parameters:

​method: today.update

​params:

​The fields in params are: pair name, lowest price, deal, opening price,
latest price, highest price, volume

​id: null

Example Event:

​{"method": "today.update", "params": ["BTC_USDT", {"low":
"23727.7", "deal": "614923080.4154", "open": "23901.05",
"last": "23769.64", "high": "24411.64", "volume":
"25664.43"}], "id": null}

**CMC API**

**1、https://www.coinp.com/api/v1/cmc/summary**

**response：**

  -------------------------- --------- ------------- ---------------------------------------
  **Name**                   **        **  Status**  **Description**
                             Type**                  

  trading_pairs              string    Mandatory     Identifier of a ticker with delimiter
                                                     to separate base/quote, eg. BTC-USD
                                                     (Price of BTC is quoted in USD)

  base_currency              string    Recommended   Symbol/currency code of base currency,
                                                     eg. BTC

  quote_currency             string    Recommended   Symbol/currency code of quote currency,
                                                     eg. USD

  last_price                 decimal   Mandatory     Last transacted price of base currency
                                                     based on given quote currency

  lowest_ask                 decimal   Mandatory     Lowest Ask price of base currency based
                                                     on given quote currency

  highest_bid                decimal   Mandatory     Highest bid price of base currency
                                                     based on given quote currency

  base_volume                decimal   Mandatory     24-hr volume of market pair denoted in
                                                     BASE currency

  quote_volume               decimal   Mandatory     24-hr volume of market pair denoted in
                                                     QUOTE currency

  price_change_percent_24h   decimal   Mandatory     24-hr % price change of market pair

  highest_price_24h          decimal   Mandatory     Highest price of base currency based on
                                                     given quote currency in the last 24-hrs

  lowest_price_24h           decimal   Mandatory     Lowest price of base currency based on
                                                     given quote currency in the last 24-hrs
  -------------------------- --------- ------------- ---------------------------------------

**response demo：**

**{**

**"code": 200,**

**"message": "success",**

**"data": [**

**{**

**"base_currency": "ETH",**

**"base_volume": 45868.4802,**

**"highest_bid": 1874.48,**

**"highest_price_24h": 1956.26,**

**"last_price": 1875.27,**

**"lowest_ask": 1874.49,**

**"lowest_price_24h": 1831.02,**

**"price_change_percent_24h": "-1.95",**

**"quote_currency": "USDT",**

**"quote_volume": 86811598.168002,**

**"trading_pairs": "ETH_USDT"**

**},**

**{**

**"base_currency": "BTC",**

**"base_volume": 3219.64956,**

**"highest_bid": 83306.82,**

**"highest_price_24h": 84428.16,**

**"last_price": 83294.76,**

**"lowest_ask": 83306.86,**

**"lowest_price_24h": 80607.65,**

**"price_change_percent_24h": "1.28",**

**"quote_currency": "USDT",**

**"quote_volume": 267016686.3116693,**

**"trading_pairs": "BTC_USDT"**

**},**

**{**

**"base_currency": "XRP",**

**"base_volume": 17583503.0551,**

**"highest_bid": 2.22,**

**"highest_price_24h": 2.27,**

**"last_price": 2.22,**

**"lowest_ask": 2.23,**

**"lowest_price_24h": 2.13,**

**"price_change_percent_24h": "1.83",**

**"quote_currency": "USDT",**

**"quote_volume": 38905836.279668,**

**"trading_pairs": "XRP_USDT"**

**},**

**{**

**"base_currency": "DOGE",**

**"base_volume": 91954132.30238,**

**"highest_bid": 0.17,**

**"highest_price_24h": 0.17,**

**"last_price": 0.17,**

**"lowest_ask": 0.18,**

**"lowest_price_24h": 0.16,**

**"price_change_percent_24h": "0",**

**"quote_currency": "USDT",**

**"quote_volume": 14997390.86272,**

**"trading_pairs": "DOGE_USDT"**

**},**

**{**

**"base_currency": "TRX",**

**"base_volume": 1135069052.43698,**

**"highest_bid": 0.2239,**

**"highest_price_24h": 0.2257,**

**"last_price": 0.2244,**

**"lowest_ask": 0.2246,**

**"lowest_price_24h": 0.22,**

**"price_change_percent_24h": "2",**

**"quote_currency": "USDT",**

**"quote_volume": 252747151.7249056,**

**"trading_pairs": "TRX_USDT"**

**},**

**{**

**"base_currency": "LINK",**

**"base_volume": 448287.0679,**

**"highest_bid": 13.17,**

**"highest_price_24h": 13.82,**

**"last_price": 13.18,**

**"lowest_ask": 13.18,**

**"lowest_price_24h": 12.77,**

**"price_change_percent_24h": "0.77",**

**"quote_currency": "USDT",**

**"quote_volume": 5957831.983348,**

**"trading_pairs": "LINK_USDT"**

**},**

**{**

**"base_currency": "BNB",**

**"base_volume": 27505.3084,**

**"highest_bid": 577.9,**

**"highest_price_24h": 582.2,**

**"last_price": 578.1,**

**"lowest_ask": 578,**

**"lowest_price_24h": 548.3,**

**"price_change_percent_24h": "4.13",**

**"quote_currency": "USDT",**

**"quote_volume": 15538191.584049,**

**"trading_pairs": "BNB_USDT"**

**},**

**{**

**"base_currency": "SOL",**

**"base_volume": 273978.3075,**

**"highest_bid": 124.22,**

**"highest_price_24h": 131.29,**

**"last_price": 124.24,**

**"lowest_ask": 124.23,**

**"lowest_price_24h": 122.9,**

**"price_change_percent_24h": "0.75",**

**"quote_currency": "USDT",**

**"quote_volume": 34431660.112006,**

**"trading_pairs": "SOL_USDT"**

**},**

**{**

**"base_currency": "ADA",**

**"base_volume": 18903450.9988,**

**"highest_bid": 0.72,**

**"highest_price_24h": 0.76,**

**"last_price": 0.71,**

**"lowest_ask": 0.73,**

**"lowest_price_24h": 0.71,**

**"price_change_percent_24h": "-2.74",**

**"quote_currency": "USDT",**

**"quote_volume": 13804909.164911,**

**"trading_pairs": "ADA_USDT"**

**},**

**{**

**"base_currency": "SUI",**

**"base_volume": 4558265.3645,**

**"highest_bid": 2.24,**

**"highest_price_24h": 2.37,**

**"last_price": 2.23,**

**"lowest_ask": 2.25,**

**"lowest_price_24h": 2.17,**

**"price_change_percent_24h": "-0.45",**

**"quote_currency": "USDT",**

**"quote_volume": 10311123.913395,**

**"trading_pairs": "SUI_USDT"**

**}**

**]**

**}**

2.  [**https://www.coinp.com/api/v1/cmc/assets**](https://www.coinp.com/api/v1/cmc/assets)

    **response：**

  ------------------------------------------------------------------------------------------------------------------------------ --------- ------------- --------------------------------------------------------------------------------------------------------------------------------------------
  **Name**                                                                                                                       **        **  Status**  **Description**
                                                                                                                                 Type**                  

  name                                                                                                                           string    Recommended   **[Full name]{.underline}** of cryptocurrency.

  [[unified_cryptoasset_id]{.underline}](https://docs.google.com/document/d/1a5JfNE8aXusvfZBnEokwzp1-vGNJ_SPo-jIXhfnnEYE/edit)   integer   Recommended   Unique ID of cryptocurrency assigned by [[Unified Cryptoasset
                                                                                                                                                         ID]{.underline}](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active).

  can_withdraw                                                                                                                   boolean   Recommended   Identifies whether withdrawals are enabled or disabled.

  can_deposit                                                                                                                    boolean   Recommended   Identifies whether deposits are enabled or disabled.

  min_withdraw                                                                                                                   decimal   Recommended   Identifies the single minimum withdrawal amount of a cryptocurrency.

  max_withdraw                                                                                                                   decimal   Recommended   Identifies the single maximum withdrawal amount of a cryptocurrency.

  maker_fee                                                                                                                      decimal   Recommended   Fees applied when liquidity is added to the order book.

  taker_fee                                                                                                                      decimal   Recommended   Fees applied when liquidity is removed from the order book.

  contractAddressUrl                                                                                                             string    Recommended   URL of contract address on **[each]{.underline}** chain

  contractAddress                                                                                                                string    Recommended   Contract address of the asset on **[each]{.underline}** chain
                                                                                                                                                         (e.g. 0x32353a6c91143bfd6c7d363b546e62a9a2489a20)
  ------------------------------------------------------------------------------------------------------------------------------ --------- ------------- --------------------------------------------------------------------------------------------------------------------------------------------

**{**

**"code": 200,**

**"message": "success",**

**"data": {**

**"ADA": {**

**"name": "Cardano",**

**"unified_cryptoasset_id": 4885,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 15,**

**"max_withdraw": 3000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"BNB": {**

**"name": "BNB",**

**"unified_cryptoasset_id": 4237,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 0.005,**

**"max_withdraw": 50,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"BTC": {**

**"name": "Bitcoin",**

**"unified_cryptoasset_id": 5399,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 0.0007,**

**"max_withdraw": 3,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"DOGE": {**

**"name": "Dogecoin",**

**"unified_cryptoasset_id": 3956,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 100,**

**"max_withdraw": 1000000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"ETH": {**

**"name": "Ethereum",**

**"unified_cryptoasset_id": 4945,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 0.02,**

**"max_withdraw": 20,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"LINK": {**

**"name": "LINK",**

**"unified_cryptoasset_id": 4210,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 5,**

**"max_withdraw": 100000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"SOL": {**

**"name": "SOL",**

**"unified_cryptoasset_id": 4314,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 0.1,**

**"max_withdraw": 2000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"SUI": {**

**"name": "Sui",**

**"unified_cryptoasset_id": 5426,**

**"can_withdraw": false,**

**"can_deposit": true,**

**"min_withdraw": 0.01,**

**"max_withdraw": 10000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"TRX": {**

**"name": "TRX",**

**"unified_cryptoasset_id": 4044,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 50,**

**"max_withdraw": 200000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"USDT": {**

**"name": "Tether USD",**

**"unified_cryptoasset_id": 3356,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 120,**

**"max_withdraw": 9999,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**},**

**"XRP": {**

**"name": "Ripple",**

**"unified_cryptoasset_id": 3953,**

**"can_withdraw": true,**

**"can_deposit": true,**

**"min_withdraw": 10,**

**"max_withdraw": 1000000,**

**"maker_fee": 0.002,**

**"taker_fee": 0.002**

**}**

**}**

**}**

3.  [**https://www.coinp.com/api/v1/cmc/ticker**](https://www.coinp.com/api/v1/cmc/ticker)

    **response：**

  --------------------------- --------- ------------- --------------------------------------------------------------------------------------------------------------------------------------------
  **Name**                    **        **Status**    **Description**
                              Type**                  

  base_id                       integer Recommended   The quote pair [[Unified Cryptoasset
                                                      ID]{.underline}](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active).

  quote_id                      integer Recommended   The base pair [[Unified Cryptoasset
                                                      ID]{.underline}](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active).

  last_price                    decimal Mandatory     Last transacted price of base currency based on given quote currency

  base_volume                   decimal Mandatory     24-hour trading volume denoted in BASE currency

  quote_volume                  decimal Mandatory     24 hour trading volume denoted in QUOTE currency

  isFrozen                      integer Recommended   Indicates if the market is currently enabled (0) or disabled (1).

  **Name**                    **        **Status**    **Description**
                              Type**                  

  base_id                       integer Recommended   The quote pair [[Unified Cryptoasset
                                                      ID]{.underline}](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active).

  quote_id                      integer Recommended   The base pair [[Unified Cryptoasset
                                                      ID]{.underline}](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active).

  last_price                    decimal Mandatory     Last transacted price of base currency based on given quote currency
  --------------------------- --------- ------------- --------------------------------------------------------------------------------------------------------------------------------------------

**{**

**"code": 200,**

**"message": "success",**

**"data": {**

**"ADA_USDT": {**

**"base_id": 4885,**

**"base_volume": 18866891.2349,**

**"isFrozen": 0,**

**"last_price": 0.71,**

**"quote_id": 3356,**

**"quote_volume": 13777008.00858**

**},**

**"BNB_USDT": {**

**"base_id": 4237,**

**"base_volume": 27452.1939,**

**"isFrozen": 0,**

**"last_price": 578.4,**

**"quote_id": 3356,**

**"quote_volume": 15509713.969765**

**},**

**"BTC_USDT": {**

**"base_id": 5399,**

**"base_volume": 3201.92756,**

**"isFrozen": 0,**

**"last_price": 83244.06,**

**"quote_id": 3356,**

**"quote_volume": 265567340.2516875**

**},**

**"DOGE_USDT": {**

**"base_id": 3956,**

**"base_volume": 91473874.91511,**

**"isFrozen": 0,**

**"last_price": 0.17,**

**"quote_id": 3356,**

**"quote_volume": 14922006.7702447**

**},**

**"ETH_USDT": {**

**"base_id": 4945,**

**"base_volume": 45722.7506,**

**"isFrozen": 0,**

**"last_price": 1875.74,**

**"quote_id": 3356,**

**"quote_volume": 86529049.34064**

**},**

**"LINK_USDT": {**

**"base_id": 4210,**

**"base_volume": 447594.9533,**

**"isFrozen": 0,**

**"last_price": 13.14,**

**"quote_id": 3356,**

**"quote_volume": 5948863.359292**

**},**

**"SOL_USDT": {**

**"base_id": 4314,**

**"base_volume": 272357.7339,**

**"isFrozen": 0,**

**"last_price": 124.34,**

**"quote_id": 3356,**

**"quote_volume": 34232563.957067**

**},**

**"SUI_USDT": {**

**"base_id": 5426,**

**"base_volume": 4523349.5932,**

**"isFrozen": 0,**

**"last_price": 2.23,**

**"quote_id": 3356,**

**"quote_volume": 10232832.253234**

**},**

**"TRX_USDT": {**

**"base_id": 4044,**

**"base_volume": 1141135062.73301,**

**"isFrozen": 0,**

**"last_price": 0.2236,**

**"quote_id": 3356,**

**"quote_volume": 254107561.2922422**

**},**

**"XRP_USDT": {**

**"base_id": 3953,**

**"base_volume": 17560421.6874,**

**"isFrozen": 0,**

**"last_price": 2.23,**

**"quote_id": 3356,**

**"quote_volume": 38857654.379469**

**}**

**}**

**}**

**4、https://www.coinp.com/api/v1/cmc/trades/ETH_USDT**

Parameters:

  ---------------- ---------- --------------- ---------------------------
  **Name**         **Type**   **Status**      **  Description**

  market_pair      string     Mandatory         A pair such as LTC_BTC.
  ---------------- ---------- --------------- ---------------------------

**response：**

+---------+------+-----+----------------------------------------------+
| *       | **   | *   | **Description**                              |
| *Name** | Ty   | *St |                                              |
|         | pe** | atu |                                              |
|         |      | s** |                                              |
+---------+------+-----+----------------------------------------------+
| t       |      | Man | A unique ID associated with the trade for    |
| rade_id | int  | dat | the currency pair transaction                |
|         | eger | ory |                                              |
|         |      |     |  *Note:* Unix timestamp does not qualify as  |
|         |      |     | trade_id.                                    |
+---------+------+-----+----------------------------------------------+
| price   |      | Man | Last transacted price of base currency based |
|         | dec  | dat | on given quote currency                      |
|         | imal | ory |                                              |
+---------+------+-----+----------------------------------------------+
| base    |      | Man | Transaction amount in BASE currency.         |
| _volume | dec  | dat |                                              |
|         | imal | ory |                                              |
+---------+------+-----+----------------------------------------------+
| quote   |      | Man | Transaction amount in QUOTE currency.        |
| _volume | dec  | dat |                                              |
|         | imal | ory |                                              |
+---------+------+-----+----------------------------------------------+
| ti      | Inte | Man | Unix timestamp in milliseconds for when the  |
| mestamp | ger  | dat | transaction occurred.                        |
|         |      | ory |                                              |
|         | (UTC |     |                                              |
|         | t    |     |                                              |
|         | imes |     |                                              |
|         | tamp |     |                                              |
|         | in   |     |                                              |
|         | ms)  |     |                                              |
+---------+------+-----+----------------------------------------------+
| type    |      | Man | Used to determine whether or not the         |
|         | st   | dat | transaction originated as a buy or sell.     |
|         | ring | ory |                                              |
|         |      |     |   Buy -- Identifies an ask was removed from  |
|         |      |     | the order book.                              |
|         |      |     |                                              |
|         |      |     |   Sell -- Identifies a bid was removed from  |
|         |      |     | the order book.                              |
+---------+------+-----+----------------------------------------------+

**{**

**"code": 200,**

**"message": "success",**

**"data": [**

**{**

**"base_volume": 0.124,**

**"price": 1875.49,**

**"quote_volume": 232.56076,**

**"timestamp": 1741852648469,**

**"trade_id": 20070874,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1062,**

**"price": 1875.49,**

**"quote_volume": 199.177038,**

**"timestamp": 1741852647622,**

**"trade_id": 20070868,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0151,**

**"price": 1875.35,**

**"quote_volume": 28.317785,**

**"timestamp": 1741852645916,**

**"trade_id": 20070854,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0126,**

**"price": 1875.35,**

**"quote_volume": 23.62941,**

**"timestamp": 1741852644506,**

**"trade_id": 20070840,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0399,**

**"price": 1875.21,**

**"quote_volume": 74.820879,**

**"timestamp": 1741852643352,**

**"trade_id": 20070830,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1035,**

**"price": 1875.21,**

**"quote_volume": 194.084235,**

**"timestamp": 1741852642638,**

**"trade_id": 20070826,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0635,**

**"price": 1875.21,**

**"quote_volume": 119.075835,**

**"timestamp": 1741852641623,**

**"trade_id": 20070817,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.2239,**

**"price": 1875.21,**

**"quote_volume": 419.859519,**

**"timestamp": 1741852640817,**

**"trade_id": 20070809,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0386,**

**"price": 1875.31,**

**"quote_volume": 72.386966,**

**"timestamp": 1741852639690,**

**"trade_id": 20070799,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.2539,**

**"price": 1875.49,**

**"quote_volume": 476.186911,**

**"timestamp": 1741852639005,**

**"trade_id": 20070793,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0403,**

**"price": 1875.49,**

**"quote_volume": 75.582247,**

**"timestamp": 1741852638032,**

**"trade_id": 20070786,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0705,**

**"price": 1875.49,**

**"quote_volume": 132.222045,**

**"timestamp": 1741852637481,**

**"trade_id": 20070780,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.018,**

**"price": 1875.49,**

**"quote_volume": 33.75882,**

**"timestamp": 1741852635836,**

**"trade_id": 20070767,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.31,**

**"price": 1875.49,**

**"quote_volume": 581.4019,**

**"timestamp": 1741852634867,**

**"trade_id": 20070760,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0062,**

**"price": 1875.49,**

**"quote_volume": 11.628038,**

**"timestamp": 1741852633124,**

**"trade_id": 20070745,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0412,**

**"price": 1875.49,**

**"quote_volume": 77.270188,**

**"timestamp": 1741852631751,**

**"trade_id": 20070735,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.1004,**

**"price": 1875.49,**

**"quote_volume": 188.299196,**

**"timestamp": 1741852630936,**

**"trade_id": 20070731,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.046,**

**"price": 1875.49,**

**"quote_volume": 86.27254,**

**"timestamp": 1741852629944,**

**"trade_id": 20070722,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.1259,**

**"price": 1875.49,**

**"quote_volume": 236.124191,**

**"timestamp": 1741852629045,**

**"trade_id": 20070712,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0458,**

**"price": 1875.49,**

**"quote_volume": 85.897442,**

**"timestamp": 1741852628276,**

**"trade_id": 20070706,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.7874,**

**"price": 1875.49,**

**"quote_volume": 1476.760826,**

**"timestamp": 1741852626329,**

**"trade_id": 20070688,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0247,**

**"price": 1875.49,**

**"quote_volume": 46.324603,**

**"timestamp": 1741852625693,**

**"trade_id": 20070682,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.1843,**

**"price": 1875.6,**

**"quote_volume": 345.67308,**

**"timestamp": 1741852624365,**

**"trade_id": 20070673,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0175,**

**"price": 1875.82,**

**"quote_volume": 32.82685,**

**"timestamp": 1741852623187,**

**"trade_id": 20070664,**

**"type": "sell"**

**},**

**{**

**"base_volume": 1.0106,**

**"price": 1875.95,**

**"quote_volume": 1895.83507,**

**"timestamp": 1741852622547,**

**"trade_id": 20070658,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0276,**

**"price": 1875.95,**

**"quote_volume": 51.77622,**

**"timestamp": 1741852621249,**

**"trade_id": 20070645,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0584,**

**"price": 1875.95,**

**"quote_volume": 109.55548,**

**"timestamp": 1741852620077,**

**"trade_id": 20070636,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.5183,**

**"price": 1875.95,**

**"quote_volume": 972.304885,**

**"timestamp": 1741852618722,**

**"trade_id": 20070627,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0817,**

**"price": 1875.84,**

**"quote_volume": 153.256128,**

**"timestamp": 1741852617222,**

**"trade_id": 20070612,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0357,**

**"price": 1875.84,**

**"quote_volume": 66.967488,**

**"timestamp": 1741852615865,**

**"trade_id": 20070601,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.5314,**

**"price": 1875.5,**

**"quote_volume": 996.6407,**

**"timestamp": 1741852614313,**

**"trade_id": 20070588,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0367,**

**"price": 1875.64,**

**"quote_volume": 68.835988,**

**"timestamp": 1741852613612,**

**"trade_id": 20070582,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0739,**

**"price": 1875.91,**

**"quote_volume": 138.629749,**

**"timestamp": 1741852612517,**

**"trade_id": 20070571,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.1067,**

**"price": 1875.87,**

**"quote_volume": 200.155329,**

**"timestamp": 1741852611019,**

**"trade_id": 20070560,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0353,**

**"price": 1875.87,**

**"quote_volume": 66.218211,**

**"timestamp": 1741852609759,**

**"trade_id": 20070552,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0683,**

**"price": 1875.75,**

**"quote_volume": 128.113725,**

**"timestamp": 1741852609010,**

**"trade_id": 20070546,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0555,**

**"price": 1875.74,**

**"quote_volume": 104.10357,**

**"timestamp": 1741852607903,**

**"trade_id": 20070533,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0191,**

**"price": 1875.74,**

**"quote_volume": 35.826634,**

**"timestamp": 1741852606679,**

**"trade_id": 20070519,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.3578,**

**"price": 1875.57,**

**"quote_volume": 671.078946,**

**"timestamp": 1741852604386,**

**"trade_id": 20070508,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.029,**

**"price": 1875.57,**

**"quote_volume": 54.39153,**

**"timestamp": 1741852603808,**

**"trade_id": 20070500,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.1956,**

**"price": 1875.66,**

**"quote_volume": 366.879096,**

**"timestamp": 1741852603007,**

**"trade_id": 20070496,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0462,**

**"price": 1875.66,**

**"quote_volume": 86.655492,**

**"timestamp": 1741852601951,**

**"trade_id": 20070490,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0488,**

**"price": 1875.66,**

**"quote_volume": 91.532208,**

**"timestamp": 1741852601129,**

**"trade_id": 20070483,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.3311,**

**"price": 1875.76,**

**"quote_volume": 621.064136,**

**"timestamp": 1741852600104,**

**"trade_id": 20070472,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1632,**

**"price": 1875.96,**

**"quote_volume": 306.156672,**

**"timestamp": 1741852598669,**

**"trade_id": 20070460,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0379,**

**"price": 1875.96,**

**"quote_volume": 71.098884,**

**"timestamp": 1741852597565,**

**"trade_id": 20070451,**

**"type": "sell"**

**},**

**{**

**"base_volume": 2.2838,**

**"price": 1875.96,**

**"quote_volume": 4284.317448,**

**"timestamp": 1741852596665,**

**"trade_id": 20070440,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0568,**

**"price": 1876.11,**

**"quote_volume": 106.563048,**

**"timestamp": 1741852595006,**

**"trade_id": 20070429,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.3978,**

**"price": 1876.11,**

**"quote_volume": 746.316558,**

**"timestamp": 1741852594372,**

**"trade_id": 20070425,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0311,**

**"price": 1876.11,**

**"quote_volume": 58.347021,**

**"timestamp": 1741852593711,**

**"trade_id": 20070418,**

**"type": "buy"**

**},**

**{**

**"base_volume": 3.2777,**

**"price": 1875.75,**

**"quote_volume": 6148.145775,**

**"timestamp": 1741852592396,**

**"trade_id": 20070407,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0481,**

**"price": 1875.49,**

**"quote_volume": 90.211069,**

**"timestamp": 1741852590797,**

**"trade_id": 20070392,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0451,**

**"price": 1875.49,**

**"quote_volume": 84.584599,**

**"timestamp": 1741852590036,**

**"trade_id": 20070385,**

**"type": "sell"**

**},**

**{**

**"base_volume": 1.5728,**

**"price": 1875.49,**

**"quote_volume": 2949.770672,**

**"timestamp": 1741852588390,**

**"trade_id": 20070372,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0235,**

**"price": 1875.32,**

**"quote_volume": 44.07002,**

**"timestamp": 1741852587356,**

**"trade_id": 20070362,**

**"type": "sell"**

**},**

**{**

**"base_volume": 10.5296,**

**"price": 1875.3,**

**"quote_volume": 19746.15888,**

**"timestamp": 1741852586579,**

**"trade_id": 20070354,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0118,**

**"price": 1875.36,**

**"quote_volume": 22.129248,**

**"timestamp": 1741852585978,**

**"trade_id": 20070350,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0301,**

**"price": 1874,**

**"quote_volume": 56.4074,**

**"timestamp": 1741852584780,**

**"trade_id": 20070343,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0345,**

**"price": 1874,**

**"quote_volume": 64.653,**

**"timestamp": 1741852584231,**

**"trade_id": 20070341,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0212,**

**"price": 1874,**

**"quote_volume": 39.7288,**

**"timestamp": 1741852582819,**

**"trade_id": 20070333,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.4541,**

**"price": 1874,**

**"quote_volume": 850.9834,**

**"timestamp": 1741852582133,**

**"trade_id": 20070324,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.4181,**

**"price": 1874,**

**"quote_volume": 783.5194,**

**"timestamp": 1741852581114,**

**"trade_id": 20070313,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.056,**

**"price": 1874,**

**"quote_volume": 104.944,**

**"timestamp": 1741852579645,**

**"trade_id": 20070302,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.2883,**

**"price": 1874,**

**"quote_volume": 540.2742,**

**"timestamp": 1741852578996,**

**"trade_id": 20070295,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.5394,**

**"price": 1874.27,**

**"quote_volume": 1010.981238,**

**"timestamp": 1741852577701,**

**"trade_id": 20070282,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0449,**

**"price": 1874.56,**

**"quote_volume": 84.167744,**

**"timestamp": 1741852575948,**

**"trade_id": 20070272,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1653,**

**"price": 1874.94,**

**"quote_volume": 309.927582,**

**"timestamp": 1741852574657,**

**"trade_id": 20070262,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0363,**

**"price": 1874.94,**

**"quote_volume": 68.060322,**

**"timestamp": 1741852573910,**

**"trade_id": 20070256,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.3048,**

**"price": 1874.94,**

**"quote_volume": 571.481712,**

**"timestamp": 1741852572808,**

**"trade_id": 20070248,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0462,**

**"price": 1874.94,**

**"quote_volume": 86.622228,**

**"timestamp": 1741852571793,**

**"trade_id": 20070238,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.4565,**

**"price": 1874.94,**

**"quote_volume": 855.91011,**

**"timestamp": 1741852570319,**

**"trade_id": 20070227,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0657,**

**"price": 1874.86,**

**"quote_volume": 123.178302,**

**"timestamp": 1741852568886,**

**"trade_id": 20070213,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1824,**

**"price": 1874.74,**

**"quote_volume": 341.952576,**

**"timestamp": 1741852568306,**

**"trade_id": 20070209,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.3102,**

**"price": 1874.74,**

**"quote_volume": 581.544348,**

**"timestamp": 1741852567486,**

**"trade_id": 20070200,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0214,**

**"price": 1874.74,**

**"quote_volume": 40.119436,**

**"timestamp": 1741852566320,**

**"trade_id": 20070193,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0234,**

**"price": 1874.44,**

**"quote_volume": 43.861896,**

**"timestamp": 1741852565272,**

**"trade_id": 20070184,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1804,**

**"price": 1874.42,**

**"quote_volume": 338.145368,**

**"timestamp": 1741852564597,**

**"trade_id": 20070178,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0218,**

**"price": 1874.42,**

**"quote_volume": 40.862356,**

**"timestamp": 1741852563990,**

**"trade_id": 20070172,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0218,**

**"price": 1874.42,**

**"quote_volume": 40.862356,**

**"timestamp": 1741852563423,**

**"trade_id": 20070165,**

**"type": "buy"**

**},**

**{**

**"base_volume": 1.3972,**

**"price": 1874.42,**

**"quote_volume": 2618.939624,**

**"timestamp": 1741852562308,**

**"trade_id": 20070157,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0246,**

**"price": 1874.42,**

**"quote_volume": 46.110732,**

**"timestamp": 1741852560847,**

**"trade_id": 20070144,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0641,**

**"price": 1874.27,**

**"quote_volume": 120.140707,**

**"timestamp": 1741852560205,**

**"trade_id": 20070141,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0336,**

**"price": 1874.27,**

**"quote_volume": 62.975472,**

**"timestamp": 1741852559460,**

**"trade_id": 20070134,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0282,**

**"price": 1874.27,**

**"quote_volume": 52.854414,**

**"timestamp": 1741852558316,**

**"trade_id": 20070125,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0285,**

**"price": 1874.27,**

**"quote_volume": 53.416695,**

**"timestamp": 1741852557556,**

**"trade_id": 20070119,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0464,**

**"price": 1874.27,**

**"quote_volume": 86.966128,**

**"timestamp": 1741852556042,**

**"trade_id": 20070104,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.4636,**

**"price": 1874.27,**

**"quote_volume": 868.911572,**

**"timestamp": 1741852554938,**

**"trade_id": 20070096,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0297,**

**"price": 1874.27,**

**"quote_volume": 55.665819,**

**"timestamp": 1741852553761,**

**"trade_id": 20070084,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0534,**

**"price": 1874.27,**

**"quote_volume": 100.086018,**

**"timestamp": 1741852553073,**

**"trade_id": 20070080,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0018,**

**"price": 1874.27,**

**"quote_volume": 3.373686,**

**"timestamp": 1741852552240,**

**"trade_id": 20070077,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0247,**

**"price": 1874.27,**

**"quote_volume": 46.294469,**

**"timestamp": 1741852551152,**

**"trade_id": 20070067,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0923,**

**"price": 1874.27,**

**"quote_volume": 172.995121,**

**"timestamp": 1741852550134,**

**"trade_id": 20070057,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0447,**

**"price": 1874.26,**

**"quote_volume": 83.779422,**

**"timestamp": 1741852549214,**

**"trade_id": 20070049,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.1059,**

**"price": 1874.26,**

**"quote_volume": 198.484134,**

**"timestamp": 1741852548215,**

**"trade_id": 20070041,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0664,**

**"price": 1874.26,**

**"quote_volume": 124.450864,**

**"timestamp": 1741852546831,**

**"trade_id": 20070030,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0405,**

**"price": 1874.26,**

**"quote_volume": 75.90753,**

**"timestamp": 1741852546313,**

**"trade_id": 20070023,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0382,**

**"price": 1874.26,**

**"quote_volume": 71.596732,**

**"timestamp": 1741852545611,**

**"trade_id": 20070016,**

**"type": "buy"**

**},**

**{**

**"base_volume": 0.0115,**

**"price": 1874.26,**

**"quote_volume": 21.55399,**

**"timestamp": 1741852544490,**

**"trade_id": 20070009,**

**"type": "buy"**

**},**

**{**

**"base_volume": 1.0297,**

**"price": 1874.26,**

**"quote_volume": 1929.925522,**

**"timestamp": 1741852543165,**

**"trade_id": 20069995,**

**"type": "sell"**

**},**

**{**

**"base_volume": 0.0213,**

**"price": 1874.26,**

**"quote_volume": 39.921738,**

**"timestamp": 1741852541517,**

**"trade_id": 20069985,**

**"type": "sell"**

**}**

**]**

**}**

**5、https://www.coinp.com/api/v1/cmc/orderbook/BTC_USDT**

Parameters:

+--------+----+------------------------------+------------------------+
| **     | ** | **Status**                   | **  Description**      |
| Name** | Ty |                              |                        |
|        | pe |                              |                        |
|        | ** |                              |                        |
+--------+----+------------------------------+------------------------+
| marke  | st | Mandatory                    |   A pair such as       |
| t_pair | ri |                              | "LTC_BTC"              |
|        | ng |                              |                        |
+--------+----+------------------------------+------------------------+
| depth  | i  | Recommended (used to         | Orders depth quantity: |
|        | nt | calculate liquidity score    | [                     |
|        |    | for rankings)                | 0,5,10,20,50,100,500] |
|        |    |                              |                        |
|        |    |                              |  Not defined or 0 =    |
|        |    |                              | full order book        |
|        |    |                              |                        |
|        |    |                              | Depth = 100 means 50   |
|        |    |                              | for each bid/ask side. |
+--------+----+------------------------------+------------------------+
| level  | i  | Recommended                  |   Level 1 -- Only the  |
|        | nt |                              | best bid and ask.      |
|        |    | (used to calculate liquidity |                        |
|        |    | score for rankings)          |   Level 2 -- Arranged  |
|        |    |                              | by best bids and asks. |
|        |    |                              |                        |
|        |    |                              |   Level 3 -- Complete  |
|        |    |                              | order book, no         |
|        |    |                              | aggregation.           |
|        |    |                              |                        |
|        |    |                              |                        |
+--------+----+------------------------------+------------------------+

**response**

+-------+------+------+-----------------------------------------------+
| **N   | **   | **   | **Description**                               |
| ame** | Ty   | Stat |                                               |
|       | pe** | us** |                                               |
+-------+------+------+-----------------------------------------------+
| time  | Inte |      | Unix timestamp in milliseconds for when the   |
| stamp | ger  | M    | last updated time occurred.                   |
|       |      | anda |                                               |
|       | (UTC | tory |                                               |
|       | t    |      |                                               |
|       | imes |      |                                               |
|       | tamp |      |                                               |
|       | in   |      |                                               |
|       | ms)  |      |                                               |
+-------+------+------+-----------------------------------------------+
| bids  |      |      | An array containing 2 elements. The offer     |
|       | dec  | M    | price and quantity for each bid order.        |
|       | imal | anda |                                               |
|       |      | tory |                                               |
+-------+------+------+-----------------------------------------------+
| asks  |      |      | An array containing 2 elements. The ask price |
|       | dec  | M    | and quantity for each ask order.              |
|       | imal | anda |                                               |
|       |      | tory |                                               |
+-------+------+------+-----------------------------------------------+

**{**

**"code": 200,**

**"message": "success",**

**"data": {**

**"asks": [**

**[**

**"83171.87",**

**"0.00132"**

**],**

**[**

**"83171.88",**

**"0.09336"**

**],**

**[**

**"83171.89",**

**"0.09136"**

**],**

**[**

**"83171.9",**

**"0.11641"**

**],**

**[**

**"83171.91",**

**"0.11133"**

**],**

**[**

**"83171.92",**

**"0.11413"**

**],**

**[**

**"83171.93",**

**"0.01985"**

**],**

**[**

**"83171.94",**

**"0.10143"**

**],**

**[**

**"83171.95",**

**"0.05246"**

**],**

**[**

**"83171.96",**

**"0.02448"**

**],**

**[**

**"83171.97",**

**"0.10482"**

**],**

**[**

**"83171.98",**

**"0.10788"**

**],**

**[**

**"83171.99",**

**"0.05683"**

**],**

**[**

**"83172",**

**"0.03036"**

**],**

**[**

**"83172.01",**

**"0.06608"**

**],**

**[**

**"83172.02",**

**"0.08092"**

**],**

**[**

**"83172.03",**

**"0.04195"**

**],**

**[**

**"83172.04",**

**"0.01977"**

**],**

**[**

**"83172.05",**

**"0.0397"**

**],**

**[**

**"83172.06",**

**"0.03744"**

**]**

**],**

**"bids": [**

**[**

**"83171.83",**

**"0.00132"**

**],**

**[**

**"83171.82",**

**"0.0388"**

**],**

**[**

**"83171.81",**

**"0.0504"**

**],**

**[**

**"83171.8",**

**"0.04521"**

**],**

**[**

**"83171.79",**

**"0.02865"**

**],**

**[**

**"83171.78",**

**"0.03658"**

**],**

**[**

**"83171.77",**

**"0.04926"**

**],**

**[**

**"83171.76",**

**"0.05352"**

**],**

**[**

**"83171.75",**

**"0.02184"**

**],**

**[**

**"83171.74",**

**"0.05325"**

**],**

**[**

**"83171.73",**

**"0.03885"**

**],**

**[**

**"83171.72",**

**"0.05032"**

**],**

**[**

**"83171.71",**

**"0.03134"**

**],**

**[**

**"83171.7",**

**"0.01675"**

**],**

**[**

**"83171.69",**

**"0.03029"**

**],**

**[**

**"83171.68",**

**"0.05886"**

**],**

**[**

**"83171.67",**

**"0.02456"**

**],**

**[**

**"83171.66",**

**"0.04321"**

**],**

**[**

**"83171.65",**

**"0.05188"**

**],**

**[**

**"83171.64",**

**"0.0147"**

**]**

**],**

**"timestamp": 1741852729000**

**}**

**}**

**6、https://www.coinp.com/api/v1/cmc/contracts**

**response：**

+----------------+-----+----------+----------------------------------+
| **Name**       | **  | **       | **Description**                  |
|                | Typ | Status** |                                  |
|                | e** |          |                                  |
+----------------+-----+----------+----------------------------------+
| ticker_id      | str | M        | Identifier of a ticker with      |
|                | ing | andatory | delimiter to separate            |
|                |     |          | base/quote, eg. BTC-PERPUSD,     |
|                |     |          | BTC-PERPETH, BTC-PERPEUR         |
+----------------+-----+----------+----------------------------------+
| base_currency  | str | M        | Symbol/currency code of base     |
|                | ing | andatory | pair, eg. BTC                    |
+----------------+-----+----------+----------------------------------+
| quote_currency | str | M        | Symbol/currency code of quote    |
|                | ing | andatory | pair, eg. ETH                    |
+----------------+-----+----------+----------------------------------+
| last_price     | d   | M        | Last transacted price of base    |
|                | eci | andatory | currency based on given quote    |
|                | mal |          | currency                         |
+----------------+-----+----------+----------------------------------+
| base_volume    | d   | M        | 24 hour trading volume in BASE   |
|                | eci | andatory | currency                         |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| USD_volume     | d   | Rec      | 24 hour trading volume in USD    |
|                | eci | ommended |                                  |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| quote_volume   | d   | M        | 24 hour trading volume in QUOTE  |
|                | eci | andatory | currency                         |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| bid            | d   | M        | Current highest bid price        |
|                | eci | andatory |                                  |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| ask            | d   | M        | Current lowest ask price         |
|                | eci | andatory |                                  |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| high           | d   | M        | Rolling 24-hour highest          |
|                | eci | andatory | transaction price                |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| low            | d   | M        | Rolling 24-hour lowest           |
|                | eci | andatory | transaction price                |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| product_type   | str | M        | Futures, Perpetual, Options      |
|                | ing | andatory |                                  |
+----------------+-----+----------+----------------------------------+
| open_interest  | d   | M        | The number of outstanding        |
|                | eci | andatory | derivatives  contracts that have |
|                | mal |          | not been settled                 |
+----------------+-----+----------+----------------------------------+
| ope            | d   | M        | The sum of the Open Positions    |
| n_interest_usd | eci | andatory | (long or short) in USD Value of  |
|                | mal |          | the contract                     |
+----------------+-----+----------+----------------------------------+
| index_price    | d   | M        | Last calculated index price for  |
|                | eci | andatory | underlying of contract           |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| crea           | In  | Ma       | Start date of derivative (**[not |
| tion_timestamp | teg | ndatory  | needed for perpetual             |
|                | er  |          | swaps]{.underline}**)            |
|                |     | (only    |                                  |
|                | (   | for      |                                  |
|                | UTC | e        |                                  |
|                | tim | xpirable |                                  |
|                | est | futures/ |                                  |
|                | amp | options) |                                  |
|                | in  |          |                                  |
|                | ms) |          |                                  |
+----------------+-----+----------+----------------------------------+
| ex             | In  | M        | End date of derivative (**[not   |
| piry_timestamp | teg | andatory | needed for perpetual             |
|                | er  |          | swaps]{.underline}**)            |
|                |     | (only    |                                  |
|                | (   | for      |                                  |
|                | UTC | e        |                                  |
|                | tim | xpirable |                                  |
|                | est | futures/ |                                  |
|                | amp | options) |                                  |
|                | in  |          |                                  |
|                | ms) |          |                                  |
+----------------+-----+----------+----------------------------------+
| funding_rate   | d   | M        | Current funding rate             |
|                | eci | andatory |                                  |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| nex            | d   | Rec      | Upcoming predicted funding rate  |
| t_funding_rate | eci | ommended |                                  |
|                | mal |          |                                  |
+----------------+-----+----------+----------------------------------+
| next_funding_  | In  | M        | Timestamp of the next funding    |
| rate_timestamp | teg | andatory | rate change                      |
|                | er  |          |                                  |
|                |     |          |                                  |
|                | (   |          |                                  |
|                | UTC |          |                                  |
|                | tim |          |                                  |
|                | est |          |                                  |
|                | amp |          |                                  |
|                | in  |          |                                  |
|                | ms) |          |                                  |
+----------------+-----+----------+----------------------------------+
| maker_fee      | d   | Rec      | Fees for filling a "maker" order |
|                | eci | ommended | (can be negative if rebate is    |
|                | mal |          | given)                           |
+----------------+-----+----------+----------------------------------+
| taker_fee      | d   | Rec      | Fees for filling a "taker" order |
|                | eci | ommended | (can be negative if rebate is    |
|                | mal |          | given)                           |
+----------------+-----+----------+----------------------------------+

**7、https://www.coinp.com/api/v1/cmc/contract/orderbook**

**response**

+------+------+------+------------------------------------------------+
| Name | Data | Cate | Description                                    |
|      | Type | gory |                                                |
+------+------+------+------------------------------------------------+
| t    | st   |      | A pair such as "BTC-PERPUSD", with delimiter |
| icke | ring | M    | between different cryptoassets                 |
| r_id |      | anda |                                                |
|      |      | tory |                                                |
+------+------+------+------------------------------------------------+
| t    | Inte |      | Unix timestamp in milliseconds for when the    |
| imes | ger  | M    | last updated time occurred.                    |
| tamp |      | anda |                                                |
|      | (UTC | tory |                                                |
|      | t    |      |                                                |
|      | imes |      |                                                |
|      | tamp |      |                                                |
|      | in   |      |                                                |
|      | ms)  |      |                                                |
+------+------+------+------------------------------------------------+
| bids |      |      | An array containing 2 elements. The offer      |
|      | dec  | M    | price and quantity fyor each bid order         |
|      | imal | anda |                                                |
|      |      | tory |                                                |
+------+------+------+------------------------------------------------+
| asks |      |      | An array containing 2 elements. The ask price  |
|      | dec  | M    | and quantity for each ask order                |
|      | imal | anda |                                                |
|      |      | tory |                                                |
+------+------+------+------------------------------------------------+

**{**

**"code": 200,**

**"message": "success",**

**"data": {**

**"asks": [**

**[**

**1876.07,**

**308**

**],**

**[**

**1876.09,**

**151**

**],**

**[**

**1876.11,**

**156**

**],**

**[**

**1876.13,**

**180**

**],**

**[**

**1876.15,**

**3464**

**],**

**[**

**1876.18,**

**3882**

**],**

**[**

**1876.19,**

**3883**

**],**

**[**

**1876.22,**

**392**

**],**

**[**

**1876.24,**

**166**

**],**

**[**

**1876.27,**

**299**

**]**

**],**

**"bids": [**

**[**

**1876.05,**

**167**

**],**

**[**

**1876.03,**

**159**

**],**

**[**

**1876.01,**

**158**

**],**

**[**

**1875.99,**

**317**

**],**

**[**

**1875.98,**

**3878**

**],**

**[**

**1875.96,**

**3884**

**],**

**[**

**1875.93,**

**3872**

**],**

**[**

**1875.92,**

**233**

**],**

**[**

**1875.9,**

**664**

**],**

**[**

**1875.87,**

**1282**

**]**

**],**

**"ticker_id": "ETH_USDT",**

**"timestamp": 1741854107**

**}**

**}**

**1**

