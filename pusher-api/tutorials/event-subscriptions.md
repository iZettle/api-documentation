Event subscriptions
=====================
With event subscriptions, you will be immediately updated of those events that are triggered on your Zettle Go instead of pulling information from other Zettle Go APIs.

* [Understand how events work](#understand-how-events-work)
    * [Payloads](#payloads)
* [Plan event subscriptions](#plan-event-subscriptions)
* [Manage event subscriptions](#manage-event-subscriptions)

## Understand how events work
The Pusher API provides events for you to listen to certain activities of the Zettle Go app at a working HTTPS endpoint on your server.

After you subscribe to events, when an event is triggered, the Pusher API sends a `POST` request that contains a `payload` field with event information to the HTTPS endpoint in real time.

### Payloads
The `payload` field is the response body from other APIs, such as the Inventory API. The Pusher API sends `POST` requests with the `payload` field in the following JSON format:

```
{
    "organizationUuid" : "<organization uuid>",
    "messageUuid" : "<UUID v1 based on timestamp and messageId>",
    "eventName" : "<one of the eventnames>",
    "messageId" : "<unique UUID of the message>",
    "payload": {
      "specific payload of the event"
    },
    "timestamp": "<event timestamp in ISO-8601 format>"
  }
```
For example, if you subscribe to the `InventoryTrackingStarted` event, when that event is triggered at the Zettle Go app, you will receive a `POST` request that looks similar to the following:

```json
{
  "eventName" : "InventoryTrackingStarted",
  "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
  "messageId" : "52662705-98de-588c-810b-75d274d6fa8b",
  "payload" : {
    "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
    "productUuid" : "18380ac0-fab5-11e7-94b4-842bd3fbd22c",
    "created" : {
      "uuid" : "0f674460-fab5-11e7-a310-0002ebd6a43c",
      "timestamp" : "2018-01-16T12:02:16.569+0000",
      "userType" : "USER"
    }
  }
}
```

For more information on event payloads, see the following table for the APIs that provide the event payloads.

> **Note:** As payloads can be updated due to changes of the other APIs, you can ignore unknown fields.

|<a name="payloadAPITable"/>Events |API providing payloads
|--- |---
|CardPaymentAuthorized<br>CardPaymentInvalid<br>PaymentCanceled<br>PaymentCreated<br>PaymentInitiated<br>InvoiceCreated |[Finance API reference](../../finance.adoc)
|InventoryBalanceChanged<br>InventoryTrackingStarted<br>InventoryTrackingStopped |[Inventory API reference](../../inventory.adoc)
|ProductCreated<br>ProductDeleted<br>ProductUpdated |[Product Library API reference](../../product-library.adoc)
|PurchaseCreated |[Purchase API reference](../../purchase.adoc)

<!-- Ask the team: are the payloads for ApplicationConnectionRemoved, PersonalAssertionDeleted, OrganizationUpdated, and OrganizationFeatureUpdated from the Pusher API? --> 


## Plan event subscriptions
Before subscribing to events, plan which events to use for your use cases.

To get started, you may want to subscribe to the following events:

* To monitor inventory changes in Zettle Point of Sales (POS): `InventoryBalanceChanged`, `InventoryTrackingStarted`, and `InventoryTrackingStopped`
* To monitor product library changes: `ProductCreated`, `ProductUpdated`, and `ProductDeleted`
* To monitor purchases: `PurchaseCreated`
<!-- We can extend this section to be more focused on use cases later on. -->

For more events that you can subscribe, see [Pusher API reference](../api-reference.md).

## Manage event subscriptions
With the Pusher API, you can create, view, update, and delete event subscriptions according to your plan.

* [Create event subscriptions](create-event-subscriptions.md)
* [View event subscriptions](view-event-subscriptions.md)
* [Update event subscriptions](update-event-subscriptions.md)
* [Delete event subscriptions](delete-event-subscriptions.md)

