Gift Card API
======

## Service description
The Gift Card API provides extra information about purchases made through Zettle with a gift card.

### Base URL
`https://giftcard.izettle.com`

### OAuth scope
`READ:PURCHASE`

## Get gift card details

```http
GET organizations/self/giftcards/external/{uuid}
```
Where `uuid` is the UUID of a gift card.

### Errors
`404 Giftcard not found`

### Example
Request
```http
GET /organizations/self/giftcards/external/e876c3ae-750d-4638-b98a-78868a434b89
```

Response
```json
{
  "uuid": "e876c3ae-750d-4638-b98a-78868a434b89",
  "created": "2015-02-08",
  "validTo": "2017-02-08",
  "initialAmount": 1500,
  "remainingAmount": 900
}
```

### Giftcard attributes
The `giftcards` data type contains the following attributes:

* `uuid` is the UUID of the gift card.
* `created` is the date when the gift card was created.
* `validTo` is the date after which the gift card will be expired and can no longer be used. If the value is null, the gift card will never expire.
* `initialAmount` is the amount that the giftcard was initially loaded with.
* `remainingAmount` is current balance of the gift card.
