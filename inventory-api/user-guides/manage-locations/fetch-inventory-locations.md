Fetch inventory locations
=====

Through access to the data of a merchant’s locations, you can retrieve a list of all location UUIDs. These are used when working with inventory balances and stock management. 

By default, an organization has four locations: `STORE`, `SOLD`, `SUPPLIER` and `BIN`. They each represent a conceptual location with an inventory level. The most commonly used location is `STORE`. This represents the stock level in the customer-facing store or warehouse. However, you can use any of the available locations to read inventory stats for other types of locations.

## Prerequisites
* Authorization setup with the correct scope, and a valid authorization token. See [How inventories work](../../concepts/how-inventories-work.md).
* Required access right: `READ:PRODUCT`

## Fetch all inventory locations
Use this request to retrieve all inventory location UUIDs for the organization the client is signed in as.

```http request
GET /organizations/self/locations
```

**Example:** This example retrieves the location UUIDs for the organization associated with the authorization token. The token is passed in the request header and is of type `Authorization: Bearer eyJraWQiOiIxNjM3MTg1Mjg0Nz...` in this example.

**Request**
 
```http request
GET /organizations/self/locations
Host: inventory.izettle.com
Authorization: Bearer eyJraWQiOiIxNjM3MTg1Mjg0Nz...
```

**Response**

```json
  [
    {
        "uuid": "fd4a39a0-e2ef-11e6-ba64-85247ae2a737",
        "type": "STORE",
        "name": "STORE",
        "description": "System generated location ",
        "default": true
    },
    {
        "uuid": "fd4ad5e0-e2ef-11e6-a2b0-e5c90fd52c77",
        "type": "BIN",
        "name": "BIN",
        "description": "System generated location ",
        "default": false
    },
    {
        "uuid": "fd4a87c0-e2ef-11e6-bfe3-78ba29c12242",
        "type": "SOLD",
        "name": "SOLD",
        "description": "System generated location ",
        "default": false
    },
    {
        "uuid": "fd40ead0-e2ef-11e6-8c44-45d2bd8a4fd8",
        "type": "SUPPLIER",
        "name": "SUPPLIER",
        "description": "System generated location ",
        "default": false
    }
  ]
```

Ensure location UUIDs are saved so they are available for access by the client system. They will be used in most other calls when managing inventory balances. 

## Related tasks
* [Enable inventory tracking](../../user-guides/manage-inventory-tracking/enable-tracking.md)
* [Disable inventory tracking](../../user-guides/manage-inventory-tracking/disable-tracking.md)
