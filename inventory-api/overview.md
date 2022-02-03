Inventory API overview
=====
The Inventory service keeps track of stock levels for product variants. The Inventory API is used for example when building integrations between POS systems and e-commerce systems.

## Getting started
Register on the Zettle Developer Portal to get API credentials for the app you are building. See [Sign up for a Zettle developer account](../get-started/user-guides/sign-up-for-a-developer-account.md). 

Explore the sections in the following to get an overview of the different parts of the Inventory API.

## Understanding inventories
API clients can list current stock levels and update inventory balances for products. The service automatically decreases the stock when a purchase is made with the Zettle POS application. Tracking is based on moving product items between *Locations*. See [How inventories work](concepts/how-inventories-work.md) for concept descriptions.

## Working with inventories

### Manage inventory tracking
To be able to work with inventory balances, tracking must be enabled for the products you want to manage inventory balances for. Inventory tracking is enabled on a product basis and can be enabled/disabled at any time.
* [Enable tracking](user-guides/manage-inventory-tracking/enable-tracking.md)
* [Disable tracking](user-guides/manage-inventory-tracking/disable-tracking.md)

### Manage locations
When an organization starts using the inventory, the inventory service will generate a set of default locations. Location endpoints are used for retrieving these locations. The location UUIDs are then used with other inventory management functionality to manage stock levels.
* [Fetch locations](user-guides/manage-locations/fetch-inventory-locations.md)

### Manage inventory balances
This area contains endpoints used for managing tracked products, and for retrieving and updating stock information. Here you can fetch and update inventory balances for specific products and locations.
* [Fetch inventory balance](user-guides/manage-inventory-balances/fetch-inventory-balance.md)
* [Update inventory balance](user-guides/manage-inventory-balances/update-inventory-balance.md)

### Manage low stock levels
This functionality is used for defining low stock levels. This is done to ensure merchants have the right inventory balance to meet current demand without having an excess in stock. Through these endpoints you can retrieve and update low stock levels.
* [Set or update low stock level](user-guides/manage-low-stock-levels/set-low-stock-level.md)
* [Fetch low stock level](user-guides/manage-low-stock-levels/fetch-low-stock-level.md)
* [Delete low stock level](user-guides/manage-low-stock-levels/delete-low-stock-level.md)

