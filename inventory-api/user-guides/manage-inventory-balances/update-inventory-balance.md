Update inventory balance
=====

There are several calls available to update the inventory balance of a variant. The general term for balance updates are "movements". Each movement has product/variant UUIDs, a "from" location, a "to" location, and the number of items that should be moved. 

## Prerequisites
* Authorization setup with the correct scope, and a valid authorization token. See [How inventories work](../../concepts/how-inventories-work.md).
* The "from" and "to" location UUIDs for locations involved in the movement. See [Fetch inventory locations](../manage-locations/fetch-inventory-locations.md).
* The product UUID for a specific product. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md#fetch-all-products).
* The variant UUID for which to update the balance. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md#fetch-all-products).

## Update inventory balance 

Use the following request to update the inventory balance for an organization.

```http
PUT /organizations/self/inventory
```

```json
{
  "changes": [
    {
      "productUuid": <UUID>,
      "variantUuid": <UUID>,
      "fromLocationUuid": <UUID>,
      "toLocationUuid": <UUID>,
      "change": <NUMBER>
    }
  ],
  "returnBalanceForLocationUuid": <UUID>,
  "externalUuid": <UUID>
}
```

**Example:** Updating the balance for product UUID `65314d7e-5f0f-11ec-bf63-0242ac130002`, variant UUIDs `78194842-5f0f-11ec-bf63-0242ac130002`and `7c6be742-5f0f-11ec-bf63-0242ac130002`, from location UUID `89ac1e4a-5f0f-11ec-bf63-0242ac130002` to location `92646e02-5f0f-11ec-bf63-0242ac130002`, with values `42`and `1337`.

The response will return the balances for the `STORE` location since we are not providing any value for `returnBalanceForLocationUuid`.

**Request**

```http
PUT /organizations/self/inventory
```

**Request body**

```json
{
  "changes": [
    {
      "productUuid": "65314d7e-5f0f-11ec-bf63-0242ac130002",
      "variantUuid": "78194842-5f0f-11ec-bf63-0242ac130002",
      "fromLocationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
      "toLocationUuid": "92646e02-5f0f-11ec-bf63-0242ac130002",
      "change": 42
    },
    {
      "productUuid": "65314d7e-5f0f-11ec-bf63-0242ac130002",
      "variantUuid": "7c6be742-5f0f-11ec-bf63-0242ac130002",
      "fromLocationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
      "toLocationUuid": "92646e02-5f0f-11ec-bf63-0242ac130002",
      "change": 1337
    }
  ]
}
```

**Response**

```json
{
  "locationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
  "variants": [
    {
      "locationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
      "locationType": "STORE",
      "productUuid": "65314d7e-5f0f-11ec-bf63-0242ac130002",
      "variantUuid": "78194842-5f0f-11ec-bf63-0242ac130002",
      "balance": 196
    },
    {
      "locationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
      "locationType": "STORE",
      "productUuid": "65314d7e-5f0f-11ec-bf63-0242ac130002",
      "variantUuid": "7c6be742-5f0f-11ec-bf63-0242ac130002",
      "balance": 10696
    }
  ]
}

```

## Update balances using the bulk endpoint 
An alternative way to update balances is through the `bulk` API endpoint. Using this endpoint you can both start tracking and update the stock levels within a single request. You can also do this for multiple products at the same time.

**Example:** Updating the balance for two variants.

```http
POST /organizations/self/v2/inventory/bulk
```

**Request body**

```json
{
  "returnBalanceForLocationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
  "productChanges": [
    {
      "productUuid": "3cc3b9f6-623b-11ec-90d6-0242ac120003",
      "trackingStatusChange": "START_TRACKING",
      "variantChanges": [
        {
          "variantUuid": "4066f6cc-623b-11ec-90d6-0242ac120003",
          "fromLocationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
          "toLocationUuid": "92646e02-5f0f-11ec-bf63-0242ac130002",
          "change": 42
        },
        {
          "variantUuid": "45900c4c-623b-11ec-90d6-0242ac120003",
          "fromLocationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
          "toLocationUuid": "92646e02-5f0f-11ec-bf63-0242ac130002",
          "change": 1337
        }
      ]
    }
  ]
}
```

**Response**

```json
{
  "locationUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
  "variants": [
    {
      "productUuid": "3cc3b9f6-623b-11ec-90d6-0242ac120003",
      "variantUuid": "89ac1e4a-5f0f-11ec-bf63-0242ac130002",
      "balance": 196
    },
    {
      "productUuid": "3cc3b9f6-623b-11ec-90d6-0242ac120003",
      "variantUuid": "45900c4c-623b-11ec-90d6-0242ac120003",
      "balance": 10696
    }
  ]
}
```

## Related tasks
* [Fetch inventory balance](fetch-inventory-balance.md)
* [Enable inventory tracking](../manage-inventory-tracking/enable-tracking.md)
* [Disable inventory tracking](../manage-inventory-tracking/disable-tracking.md)
