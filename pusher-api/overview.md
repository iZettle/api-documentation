Pusher API overview
=====================
With subscriptions, the Pusher API will immediately update you of those events that are triggered on your Zettle Go. So you don't need to pull information from other Zettle Go APIs.

* [Understand how subscriptions work](#understand-how-subscriptions-work)
* [Plan subscriptions](#plan-subscriptions)
* [Manage subscriptions](#manage-subscriptions)


## Understand how subscriptions work
The Pusher API provides events for you to listen to certain activities of the Zettle Go app at a working HTTPS endpoint on your server.

After you subscribe to events, when an event is triggered, the Pusher API sends the event in a `POST` request to the HTTPS endpoint. The request contains a `payload` field with event information in real time.

For more information about event payloads, see [event payloads](concept/event-payloads.md).

## Plan subscriptions
Before subscribing to events, plan which events to use for your use cases.

To get started, you may want to subscribe to the following events:

* To monitor inventory changes in Zettle Point of Sales (POS): `InventoryBalanceChanged`, `InventoryTrackingStarted`, and `InventoryTrackingStopped`
* To monitor product library changes: `ProductCreated`, `ProductUpdated`, and `ProductDeleted`
* To monitor purchases: `PurchaseCreated`
<!-- We can extend this section to be more focused on use cases later on. -->

For more events that you can subscribe, see [Pusher API reference](api-reference.md).

## Manage subscriptions
With the Pusher API, you can create, view, update, and delete subscriptions as you need.

* [Create subscriptions](user-guides/create-subscriptions.md)
* [View subscriptions](user-guides/view-subscriptions.md)
* [Update subscriptions](user-guides/update-subscriptions.md)
* [Delete subscriptions](user-guides/delete-subscriptions.md)
