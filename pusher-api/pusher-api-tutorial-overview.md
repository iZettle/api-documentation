# Webhook event subscriptions
Using the Zetle Go pusher API, you can subscribe to webhook events toward other Zettle Go APIs, such as the purchase API. With webhook event subscriptions, you will be updated of those events that happen on your Zettle Go instead of pulling information from other Zettle Go APIs.

* [Understand how webhook events work](#understand-how-webhook-events-work)
    * [Payloads](#payloads)
* [Start with webhook event subscriptions](#start-with-webhook-event-subscriptions)
* [Manage event subscriptions](#manage-event-subscriptions)

## Understand how webhook events work
The pusher API provides webhooks events for you to listen to activities of the Zettle Go app at a working HTTPS endpoint on your server.

After you subscribe to webhook events, when an event happens, the pusher API sends a `POST` request with the activity information in a payload to the HTTPS endpoint.

### Payloads
Payloads are response bodies from other APIs, such as the product API. The pusher API sends `POST` requests with payloads in the following format:

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
For example, if you subscribe to the `InventoryTrackingStarted` event, when that event happens at the Zettle Go app, you will receive a notification that looks similar to the following:

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

For more information on payloads, see references of other APIs.
> **Note:** As payloads can be updated due to changes of the other APIs, you can ignore unknown fields.

## Start with webhook event subscriptions
To get started, you may want to subscribe to the following events:

* To monitor inventory changes in Zettle POS: `InventoryBalanceChanged`, `InventoryTrackingStarted`, and `InventoryTrackingStopped`
* To monitor product library changes: `ProductCreated`, `ProductUpdated`, and `ProductDeleted`
* To monitor purchases: `PurchaseCreated`

For more webhooks events that you can subscribe, see [Pusher API reference](api-reference-template-manual.md).

## Manage event subscriptions
With the pusher API, you can create, update, and delete webhook event subscriptions as needed.

* [Create webhook event subscriptions](pusher-api-tutotrial-create-subscriptions.md)
* [Update webhook event subscriptions](pusher-api-tutorial-update-subscriptions.md)
* [Delete webhook event subscriptions](pusher-api-tutorial-delete-subscriptions.md)

