Finance API reference
===
## About Finance API
The Finance API fetches information about transactions that are made through Zettle. For example, card payments made with a Zettle card reader.

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
  * [Fetch balance for a liquid account](#fetch-balance-for-a-liquid-account)
  * [Fetch transactions for a liquid account](#fetch-transactions-for-a-preliminary-account)
  * [Fetch payout information on a specific day](#fetch-payout-information-on-a-specific-day)
* [Related resources](#related-resources)
* [Related API reference](#related-api-reference)
  
### Base URL
https://finance.zettle.com

### OAuth Scope
`READ:FINANCE`
<!-- For more information on how to get authorisaition for the scope, see See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) -->

## Fetch account balance
Returns the balance that is available in a merchant's Zettle preliminary or liquid account at a specific time.

After a payment has been made from a merchant's liquid account to their bank account (daily, monthly or weekly), the balance is usually zero or the minimum balance in the merchant's Zettle Back Office. In other cases, you can see how much money is in the merchant's Zettle account.

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/balance
```

See example [Fetch current balance for a liquid account](#fetch-current-balance-for-a-liquid-account).

### Parameters

<details open="true">
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|accountTypeGroup |string |path |required |The type of a merchant's Zettle account. You can use one of the following account types: <br/><ul><li> `PRELIMINARY` account where transactions are to be confirmed by the merchant's issuing bank.</li><li> `LIQUID` account where transactions are to be paid out to the merchant.</li></ul>
|at |string |query |optional |Used to fetch account balance that is available at a UTC time. If it's used, any transaction after that point will be ignored. If it's not used, the balance of all transactions at the current point of time is returned. <br/>You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
</details>


### Responses
<details open="true">
<summary>HTTP status codes</summary>

|Status code |Description
|:---- |:----
|200 OK |Returned when the account balance is successfully fetched.  
|403 Forbidden |Returned when you are not authorised to fetch the account balance.
|400 Bad Request |Returned when a required parameter is missing or in a wrong format in the request .  
</details>

<details open="true">
<summary>Response attributes</summary>

|Name |Type |Description
|:---- |:---- |:----
|data |object|Information about the account balance at a point in time. It's represented by `totalBalance` and `currencyId`.
|totalBalance |integer |The account balance in the currency's smallest unit. It can be negative, such as when refunds are greater than sales. In the following example, the account balance is -3 SEK. <br/><pre>{ <br/>totalBalance: -300, <br/>currencyId: "SEK" <br/>}</pre>
|currencyId |string |The currency of the account. For example, `SEK`.
</details>


## Fetch account transactions
Returns all transactions or transactions of certain types from a merchant's Zettle preliminary or liquid account during a specific period.

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}
```

See example [Fetch transactions for a liquid account](#fetch-transactions-for-a-liquid-account).

### Parameters

<details open="true">
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|accountTypeGroup |string |path |required |The account type from which the data like transactions is retrieved. You can use one of the following account types: <br/><ul><li> `PRELIMINARY` account where transactions are to be confirmed by third-party acquirers.</li><li> `LIQUID` account where transactions are to be paid out to the merchant.</li></ul>
|start |string |query |required |A start time in UTC (inclusive) from when the transactions will be fetched. You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
|end |string |query |required |An end time in UTC (exclusive) before when the transactions will be fetched. You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
|includeTransactionType |string |query |optional |Which transaction types to fetch. <br>You can include more than one [supported transaction types](#supported-transaction-types) in a request.  
|limit |integer |query |optional |The maximum number of transactions to return in a response. You can specify `limit` with any integer greater than 0. Use `limit` and `offset` together to set response pagination.<br/>For example, to return three transaction at a time for all transactions during a specific period, set `limit` as `3` and `offset` as `0` in the first request. Then set `limit` as `3` and `offset` as `0` in the second request. 
|offset |integer |query |optional |The number of transactions to skip before beginning to return in a response. You can specify `offset` with any integer greater than 0.  Use `limit` and `offset` together to set response pagination. Use `limit` and `offset` together to set response pagination.
</details>

#### Supported transaction types
> **Note:** Deprecated transaction types are no longer in use, but may appear in historic data.
<details open="true">
<summary>Supported transaction types</summary>

|Type |Description
|:---- |:----
|ADJUSTMENT |A bookkeeping adjustment.
|ADVANCE |The cash advance given by Zettle to a merchant. A cash advance is a type of financing that is offered to merchants based on their sales history. The advance is paid back with monthly down payments.
|ADVANCE_DOWNPAYMENT |A down payment on a previously paid out cash advance.
|ADVANCE_FEE_DOWNPAYMENT |The netting of a cash advance fee.
|BANK_ACCOUNT_VERIFICATION (Deprecated) |No available description.
|CARD_PAYMENT |A card payment. Contains a reference to the card payment in the Purchase API.
|CARD_PAYMENT_FEE |The commission part of a card payment.
|CARD_PAYMENT_FEE_REFUND |The commission part of a refund.
|CARD_REFUND |A card refund. <!-- Is it not obvious? -- Accompanied by CARD_PAYMENT_FEE_REFUND. --> Contains a reference to the card payment refund in the Purchase API.
|CASHBACK |Money given to a merchant to retroactively adjust the card payment fee rate.
|CASHBACK_PAYOUT (Deprecated) |No available description.
|EMONEY_TRANSFER (Deprecated) |No available description.
|FAILED_PAYOUT |A previous payout transaction has failed and been made void. The payout money is returned to the merchant's Zettle liquid account.
|FEE_DISCOUNT_REVOCATION |An internal reclaim of an outstanding fee discount that is not consumed within a certain time frame. As these funds are reclaimed from a special fee discount account, the transaction will not be visible in the Zettle liquid account of the merchant.
|FROZEN_FUNDS |The money that is frozen to cover a chargeback. When the issuing back initiates a chargeback, the money will be removed from the merchant's liquid account and marked as frozen to cover the chargeback. If the chargeback is later revoked, the money will be returned to the merchants liquid account with a new and positive transaction of the same type. It effectively makes the initial FROZEN_FUNDS transaction void.
|INVOICE_PAYMENT |An invoice payment.
|INVOICE_PAYMENT_FEE |An invoice payment fee.
|PAYMENT |An alternative third-party payment method where Zettle handles the funds. For example, PayPal QR code and Klarna QR code.
|PAYMENT_FEE |The fee for a third-party payment method. For example,PayPal QR code and Klarna QR code.
|PAYOUT |A payout from the merchant's Zettle liquid account to the merchant’s bank account. If the merchant is a PayPal user, the payout will be made to their PayPal Wallet.  
|TELL_FRIEND (Deprecated) |No available description.
|VOUCHER_ACTIVATION |Used when activating a voucher. The money is inserted to the merchant’s fee discount account instead of preliminary and liquid accounts.
 
</details>


### Responses
<details open="true">
<summary>HTTP status codes</summary>

|Status code |Description
|:---- |:----
|200 OK |Returned when the account transactions are successfully fetched.  
|403 Forbidden |Returned when you are not authorised to fetch the account transactions.
|400 Bad Request |Returned when a required parameter is missing or in a wrong format in the request.  
</details>

<details open="true">
<summary>Response attributes</summary>

|Name |Type |Description
|:---- |:---- |:----
|data |an array of objects|A list of transactions for the given account in the descending order that shows the most recent transaction first.
|timestamp |string |Time when a transaction happens in the merchant's Zettle account. It's not the timestamp when a card transaction or a purchase happens. <br/>For example, transactions in the merchant's liquid account may happen hours after card transactions or purchases take place.
|amount |integer |The amount of money of a transaction. If the transaction is a refund or a payment fee, the amount is negative.
|originatorTransactionType |string |The transaction type. See [supported transaction types](#supported-transaction-types). 
|originatingTransactionUuid |string |The transaction identifier as UUID version 1.  
</details>


## Fetch payout information
Returns payout related information from a merchant's Zettle liquid account.

```
GET /organizations/{organizationUuid}/payout-info
```

See example [Fetch payout information on a specific period](#fetch-payout-information-on-a-specific-day).

### Parameters

<details open="true">
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|at |string |query |optional |Used to fetch payouts at a certain point in UTC time. If it's used, any transaction after that time will be ignored. If it's not used, the balance of all transactions at the current point of time is returned. <br/>You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
</details>


### Responses
<details open="true">
<summary>HTTP status codes</summary>

|Status code |Description
|:---- |:----
|200 OK |Returned when the payout information is successfully fetched.  
|403 Forbidden |Returned when you are not authorised to fetch the payout information for the account.
|400 Bad Request |Returned when a required parameter is missing or in a wrong format in the request.  
</details>

<details open="true">
<summary>Response attributes</summary>

|Name |Type |Description
|:---- |:---- |:----
|data |object|Information about the upcoming payout. 
|totalBalance |integer |The account balance in the currency's smallest unit. It can be negative, such as when refunds are greater than sales. In the following example, the account balance is -3 SEK. <br/><pre>{ <br/>totalBalance: -300, <br/>currencyId: "SEK" <br/>}</pre> 
|nextPayoutAmount |integer |The amount of money to be paid out to the merchant.  
|discountRemaining |integer |The amount of discounts that remains in merchant's vouchers. The vouchers are offered by Zettle.<br/>For example, a merchant has a voucher worthy 100 SEK from a Zettle marketing campaign. For a transaction of 200 SEK, Zettle will subtract two SEK for the transaction fee from the voucher. Then the merchant will have a remaining discount of 98 SEK. 
|periodicity |string |The period between each payout that is set by the merchant . It can be `DAILY`, `WEEKLY`, and `MONTHLY`.   
</details>


## Examples

### Fetch current balance for a liquid account
In the following example, current balance is fetched from Zettle liquid account of the merchant that has `organizationUuid` as `18071066-bfc7-11eb-8529-0242ac130003`. As `at` is not specified, by default, the current balance is fetched.

Request
```
GET/organizations/18071066-bfc7-11eb-8529-0242ac130003/accounts/liquid/balance
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
The following example fetches all transactions from Zettle liquid account of the merchant that has `organizationUuid` as `18071066-bfc7-11eb-8529-0242ac130003` from 1 January, 2020 to 7 December, 2020. With pagination set by `limit` and `offset`, three transactions are returned at a time until all transactions are fetched during that specific period.

Request
```
GET/organizations/18071066-bfc7-11eb-8529-0242ac130003/accounts/liquid/transactions?start=2021-01-01&end=2021-06-07&limit=3&offset=0
```
```
GET/organizations/18071066-bfc7-11eb-8529-0242ac130003/accounts/liquid/transactions?start=2021-01-01&end=2021-06-07&limit=3&offset=3
```

Response
```json
{
    "data": [
        {
            "timestamp": "2020-11-21T04:00:15.704+0000",
            "amount": -621,
            "originatorTransactionType": "PAYMENT_FEE",
            "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
        },
        {
            "timestamp": "2020-11-21T04:00:15.697+0000",
            "amount": 1100,
            "originatorTransactionType": "PAYMENT",
            "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
        },
        {
            "timestamp": "2020-09-10T09:51:35.162+0000",
            "amount": 5925,
            "originatorTransactionType": "FAILED_PAYOUT",
            "originatingTransactionUuid": "d8550d7a-f347-11ea-9612-3bce5300b9a9"
        }
    ]
}
```

### Fetch payout information on a specific day
The following example fetches all payout information from Zettle liquid account of the merchant that has `organizationUuid` as `18071066-bfc7-11eb-8529-0242ac130003` on 7 June, 2021.

Request
```
GET/organizations/18071066-bfc7-11eb-8529-0242ac130003/payout-info?at=2021-06-07
```
Response
```json
{
    "data": {
        "totalBalance": 195,
        "currencyId": "GBP",
        "nextPayoutAmount": 0,
        "discountRemaining": 2000,
        "periodicity": "DAILY"
    }
}
```

## Related resources
[how card payments work at Zettle](concepts/how-card-payments-work-at-Zettle.md)
[Finance API user guide](user-guides)

## Related API reference
[Purchase API reference](../purchase.adoc)