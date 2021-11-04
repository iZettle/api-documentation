Fetch payout information
===
Use the Finance API to fetch payout information from a merchant's account. The payout information includes the account balance, currency, payout, periodicity, and remaining discounts. 

A payout is to deposit the account balance to the merchant's bank account or their PayPal Wallet for PayPal users at the [scheduled time](https://www.zettle.com/help/articles/1084784-deposits). The scheduled time varies depending on the country.

If the merchant's configuration has a minimum account balance, then the payout will deposit the account balance minus the minimum account balance.

* [Prerequisites](#prerequisites)
* [Fetch the payout information](#fetch-the-payout-information)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorisation is set up with the following OAuth scope using [OAuth2 API](../../authorization.md):
    * `READ:FINANCE`

## Fetch the payout information
You can fetch payout information from a merchant's Zettle account for any given day.

Fetch payout information on a specific day. If you don't specify `at`, you will get the latest payout information by default.
     
   ```
   GET /organizations/self/payout-info?at={when_to_fetch_payout}
   ```

   Example:
   
   The following example fetches the payout information for 7 June, 2021. The next payout amount will be £6605.89.
   
   Request   
   ```
   GET /organizations/self/payout-info?at=2021-06-07
   ```
   Response   
   ```json
      {
           "data": {
               "totalBalance": 660589,
               "currencyId": "GBP",
               "nextPayoutAmount": 660589,
               "discountRemaining": 0,
               "periodicity": "DAILY"
           }
      }        
   ```

   > **Tip**: The periodicity can be `DAYLY`, `WEEKLY` or `MONTHLY` according to the merchant's configuration.        

## Related task
* [Fetch account balance](fetch-account-balance.md)
* [Fetch account transactions](fetch-account-transactions.md)

## Related API reference
* [Finance API reference](../api-reference.md)