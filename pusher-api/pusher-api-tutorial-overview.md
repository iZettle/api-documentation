# Manage event subscriptions
Using the Zetle Go pusher API, you can subscribe to webhook events toward other Zettle Go APIs, such as the purchase API. With webhook event subscriptions, you will be updated of those events that happen on your Zettle Go instead of pulling information from other Zettle Go APIs. 

* [Create webhook event subscriptions](pusher-api-tutotrial-create-subscriptions.md)
* [Update webhook event subscriptions](pusher-api-tutorial-update-subscriptions.md)
* [Delete webhook event subscriptions](pusher-api-tutorial-delete-subscriptions.md)

To get started, you may want to subscribe to the following events:

* To monitor inventory changes in Zettle POS: `InventoryBalanceChanged`, `InventoryTrackingStarted`, and `InventoryTrackingStopped`
* To monitor product library changes: `ProductCreated`, `ProductUpdated`, and `ProductDeleted`
* To monitor purchases: `PurchaseCreated`

For more webhooks events that you can subscribe, see [Pusher API reference](api-reference-template-manual.md).