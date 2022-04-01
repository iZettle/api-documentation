Fetch account balance v2
===
Use the Finance API to fetch the account balance of the merchant's preliminary or liquid account.

* [Prerequisites](#prerequisites)
* [Fetch the account balance](#fetch-the-account-balance)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorisation is set up with the following OAuth scope using [OAuth2 API](../../authorization.md):
    * `READ:FINANCE`

## Fetch the account balance 
Fetch the balance in one or both of the merchant's preliminary and liquid accounts as follows:
   * To check the balance of transactions that are still being checked by Zettle whether they should be paid out, fetch the preliminary account balance:
     ```
     GET /v2/accounts/preliminary/balance
     ```
   * To check the balance of transactions that are ready to be paid out to the merchant, fetch the liquid account balance:
     ```
     GET /v2/accounts/liquid/balance
     ```

   Example:
   
   The following example fetches account balance in the merchant's preliminary account. The response shows that £1 is still being cleared by the acquiring bank.
   
   Request
   
   ```
   GET /v2/accounts/preliminary/balance
   ```
   Response

   ```json
    {
        "data": {
            "totalBalance": 100,
            "currencyId": "GBP"
        }
    }    
   ```
 
## Related task
* [Fetch account transactions](fetch-account-transactions-v2.md)
* [Fetch payout information](fetch-payout-info-v2.md)

## Related API reference
* [Finance API reference](../api-reference-v2.yaml)