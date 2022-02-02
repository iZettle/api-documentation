Disable tracking
=====

You can stop inventory tracking for products at any time. Saved stock levels will not be deleted, but will no longer be returned when requesting the inventory balance. Additionally, stock levels will not be updated automatically on purchases. You can re-activate the product tracking in the future, and stock levels will still be there.


## Prerequisites
* Authorization setup with the correct scope, and a valid authorization token. See [How inventories work](../../concepts/how-inventories-work.md).
* Required OAuth scope: `WRITE:PRODUCT`
* The product UUID for the product that should no longer be tracked. See [Fetch products](https://github.com/iZettle/devx-doc-experiment/blob/main/api-documentation/product-library-api/user-guides/manage-products/fetch-products.md).

## Disable tracking of a product

The following request stops tracking of a specific product with a given UUID.

```http request
DELETE /organizations/self/inventory/products/{productUuid}
```

**Example:** This request disables tracking for the product with UUID `d42260ae-423d-11ec-95a4-10307542d8a2`.

**Request**

```http request
DELETE /organizations/self/inventory/products/d42260ae-423d-11ec-95a4-10307542d8a2
Authorization: Bearer eyJraWQiOiIxNjM3MTg1Mjg0Nz...
```

**Response**

This endpoint returns the status code 204 and an empty response body if successful. If the request was handled successfully, the product will no longer be tracked. You can enable tracking of the product again whenever needed.

## Related tasks

* [Enable tracking](enable-tracking.md) 
