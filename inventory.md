Inventory API
===

## Service description
The Inventory service keeps track of the stock level for variants within products. The API clients can list current stock levels and update balance for products. The service automatically decreases the stock when a purchase is made within the Zettle POS application.

### Base URL
`https://inventory.izettle.com`

### API documentation
https://inventory.izettle.com/swagger

## How it works

The service uses a concept of transactions where stock is moved between so called `Locations`.
Locations are created automatically when tracking is enabled on any product. Currently existing locations and the natural flow of stock balance are explained in the following.

![Stock balance flow from SUPPLIER to STORE and either BIN or SOLD](/inventory-api/images/Location_Flow.png)

`SUPPLIER` is an abstraction of a supplier and holds infinite stock.

`STORE` is the location of the current stock for a variant.

`BIN` and `SOLD` are abstractions for when a product is sold or discarded.

It is important to note that you cannot set a stock balance for a variant directly.
To set an absolute value of a variant you need to calculate the change needed to reach that stock value.

#### Example 1:
```http
PUT /organizations/{organizationUuid}/inventory
```

You have 30 stock in `STORE` and want to set that to 20. To do this you move 10 stock to the `SOLD`, `BIN` or `SUPPLIER` location, depending on the reason for the stock change. Note that you always move positive numbers.

```
{
  "productUuid": <uuid>,
  "variantUuid": <uuid>,
  "fromLocationUuid": <STORE_location_uuid>,
  "toLocationUuid": “<SOLD_location_uuid>,
  "change": 10
}
```

#### Example 2:
```http
PUT /organizations/{organizationUuid}/inventory
```

You have 30 in stock and want to set to 40. You then move 10 stock from `SUPPLIER` to `STORE`.

```
  {
  "productUuid": <uuid>
  "variantUuid": <uuid>,
  "fromLocationUuid": <SUPPLIER_location_uuid>,
  "toLocationUuid": “<STORE_location_uuid>,
  "change": 10
  }
```

