Enable tracking
=====
Tracking of products must be enabled in order to manage inventory balances. You can start tracking for a single product. You can also start tracking and set the initial inventory balance in one call as described in the following.

## Prerequisites
* Authorization setup with the correct scope, and a valid authorization token. See [How inventories work](../../concepts/how-inventories-work.md).
* Required access right: `WRITE:PRODUCT`
* The product UUID for the product that should be tracked. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md).

## Enable tracking of products
Starts the tracking of products in an inventory. Lists existing inventory balances for a product, for example if it has been tracked in the past. 

The request always returns balances for the location type `STORE`. The response will contain product variants with non-zero inventory balances, and the UUID of the location where the product is tracked.

Use this request to start product tracking for a given product UUID. The UUID of the product is passed in the request body.

```http request
POST /organizations/self/inventory
```

**Example:** Enabling of tracking and retrieval of balance for variants of the product with UUID `d42260ae-423d-11ec-95a4-10307542d8a2`.

**Request body**

```http request
POST /organizations/self/inventory
Authorization: Bearer eyJraWQiOiIxNjM3MTg1Mjg0Nz...
Content-Type: application/json

{
  "productUuid": "d42260ae-423d-11ec-95a4-10307542d8a2"
}
```

**Response**

```json
{
  "locationUuid": "fd4a39a0-e2ef-11e6-ba64-85247ae2a737",
  "variants": []
}
```

If the response has a 2XX status code indicating success, then tracking has been enabled for the product.

The response body will contain an overview of the current inventory stock levels for the product in the `STORE` location. For a new product, this overview is usually empty.

## Enable tracking using the bulk endpoint 
An alternative way to enable tracking for products is through the `bulk` API endpoint. Using this endpoint you can also include balance updates. This allows you to start the tracking and update the stock levels within a single request. 

```http request
POST /organizations/self/v2/inventory/bulk
```
You can also update multiple products in a single request through the `bulk` endpoint. In the request body, list the product UUIDs that you want to modify, and set the `trackingStatusChange` to `"START_TRACKING"`.

**Example:**  This example enables inventory tracking for a single product. This example request does not contain simultaneous stock level updates.

```http request
POST /organizations/self/v2/inventory/bulk
```

**Request**

```http request
POST /organizations/self/v2/inventory/bulk
Authorization: Bearer eyJraWQiOiIxNjM3MTg1Mjg0Nz...
Content-Type: application/json

{
  "returnBalanceForLocationUuid": "61f6bfae-092f-11eb-8018-0bf432cc48c7",
  "productChanges": [
    {
      "productUuid": "8d772d4a-111d-11ea-9ee7-141cd50f3f99",
      "trackingStatusChange": "START_TRACKING"
    }
  ]
}
```

**Response**

```json
{
  "locationUuid": "61f6bfae-092f-11eb-8018-0bf432cc48c7",
  "variants": []
}
```

If the response has a 2XX status code indicating success, then tracking has been enabled for the product.

The response body will contain the modified inventory stock levels for the location given in the request body. This response will only contain the products and variants for which stock levels were affected by the request. If the request only contained tracking status changes, and no stock level updates, no stock levels will be reported back.

## Related tasks

* [Fetch inventory balance](../manage-inventory-balances/fetch-inventory-balance.md)
* [Update inventory balance](../manage-inventory-balances/update-inventory-balance.md)
