Fetch low stock level
=====

There are two operations available for retrieving low stock threshold levels for products and variants in a specific location. One gives the entire set of levels for a given location. The other returns levels only for a given product in a specific location. Both of them accepts and returns ETags to allow for more performant operation when the data has not changed since the last invocation.

## Prerequisites
* The organization UUID for the merchant organization. See [How inventories work](../../concepts/how-inventories-work.md).
* The location UUID for the inventory. See [Fetch inventory locations](../manage-locations/fetch-inventory-locations.md).
* The product UUID for a specific product/variant. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md).

## Fetch low stock level
Retrieves the custom low stock levels that have been set on any product/variant for a given location UUID. 

```http
GET /custom-low-stock/locations/{locationUuid}
```

**Example:** Retrieve all the previously set custom low stock levels for a given location UUID `efc7ea52-51c1-11ec-bf63-0242ac130002`.

**Request**

```http
GET /custom-low-stock/locations/efc7ea52-51c1-11ec-bf63-0242ac130002
```

**Response**

```json
  {
    "organizationUuid": "4b8784a6-51c2-11ec-bf63-0242ac130002",
    "locationUuid": "efc7ea52-51c1-11ec-bf63-0242ac130002",
    "customLowStockLevelsForProducts": [
      {
        "productUuid": "6f25a2bc-51c2-11ec-bf63-0242ac130002",
        "customLowStockLevelForVariants": [
          {
            "variantUuid": "8a9181c4-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 5.0
          },
          {
            "variantUuid": "a466e436-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 7.0
          }
        ]
      },
      {
        "productUuid": "bf0ae350-51c2-11ec-bf63-0242ac130002",
        "customLowStockLevelForVariants": [
          {
            "variantUuid": "cdd1dcb8-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 3.0
          },
          {
            "variantUuid": "d42d9f70-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 8.0
          }
        ]
      }
    ]
  }
```


## Fetch low stock level for a product 
Retrieves the custom low stock level for exactly one product in a specific location. You pass in the location and product UUIDs for which you want to fetch custom low stock levels.

```http
GET /custom-low-stock/locations/{locationUuid}/products/{productUuid}
```

**Example:** Retrieve all the previously set custom low stock levels for a given location UUID `efc7ea52-51c1-11ec-bf63-0242ac130002`, and product `6f25a2bc-51c2-11ec-bf63-0242ac130002`.

**Request**

```http
GET /custom-low-stock/locations/efc7ea52-51c1-11ec-bf63-0242ac130002/products/6f25a2bc-51c2-11ec-bf63-0242ac130002
```

**Response**

```json
  {
    "productUuid": "6f25a2bc-51c2-11ec-bf63-0242ac130002",
    "customLowStockLevelForVariants": [
      {
        "variantUuid": "8a9181c4-51c2-11ec-bf63-0242ac130002",
          "customLowStockLevel": 5.0
      },
      {
        "variantUuid": "a466e436-51c2-11ec-bf63-0242ac130002",
        "customLowStockLevel": 7.0
      }
    ]
  }
```

## Related tasks
* [Delete low stock level](delete-low-stock-level.md)
* [Set low stock level](set-low-stock-level.md)
