Fetch account balance
===
Use the Finance API to fetch the account balance of the merchant's preliminary or liquid account.

* [Prerequisites](#prerequisites)
* [Fetch the account balance](#fetch-the-account-balance)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up with the required OAuth scope using [Authorization OAuth2 API](../../authorization.adoc).  
<!-- to be continued if any -->

## Fetch the account balance 
Fetch the balance in one or both of the merchant's preliminary and liquid accounts as follows:
   * To check the balance of transactions that are still being checked by Zettle whether they should be paid out, fetch the preliminary account balance:
     ```
     GET /organizations/self/accounts/preliminary/balance
     ```
   * To check the balance of transactions that are ready to be paid out to the merchant, fetch the liquid account balance:
     ```
     GET /organizations/self/accounts/liquid/balance
     ```

   Example:
   
   The following example fetches account balance in the merchant's preliminary account. The response shows that £1 is still being cleared by the acquiring bank.
   
   Request
   
   ```
   GET /organizations/self/accounts/preliminary/balance
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
* [Fetch account transactions](fetch-account-transactions.md)
* [Fetch payout information](fetch-payout-info.md)

## Related API reference
* [Finance API reference](../api-reference.md)