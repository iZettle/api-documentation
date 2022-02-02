Set or update low stock level
=====

The following describes endpoints for setting or updating low stock threshold levels for products and variants in a specific location. 

## Prerequisites
* Authorization setup with the correct scope, and a valid authorization token. See [How inventories work](../../concepts/how-inventories-work.md).
* The location UUID for the target location. See [Fetch inventory locations](../manage-locations/fetch-inventory-locations.md).
* The product/variant UUIDs for the products/variants to be updated. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md).

## Update low stock level for a product
Saves a low stock level for exactly one product/variant in a specific location. The product and variant UUIDs are defined in the request body.

```http
PUT /custom-low-stock/locations/{locationUuid}
```

**Example:** This example updates the custom low stock level to `10` for `product_uuid` `de71da94-5cb4-11ec-bf63-0242ac130002`, and `variant_uuid` `e58d826a-5cb4-11ec-bf63-0242ac130002`, for `location_uuid` `f2e4d45e-5cb4-11ec-bf63-0242ac130002`.

**Request**

```http
PUT /custom-low-stock/locations/f2e4d45e-5cb4-11ec-bf63-0242ac130002
```
```json
{
  "productUuid": "de71da94-5cb4-11ec-bf63-0242ac130002",
  "variantUuid": "e58d826a-5cb4-11ec-bf63-0242ac130002",
  "lowStockLevel": 10
}
```

**Response**

No response body is returned, only a status code of 204 - No Content.

**Note:** The Zettle clients (apps and back office) use caching. Therefore, it can take some time until they sync with the backend to pick up changes made through the API.

## Update low stock level for several products
This call updates multiple variants at the same time. This is convenient method to use if you have many updates to perform at the same time. The payload is essentially a list in the single update call. You can construct the payload in the same way you did for the single update call providing a list instead.

```http
POST /custom-low-stock/locations/{locationUuid}
```

**Example:** This example updates the custom low stock level to `10` and `8` respectively, for `product_uuid` `de71da94-5cb4-11ec-bf63-0242ac130002` and `variant_uuids` `e58d826a-5cb4-11ec-bf63-0242ac130002` and `9258b222-5cb9-11ec-bf63-0242ac130002`, for `location_uuid` `f2e4d45e-5cb4-11ec-bf63-0242ac130002`.

**Request**

```http
POST /custom-low-stock/locations/f2e4d45e-5cb4-11ec-bf63-0242ac130002
```

```json
{
  "requests": [
    {
      "productUuid": "de71da94-5cb4-11ec-bf63-0242ac130002",
      "variantUuid": "e58d826a-5cb4-11ec-bf63-0242ac130002",
      "lowStockLevel": 10
    },
    {
      "productUuid": "de71da94-5cb4-11ec-bf63-0242ac130002",
      "variantUuid": "9258b222-5cb9-11ec-bf63-0242ac130002",
      "lowStockLevel": 8
    }
  ]
}
```

**Response**

No response body is returned only a status code of 204 - No Content.

## Related tasks
* [Delete low stock level](delete-low-stock-level.md)
* [Fetch low stock level](fetch-low-stock-level.md)
