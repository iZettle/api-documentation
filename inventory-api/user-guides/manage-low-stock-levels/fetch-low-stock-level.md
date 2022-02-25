Fetch low stock level
=====

There are two operations available for retrieving low stock threshold levels and the stock alert settings for products and variants in a specific location. One gives the entire set of levels for a given location. The other returns levels only for a given product in a specific location. Both of them accept and return ETags to allow for more performant operation when the data has not changed since the last invocation.

## Prerequisites
* The organization UUID for the merchant organization. See [How inventories work](../../concepts/how-inventories-work.md).
* The location UUID for the inventory. See [Fetch inventory locations](../manage-locations/fetch-inventory-locations.md).
* The product UUID for a specific product/variant. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md).

## Fetch low stock level and stock alert setting
Retrieves the custom low stock configurations that have been set on any product/variant for a given location UUID. 

```http
GET /custom-low-stock/v2/locations/{locationUuid}
```

**Example:** Retrieve all the previously set custom low stock levels and stock alert settings for a given location UUID `efc7ea52-51c1-11ec-bf63-0242ac130002`.

**Request**

```http
GET /custom-low-stock/v2/locations/efc7ea52-51c1-11ec-bf63-0242ac130002
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
            "lowStockAlert": true
          },
          {
            "variantUuid": "a466e436-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 7.0
            "lowStockAlert": true
          }
        ]
      },
      {
        "productUuid": "bf0ae350-51c2-11ec-bf63-0242ac130002",
        "customLowStockLevelForVariants": [
          {
            "variantUuid": "cdd1dcb8-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 3.0
            "lowStockAlert": false
          },
          {
            "variantUuid": "d42d9f70-51c2-11ec-bf63-0242ac130002",
            "customLowStockLevel": 8.0
            "lowStockAlert": true
          }
        ]
      }
    ]
  }
```


## Fetch low stock level and stock alert setting for a product 
Retrieves the custom low stock configuration for exactly one product in a specific location. You pass in the location and product UUIDs for which you want to fetch custom low stock levels.

```http
GET /custom-low-stock/v2/locations/{locationUuid}/products/{productUuid}
```

**Example:** Retrieve all the previously set custom low stock levels and stock alert settings for a given location UUID `efc7ea52-51c1-11ec-bf63-0242ac130002`, and product `6f25a2bc-51c2-11ec-bf63-0242ac130002`.

**Request**

```http
GET /custom-low-stock/v2/locations/efc7ea52-51c1-11ec-bf63-0242ac130002/products/6f25a2bc-51c2-11ec-bf63-0242ac130002
```

**Response**

```json
  {
    "productUuid": "6f25a2bc-51c2-11ec-bf63-0242ac130002",
    "customLowStockLevelForVariants": [
      {
        "variantUuid": "8a9181c4-51c2-11ec-bf63-0242ac130002",
          "customLowStockLevel": 5.0
          "lowStockAlert": true
      },
      {
        "variantUuid": "a466e436-51c2-11ec-bf63-0242ac130002",
        "customLowStockLevel": 7.0
        "lowStockAlert": false
      }
    ]
  }
```

## Related tasks
* [Delete low stock level](delete-low-stock-level.md)
* [Set low stock level](set-low-stock-level.md)
