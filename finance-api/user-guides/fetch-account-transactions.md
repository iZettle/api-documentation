Fetch account transactions
===
Use the Finance API to fetch transactions or transactions of certain types from a merchant's liquid account during a specific period.

> **Note**: A merchant's liquid account contains all confirmed transactions that are already paid out or to be paid out to the merchant. If you want to check preliminary transactions that are being checked by Zettle whether they should be paid out, you can fetch them from a merchant's preliminary account.

* [Prerequisites](#prerequisites)
* [Fetch transactions during a specific period](#fetch-transactions-during-a-specific-period)
* [Fetch transactions of certain types during a specific period](#fetch-transactions-of-certain-types-during-a-specific-period)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorisation is set up with the following OAuth scope using [OAuth2 API](../../authorization.adoc):
    * `READ:FINANCE`

## Fetch transactions during a specific period
When fetching transactions from a merchant's Zettle account during a specific period, set pagination to avoid a big dataset in a response. The transactions should be fetched from the merchant's liquid account.


1. Send a request where you set `limit` as the number of transactions to fetch and `offset` as `0`.
     
   ```
   GET /organizations/self/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}&limit={limit_value}&offset={offset_value}
   ```

   Example:
   
   The following example fetches the latest three transactions from the merchant's liquid account from 1 January, 2020 to 5 July, 2020. 
   
   Request   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-07-05&limit=3&offset=0
   ```
   Response         
   ```json
       {
           "data": [
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
               {
                   "timestamp": "2020-06-27T23:52:18.327+0000",
                   "amount": 649, // Positive in case of refund
                   "originatorTransactionType": "PAYMENT_FEE",
                   "originatingTransactionUuid": "690c99ea-b6ef-11ea-9730-7ef7aeff642d"
               },
           ]
       }
   ```
   
2. Send another request where you keep `limit` the same as in step 1 and increment `offset` with the value of `limit` . 
     
   ```
   GET /organizations/self/accounts/{accountTypeGroup}/transactions?start={start_time}&end={end_time}&limit={limit_value}&offset={offset_value}
   ```
   Example:
      
   The following example fetches the next three transactions from the merchant's liquid account from 1 January, 2020 to 5 July, 2020.
   
   Request
    
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-07-05&limit=3&offset=3
   ```
   Response
   
    ```json
    {
        "data": [
            {
                "timestamp": "2020-06-27T23:52:18.327+0000",
                "amount": -35100, // Negative in case of refund
                "originatorTransactionType": "PAYMENT",
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
        ]
    }
    ```
3. Repeat step 2 until the response is empty or it contains fewer transactions than the `limit` . 

   Example:
      
   The following example shows that last two transactions are fetched from the merchant's liquid account from 1 January, 2020 to 5 July, 2020.
   
   Request
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-01-01&end=2020-07-05&limit=3&offset=30
   ```
   Response
   
    ```json
    {
        "data": [

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

## Fetch transactions of certain types during a specific period
You can fetch transactions of certain types from a merchant's Zettle account during a specific period. For example, you can fetch all card transactions. The transactions should be fetched from the merchant's liquid account.

Send a request where you specify transaction types as you need. See [supported transaction types](../api-reference.md#supported-transaction-types).
        
   ```
   GET /organizations/self/accounts/liquid/transactions?start={start_date}&end={end_date}&includeTransactionType={includeTransactionType}
   ```
### Fetch card payment fee
The following example fetches all card payments and associated card payment fees from the merchant's liquid account. The transactions are fetched from 1 January, 2020 to 31 December, 2020.
   
   Example:
   
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

### Fetch transactions for payout
The following example fetches all the transactions that are are included in the payout from the merchant's liquid account from 6 September, 2020 to 10 September, 2020.
   
   Example:
   
   Request   
   ```
   {{FINANCE_URL}}/organizations/self/accounts/liquid/transactions?start=2020-09-06&end=2020-09-10&includeTransactionType=PAYOUT
   ```
       
   Response

   ```json
    {
        "data": [
            {
                "timestamp": "2020-09-10T09:27:28.590+0000",
                "amount": -7925,
                "originatorTransactionType": "PAYOUT",
                "originatingTransactionUuid": "d8550d7a-f347-11ea-9612-3bce5300b9a9"
            },
            {
                "timestamp": "2020-09-07T09:27:33.871+0000",
                "amount": -5925,
                "originatorTransactionType": "PAYOUT",
                "originatingTransactionUuid": "5c3db780-f0ec-11ea-8341-8d3f8a575c00"
            }
            ...
        ]
    }
   ```

## Related task
* [Fetch account balance](fetch-account-balance.md)
* [Fetch payout information](fetch-payout-info.md)
* [Fetch purchase information for transactions](fetch-purchase-information-for-transactions.md)
* [Fetch a list of purchases](../../purchase.adoc#fetch-a-list-of-purchases)


## Related API reference
* [Finance API reference](../api-reference.md)
* [Purchase API reference](../../purchase.adoc)