Fetch purchase information for transactions v1 (Deprecated)
===
Using the Finance API and the Purchase API, you can fetch purchase information for transactions. The `originatingTransactionUuid` of a transaction in the Finance API corresponds to the `uuid` of `payments` in the Purchase API.  
> **Note:** Currently, fetching purchase information is available only for transactions that are made with cards and alternative payment methods (APMs) like PayPal QRC through Zettle.

The Finance API records transactions for payment types that are supported by the Purchase API except `IZETTLE_CASH`, `SWISH`, `VIPPS`, `MOBILE_PAY`, `GIFTCARD`, `STORE_CREDIT`, and `IZETTLE_INVOICE`. 
> **Note:**  If `IZETTLE_INVOICE` is paid with a card, the purchase will be recorded as `CARD_PAYMENT` in the Finance API.

The following table shows the payment methods, payment types that are supported by the Purchase API, and the corresponding transaction types that are supported by the Finance API. 

|Payment methods |Payment types by the Purchase API |Corresponding transaction type by the Finance API |
|:---|:--- |:--- 
|Card payment |`IZETTLE_CARD` |`CARD_PAYMENT`, `CARD_PAYMENT_FEE` |
|Card payment online |`IZETTLE_CARD_ONLINE` |`CARD_PAYMENT`, `CARD_PAYMENT_FEE` | 
|Card refund |`IZETTLE_CARD` |`CARD_REFUND`, `CARD_PAYMENT_FEE_REFUND` |
|APM |`PayPal QRC`, `KLARNA` |`PAYMENT`, `PAYMENT_FEE` |  

* [Prerequisites](#prerequisites)
* [Step 1: Find the transaction UUID with the Finance API](#step-1-find-the-transaction-uuid-with-the-finance-api)
* [Step 2: Fetch purchase information for transactions with the Purchase API](#step-2-fetch-purchase-information-for-transactions-with-the-purchase-api)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorisation is set up with the following OAuth scope using [OAuth2 API](../../../authorization.md):
    * `READ:FINANCE`
    * `READ:PURCHASE`

## Step 1: Find the transaction UUID with the Finance API
Currently, fetching purchase information is available only for transactions that are made with cards and APMs like PayPal QRC through Zettle. 

Before you can find the transaction UUID, you need to specify the transactions types to fetch all transactions of a specific payment method.   

1. Fetch all transactions of one or more transaction types during a specific period.
   > **Tip**: If you plan to look up purchase information for transactions that are ready for payout, specify `accountTypeGroup` as `liquid`.  

    ```
    GET /organizations/self/accounts/{accountTypeGroup}/transactions?start={start}&end={end}&includeTransactionType={includeTransactionType}
    ```
   Example:
   
   The following example request fetches APM transactions of the merchant's Zettle liquid account.
   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2020-06-01&end=2020-12-31&includeTransactionType=PAYMENT&includeTransactionType=PAYMENT_FEE
   ```
       
   The following example response returns all transactions for payments and payment fees from 1 June, 2020 to 31 December, 2020.

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
                "timestamp": "2020-06-01T04:00:06.680+0000",
                "amount": -75,
                "originatorTransactionType": "PAYMENT_FEE",
                "originatingTransactionUuid": "e548933c-53fb-4a99-bbc5-c31f7861bcc3"
            },
            {
                "timestamp": "2020-06-01T04:00:06.672+0000",
                "amount": 3000,
                "originatorTransactionType": "PAYMENT",
                "originatingTransactionUuid": "e548933c-53fb-4a99-bbc5-c31f7861bcc3"
            }
        ]
    }
    ```

3. In the response, find and save the value of `originatingTransactionUuid` for transactions for which you want to fetch purchase information.

## Step 2: Fetch purchase information for transactions with the Purchase API
Using the value of `originatingTransactionUuid` of a transaction in the Finance API, you can use the Purchase API to locate the purchase information for the transaction.

> **Note:** For refunds, a new "refund purchase" record is created with a negative amount instead. It represents a refund of an original purchase. Transactions of the type `CARD_REFUND` in the Finance API refers to this new "refund purchase" instead of the original purchase.

1. Fetch purchases regularly, for example every midnight, and store the data in a local database. In the request parameters, set the `startDate` to the last time of fetching purchases.
    
    ```
    GET /purchases/v2?startDate={startDate}
    ```
      
    Example:
    
    The following example request fetches purchase information from 19 November, 2020.
    ```
    /purchases/v2/?startDate=2020-11-19
    ```

2. In the local database, find the purchase by iterating through each purchase's payments list and searching for a payment with the same `uuid` as the transaction's `originatingTransactionUuid` that you saved in [Step 1: Find the transaction UUID with the Finance API](#step-1-find-the-transaction-uuid-with-the-finance-api).

   > **Tip:** In the response, you can search through all payments in all purchases using a JSONPath `$.purchases[*].payments[*].uuid`.

   Example:
   
   In the following example response that returns purchase information from 19 November, 2020, search for `6820265b-953e-43a7-bb65-abac1ef104bf` that is the value of `originatingTransactionUuid` of the transaction. It's the same value of `uuid` of the `payments` that belongs to the purchase with `purchaseUUID1` as `2252b412-f531-405b-a073-1b3b9f2bbdd4`.
       
   ```json
    {
        "purchases": [
            {
                "source": "POS",
                "purchaseUUID": "GV2NDCpCEeuqwlj953Nabw",
                "amount": 49950,
                "vatAmount": 9990,
                "taxAmount": 9990,
                "country": "SE",
                "currency": "SEK",
                "timestamp": "2020-11-19T08:35:04.230+0000",
                "purchaseNumber": 12496,
                "globalPurchaseNumber": 12496,
                "userDisplayName": "iZettle DEMO SE",
                "userId": 872591,
                "organizationId": 6394297,
                "products": [
                     ...
                ],
                "discounts": [
                     ...
                ],
                "payments": [
                    {
                    "uuid": "6820265b-953e-43a7-bb65-abac1ef104bf",
                    "receiverOrganization": "48eb8d8a-ae8f-4940-83e9-485d80f21aa0",
                    "amount": 1100,
                    "type": "KLARNA",
                    "currency": "SEK",
                    "country": "SE",
                    ...
                        }
                    }
                ],
                ...
            "created": "2020-11-20T08:36:56.419+0000",
            "refunded": false,
            "purchaseUUID1": "2252b412-f531-405b-a073-1b3b9f2bbdd4",
            "groupedVatAmounts": {
                "12.0": 1000,
                "25.0": 100
            },
            "refund": false   
            },
            ...
        ],
        "firstPurchaseHash": "1605774904230GV2NDCpCEeuqwlj953Nabw",
        "lastPurchaseHash": "1605882295421KEx7SCs8EeuzPiiU-CPahg",
        "linkUrls": []
    }
   ```

## Related task
* [Fetch account transactions](fetch-account-transactions.md)
* [Fetch a list of purchases](../../../purchase.adoc#fetch-a-list-of-purchases)

## Related API reference
* [Finance API reference](../api-reference.md)
* [Purchase API reference](../../../purchase.adoc)