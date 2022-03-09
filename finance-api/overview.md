Finance API overview
===
Merchants who take payments with Zettle have an Zettle account that processes transactions for the payments. For example, card payments made with Zettle card readers.

The Finance API is used to fetch finance-related information that is processed through Zettle. The information includes account balance, all the transactions processed through Zettle, and payout.

The current version of the Finance API is v2. Version v1 is Deprecated but will remain available through 2022.  

## Understand how payments work at Zettle
Payments are card payments, card refunds, or alternative payment methods (APMs) like PayPal QR code (QRC).

For information on how Zettle handles transactions for payments, see [how payments work at Zettle](concepts/how-payments-work-at-Zettle.md).  

## Understand the Finance API
See [Finance API reference](api-reference.md).

## Fetch transactions
Using the Finance API, you can fetch finance-related information as follows:

* Fetch account balance
  * [v1 (Deprecated)](user-guides/fetch-account-balance.md) 
  * [v2](user-guides/fetch-account-balance_v2.md)
* Fetch account transactions
  * [v1 (Deprecated)](user-guides/fetch-account-transactions.md)
  * [v2](user-guides/fetch-account-transactions-v2.md)
* Fetch payout information
  * [v1 (Deprecated)](user-guides/fetch-payout-info.md)
  * [v2](user-guides/fetch-payout-info-v2.md)
* Fetch purchase information for transactions
  * [v1 (Deprecated)](user-guides/fetch-purchase-information-for-transactions.md)
  * [v2](user-guides/fetch-purchase-information-for-transactions-v2.md)