Incidents
=====================
If you have questions about any incident, contact our [Integrations team](mailto:api@zettle.com).

## March 2021
### Finance API returned wrong transaction type for transactions made with cards.

Mar 30, 16:03 - 16:59 UTC<br>
This incident has been resolved. __Action needed to fix data.__<br>
We apologize for the inconvenience.
<details><!-- start tag of the incident section-->
<summary>Click for details</summary>

### Incident summary
There was a problem with the `originatorTransactionType` field in the response for transactions made with cards for the following endpoint:

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions
```

Table below enlists the expected and actual values for the field:

|Expected value in `originatorTransactionType` |Actual value in `originatorTransactionType` during the incident
|:---- |:----
|CARD_PAYMENT |PAYMENT
|CARD_PAYMENT_FEE |PAYMENT
|CARD_REFUND |PAYMENT
|CARD_PAYMENT_FEE_REFUND |PAYMENT

Transactions made between the following timestamps were affected:

Start time:  2021-03-30 16:03:11.437237 UTC<br>
End time:  2021-03-30 16:59:45.00856  UTC<br>
The total duration was approximately 56 minutes.

### What do you need to do as a consumer?
To fix your data you would need to refetch transactions for merchants your integration serves between the timestamps given above.

If your integration disregards transactions with `originatorTransactionType` `PAYMENT` and `PAYMENT_FEE` you would need to handle those transactions as you used to do with card related types. In other words `CARD_PAYMENT` and `CARD_REFUND` should be expected as `PAYMENT`.<br>
`CARD_PAYMENT_FEE` and `CARD_PAYMENT_FEE_REFUND` should be expected as `PAYMENT_FEE`.

See example below to understand better:

```
GET/organizations/self/accounts/liquid/transactions?start=2021-03-30T16:03:10&end=2021-03-30T16:59:46
```

__Actual response during the time of incident after a partial fix when refetching:__
 

```
{
  "data": [
    {
      "timestamp": "2021-04-23T00:08:40.171+0000",
      "amount": 5533,
      "originatorTransactionType": "PAYMENT_FEE", // Instead of CARD_PAYMENT_FEE_REFUND
      "originatingTransactionUuid": "63a5073a-a386-11eb-9017-db0e39c91814"
    },
    {
      "timestamp": "2021-04-23T00:08:40.168+0000",
      "amount": -201200,
      "originatorTransactionType": "PAYMENT", // Instead of CARD_REFUND
      "originatingTransactionUuid": "63a5073a-a386-11eb-9017-db0e39c91814"
    },
    {
      "timestamp": "2021-04-23T00:08:40.165+0000",
      "amount": -110,
      "originatorTransactionType": "PAYMENT_FEE", // Instead of CARD_PAYMENT_FEE
      "originatingTransactionUuid": "40a35d88-a387-11eb-850d-127e2d49c7a5"
    },
    {
      "timestamp": "2021-04-23T00:08:40.163+0000",
      "amount": 3990,
      "originatorTransactionType": "PAYMENT", // Instead of CARD_PAYMENT
      "originatingTransactionUuid": "40a35d88-a387-11eb-850d-127e2d49c7a5"
    }
  ]
}

```


</details>
