Finance API migration guide
===

A new version V2 of the Finance API is released, replacing the former version (called V1 in this document). V1 is 
now deprecated but will remain available until 2022-12-31.

This document highlights the differences between V1 and V2 and supports the migration to V2.

See [Finance API V2 reference](api-reference-v2.yaml). 

## Overview of changes

### Changes to request paths
- `v2` has been added to the request paths.

- `organizations/{organizationUuid}` has been removed from the request paths. Instead, the organization is identified 
  from the authentication data.

### Changes to the parameter format

- Timestamps (`timestamp`, `at`, `start`, `end`) must now be expressed as a full UTC date/time. Example: 
  `2022-03-15T14:00:00` 

### Changes to transaction types

All transaction types relating to payment processing have been merged into one transaction type `PAYMENT`. All 
transaction types relating to fees from processing payments have been merged into `PAYMENT_FEE`.

- Transaction type `CARD_PAYMENT` is now represented by `PAYMENT`.

- Transaction type `CARD_PAYMENT_FEE` is now represented by `PAYMENT_FEE`. 

- Transaction type `CARD_REFUND` is now represented by `PAYMENT` but with the amount being negative.

- Transaction type `CARD_PAYMENT_FEE_REFUND` is now represented by `PAYMENT_FEE` but with the amount being positive.

The transaction type `PAYMENT_PAYOUT` has been added to reflect transfers of money related to a single payment. These 
transfers happen between the merchant's Zettle liquid account and their PayPal Wallet. For payouts of regular payments, 
this will be negative and for refunds using the PayPal Wallet this will be positive.

These changes apply to both input parameter `includeTransactionType` and `originatorTransactionType` in the response
and also apply to historical data.

## Support
If you have any questions, please contact our developer support at [Zettle Developer Portal](https://developer.zettle.com).