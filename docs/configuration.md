# Configure the bot
This page explains how to configure your `config.json` file.

## Table of Contents
- [Bot commands](#bot-commands)
- [Backtesting commands](#backtesting-commands)
- [Hyperopt commands](#hyperopt-commands)

## Setup config.json
We recommend to copy and use the `config.json.example` as a template
for your bot configuration.

The table below will list all configuration parameters. 

|  Command | Default | Mandatory | Description |
|----------|---------|----------|-------------|
| `max_open_trades` | 3 | Yes | Number of trades open your bot will have.
| `stake_currency` | BTC | Yes | Crypto-currency used for trading.
| `stake_amount` | 0.05 | Yes | Amount of crypto-currency your bot will use for each trade. Per default, the bot will use (0.05 BTC x 3) = 0.15 BTC in total will be always engaged.
| `fiat_display_currency` | USD | Yes | Fiat currency used to show your profits. More information below. 
| `dry_run` | true | Yes | Define if the bot must be in Dry-run or production mode. 
| `minimal_roi` | See below | Yes | Set the threshold in percent the bot will use to sell a trade. More information below. 
| `stoploss` | -0.10 | No | Value of the stoploss in percent used by the bot. More information below. 
| `bid_strategy.ask_last_balance` | 0.0 | Yes | Set the bidding price. More information below.
| `exchange.name` | bittrex | Yes | Name of the exchange class to use.
| `exchange.key` | key | No | API key to use for the exchange. Only required when you are in production mode.
| `exchange.secret` | secret | No | API secret to use for the exchange. Only required when you are in production mode.
| `exchange.pair_whitelist` | [] | No | List of currency to use by the bot. Can be overrided with `--dynamic-whitelist` param.
| `exchange.pair_blacklist` | [] | No | List of currency the bot must avoid. Useful when using `--dynamic-whitelist` param.
| `experimental.use_sell_signal` | false | No | Use your sell strategy in addition of the `minimal_roi`.
| `telegram.enabled` | true | Yes | Enable or not the usage of Telegram.
| `telegram.token` | token | No | Your Telegram bot token. Only required is `enable` is `true`.
| `telegram.chat_id` | chat_id | No | Your personal Telegram account id. Only required is `enable` is `true`.
| `initial_state` | running | No | Defines the initial application state. More information below.
| `internals.process_throttle_secs` | 5 | Yes | Set the process throttle. Value in second.

The definition of each config parameters is in 
[misc.py](https://github.com/gcarq/freqtrade/blob/develop/freqtrade/misc.py#L205).

### Understand minimal_roi
`minimal_roi` is a JSON object where the key is a duration
in minutes and the value is the minimum ROI in percent.
See the example below:
```
"minimal_roi": {
    "40": 0.0,    # Sell after 40 minutes if the profit is not negative
    "30": 0.01,   # Sell after 30 minutes if there is at least 1% profit
    "20": 0.02,   # Sell after 20 minutes if there is at least 2% profit
    "0":  0.04    # Sell immediately if there is at least 4% profit
},
```

### Understand stoploss
`stoploss` is loss in percentage that should trigger a sale.
For example value `-0.10` will cause immediate sell if the
profit dips below -10% for a given trade. This parameter is optional.

### Understand initial_state
`initial_state` is an optional field that defines the initial application state.
Possible values are `running` or `stopped`. (default=`running`)
If the value is `stopped` the bot has to be started with `/start` first.

### Understand ask_last_balance
`ask_last_balance` sets the bidding price. Value `0.0` will use `ask` price, `1.0` will
use the `last` price and values between those interpolate between ask and last
price. Using `ask` price will guarantee quick success in bid, but bot will also
end up paying more then would probably have been necessary.

### What values for fiat_display_currency?
`fiat_display_currency` set the fiat to use for the conversion form coin to fiat in Telegram. 
The valid value are: "AUD", "BRL", "CAD", "CHF", "CLP", "CNY", "CZK", "DKK", "EUR", "GBP", "HKD", "HUF", "IDR", "ILS", "INR", "JPY", "KRW", "MXN", "MYR", "NOK", "NZD", "PHP", "PKR", "PLN", "RUB", "SEK", "SGD", "THB", "TRY", "TWD", "ZAR", "USD".

## Switch to dry-run mode
We recommend starting the bot in dry-run mode to see how your bot will
behave and how is the performance of your strategy. In Dry-run mode the
bot does not engage your money. It only runs a live simulation without
creating trades.

### To switch your bot in Dry-run mode:
1. Edit your `config.json`  file
2. Switch dry-run to true
```json
"dry_run": true,
```

3. Remove your Bittrex API key (change them by fake api credentials)
```json
"exchange": {
        "name": "bittrex",
        "key": "key",
        "secret": "secret",
        ...
}   
```

Once you will be happy with your bot performance, you can switch it to 
production mode.

## Switch to production mode
In production mode, the bot will engage your money. Be careful a wrong 
strategy can lose all your money. Be aware of what you are doing when 
you run it in production mode.

### To switch your bot in production mode:
1. Edit your `config.json`  file

2. Switch dry-run to false
```json
"dry_run": false,
```

3. Insert your Bittrex API key (change them by fake api keys)
```json
"exchange": {
        "name": "bittrex",
        "key": "af8ddd35195e9dc500b9a6f799f6f5c93d89193b",
        "secret": "08a9dc6db3d7b53e1acebd9275677f4b0a04f1a5",
        ...
}       
```
If you have not your Bittrex API key yet, 
[see our tutorial](https://github.com/gcarq/freqtrade/blob/develop/docs/pre-requisite.md).


## Next step
Now you have configured your config.json, the next step is to 
[start your bot](https://github.com/gcarq/freqtrade/blob/develop/docs/bot-usage.md).
