Pusher API reference
=====================
## About Pusher API
The Pusher API publishes information to your service. This information is data related to products, purchases, inventory etc.
 
You can subscribe to specific events in the Pusher API, so that you don't need to poll for data related to the events.  When these events occur, the Pusher API will publish information about the events to your service.

Examples of events:
* `PurchaseCreated` is triggered when a purchase gets created.
* `ProductUpdated` is triggered when product information gets updated in the product library. See the list of all [supported events](#supported-events).

The Pusher API uses webhooks. Webhooks are HTTP callbacks that receive notification messages for events. To create a webhook, users configure a webhook listener and subscribe it to events. A webhook listener is a server that listens at a specific URL for incoming HTTP POST notification messages. The messages are triggered when events occur.

* [About Pusher API](#about-pusher-api)
  * [Base URL](#base-url)
  * [OAuth scope](#oauth-scope)
* [Create a subscription](#create-a-subscription)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Get all subscriptions](#get-all-subscriptions)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Update a subscription](#update-a-subscription)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Delete a subscription](#delete-a-subscription)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Supported events](#supported-events)
* [Examples](#examples)
  * [Create a webhook subscription](#create-a-webhook-subscription)
  * [Get webhook subscriptions](#get-webhook-subscriptions)
  * [Update a webhook subscription](#update-a-webhook-subscription)
  * [Delete a webhook subscription](#delete-a-webhook-subscription)
* [Related resources](#related-resources)
* [Related API reference](#related-api-reference)

### Base URL
`https://pusher.izettle.com`

### OAuth Scope
You can create webhook subscriptions using an access token or API key that corresponds to an organization. To update, delete, or view a subscription, use the same access token or API key.

To create or update a subscription to an event, you will need to be authorized with the corresponding scope.

E.g. you will require the `READ:PURCHASE` scope if you want to subscribe to the `PurchaseCreated` event. See [the list of scopes for supported events](#supported-events).

For more information on how to get authorization for a particular scope, see [OAuth2 API](../authorization.md).

## Create a subscription

Creates a webhook subscription to a specific event.

Once the subscription for an event gets created, the Pusher API will publish data on your service whenever that event occurs.

E.g. You created a subscription for the ProductUpdated event. Whenever a product gets updated in the product library, the ProductUpdated event occurs. Then you will receive event data on the destination server that you have exposed publicly. The event data is a payload for the updated product. See the list of [payloads for all events](concept/event-payloads.md#available-event-payloads).

> **Note:** The Pusher API will push data for an event only once. However, there may be cases where it gets published more than once. You don't need to save the data more than once.

```http
POST /organizations/{organizationUuid}/subscriptions
```

See example [Create a webhook subscription](#create-a-webhook-subscription).


### Parameters

<details open="true"><!-- start tag of the Parameters section-->
<summary>Click to hide all request parameters for creating a subscription.</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li>Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li>Get it by using the `users/me` endpoint of OAuth2 API. For more information, see [OAuth2 API](../authorization.md).</li></ul> 
|uuid |string |query |required | Unique identifier for the subscription as UUID version 1.
|transportName |string |query |required | The message option used by Pusher API. Currently only `WEBHOOK` is supported. 
|eventNames |array |query |required | Event names for events that you want to create subscription for. The events are specified in an array. <br/>If you pass an empty array, you will subscribe to all events that the Pusher API supports. In this case, make sure that you have all the corresponding authorization scopes issued. See [the list of scopes](#supported-events).
|destination |string |query |required | Your service URL publicly exposed where the Pusher API will publish messages for subscribed events.
|contactEmail |string |query |required | The email address used to notify in case of any errors in subscription or the destination. <br/>The email must be a valid email address and should not exceed 512 characters.


### Responses
<details open="true">
<summary name="createHttpStatusCode">Click to hide HTTP status codes.</summary>

|Status code |Description
|---- |----
|200 OK |Returned when a subscription is successfully created.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li>The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|403 Forbidden | Returned when the scope being used in the request is incorrect. <br/> E.g. If you provide a permission scope of `READ:PRODUCT` while creating a subscription for `PurchaseCreated` then the Pusher API will return 403 in the response.
|400 Bad Request |Returned when one of the following occurs: <br/><ul><li>The `transportName` or `uuid` is missing in the request.</li><li>A subscription with the `uuid` passed in request already exists.</li><li>The `destination` is not accessible.</li></ul>
|422 Unprocessable Entity |Returned when one of the following occurs: <br/><ul><li>The `destination` or `contactEmail` is missing in the request.</li><li>The `contactEmail` has an invalid value for email address.</li></ul>
|500 Internal Server Error|Returned when one of the following occurs: <br/><ul><li>The `destination` responded with a non-successful HTTP response code.</li><li>The Pusher API encountered an internal server error. In case this error persists, contact [support](mailto:api@zettle.com).</li></ul>
</details>

<details open="true">
<summary>Click to hide response attributes.</summary>
<p>A successful <code>200 OK</code> response will have the following attributes:</p>

|Name |Type |Description
|---- |---- |----
|uuid |string |Unique identifier for the created subscription as UUID version 1. 
|transportName |string |The message option used by Pusher API. Currently only `WEBHOOK` is supported.
|eventNames|array|The event names for the created subscription.
|updated|string|The timestamp (in UTC) when the subscription was last updated.
|destination|string|The public URL exposed by you where the Pusher API will publish messages for subscribed events.
|contactEmail|string|The email used to notify any errors for subscriptions or the destination.
|status|string|The status of the created subscription.
|signingKey|string| The key used to verify that all incoming webhook messages from the Pusher API originate from Zettle.
</details>


## Get all subscriptions

Gets all the webhook subscriptions for an organisation.

```http
GET /organizations/{organizationUuid}/subscriptions
```

See example [Get webhook subscriptions](#get-webhook-subscriptions).

### Parameters

<details open="true">
<summary>Click to hide all request parameters for getting all subscriptions.</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li>Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the `users/me` endpoint of OAuth2 API. For more information, see [OAuth2 API](../authorization.md).</li></ul>  
</details>


### Responses
<details open="true">
<summary>Click to hide HTTP status codes.</summary>

|Status code |Description
|---- |----
|200 OK| Returned when the Pusher API returns a collection of subscriptions for the client. 
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li>The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul> 
|500 Internal Server Error| Returned when the Pusher API encounters an internal server error. In case this error persists, contact [support](mailto:api@zettle.com).
</details>



<details open="true">
<summary>Click to hide response attributes.</summary>
<p>A successful <code>200 OK</code> response will return an array of subscriptions. Each subscription contains the following response attributes:</p>


|Name |Type |Description
|---- |---- |----
|uuid |string |Unique identifier for the created subscription as UUID version 1.
|transportName |string |The message option used by Pusher API. Currently only `WEBHOOK` is supported.
|eventNames|array|All the events for the created subscription.
|updated|string|The timestamp (in UTC) when the subscription was last updated.
|destination|string|The destination URL where the Pusher API will push messages for subscribed events.
|contactEmail|string|The email used to notify any errors for subscriptions or the destination.
|status|string|The status of the created subscription.
|signingKey|string| The key used to verify that all incoming messages from the Pusher API originate from Zettle. 
</details>

## Update a subscription

Updates an existing webhook subscription.

```http
PUT /organizations/{organizationUuid}/subscriptions/{subscriptionUuid}
```

See example [Update a webhook subscription](#update-a-webhook-subscription).

### Parameters

<details open="true">
<summary>Click to hide all request parameters for updating a subscription.</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li>Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the `users/me` endpoint of OAuth2 API. For more information, see [OAuth2 API](../authorization.md).</li></ul> 
|subscriptionUuid |string |path |required |Unique identifier for an existing subscription as UUID version 1.
|transportName |string |query |optional | The message option used by the Pusher API. E.g. `WEBHOOK`. You need to specify the same option that you used while creating the subscription.
|eventNames |array |query |optional | Events that you want to update on the existing subscription. The events are specified in an array.
|destination |string |query |optional | The destination URL where the Pusher API will push data for the updated subscription.
|contactEmail |string |query |optional | The email address used to notify in case of any errors in subscription or the destination. <br/>The email must be a valid email address and should not exceed 512 characters.
</details>


### Responses
<details open="true">
<summary name="updateHttpStatusCode">Click to hide HTTP status codes.</summary>

|Status code |Description
|---- |----
|200 OK| Returned when the Pusher API updates the subscription successfully.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li>The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|404 Not Found | Returned when the subscription to be updated does not exist or cannot be found.
|405 Method Not Allowed | Returned when the `subscriptionUuid` is missing in the request.
|400 Bad Request| Returned when the `eventNames` parameter contains events that are not supported by the Pusher API.
|422 Unprocessable Entity| Returned when one of the following occurs: <br/><ul><li> Returned if the `destination` specified in the request is empty.</li><li>The `destination` value is not a valid HTTPS URL.</li></ul>
|500 Internal Server Error| Returned when the Pusher API encounters an internal server error. In case this error persists, contact [support](mailto:api@zettle.com).
</details>

<details open="true">
<summary>Click to hide response attributes.</summary><br/>
<p>The Pusher API returns a <code>200 OK</code> response without any content.</p>
</details>


## Delete a subscription

Deletes an existing webhook subscription.

```http
DELETE /organizations/{organizationUuid}/subscriptions/{subscriptionUuid}
```

See example [Delete a webhook subscription](#delete-a-webhook-subscription).

### Parameters

<details open="true">
<summary>Click to hide all request parameters for deleting a subscription.</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li>Use `self` as the value. This will retrieve your organizationUuid from the authentication token in the request.</li><li> Get it by using the `users/me` endpoint of OAuth2 API. For more information, see [OAuth2 API](../authorization.md).</li></ul> 
|subscriptionUuid |string |path |required |Unique identifier for an existing subscription as UUID version 1.
</details>


### Responses
<details open="true">
<summary name="deleteHttpStatusCode">Click to hide HTTP status codes.</summary>

|Status code |Description
|---- |----
|204 No Content| Returned when the Pusher API deletes the subscription successfully.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li>The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|404 Not Found|Returned when one of the following occurs: <br/><ul><li>The `subscriptionUuid` is missing in the request.</li><li>A subscription was not found for the `subscriptionUuid` passed in the request</li></ul> 
|500 Internal Server Error| Returned when the Pusher API encounters an internal server error. In case this error persists, contact [support](mailto:api@zettle.com).
</details>

<details open="true">
<summary>Click to hide response attributes.</summary><br/>
<p>The Pusher API returns a <code>204 No Content</code> response without any content.</p>
</details>

## Supported events

<details open="true">
<summary>Click to hide a list all the events supported by the Pusher API and their corresponding authorization scopes.</summary>

|Event name |Required scope | Trigger
|---- |---- |----
|PurchaseCreated|READ:PURCHASE|A purchase is finalized and a receipt is created.
|OrganizationUpdated|READ:USERINFO|A property of an organization is updated. E.g. name, email address etc.
|ProductCreated|READ:PRODUCT|A product gets created in the product library.
|ProductUpdated|READ:PRODUCT|A product gets updated in the product library.
|ProductDeleted|READ:PRODUCT|A product gets deleted in the product library.
|InventoryBalanceChanged|READ:PRODUCT|Either: <br/><ul><li>A sale is made for a product and purchase gets created. This leads to change in the balance of the product in the inventory.</li><li>The stock of a product gets updated in the product library.</li></ul>
|InventoryTrackingStarted|READ:PRODUCT|Inventory tracking for a product is enabled in the Zettle Go app.
|InventoryTrackingStopped|READ:PRODUCT|Inventory tracking for a product is disabled in the Zettle Go app.
|ApplicationConnectionRemoved| any scope| The application was disconnected from Zettle organization and the OAuth refresh token has been invalidated. 
|PersonalAssertionDeleted|any scope| The API key used to create subscriptions is deleted.


</details>


## Examples

### Create a webhook subscription
Request 
```http
POST /organizations/{organizationUuid}/subscriptions
```
```json
{
  "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
  "transportName": "WEBHOOK",
  "eventNames": ["ProductUpdated"],
  "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
  "contactEmail": "your_email@domain.com"
}
```
Response
```json
{
    "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
    "transportName": "WEBHOOK",
    "eventNames": [
        "ProductUpdated"
    ],
    "updated": "2021-03-29T16:31:47.087507Z",
    "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
    "contactEmail": "your_email@domain.com",
    "status": "ACTIVE",
    "signingKey": "zLzClQLQN8yfH8aEjONeXzgJRAHR0zpD7RonFCpizujCUCectBlln0vFArTbLPYa"
}
```


### Get webhook subscriptions
Request 
```http
GET /organizations/self/subscriptions
``` 

Response
```json
[
    {
        "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
        "transportName": "WEBHOOK",
        "eventNames": [
            "ProductUpdated"
        ],
        "updated": "2021-03-30T07:48:52.228Z",
        "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
        "contactEmail": "your_email@domain.com",
        "status": "ACTIVE",
        "signingKey": "U0Ani1WTTwwbjUnHlQAEDCRXQoQn9z9e4q55UUTyeZJt89z9XXN5ssd4Cgh0evJa"
    },
    {
        "uuid": "6eefcb26-912c-11eb-a8b3-0242ac130003",
        "transportName": "WEBHOOK",
        "eventNames": [
            "ProductDeleted"
        ],
        "updated": "2021-03-30T07:49:35.294Z",
        "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
        "contactEmail": "your_email@domain.com",
        "status": "ACTIVE",
        "signingKey": "PSjClzC2HNEZZoFU7aaF9DSmn9WJBXLfEIAkbBq15lNknnijPT2AKEsvf2YHPrsQ"
    },
    {
        "uuid": "7c9e43ce-912c-11eb-a8b3-0242ac130003",
        "transportName": "WEBHOOK",
        "eventNames": [
            "InventoryBalanceChanged",
            "ProductCreated"
        ],
        "updated": "2021-03-30T07:50:13.489Z",
        "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
        "contactEmail": "your_email@domain.com",
        "status": "ACTIVE",
        "signingKey": "H2XDdE5REqT55rDTvAPqpA8UDY4mO3QSmcLO7h0PxvJ4qETKfx9rfzVxuKplUOYz"
    },
    {
        "uuid": "08db4d6e-912d-11eb-a8b3-0242ac130003",
        "transportName": "WEBHOOK",
        "eventNames": [
            "InventoryBalanceChanged",
            "ProductCreated"
        ],
        "updated": "2021-03-30T07:54:11.798Z",
        "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
        "contactEmail": "your_email@domain.com",
        "status": "ACTIVE",
        "signingKey": "s4T28XWVGBl8us8fvRxmY4HnFQgTbMiFtqniXpWrAIx0rv5YN7RFAygyjWSg7Nip"
    }
]
```

### Update a webhook subscription
Request
```http
PUT /organizations/self/subscriptions/df209936-8f31-11eb-8dcd-0242ac130003
```
```json
{
  "eventNames": ["ProductUpdated", "CardPaymentInvalid"]
}
```

Response
```http
200 OK
```

### Delete a webhook subscription

Request
```http
DELETE /organizations/self/subscriptions/uuid/f02f80f8-8f35-11eb-8dcd-0242ac130003
```

Response
```http
204 No content
```


## Related resources
[Pusher API user guide](user-guides)

## Related API reference
[OAuth2 API Reference](../authorization.md)
