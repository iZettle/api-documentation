Fetch purchase information for card transactions
===
Using the Finance API and the Purchase API, you can fetch purchase information for transactions of payments and refunds that are made with a Zettle card reader. 

* [Prerequisites](#prerequisites)
* [Step 1: Find the card transaction UUID](#step-1-find-the-card-transaction-uuid)
* [Step 2: Fetch purchase information for card transactions](#step-2-fetch-purchase-information-for-card-transactions)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up using [Authorization OAuth2 API](../../authorization.adoc). 
<!-- to be continued if any -->

## Step 1: Find the card transaction UUID
Using the Finance API, find the transaction UUID for card transactions for which you will fetch purchase information.  

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
       
2. Fetch the account card transactions. 
   > **Tip:** You specify the organisation UUID as `self`.

    ```
    GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions?{start}&{end}&includeTransactionType=CARD_PAYMENT&includeTransactionType=CARD_REFUND
    ```
   Example:
   
   The following example request fetches card transactions of the merchant's Zettle liquid account.
   
   ```
   GET /organizations/self/accounts/liquid/transactions?start=2017-01-01&end=2021-06-07&includeTransactionType=CARD_PAYMENT&includeTransactionType=CARD_REFUND
   ```
       
   The following example response returns `originatingTransactionUuid` as `aefbced2-9728-11eb-aa46-1cae508bb7b5` for a card payment transaction and `originatingTransactionUuid` as `a4b1fc70-3427-11e9-89d7-88df8a0a0855` for a card refund transaction.

    ```json
    {
       "data": [
                   {
                       "timestamp": "2021-04-06T22:37:37.621+0000",
                       "amount": 100,
                       "originatorTransactionType": "CARD_PAYMENT",
                       "originatingTransactionUuid": "aefbced2-9728-11eb-aa46-1cae508bb7b5"
                   },
                   {
                       "timestamp": "2021-04-06T10:30:12.293+0000",
                       "amount": -260,
                       "originatorTransactionType": "CARD_REFUND",
                       "originatingTransactionUuid": "a4b1fc70-3427-11e9-89d7-88df8a0a0855"
                   },
           ...
       ]
   }
    ```

3. In the response, find and save the value of `originatingTransactionUuid` and `timestamp` for transactions for which you want to fetch purchase information. This is the key used to sign all requests and should be stored so that you can validate the request.


## Step 2: Fetch purchase information for card transactions
Using the Purchase API and the value of `originatingTransactionUuid` and `timestamp` of card transactions, you can fetch purchase information for the transaction.

1. Fetch purchase information for the card transactions. In the request path, set `startDate` and `endDate` to include the time in `timestamp` of the transactions that you saved in [Step 1: Find the card transaction UUID](#step-1-find-the-card-transaction-uuid).
   > **Tip:** Set `startDate` and `endDate` with a few minutes before and after the time in `timestamp` to fetch a small amount of purchase information for pinpointing the transactions.
    
    ```
    GET /purchases/v2/?{startDate}&{endDate}
    ```
    
   Example:
    
   The following example request fetches purchase information from a few minutes before the `timestamp` as `2021-04-06T22:37:37.621+0000` to a few minutes after that time on 6 April, 2021.
    
    ```
    GET /purchases/v2/?startDate=2021-04-06T22:32&endDate=2021-04-06T22:40
    ```

2. In the response, search for the value of `originatingTransactionUuid` of the transactions that you saved in [Step 1: Find the card transaction UUID](#step-1-find-the-card-transaction-uuid).

    In the following example response that returns purchase information from 6 April, 2021 to 7 April, 2021, search for `aefbced2-9728-11eb-aa46-1cae508bb7b5` that is the value of `originatingTransactionUuid`.
    
    ```json
    {
        "purchases": [
        ...
            {
                "source": "POS",
                "purchaseUUID": "XTmwwJcoEeuQQ9Xdh4qONQ",
                ...
                "products": [
                    {
                        "quantity": "1",
                        "productUuid": "979d7560-92ca-11eb-bd1f-3c495a76a2c0",
                        "variantUuid": "97a11ee0-92ca-11eb-af1b-8e697243a8ff",
                        "vatPercentage": 0.0,
                        "taxRates": [
                            {
                                "percentage": 0
                            }
                        ],
                        "taxExempt": false,
                        "unitPrice": 100,
                        "rowTaxableAmount": 100,
                        "name": "cat grass",
                        "description": "",
                        "barcode": "",
                        "autoGenerated": false,
                        "id": "3",
                        "type": "PRODUCT",
                        "grossValue": 100,
                        "grossTax": 0,
                        "libraryProduct": true
                    }
                ],
                "discounts": [],
                "payments": [
                    {
                        "uuid": "aefbced2-9728-11eb-aa46-1cae508bb7b5",
                        "amount": 100,
                        "type": "IZETTLE_CARD_ONLINE",
                        "attributes": {
                            ...
                            "referenceNumber": "3CBIZN4SVF",
                            "paymentlinkOrderUuid": "5d39b0c0-9728-11eb-9043-d5dd878a8e35"
                        }
                    }
                ],
                "receiptCopyAllowed": true,
                "references": {
                    "paymentlinkOrderLink": "https://pay.izettle.com/?-JN_dRcfX",
                    "paymentlinkOrderUuid": "5d39b0c0-9728-11eb-9043-d5dd878a8e35",
                    "paymentlinkOrderCreated": "2021-04-06T22:37:37.695+0000",
                    "paymentlinkOrderReference": "Zettle"
                },
                "note": "",
                "taxationMode": "INCLUSIVE",
                "taxationType": "VAT",
                "taxValues": [
                    {
                        "label": null,
                        "taxValue": 0,
                        "taxAmount": 0,
                        "totalAmount": 100,
                        "taxableAmount": 100
                    }
                ],
                "created": "2021-04-06T22:37:38.042+0000",
                "refunded": false,
                "purchaseUUID1": "5d39b0c0-9728-11eb-9043-d5dd878a8e35",
                "groupedVatAmounts": {
                    "0.0": 100
                },
                "refund": false
            }
        ],
        "firstPurchaseHash": "1617706096329lmGFrJbFEeuVcBL7n_fOsw",
        "lastPurchaseHash": "1617748658042XTmwwJcoEeuQQ9Xdh4qONQ",
        "linkUrls": []
    }
    ```

## Related task
* [Fetch account transactions](fetch-account-transactions.md)
* [Fetch a purchase](../../purchase.adoc#fetch-a-purchase)

## Related API reference
* [Finance API reference](../api-reference.md)
* [Purchase API reference](../../purchase.adoc)