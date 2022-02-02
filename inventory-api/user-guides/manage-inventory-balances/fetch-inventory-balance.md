Fetch inventory balance
=====

You can for example retrieve inventory balances for a specific location, or location type. You can also get inventory balances for specific products or variants.

## Prerequisites
* Authorization setup with the correct scope, and a valid authorization token. See [How inventories work](../../concepts/how-inventories-work.md).
* A location UUID belonging the inventory, see [Fetch inventory locations](../manage-locations/fetch-inventory-locations.md).

## Fetch inventory balance

This request returns inventory balances for the `STORE` location type of an organization. You can add a `since` parameter to limit the data returned. In that case only products/variants changed since the specified date will be included in the response. 


```http
GET /inventory
```

**Example:** Fetch balances for location type STORE.

**Request**

```http
GET /inventory
```
**Response**

```json
{
    "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
    "trackedProducts": [
      "b9675342-5ce0-11ec-bf63-0242ac130002",
      "24977020-5ce1-11ec-bf63-0242ac130002"
    ],
    "variants": [
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "b9675342-5ce0-11ec-bf63-0242ac130002",
        "variantUuid": "d3b93e04-5ce0-11ec-bf63-0242ac130002",
        "balance": "42"
      },
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "24977020-5ce1-11ec-bf63-0242ac130002",
        "variantUuid": "f5c98ab2-5ce0-11ec-bf63-0242ac130002",
        "balance": "1337"
      }
    ],
    "latest": 0
}
```

## Fetch inventory balance for a given location

This request returns inventory balances for products in a specific location defined by the provided location UUID.


```http
GET /organizations/self/inventory/locations/{locationUuid}

```

**Example:** This call retrieves balances for the location `6e5e8d52-5ce0-11ec-bf63-0242ac130002`.

**Request**

```http
GET /organizations/self/inventory/locations/6e5e8d52-5ce0-11ec-bf63-0242ac130002

```

**Response**

```json
{
    "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
    "trackedProducts": [
      "b9675342-5ce0-11ec-bf63-0242ac130002",
      "24977020-5ce1-11ec-bf63-0242ac130002"
    ],
    "variants": [
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "b9675342-5ce0-11ec-bf63-0242ac130002",
        "variantUuid": "d3b93e04-5ce0-11ec-bf63-0242ac130002",
        "balance": "42"
      },
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "24977020-5ce1-11ec-bf63-0242ac130002",
        "variantUuid": "f5c98ab2-5ce0-11ec-bf63-0242ac130002",
        "balance": "1337"
      }
    ],
    "latest": 0
}
```

## Fetch inventory balance for a given location type

This request returns inventory balances for products in a specific location defined by the provided location type.


```http
GET /organizations/self/inventory/locations/?type={STORE|SOLD|BIN|SUPPLIER}

```

**Example:** This call retrieves balances for the location type `STORE`.

**Request**

```http
GET /organizations/self/inventory/locations/?type=STORE

```

**Response**

```json
{
    "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
    "trackedProducts": [
      "b9675342-5ce0-11ec-bf63-0242ac130002",
      "24977020-5ce1-11ec-bf63-0242ac130002"
    ],
    "variants": [
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "b9675342-5ce0-11ec-bf63-0242ac130002",
        "variantUuid": "d3b93e04-5ce0-11ec-bf63-0242ac130002",
        "balance": "42"
      },
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "24977020-5ce1-11ec-bf63-0242ac130002",
        "variantUuid": "f5c98ab2-5ce0-11ec-bf63-0242ac130002",
        "balance": "1337"
      }
    ],
    "latest": 0
}
```

## Fetch inventory balance for a list of products in a location

A pure retrieval endpoint where no data is modified. In the request body, you specify a location and a list of products. The endpoint retrieves and returns the current inventory balances for these products in the given location.

The request does the same as "Fetch inventory balance for a location" (`GET/organizations/self/inventory/locations/{locationUuid}`). The difference is that this request applies to a specified list of products defined in the header.

```http
POST /organizations/self/inventory/products
```

**Example:** This example retrieves the balances for products `b9675342-5ce0-11ec-bf63-0242ac130002`and `24977020-5ce1-11ec-bf63-0242ac130002` in a single call.

**Request**

```http
POST /organizations/self/inventory/products
```

```json
{
  "locationType": "STORE",
  "productUuids": [
    "b9675342-5ce0-11ec-bf63-0242ac130002", 
    "24977020-5ce1-11ec-bf63-0242ac130002"
  ]
}
```

**Response**

```json
{
    "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
    "trackedProducts": [
      "b9675342-5ce0-11ec-bf63-0242ac130002",
      "24977020-5ce1-11ec-bf63-0242ac130002"
    ],
    "variants": [
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "b9675342-5ce0-11ec-bf63-0242ac130002",
        "variantUuid": "d3b93e04-5ce0-11ec-bf63-0242ac130002",
        "balance": "42"
      },
      {
        "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
        "locationType": "STORE",
        "productUuid": "24977020-5ce1-11ec-bf63-0242ac130002",
        "variantUuid": "f5c98ab2-5ce0-11ec-bf63-0242ac130002",
        "balance": "1337"
      }
    ],
    "latest": 0
}
```

The product list acts as a filter and only returns the requested products.

## Fetch inventory balance for a product in a location

Returns the inventory balance for a single specific product in a specific location. Locations and products are specified by their UUIDs.

```http
GET /organizations/self/inventory/locations/{locationUuid}/products/{productUuid}
```

**Example:** This example retrieves the inventory balance in location `6e5e8d52-5ce0-11ec-bf63-0242ac130002` for product `b9675342-5ce0-11ec-bf63-0242ac130002`.

**Request**

```http
GET /organizations/self/inventory/locations/6e5e8d52-5ce0-11ec-bf63-0242ac130002/products/b9675342-5ce0-11ec-bf63-0242ac130002
```

**Response**

```json
{
  "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
  "variants": [
    {
      "locationUuid": "6e5e8d52-5ce0-11ec-bf63-0242ac130002",
      "locationType": "STORE",
      "productUuid": "b9675342-5ce0-11ec-bf63-0242ac130002",
      "variantUuid": "d3b93e04-5ce0-11ec-bf63-0242ac130002",
      "balance": "42"
    }
  ]
}
```

## Related tasks
* [Fetch inventory locations](../manage-locations/fetch-inventory-locations.md)
* [Update inventory balance](update-inventory-balance.md)
