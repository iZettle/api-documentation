Fetch account balance
===
Using the Finance API, you can fetch the balance that is available in the merchant's Zettle preliminary or liquid account.

* [Prerequisites](#prerequisites)
* [Step 1: Fetch the account balance](#step-1-fetch-the-account-balance)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up using [Authorization OAuth2 API](../../authorization.adoc). 
<!-- to be continued if any -->

## Step 1: Fetch the account balance 
Fetch the account balance in the merchant's Zettle preliminary or liquid account.  

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

2. Fetch the balance in one or both of the merchant's preliminary and liquid accounts as follows:
   > **Tip:** You specify the organisation UUID as `self`.
   * To check the balance of transactions that are still being confirmed by the third-party acquirer with the buyer's bank, fetch the preliminary account balance:
     ```
     GET /organizations/{organizationUuid}/accounts/preliminary/balance
     ```
   * To check the balance of transactions that are ready to be paid out to the merchant's bank, fetch the liquid account balance:
     ```
     GET /organizations/{organizationUuid}/accounts/liquid/balance
     ```

   Example:
   
   The following example request fetches account balance in the merchant's Zettle preliminary account.
   
   ```
   GET /organizations/self/accounts/preliminary/balance
   ```
       
   The following example response shows that £100 is still being confirmed by the third-party acquirer with the buyer's bank.

    ```json
    {
        "data": {
            "totalBalance": 100,
            "currencyId": "GBP"
        }
    }    

 
## Related task
* [Fetch account transactions](fetch-account-transactions)
* [Fetch payout information](fetch-payout-info)

## Related API reference
* [Finance API reference](../api-reference.md)
* [Purchase API reference](../../purchase.adoc)