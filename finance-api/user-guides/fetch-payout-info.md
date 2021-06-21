Fetch payout information
===
Using the Finance API, you can fetch payout information from a merchant's Zettle account. By default, it's fetched from the merchant's Zettle liquid account. The payout information includes the account balance, currency, payout, periodicity, and so on. 

<!-- Is there any limit for how oldest transactions can be fetched? -->  

* [Prerequisites](#prerequisites)
* [Step 1: Fetch payout information](#fetch-payout-information)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up with the required OAuth scope using [Authorization OAuth2 API](../../authorization.adoc). 
<!-- to be continued if any -->

## Fetch payout information
You can fetch payout information from a merchant's Zettle account for any given day.

1. Fetch payout information on a specific day. If you don't specify `at`, you will get the latest payout information by default.
     
   ```
   GET /organizations/self/payout-info?at={time}
   ```

   Example:
   
   The following example fetches the payout information for 7 June, 2021.
   
   Request   
   ```
   GET /organizations/self/payout-info?at=2021-06-07
   ```
       
   Response

    ```json
    {
        "data": {
            "totalBalance": 9022,
            "currencyId": "SEK",
            "nextPayoutAmount": 0,
            "discountRemaining": 10000,
            "periodicity": "WEEKLY"
        }
    }
    ```
   > **Tip**: The periodicity can be `DAYLY`, `WEEKLY`, or `MONTHLY` according to the merchant's configuration.        

## Related task
* [Fetch account balance](fetch-account-balance.md)
* [Fetch account transactions](fetch-account-transactions.md)

## Related API reference
* [Finance API reference](../api-reference.md)