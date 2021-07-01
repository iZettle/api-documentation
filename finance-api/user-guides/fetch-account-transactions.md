Fetch account transactions
===
Use the Finance API to fetch transactions or transactions of certain types from a merchant's liquid account during a specific period.

> **Note**: A merchant's liquid account contains all confirmed transactions that are already paid out or to be paid out to the merchant. If you want to check preliminary transactions that are being confirmed with the issuing banks (buyers' banks), you can fetch them from a merchant's preliminary account.

<!-- Is there any limit for how old transactions can be fetched? -->  

* [Prerequisites](#prerequisites)
* [Fetch transactions during a specific period](#fetch-transactions-during-a-specific-period)
* [Fetch transactions of certain types during a specific period](#fetch-transactions-of-certain-types-during-a-specific-period)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up with the required OAuth scope using [Authorization OAuth2 API](../../authorization.adoc). 

## Fetch transactions during a specific period
When fetching transactions from a merchant's Zettle account during a specific period, set pagination to avoid a big dataset in a response. The transactions should be fetched from the merchant's liquid account.


1. Send a request where `limit` is set as the number of transactions to fetch and `offset` set as `0`.
     
   ```
   GET /organizations/self/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}&{limit}={limit_value}&offset={offset_value}
   ```

   Example:
   
   The following example fetches the latest three transactions from the merchant's liquid account from 1 January, 2020 to 31 December, 2020. 
   
   Request   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&limit=3&offset=0
   ```
   Response         
   ```json
       {
           "data": [
               {
                   "timestamp": "2020-12-30T20:16:44.309+0000",
                   "amount": 381,
                   "originatorTransactionType": "CARD_PAYMENT_FEE_REFUND",
                   "originatingTransactionUuid": "30cef6e2-be09-11ea-a8e4-bce028663c34"
               },
               {
               "timestamp": "2020-12-30T20:16:44.309+0000",
               "amount": -20610,
               "originatorTransactionType": "CARD_REFUND",
               "originatingTransactionUuid": "30cef6e2-be09-11ea-a8e4-bce028663c34"
               },
               {
               "timestamp": "2020-12-27T23:52:18.327+0000",
               "amount": 649,
               "originatorTransactionType": "PAYMENT_FEE", // In case of refund
               "originatingTransactionUuid": "690c99ea-b6ef-11ea-9730-7ef7aeff642d"
               },
           ]
       }
   ```
   
2. Send another request where `limit` and `offset` are set the same as `limit` in the initial request. 
     
   ```
   GET /organizations/self/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}&{limit}={limit_value}&offset={offset_value}
   ```
   Example:
      
   The following example fetches the next three transactions from the merchant's liquid account from 1 January, 2020 to 31 December, 2020.
   
   Request
    
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&limit=3&offset=3
   ```
   Response
   
    ```json
    {
        "data": [
            {
                "timestamp": "2020-12-27T23:52:18.327+0000",
                "amount": -35100,
                "originatorTransactionType": "PAYMENT", // In case of refund
                "originatingTransactionUuid": "690c99ea-b6ef-11ea-9730-7ef7aeff642d"
            },
            {
                 "timestamp": "2020-12-26T19:51:38.161+0000",
                 "amount": -649,
                 "originatorTransactionType": "PAYMENT_FEE",
                 "originatingTransactionUuid": "45f51ed4-b7bf-11ea-90de-435a3f6e4738"
            },
            {
                 "timestamp": "2020-12-26T19:51:38.161+0000",
                 "amount": 35100,
                 "originatorTransactionType": "PAYMENT",
                 "originatingTransactionUuid": "45f51ed4-b7bf-11ea-90de-435a3f6e4738"
            },
        ]
    }
    ```
3. Repeat step 2 until the response is empty or it contains fewer transactions than the `limit` . 

   Example:
      
   The following example shows that last two transactions are fetched from the merchant's liquid account from 1 January, 2020 to 31 December, 2020.
   
   Request
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&limit=3&offset=3
   ```
   Response
   
    ```json
    {
        "data": [
             {
                "timestamp": "2020-01-01T11:15:40.144+0000",
                "amount": 470433,
                "originatorTransactionType": "FAILED_PAYOUT",
                "originatingTransactionUuid": "5d4846ae-b78f-11ea-b003-01f0540f969e"
             },
             {
                "timestamp": "2020-01-01T09:28:16.144+0000",
                "amount": -470433,
                "originatorTransactionType": "PAYOUT",
                "originatingTransactionUuid": "5d4846ae-b78f-11ea-b003-01f0540f969e"
             },
        ]
    }
    ```



## Fetch transactions of certain types during a specific period
You can fetch transactions of certain types from a merchant's Zettle account during a specific period. For example, you can fetch all card transactions. The transactions should be fetched from the merchant's liquid account.

Send a request where you specify transaction types as you need. See [supported transaction types](../api-reference.md#supported-transaction-types).
        
   ```
   GET /organizations/self/accounts/liquid/transactions?start={start_date}&end={end_date}&includeTransactionType={includeTransactionType}
   ```

   Example:
   
   The following example fetches all card payments and the associated card payment fees from the merchant's liquid account from 1 January, 2020 to 31 December, 2020.
   
   Request   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-12-31&includeTransactionType=CARD_PAYMENT&includeTransactionType=CARD_PAYMENT_FEE
   ```
       
   Response

   ```json
    {
        "data": [
            {
                "timestamp": "2020-11-21T04:00:15.704+0000",
                "amount": -8867,
                "originatorTransactionType": "CARD_PAYMENT_FEE",
                "originatingTransactionUuid": "6820265b-953e-43a7-bb65-abac1ef104bf"
            },
            {
                "timestamp": "2020-11-21T04:00:15.697+0000",
                "amount": 479300,
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