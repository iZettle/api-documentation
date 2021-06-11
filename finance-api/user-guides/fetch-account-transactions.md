Fetch account transactions
===
Using the Finance API, you can fetch all transactions or transactions of certain types from a merchant's Zettle liquid account during a specific period.

> **Note**: A merchant's Zettle liquid account contains all confirmed transactions that are already paid out or to be paid out to the merchant's bank account. If you want to check preliminary transactions that are being confirmed with buyers' banks, you can fetch them from a merchant's Zettle preliminary account.

<!-- Is there any limit for how old transactions can be fetched? -->  

* [Prerequisites](#prerequisites)
* [Step 1: Fetch all transactions during a specific period](#step-1-fetch-all-transactions-during-a-specific-period)
* [Step 2: Fetch transactions of certain types during a specific period](#step-2-fetch-transactions-of-certain-types-during-a-specific-period)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up using [Authorization OAuth2 API](../../authorization.adoc). 
<!-- to be continued if any -->

## Step 1: Fetch all transactions during a specific period
To fetch all transactions from a merchant's Zettle account during a specific period, you need to fetch them from their Zettle liquid account.
<!-- Check with team Ledger: Is it correct that a PAYMENT transaction can have a PAYOUT state? When I fetched all transactions, I got transactions that include PAYOUT. How can the integrator tell which PAYOUT is from which transaction type? --> 

1. Optional: Fetch your organisation UUID. 
   > **Tip:** You don't need to fetch the organisation UUID, as you can specify the organisation UUID as `self` in requests that require it.

    ```
    GET users/me
    ```
   Example:
       
   The following example response returns organisation UUID `ab305d54-75b4-431b-adb2-eb6b9e546013`.

    ```
    {
        "uuid": "de305d54-75b4-431b-adb2-eb6b9e546014",
        "organizationUuid": "ab305d54-75b4-431b-adb2-eb6b9e546013"
    }
    ```

2. Fetch all transactions from the merchant's Zettle liquid account.
      > **Tip:** You specify the organisation UUID as `self`.
     
   ```
   GET /organizations/{organizationUuid}/accounts/liquid/transactions?{start}&{end}
   ```

   Example:
   
   The following example request fetches all transactions from the merchant's Zettle liquid account from 1 January, 2020 to 7 June, 2021.
   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2021-01-01&end=2021-06-07
   ```
       
   The following example response returns all the transactions from 1 January, 2020 to 7 June, 2021.

    ```json
    {
        "data": [
            {
                "timestamp": "2021-03-04T04:00:07.285+0000",
                "amount": -682,
                "originatorTransactionType": "PAYMENT_FEE",
                "originatingTransactionUuid": "77db5940-7c1d-11eb-b812-d3f21f3c0d77"
            },
            {
                "timestamp": "2021-03-04T04:00:07.281+0000",
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

## Step 2: Fetch transactions of certain types during a specific period
To fetch transactions of certain types from a merchant's Zettle account during a specific period, you need to fetch them from their Zettle liquid account.

For example, you can fetch all card transactions.

1. Fetch transactions of certain types from the merchant's Zettle liquid account. In the request path, specify transaction types as you need. See [supported transction types](../api-reference.md#supported-transaction-types).
   > **Tip:** You specify the organisation UUID as `self`.
        
   ```
   GET /organizations/{organizationUuid}/accounts/liquid/transactions?{start}&{end}&{includeTransactionType}
   ```

   Example:
   
   The following example request fetches all card payments and the associated card payment fees from the merchant's Zettle liquid account from 1 January, 2020 to 7 June, 2021.
   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2021-01-01&end=2021-06-07includeTransactionType=CARD_PAYMENT&includeTransactionType=CARD_PAYMENT_FEE
   ```
       
   The following example response returns a card payment and the associated card payment fee that happened during the period from 1 January, 2020 to 7 June, 2021.

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
        ]
    }
    ```

## Related task
* [Fetch account balance](fetch-account-transactions)
* [Fetch payout information](fetch-payout-info)
* [Fetch a purchase](../../purchase.adoc#fetch-a-purchase)

## Related API reference
* [Finance API reference](../api-reference.md)
* [Purchase API reference](../../purchase.adoc)