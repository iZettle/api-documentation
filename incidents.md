Zettle API Incidents
=====================
If you have questions about any incident, contact our [Integrations team](mailto:api@zettle.com).

# 30 March, 2021
## (status: resolved) Finance API returned wrong `originatorTransactionType` 
The following Finance API endpoint returned wrong `originatorTransactionType`:

```
GET /organizations/{organizationUuid}/accounts/{accountTypeGroup}/transactions
```

<details><!-- start tag of the incident section-->
<summary>Click to see the incident.</summary>

### Incident summary
The `originatorTransactionType` field was returned with wrong values. The following table shows the expected and returned values for the field.

|Expected value in `originatorTransactionType` field |Expected value in `originatorTransactionType` field
|:---- |:----
|CARD_PAYMENT |PAYMENT
|CARD_PAYMENT_FEE |PAYMENT
|CARD_REFUND |PAYMENT
|CARD_PAYMENT_FEE_REFUND |PAYMENT

Transactions made between the following timestamps were affected:

Start time:  2021-03-30 16:03:11.437237 UTC<br>
End time:  2021-03-30 16:59:45.00856  UTC<br>
The total duration was approximately 56 minutes.



### What do you need to do?
To fix your data you would need to refetch transactions for merchants your integration serves between the timestamps given above once again.

If your integration disregards transactions with originatorTransactionType` `PAYMENT` and `PAYMENT_FEE` you would need to handle those transactions and bookkeep as you used to do with CARD.

See example below to understand better:

```
GET/organizations/self/accounts/liquid/transactions?start=2021-03-30T16:03:10&end=2021-03-30T16:59:46
```

__Expected response:__

```
    ...
            {
                "timestamp": "2021-03-30T16:03:31.003+0000",
                "amount": -10,
                "originatorTransactionType": "CARD_PAYMENT_FEE",
                "originatingTransactionUuid": "ff4f492e-914c-1bbb-bb86-850e353b75b8"
            },
            {
                "timestamp": "2021-03-30T16:03:31.000+0000",
                "amount": 780,
                "originatorTransactionType": "CARD_PAYMENT",
                "originatingTransactionUuid": "ff4f492e-914c-1bbb-bb86-850e353b75b8"
            }
    ...

```

__Actual response during the time of incident after a partial fix when refetching:__
 

```
    ...
            {
                "timestamp": "2021-03-30T16:03:31.003+0000",
                "amount": -10,
                "originatorTransactionType": "PAYMENT_FEE",
                "originatingTransactionUuid": "ff4f492e-914c-1bbb-bb86-850e353b75b8"
            },
            {
                "timestamp": "2021-03-30T16:03:31.000+0000",
                "amount": 780,
                "originatorTransactionType": "PAYMENT",
                "originatingTransactionUuid": "ff4f492e-914c-1bbb-bb86-850e353b75b8"
            }
    ...

```


</details>
