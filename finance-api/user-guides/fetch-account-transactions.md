Fetch account transactions
===
Using the Finance API, you can fetch all transactions or transactions of certain types from a merchant's Zettle liquid account during a specific period.

> **Note**: A merchant's Zettle liquid account contains all confirmed transactions that are already paid out or to be paid out to the merchant. If you want to check preliminary transactions that are being confirmed with the issuing banks (buyers' banks), you can fetch them from a merchant's Zettle preliminary account.

<!-- Is there any limit for how old transactions can be fetched? -->  

* [Prerequisites](#prerequisites)
* [Fetch all transactions during a specific period](#fetch-all-transactions-during-a-specific-period)
* [Fetch transactions of certain types during a specific period](#fetch-transactions-of-certain-types-during-a-specific-period)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up with the required OAuth scope using [Authorization OAuth2 API](../../authorization.adoc). 

## Fetch all transactions during a specific period
To fetch all transactions from a merchant's Zettle account during a specific period, you need to fetch them from their Zettle liquid account.
<!-- Check with team Ledger: Is it correct that a PAYMENT transaction can have a PAYOUT state? When I fetched all transactions, I got transactions that include PAYOUT. How can the integrator tell which PAYOUT is from which transaction type? --> 

1. Fetch all transactions from the merchant's Zettle liquid account with pagination.
     
   ```
   GET /organizations/self/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}&{limit}&{offset}
   ```

   Example:
   
   The following example requests fetch all transactions from the merchant's Zettle liquid account from 1 January, 2020 to 31 December, 2020. The requests fetch three transactions at a time. 
   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&limit=3&offset=0
   ```
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&limit=3&offset=3
   ```
       
   In the following example response, three transactions are returned at a time until all the transactions are returned from 1 January, 2020 to 31 December, 2020. 

    ```json
    {
        "data": [
            {
                "timestamp": "2020-12-04T04:00:07.285+0000",
                "amount": -682,
                "originatorTransactionType": "PAYMENT_FEE",
                "originatingTransactionUuid": "77db5940-7c1d-11eb-b812-d3f21f3c0d77"
            },
            {
                "timestamp": "2020-12-04T04:00:07.281+0000",
                "amount": 3300,
                "originatorTransactionType": "PAYMENT",
                "originatingTransactionUuid": "77db5940-7c1d-11eb-b812-d3f21f3c0d77"
            },
            {
                "timestamp": "2020-11-21T04:00:15.704+0000",
                "amount": -621,
                "originatorTransactionType": "CARD_PAYMENT_FEE",
                "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
            },
            {
                "timestamp": "2020-11-21T04:00:15.697+0000",
                "amount": 1100,
                "originatorTransactionType": "CARD_PAYMENT",
                "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
            },
            {
                "timestamp": "2020-09-10T09:51:35.162+0000",
                "amount": 5925,
                "originatorTransactionType": "FAILED_PAYOUT",
                "originatingTransactionUuid": "d8550d7a-f347-11ea-9612-3bce5300b9a9"
            },
            {
                "timestamp": "2020-09-10T09:27:28.590+0000",
                "amount": -5925,
                "originatorTransactionType": "PAYOUT",
                "originatingTransactionUuid": "d8550d7a-f347-11ea-9612-3bce5300b9a9"
            },
            ...
        ]
    }
    ```

## Fetch transactions of certain types during a specific period
To fetch transactions of certain types from a merchant's Zettle account during a specific period, you need to fetch them from their Zettle liquid account.

For example, you can fetch all card transactions.

1. Fetch transactions of certain types from the merchant's Zettle liquid account. In the request query, specify transaction types as you need. See [supported transaction types](../api-reference.md#supported-transaction-types).
        
   ```
   GET /organizations/self/accounts/liquid/transactions?start={start_date}&end={end_date}&includeTransactionType={includeTransactionType}
   ```

   Example:
   
   The following example request fetches all card payments and the associated card payment fees from the merchant's Zettle liquid account from 1 January, 2020 to 31 December, 2020.
   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&includeTransactionType=CARD_PAYMENT&includeTransactionType=CARD_PAYMENT_FEE
   ```
       
   In the following example response, the Finance API returns a card payment and the associated card payment fee that happened during the period from 1 January, 2020 to 31 December, 2020.

    ```json
    {
        "data": [
            {
                "timestamp": "2020-11-21T04:00:15.704+0000",
                "amount": -621,
                "originatorTransactionType": "CARD_PAYMENT_FEE",
                "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
            },
            {
                "timestamp": "2020-11-21T04:00:15.697+0000",
                "amount": 1100,
                "originatorTransactionType": "CARD_PAYMENT",
                "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
            },
            ...
        ]
    }
    ```

## Related task
* [Fetch account balance](fetch-account-balance.md)
* [Fetch payout information](fetch-payout-info.md)
* [Fetch a list of purchases](../../purchase.adoc#fetch-a-list-of-purchases)


## Related API reference
* [Finance API reference](../api-reference.md)
* [Purchase API reference](../../purchase.adoc)