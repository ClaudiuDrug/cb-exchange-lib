# cb-exchange-lib

Coinbase Exchange API client library.

---

### Installation:

```commandline
python -m pip install [--upgrade] cb-exchange-lib
```

---

### Endpoints:

> ### Info:
> Private endpoints require authentication using your Coinbase Exchange API key.
> You can generate API keys [here](https://exchange.coinbase.com/profile/api).

#### Private:
* Accounts
* AddressBook
* CoinbaseAccounts
* Conversions
* Deposits
* PaymentMethods
* Transfers
* Withdrawals
* Fees
* Fills
* Orders
* Oracle
* Profiles
* Reports
* Users

#### Public:
* Time
* Currencies
* Products
* WrappedAssets

**Any endpoint can take these keyword arguments:**
* `environment`: str - The API environment: `production` or `sandbox` (defaults to: `production`);
* `retries`: int - Total number of retries to allow (defaults to: `3`);
* `backoff`: int - A backoff factor to apply between attempts after the second try (defaults to: `1`);
* `timeout`: int - How long to wait for the server to send data before giving up (defaults to: `30`);
* `cache`: bool - Use caching (defaults to: `True`);
* `debug`: bool - Set to True to log all requests/responses to/from server (defaults to: `False`);
* `logger`: Logger - The handler to be used for logging. If given, and level is above `DEBUG`,
  all debug messages will be ignored.

**For private endpoints only:**
* `key`: str - The API key;
* `passphrase`: str - The API passphrase;
* `secret`: str - The API secret.

**Any of the endpoints can be instantiated or used as a context-manager:**
```python
from cb_exchange_lib import Time, Accounts

environment: str = "sandbox"
credentials: dict = {
    "key": "your key",
    "passphrase": "your passphrase",
    "secret": "your secret",
}  # be careful where you keep your credentials!


if __name__ == '__main__':

    print("*" * 80)

    endpoint = Accounts(**credentials, environment=environment)
    accounts = endpoint.get_accounts()

    for account in accounts:
        print(account)

    endpoint.close()

    # or

    with Time(environment=environment) as endpoint:

        print("*" * 80)
        time = endpoint.get_time()
        print(time)
```

---

### Resources:

For each mapped resource you must read the [documentation](https://docs.cloud.coinbase.com/exchange).
All the parameters these resources can take are described in the official documentation.

<details>
<summary>Time</summary>
<p>

* [get_time()](https://api.exchange.coinbase.com/time)

  Get the API server time.

</p>
</details>

<details>
<summary>Accounts</summary>
<p>

* [get_accounts()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getaccounts)

  Get a list of trading accounts from the profile of the API key.


* [get_account()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getaccount)

  Information for a single account. Use this endpoint when you know the account_id. API key must belong to the same
  profile as the account.


* [get_account_holds()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getaccountholds)

  List the holds of an account that belong to the same profile as the API key. Holds are placed on an account for any
  active orders or pending withdraw requests. As an order is filled, the hold amount is updated. If an order is
  canceled, any remaining hold is removed. For withdrawals, the hold is removed after it is completed.


* [get_account_ledger()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getaccountledger)

  Lists ledger activity for an account. This includes anything that would affect the accounts balance - transfers,
  trades, fees, etc. List account activity of the API key's profile. Account activity either increases or decreases
  your account balance.


* [get_account_transfers()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getaccounttransfers)

  Lists past withdrawals and deposits for an account.

</p>
</details>

<details>
<summary>AddressBook</summary>
<p>

* [get_addresses()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getaddressbook)

  Get all addresses stored in the address book.

</p>
</details>

<details>
<summary>CoinbaseAccounts</summary>
<p>

* [get_wallets()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getcoinbaseaccounts)

  Gets all the user's available Coinbase wallets (These are the wallets/accounts that are used for buying and selling
  on www.coinbase.com)


* [generate_crypto_address()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postcoinbaseaccountaddresses)

  Generates a one-time crypto address for depositing crypto.

  > #### Info:
  >
  > You can generate an address for crypto deposits.
  > See the [Coinbase Accounts](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getcoinbaseaccounts)
  > section for information on how to retrieve your coinbase account ID.

</p>
</details>

<details>
<summary>Conversions</summary>
<p>

* [convert_currency()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postconversion)

  Converts funds from `from` currency to `to` currency. Funds are converted on the `from` account in the `profile_id`
  profile.

  > #### Caution:
  > Users whose USD and USDC accounts are unified do not have access to the conversion endpoint, and conversions from
  > USDC to USD are automatic upon deposit.


* [get_conversion()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getconversion)

  Gets a currency conversion by id (i.e. `41554f00-5c34-4f09-b800-2a878e52f2ea`).

</p>
</details>

<details>
<summary>Currencies</summary>
<p>

* [get_currencies()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getcurrencies)

  Gets a list of all known currencies.

  > #### Note:
  > Not all currencies may be currently in use for trading.


* [get_currency()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getcurrency)

  Gets a single currency by id.

</p>
</details>

<details>
<summary>Deposits</summary>
<p>

* [deposit_from_coinbase_account()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postdepositcoinbaseaccount)

  Deposits funds from a www.coinbase.com wallet to the specified `profile_id`.


* [deposit_from_payment_method()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postdepositpaymentmethod)

  Deposits funds from a linked external payment method to the specified `profile_id`.

</p>
</details>

<details>
<summary>PaymentMethods</summary>
<p>

* [get_payment_methods()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getpaymentmethods)

  Gets a list of the user's linked payment methods.

</p>
</details>

<details>
<summary>Transfers</summary>
<p>

* [get_transfers()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_gettransfers)

  Gets a list of in-progress and completed transfers of funds in/out of any of the user's accounts.


* [get_transfer()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_gettransfer)

  Get information on a single transfer.

</p>
</details>

<details>
<summary>Withdrawals</summary>
<p>

* [withdraw_to_coinbase_account()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postwithdrawcoinbaseaccount)

  Withdraws funds from the specified `profile_id` to a www.coinbase.com wallet.


* [withdraw_to_crypto_address()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postwithdrawcrypto)

  Withdraws funds from the specified `profile_id` to an external crypto address


* [get_fee_estimate()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getwithdrawfeeestimate)

  Gets the fee estimate for the crypto withdrawal to crypto address


* [withdraw_to_payment_method()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postwithdrawpaymentmethod)

  Withdraws funds from the specified `profile_id` to a linked external payment method

</p>
</details>

<details>
<summary>Fees</summary>
<p>

* [get_fees()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getfees)

  Get fees rates and 30 days trailing volume. This request returns your current maker & taker fee rates, as well as
  your 30-day trailing volume. Quoted rates are subject to change. For more information, see What are the fees on
  Coinbase Pro?.

</p>
</details>

<details>
<summary>Fills</summary>
<p>

* [get_fills()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getfills)

  Get a list of fills. A fill is a partial or complete match on a specific order. Get a list of recent fills of the
  API key's profile.

</p>
</details>

<details>
<summary>Orders</summary>
<p>

* [get_orders()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getorders)

  List your current open orders. Only open or un-settled orders are returned by default. As soon as an order is no
  longer open and settled, it will no longer appear in the default request. Open orders may change state between the
  request and the response depending on market conditions.


* [del_orders()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_deleteorders)

  With best effort, cancel all open orders. This may require you to make the request multiple times until all of the
  open orders are deleted.


* [create_order()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postorders)

  Create an order. You can place two types of orders: limit and market. Orders can only be placed if your account has
  sufficient funds. Once an order is placed, your account funds will be put on hold for the duration of the order.
  How much and which funds are put on hold depends on the order type and parameters specified.

  > #### Caution:
  > Each profile can place a maximum of 500 open orders on a product. Once reached, the profile cannot place any new
  > orders until the total number of open orders is below 500.


* [get_order()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getorder)

  Get a single order by `order_id`.


* [del_order()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_deleteorder)

  Cancel a single open order by `order_id`.

</p>
</details>

<details>
<summary>Oracle</summary>
<p>

* [get_signed_prices()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getcoinbasepriceoracle)

  Get cryptographically signed prices ready to be posted on-chain using Compound's Open Oracle smart contract.

</p>
</details>

<details>
<summary>Products</summary>
<p>

* [get_products()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproducts)

  Gets a list of available currency pairs for trading.


* [get_product()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproduct)

  Get information on a single product.


* [get_product_book()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproductbook)

  Get a list of open orders for a product. The amount of detail shown can be customized with the `level` parameter.


* [get_product_candles()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproductcandles)

  Historic rates for a product. Rates are returned in grouped buckets. Candle schema is of the form:
  `[timestamp, price_low, price_high, price_open, price_close]`

  > #### Info:
  > Historical rate data may be incomplete. No data is published for intervals where there are no ticks.
  > Historical rates should not be polled frequently. If you need real-time information, use the trade and book
  > endpoints along with the WebSocket feed.


* [get_product_stats()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproductstats)

  Gets 30day and 24hour stats for a product.

  > #### Info:
  > The volume property is in base currency units. Properties open, high, low are in quote currency units.


* [get_product_ticker()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproductticker)

  Gets snapshot information about the last trade (tick), best bid/ask and 24h volume.

  > #### Real-time updates:
  > Coinbase recommends that you get real-time updates by connecting with the WebSocket stream and listening for match
  > messages, rather than polling.


* [get_product_trades()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getproducttrades)

  Gets a list the latest trades for a product.

</p>
</details>

<details>
<summary>Profiles</summary>
<p>

* [get_profiles()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getprofiles)

  Gets a list of all of the current user's profiles.


* [create_profile()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postprofile)

  Create a new profile. Will fail if no name is provided or if user already has max number of profiles.


* [transfer_funds()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postprofiletransfer)

  Transfer an amount of currency from one profile to another.


* [get_profile()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getprofile)

  Information for a single profile. Use this endpoint when you know the `profile_id`.


* [rename_profile()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_putprofile)

  Rename a profile. Names 'default' and 'margin' are reserved.


* [del_profile()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_putprofiledeactivate)

  Deletes the profile specified by `profile_id` and transfers all funds to the profile specified by `to`. Fails if
  there are any open orders on the profile to be deleted.

</p>
</details>

<details>
<summary>Reports</summary>
<p>

* [get_reports()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getreports)

  Gets a list of all user generated reports.

  > #### Info:
  > Once a report request has been accepted for processing, you can poll the report resource endpoint at
  > `/reports/{report_id}` for its status. When status is ready, the final report is uploaded and available at
  > `file_url`.


* [create_report()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_postreports)

  Generates a report. You can create reports with historical data for all report types. Balance reports can be
  snapshots of historical or current data. Reports provide batches of historic information about your profile in
  various human and machine readable forms.

  > #### Info:
  > A report is generated when resources are available. You can poll the report resource endpoint at
  > `/reports/{report_id}` for its status. When status is ready, the final report is uploaded and available at
  > `file_url`.


* [get_report()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getreport)

  Get a specific report by report_id.

  > #### Info:
  > Once a report request has been accepted for processing, you can poll the report resource endpoint at
  > `/reports/{report_id}` for its status. When status is ready, the final report is uploaded and available at
  > `file_url`.

</p>
</details>

<details>
<summary>Users</summary>
<p>

* [get_exchange_limits()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getuserexchangelimits)

  Gets exchange limits information for a single user.

  > #### Info:
  > This request returns information on your payment method transfer limits, as well as buy/sell limits per currency.

</p>
</details>

<details>
<summary>WrappedAssets</summary>
<p>

* [get_assets()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getwrappedassets)

  Returns a list of all supported wrapped assets details objects.


* [get_asset_details()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getwrappedasset)

  Returns the circulating and total supply of a wrapped asset, and its conversion rate.


* [get_asset_conversion_rate()](https://docs.cloud.coinbase.com/exchange/reference/exchangerestapi_getwrappedassetconversionrate)

  Returns the conversion rate of a wrapped asset.

</p>
</details>

---

### Websocket clients:

**Any of the websocket clients can take these keyword arguments:**
* `channels`: The channels for subscription;
* `product_ids`: The products IDs for subscription;
* `environment`: The API environment: `production` or `sandbox` (defaults to: `production`);
* `debug`: Set to True to log all requests/responses to/from server (defaults to: `False`);
* `logger`: The handler to be used for logging; If given, and level is above `DEBUG`,
  all debug messages will be ignored.

> **Note:**
> 
> For information about Websocket feed channels visit the
> [documentation](https://docs.cloud.coinbase.com/exchange/docs/websocket-channels).

**For `DirectMarketData` only:**
* `key`: The API key;
* `passphrase`: The API passphrase;
* `secret`: The API secret;

### Examples:

<details>
<summary>MarketData</summary>
<p>

```python
from cb_exchange_lib import MarketData

environment: str = "sandbox"

if __name__ == '__main__':

    client = MarketData(
        environment=environment,
        debug=True,
        channels=["ticker"],
        product_ids=["BTC-USD"],
    )

    client.listen()

    try:
        for item in client.queue:
            print(item)
    except KeyboardInterrupt:
        client.close()
```

</p>
</details>

<details>
<summary>DirectMarketData</summary>
<p>

```python
from cb_exchange_lib import DirectMarketData

environment: str = "sandbox"

credentials: dict = {
    "key": "your key",
    "passphrase": "your passphrase",
    "secret": "your secret",
}  # be careful where you keep your credentials!


if __name__ == '__main__':

    client = DirectMarketData(
        **credentials,
        environment=environment,
        debug=True,
        channels=[
            {
                "name": "ticker",
                "product_ids": [
                    "BTC-USD"
                ]
            }
        ]
    )
    client.listen()

    try:
        for item in client.queue:
            print(item)
    except KeyboardInterrupt:
        client.close()
```

</p>
</details>

---
