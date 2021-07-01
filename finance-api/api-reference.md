Finance API reference
===
## About Finance API
The Finance API is used to fetch information about the transactions that are made through Zettle. For example, card payments made with Zettle card readers.

* [About Finance API](#about-finance-api)
  * [Base URL](#base-url)
  * [OAuth scope](#oauth-scope)
* [Fetch account balance](#fetch-account-balance)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Fetch account transactions](#fetch-account-transactions)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Fetch payout information](#fetch-payout-information)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Examples](#examples)
  * [Fetch current balance for a liquid account](#fetch-current-balance-for-a-liquid-account)
  * [Fetch transactions for a liquid account](#fetch-transactions-for-a-liquid-account)
  * [Fetch payout information on a specific day](#fetch-payout-information-on-a-specific-day)
* [Related resources](#related-resources)
* [Related API reference](#related-api-reference)
  
### Base URL
https://finance.zettle.com

### OAuth Scope
`READ:FINANCE`

For more information on how to get authorisaition for the scope, see [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc)

## Fetch account balance
Returns the balance in a merchant's preliminary or liquid account at a specific time.

After a deposit has been made from a merchant's liquid account to their bank account or PayPal Wallet for PayPal users (daily, weekly or monthly), the balance is usually zero or the minimum balance in the merchant's Zettle account. In other cases, you can see how much money is in the merchant's Zettle account.

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/balance
```

See example [Fetch current balance for a liquid account](#fetch-current-balance-for-a-liquid-account).

### Parameters

<details open="true">
<summary>Click to hide request parameters.</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|accountTypeGroup |string |path |required |The type of a merchant's Zettle account. You can use one of the following account types: <br/><ul><li> `PRELIMINARY` account where transactions are to be confirmed.</li><li> `LIQUID` account where transactions are to be paid out to the merchant.</li></ul>
|at |string |query |optional |Used to fetch account balance that is available at a UTC time. If it's used, any transaction after that point will be ignored. If it's not used, the balance of all transactions at the current point of time is returned. <br/>You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM` to specify a month. By default, it specifies the first second of the first day in the month. For example, for `2020-11`, the time will be `2020-11-01T00:00:00`.</li><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
</details>


### Responses
<details open="true">
<summary>Click to hide HTTP status codes.</summary>

|Status code |Description
|:---- |:----
|200 OK |Returned when the operation is successful.  
|400 Bad Request |Returned when a required parameter is missing or in a wrong format in the request.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|403 Forbidden |Returned when the scope being used in the request is incorrect.   
</details>

<details open="true">
<summary>Click to hide response attributes.</summary>

|Name |Type |Description
|:---- |:---- |:----
|data |object|Information about the account balance at a point in time. It's represented by `totalBalance` and `currencyId`.
|totalBalance |integer |The account balance in the currency's smallest unit. For example, 300 with currency SEK is 3 kr. It can be negative, such as when refunds are greater than sales.
|currencyId |string |The currency of the account. For example, `SEK`.
</details>


## Fetch account transactions
Returns all transactions or transactions of certain types from a merchant's preliminary or liquid account during a specific period.

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}
```

See example [Fetch transactions for a liquid account](#fetch-transactions-for-a-liquid-account).

### Parameters

<details open="true">
<summary>Click to hide request parameters.</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|accountTypeGroup |string |path |required |The type of a merchant's Zettle account. You can use one of the following account types: <br/><ul><li> `PRELIMINARY` account where transactions are to be confirmed.</li><li> `LIQUID` account where transactions are to be paid out to the merchant.</li></ul>
|start |string |query |required |A start time in UTC (inclusive) from when the transactions will be fetched. You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM` to specify a month. By default, it specifies the first second of the first day in the month. For example, for `2020-11`, the time will be `2020-11-01T00:00:00`.</li><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
|end |string |query |required |An end time in UTC (exclusive) before when the transactions will be fetched. You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM` to specify a month. By default, it specifies the first second of the first day in the month. For example, for `2020-11`, the time will be `2020-11-01T00:00:00`.</li><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
|includeTransactionType |string |query |optional |Which transaction types to fetch. <br>You can include more than one [supported transaction types](#supported-transaction-types) in a request.  
|limit |integer |query |optional |The maximum number of transactions to return in a response. You can specify `limit` with any integer greater than 0.<br/>To avoid a big dataset in a response, use `limit` and `offset` together to set response pagination.<br/>For example, to return three transaction at a time for all transactions during a specific period, set `limit` as `3` and `offset` as `0` in the first request. Then set `limit` as `3` and increment `offset` with `3` in the second request and repeat the request until all transactions are fetched. See examples in [Fetch account transactions](fetch-account-transactions.md).
|offset |integer |query |optional |The number of transactions to skip before beginning to return in a response. You can specify `offset` with any integer greater than or equal to 0.  Use `limit` and `offset` together to set response pagination to avoid a big dataset in a response.
</details>

#### Supported transaction types
> **Note:** Deprecated transaction types are no longer in use, but may appear in historic data.
<details open="true">
<summary>Click to hide supported transaction types.</summary>

|Type |Description
|:---- |:----
|ADJUSTMENT |A bookkeeping adjustment.
|ADVANCE |The cash advance given by Zettle to a merchant. A cash advance is a type of financing that is offered to merchants based on their sales history. The advance is paid back with monthly down payments.
|ADVANCE_DOWNPAYMENT |A down payment on a previously paid out cash advance.
|ADVANCE_FEE_DOWNPAYMENT |The netting of a cash advance fee.
|BANK_ACCOUNT_VERIFICATION (Deprecated) |No available description.
|CARD_PAYMENT |A card payment. Contains a reference to the card payment in the Purchase API.
|CARD_PAYMENT_FEE |The commission part of a card payment. Contains a reference to the card payment fee in the Purchase API.
|CARD_PAYMENT_FEE_REFUND |The commission part of a refund. Contains a reference to the card payment fee refund in the Purchase API.
|CARD_REFUND |A card refund. Contains a reference to the card refund in the Purchase API.
|CASHBACK |Money given to a merchant to retroactively adjust the card payment fee rate.
|CASHBACK_PAYOUT (Deprecated) |No available description.
|EMONEY_TRANSFER (Deprecated) |No available description.
|FAILED_PAYOUT |A previous payout transaction has failed and been made void. The payout money is returned to the merchant's liquid account.
|FEE_DISCOUNT_REVOCATION |An internal reclaim of an outstanding fee discount that is not consumed within a certain time frame. As these funds are reclaimed from a special fee discount account, the transaction will not be visible in the liquid account of the merchant.
|FROZEN_FUNDS |The money that is frozen to cover a chargeback. When the issuing back initiates a chargeback, the money will be removed from the merchant's liquid account and marked as frozen to cover the chargeback. If the chargeback is later revoked, the money will be returned to the merchants liquid account with a new and positive transaction of the same type. It effectively makes the initial FROZEN_FUNDS transaction void.
|INVOICE_PAYMENT |An invoice payment.
|INVOICE_PAYMENT_FEE |An invoice payment fee.
|PAYMENT |An alternative third-party payment method where Zettle handles the funds. For example, PayPal QR code and Klarna QR code. Contains a reference to the payment in the Purchase API.
|PAYMENT_FEE |The fee for a third-party payment method. For example, PayPal QR code and Klarna QR code. Contains a reference to the payment fee in the Purchase API.
|PAYOUT |A payout from the merchant's liquid account to the merchant’s bank account. If the merchant is a PayPal user, the payout will be made to their PayPal Wallet.  
|TELL_FRIEND (Deprecated) |No available description.
|VOUCHER_ACTIVATION |Used when activating a voucher. The money is inserted to the merchant’s fee discount account instead of preliminary and liquid accounts.
 
</details>


### Responses
<details open="true">
<summary>Click to hide HTTP status codes.</summary>

|Status code |Description
|:---- |:----
|200 OK |Returned when the operation is successful.  
|400 Bad Request |Returned when a required parameter is missing or in a wrong format in the request.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|403 Forbidden |Returned when the scope being used in the request is incorrect. 
</details>

<details open="true">
<summary>Click to hide response attributes.</summary>

|Name |Type |Description
|:---- |:---- |:----
|data |an array of objects|A list of transactions for the given account in the descending order that shows the most recent transaction first.
|timestamp |string |Time when a transaction happens in the merchant's Zettle account. It's not the timestamp when a card transaction or a purchase happens. <br/>For example, transactions in the merchant's liquid account may happen hours after card transactions or purchases take place.
|amount |integer |The amount of money of a transaction. If the transaction is a refund or a payment fee, the amount is negative.
|originatorTransactionType |string |The transaction type. See [supported transaction types](#supported-transaction-types). 
|originatingTransactionUuid |string |The identifier of the originating transaction as UUID version 1.  
</details>


## Fetch payout information
Returns payout related information from a merchant's liquid account.

```
GET /organizations/{organizationUuid}/payout-info
```

See example [Fetch payout information on a specific period](#fetch-payout-information-on-a-specific-day).

### Parameters

<details open="true">
<summary>Click to hide request parameters.</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|at |string |query |optional |Used to fetch payouts at a certain point in UTC time. If it's used, any transaction after that time will be ignored. If it's not used, the balance of all transactions at the current point of time is returned. <br/>You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM` to specify a month. For example, `2020-11`. By default, it specifies the first second of the first day in the month.</li><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
</details>


### Responses
<details open="true">
<summary>Click to hide HTTP status codes.</summary>

|Status code |Description
|:---- |:----
|200 OK |Returned when the operation is successful.  
|400 Bad Request |Returned when a required parameter is missing or in a wrong format in the request.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|403 Forbidden |Returned when the scope being used in the request is incorrect.  
</details>

<details open="true">
<summary>Click to hide response attributes.</summary>

|Name |Type |Description
|:---- |:---- |:----
|data |object|Information about the upcoming payout. 
|totalBalance |integer |The account balance in the currency's smallest unit. For example, 300 with currency SEK is 3 kr. It can be negative, such as when refunds are greater than sales.
|currencyId |string |The currency of the account. For example, `SEK`. 
|nextPayoutAmount |integer |The amount of money to be paid out to the merchant.  
|discountRemaining |integer |The amount of discounts that remains in merchant's vouchers. The vouchers are offered by Zettle.<br/>For example, a merchant has a voucher worthy 100 kr from a Zettle marketing campaign. For a transaction of 200 kr, Zettle will subtract 2 kr for the transaction fee from the voucher. Then the merchant will have a remaining discount of 98 kr. 
|periodicity |string |The period between each payout that is set by the merchant. It can be `DAILY`, `WEEKLY` or `MONTHLY`.   
</details>


## Examples

### Fetch current balance for a liquid account
In the following example, as `at` is not specified, the current balance £1.95 is fetched by default.

Request
```
GET /organizations/self/accounts/liquid/balance
```
Response
```json
{
    "data": {
        "totalBalance": 195,
        "currencyId": "GBP"
    }
}
```

### Fetch transactions for a liquid account
The following example fetches transactions from the merchant's liquid account from 1 January, 2020 to 31 December, 2020.

Request
```
GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31
```

Response
```json
{
    "data": [
        ...
        {
            "timestamp": "2020-07-04T20:16:44.309+0000",
            "amount": 381,
            "originatorTransactionType": "CARD_PAYMENT_FEE_REFUND",
            "originatingTransactionUuid": "30cef6e2-be09-11ea-a8e4-bce028663c34"
        },
        {
            "timestamp": "2020-07-04T20:16:44.309+0000",
            "amount": -20610,
            "originatorTransactionType": "CARD_REFUND",
            "originatingTransactionUuid": "30cef6e2-be09-11ea-a8e4-bce028663c34"
        },
        ...
        {
            "timestamp": "2020-06-27T23:52:18.327+0000",
            "amount": 649,
            "originatorTransactionType": "PAYMENT_FEE", // In case of refund
            "originatingTransactionUuid": "690c99ea-b6ef-11ea-9730-7ef7aeff642d"
        },
        {
            "timestamp": "2020-06-27T23:52:18.327+0000",
            "amount": -35100,
            "originatorTransactionType": "PAYMENT", // In case of refund
            "originatingTransactionUuid": "690c99ea-b6ef-11ea-9730-7ef7aeff642d"
        },
        {
            "timestamp": "2020-06-26T19:51:38.161+0000",
            "amount": -649,
            "originatorTransactionType": "PAYMENT_FEE",
            "originatingTransactionUuid": "45f51ed4-b7bf-11ea-90de-435a3f6e4738"
        },
        {
            "timestamp": "2020-06-26T19:51:38.161+0000",
            "amount": 35100,
            "originatorTransactionType": "PAYMENT",
            "originatingTransactionUuid": "45f51ed4-b7bf-11ea-90de-435a3f6e4738"
        },
        {
            "timestamp": "2020-06-26T11:15:40.144+0000",
            "amount": 470433,
            "originatorTransactionType": "FAILED_PAYOUT",
            "originatingTransactionUuid": "5d4846ae-b78f-11ea-b003-01f0540f969e"
        },
        {
            "timestamp": "2020-06-26T09:28:16.144+0000",
            "amount": -470433,
            "originatorTransactionType": "PAYOUT",
            "originatingTransactionUuid": "5d4846ae-b78f-11ea-b003-01f0540f969e"
        },
        ...
        {
            "timestamp": "2020-01-02T15:16:43.945+0000",
            "amount": -8867,
            "originatorTransactionType": "CARD_PAYMENT_FEE",
            "originatingTransactionUuid": "7428bda0-2d50-11ea-9132-999363d04928"
        },
        {
            "timestamp": "2020-01-02T15:16:43.945+0000",
            "amount": 479300,
            "originatorTransactionType": "CARD_PAYMENT",
            "originatingTransactionUuid": "7428bda0-2d50-11ea-9132-999363d04928"
        }        
    ]
}  
```

### Fetch payout information on a specific day
The following example fetches all payout information from the merchant's liquid account on 7 June, 2021. The next payout amount will be 6605.89 kr.

Request
```
GET /organizations/self/payout-info?at=2021-06-07
```
Response
```json
    {
        "data": {
            "totalBalance": 660589,
            "currencyId": "SEK",
            "nextPayoutAmount": 660589,
            "discountRemaining": 0,
            "periodicity": "DAILY"
        }
    }
```

## Related resources
* [How card payments work at Zettle](concepts/how-card-payments-work-at-Zettle.md)
* [Finance API user guide](user-guides)

## Related API reference
* [OAuth2 API Reference](../authorization.adoc)
* [Purchase API reference](../purchase.adoc)