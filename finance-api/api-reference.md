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

If the balance is zero, it means the following:

* For the preliminary account, the money is moved from the merchant's preliminary account to their liquid account.
* For the liquid account, the money is paid out from Zettle to the mechant's bank account.
<!-- Returns accumulated balance at a specific point in time. After a deposit has been made from LIQUID account to merchant’s bank account (daily, monthly or weekly), the balance is usually zero. If not, it will return the amount that is still due in merchant’s Zettle account. -->

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/balance
```

See example [Fetch balance for a liquid account](#fetch-balance-for-a-liquid-account).

### Parameters

<details open="true">
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|accountTypeGroup |string |path |required |The account type from which the data like transactions is retrieved. You can use one of the following account types: <br/><ul><li> `PRELIMINARY` account where transactions are to be confirmed by third-party acquirers.</li><li> `LIQUID` account where transactions are to be paid out to the merchant.</li></ul>
|at |string |query |optional |Used to fetch account balance that is available at a time in the UTC time zone. If it's used, any transaction after that point will be ignored. If it's not used, the balance of all transactions at the current point of time is returned. <br/>You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
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
|totalBalance |integer |The account balance. It can be negative. For example, `-300`
|currencyId |string |The currency of the account balance. For example, `SEK`.
</details>


## Fetch account transactions
Returns all transactions or transactions of certain types from a merchant's Zettle preliminary or liquid account during a specific period.

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions?{start}&{end}
```

See example [Fetch transactions for a liquid account](#fetch-transactions-for-a-liquid-account).

### Parameters

<details open="true">
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|:---- |:---- |:---- |:---- |:----
|organizationUuid |string |path |required |Unique identifier for your organization. You can specify the value with one of the following: <br/><ul><li> Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li></ul> 
|accountTypeGroup |string |path |required |The account type from which the data like transactions is retrieved. You can use one of the following account types: <br/><ul><li> `PRELIMINARY` account where transactions are to be confirmed by third-party acquirers.</li><li> `LIQUID` account where transactions are to be paid out to the merchant.</li></ul>
|start |string |query |required |A start time (inclusive) in the UTC time zone from when the transactions will be fetched. You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
|end |string |query |required |An end time (exclusive) in the UTC time zone before when the transactions will be fetched. You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
|includeTransactionType |string |query |optional |Which transaction types to fetch. <br>You can include more than one [supported transaction types](#supported-transaction-types) in a request. <br/>If you want to use `includeTransactionType`, don't use `limit` and `offset`. Because `limit` and `offset` will be ignored when `includeTransactionType` is used.  
|limit |integer |query |optional |How many transactions to return in total in a response. You can specify `limit` with any integer greater than 0. <br/>If you want to use `limit` and `offset`, don't use `includeTransactionType`. Because `limit` and `offset` will be ignored when `includeTransactionType` is used. 
|offset |integer |query |optional |How many transactions to return per page in a response. You can specify `offset` with any integer greater than 0. <br/>If you want to use `limit` and `offset`, don't use `includeTransactionType`. Because `limit` and `offset` will be ignored when `includeTransactionType` is used. 
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
|BANK_ACCOUNT_VERIFICATION (Deprecated) |N/A
|CARD_PAYMENT |A card payment. Contains a reference to the card payment in the Purchase API.
|CARD_PAYMENT_FEE |The commission part of a card payment.
|CARD_PAYMENT_FEE_REFUND |The commission part of a refund.
|CARD_REFUND |A card refund. <!-- Is it not obvious? -- Accompanied by CARD_PAYMENT_FEE_REFUND. --> Contains a reference to the card payment refund in the Purchase API.
|CASHBACK |Money given to a merchant to retroactively adjust the card payment fee rate.
|CASHBACK_PAYOUT (Deprecated) |N/A
|EMONEY_TRANSFER (Deprecated) |N/A
|FAILED_PAYOUT |A previous payout transaction has failed and been made void. The payout money is returned to the merchant's Zettle liquid account.
|FEE_DISCOUNT_REVOCATION |An internal reclaim of an outstanding fee discount that is not consumed within a certain time frame. As these funds are reclaimed from a special fee discount account, the transaction will not be visible in the Zettle liquid account of the merchant.
|FROZEN_FUNDS |The money that is frozen to cover a chargeback. When the issuing back initiates a chargeback, the money will be removed from the merchant's liquid account and marked as frozen to cover the chargeback. If the chargeback is later revoked, the money will be returned to the merchants liquid account with a new and positive transaction of the same type. It effectively makes the initial FROZEN_FUNDS transaction void.
|INVOICE_PAYMENT |An invoice payment.
|INVOICE_PAYMENT_FEE |An invoice payment fee.
|PAYMENT |An alternative third-party payment method where Zettle handles the funds. For example, PayPal QR code and Klarna QR code.
|PAYMENT_FEE |The fee for a third-party payment method. For example,PayPal QR code and Klarna QR code.
|PAYOUT |A payout from the merchant's Zettle liquid account to the merchant’s bank account .  
|TELL_FRIEND (Deprecated) |N/A
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
|data |object|Information about the account transactions. 
|timestamp |string |Time when a transaction happened. 
|amount |integer |The amount of money of a transaction. If the transaction is a refund, the amount is negative.
|originatorTransactionType |string |The transaction type.
|originatingTransactionUuid |string |The transaction identifier as UUID.  
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
|at |string |query |optional |Used to fetch payouts until a time in the UTC time zone. If it's used, any transaction after that point will be ignored. If it's not used, the balance of all transactions at the current point of time is returned. <br/>You can specify a UTC time in one of the following formats: <br/><ul><li>`YYYY-MM-DD` to specify a date. For example, `2020-11-29`.</li><li>`YYYY-MM-DDThh:mm:ss` to specify a time. For example, `2020-11-29T03:10:02`.</li></ul>
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
|data |object|Information about the account payout 
|totalBalance |integer |The account balance. It can be negative. For example, `-300`.
|currencyId |string |The currency of the account balance. For example, `SEK`. 
|nextPayoutAmount |integer |The amount of money that is in the `liquid` status and is ready to be paid out to the merchant.  
|discountRemaining |integer |The amount of discounts that remains in merchant's vouchers.  
|periodicity |string |The interval between each payout. It can be `DAILY`, `WEEKLY`, and `MONTHLY`.   
</details>


## Examples

### Fetch balance for a liquid account
The following example fetches balance from Zettle liquid account of the merchant that has `organizationUuid` as `18071066-bfc7-11eb-8529-0242ac130003`. As `at` is not specified for a time point, by default, it fetches the balance that is available at the moment of fecthing.

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
The following example fetches all transactions from Zettle liquid account of the merchant that has `organizationUuid` as `18071066-bfc7-11eb-8529-0242ac130003` from 1 January, 2021 to 7 June, 2021.

Request
```
GET/organizations/18071066-bfc7-11eb-8529-0242ac130003/accounts/liquid/transactions?start=2021-01-01&end=2021-06-07
```
Response
```json
{
    "data": [
        {
            "timestamp": "2021-04-06T22:37:37.621+0000",
            "amount": 100,
            "originatorTransactionType": "CARD_PAYMENT",
            "originatingTransactionUuid": "aefbced2-9728-11eb-aa46-1cae508bb7b5"
        },
        ...
        {
            "timestamp": "2021-04-06T16:00:20.127+0000",
            "amount": 200,
            "originatorTransactionType": "CARD_PAYMENT",
            "originatingTransactionUuid": "473778b2-96f0-11eb-bd49-31909a42aea2"
        },
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
[how card payments work at Zettle](overview.md/#understand-how-card-payments-work-at-Zettle)
[Finance API user guide](user-guides)

## Related API reference
[Purchase API reference](../purchase.adoc)