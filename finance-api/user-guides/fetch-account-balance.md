Fetch account balance
===
Using the Finance API, you can fetch the balance that is available in the merchant's Zettle preliminary or liquid account.

* [Prerequisites](#prerequisites)
* [Fetch the account balance](#fetch-the-account-balance)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up with the required OAuth scope using [Authorization OAuth2 API](../../authorization.adoc).  
<!-- to be continued if any -->

## Fetch the account balance 
Fetch the account balance in the merchant's Zettle preliminary or liquid account.  

1. Fetch the balance in one or both of the merchant's preliminary and liquid accounts as follows:
   * To check the balance of transactions that are still in the process of being cleared by the acquiring bank, fetch the preliminary account balance:
     ```
     GET /organizations/self/accounts/preliminary/balance
     ```
   * To check the balance of transactions that are ready to be paid out to the merchant, fetch the liquid account balance:
     ```
     GET /organizations/self/accounts/liquid/balance
     ```

   Example:
   
   The following example request fetches account balance in the merchant's Zettle preliminary account.
   
   ```
   GET /organizations/self/accounts/preliminary/balance
   ```
       
   The following example response shows that £1.00 is still in the process of being cleared by the acquiring bank.

    ```json
    {
        "data": {
            "totalBalance": 100,
            "currencyId": "GBP"
        }
    }    

 
## Related task
* [Fetch account transactions](fetch-account-transactions.md)
* [Fetch payout information](fetch-payout-info.md)

## Related API reference
* [Finance API reference](../api-reference.md)