<!--
title: Pusher API Reference Guide
description: For example, Purchase API Guide. This template is intended for manually creating API reference guide.
author: DevX Team <devx@paypal.com> # Replace it with your team email alias.
-->

<!-- For DevX team future reference, another approach to this would be to list common objects(along with their fields) required across all endpoints at the top so as to avoid repetition. That way, if the object structure changes, developers do not have to change at multiple places. -->

<!-- Write full sentences.
Use as few as acronyms and abbreviations as possible. If you need to use an acronym, make sure spell it out at its first occurrence.
Replace anything like <__text__> with a text in regular font. Do not use italic. For syntax reference, see https://www.markdownguide.org/extended-syntax/.
-->
* [About Pusher API](#about-pusher-api)
  * [Base URL](#base-url)
  * [OAuth scope](#oauth-scope)
* [Create a webhook subscription](#create-a-webhook-subscription)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Get all the webhook subscriptions](#get-all-the-webhook-subscriptions)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Update a webhook subscription](#update-a-webhook-subscription)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Delete a webhook subscription](#delete-a-webhook-subscription)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Supported events](#supported-events)
* [Examples](#examples)
  * [Create a subscription](#create-a-subscription)
  * [Get subscriptions](#get-subscriptions)
  * [Update a subscription](#update-a-subscription)
  * [Delete a subscription](#delete-a-subscription)

## About Pusher API
Pusher is a service that publishes information to the integrator's service. This information is data related to products, purchases, inventory etc. The purpose of this service is to ensure that integrators do not have to poll for data related to specific events. <br/> 
An integrator can subscribe to specific events in the Pusher service. When these events get triggered, Pusher service will publish information corresponding to the event to the integrator's service.

Examples of events:
* PurchaseCreated - This gets triggered when a purchase gets created.
* ProductUpdated - This gets triggered when product information gets updated in the product library. 
See the list of [Supported events](#_supported-events_).

The Pusher service uses Webhooks. Webhooks are custom callbacks which are used to send data from one application to another when a specific event gets triggered. <br/>
The other option that Pusher service supports is [AWS SQS](https://aws.amazon.com/sqs/). However, this is currently available only to selected integrators and can be made available to others as per the use case. 

### Base URL
https://pusher.izettle.com

### OAuth Scope
In order to create or update a subscription to an event, you will need to be authorized with the corresponding scope.

E.g. you will require the `READ:PURCHASE` scope if you want to subscribe to the `PurchaseCreated` event.

See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information on how to get authorization for a particular scope.

## Create a webhook subscription

Creates a webhook subscription to a specific event. 

```
POST /organizations/{organizationUuid}/subscriptions
```

See [example use scenario 1](#_example-use-scenario-1_).


### Parameters

<details><!-- start tag of the Parameters section-->
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li><li> Use "self" as the value. This will retrieve your organizationUuid through the authentication information in the request.</li></ul> 
|uuid |string |query |required | Unique identifier for the subscription as UUID version 1.
|transportName |string |query |required | The message option used by Pusher service. Currently only `WEBHOOK` is supported. `SQS` is available only to certain clients based on the use case.
|eventNames |array |query |required | Events that you want to create subscription for. The events are specified in an array. If you pass an empty array, you will subscribe to all events that the service supports. See the list of [supported events](#supported-events).
|destination |string |query |required | The public url exposed by the integrator where the Pusher service will publish messages for subscribed events.
|contactEmail |string |query |required | The email address used to notify in case of any errors in subscription or the destination. <br/> The email must be a valid email address and should not exceed 512 characters.
</details><!-- end tag of the Parameters section-->


### Responses
<details>
<summary>HTTP status codes</summary>

|Status code |Description
|---- |----
|200 OK |Returned when a subscription is successfully created.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|403 Forbidden | Returned when the scope being used in the request is incorrect. <br/> E.g. If you provide a permission scope of `READ:PRODUCT` while creating a subscription for `PurchaseCreated` then the service will return 403 in the response.
|400 Bad Request |Returned when one of the following occurs: <br/><ul><li> The `transportName` or `uuid` is missing in the request.</li><li>A subscription with the `uuid` passed in request already exists.</li></ul>
|422 Unprocessable Entity |Returned when the `destination` or `contactEmail` is missing in the request.

</details>

<details>
<summary>Response attributes</summary>
<p>A successful <code>200 OK</code> response will have the following attributes:</p>

|Name |Type |Description
|---- |---- |----
|uuid |string |Unique identifier for the created subscription as UUID version 1. 
|transportName |string |Pusher service option used for creating the subscription.
|eventNames|array|All the events for the created subscription.
|updated|string|The timestamp (in UTC) when the subscription was last updated.
|destination|string|The public url exposed by the integrator where the Pusher service will publish messages for subscribed events.
|contactEmail|string|The email used to notify any errors for subscriptions or the destination.
|status|string|The status of the created subscription.
|signingKey|string| The key used to verify that all incoming webhook messages from Pusher service originate from Zettle.
</details>


Once the subscription for an event gets created successfully, the service will publish data on the integrator's service when that event gets triggered.<br/>
E.g. when you create a subscription for the ```ProductUpdated``` event and whenever a product gets updated in the product library, the `ProductUpdated` event gets triggered. You will then receive event data i.e, payload for the updated product on the ```destination``` that you have exposed publicly.
See a list of payloads for all events at #_payloads_

The service will push data for an event only once. However, there may be cases where it gets published more than once. The integrator will then have to take care to not save the data more than once.


## Get all the webhook subscriptions

Gets all the webhook subscriptions for an integrator.

```
GET /organizations/{organizationUuid}/subscriptions
```

See [example use scenario 2](#_example-use-scenario-2_).

### Parameters

<details>
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li><li> Use "self" as the value. This will retrieve your organizationUuid through the authentication information in the request.</li></ul> 
</details>


### Responses
<details>
<summary>HTTP status codes</summary>

|Status code |Description
|---- |----
|200 OK| Returned when the service returns a collection of subscriptions for the client. 
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul> 
</details>



<details>
<summary>Response attributes</summary>
<p>A successful <code>200 OK</code> response will return an array of subscriptions. Each subscription contains the following response attributes:</p>


|Name |Type |Description
|---- |---- |----
|uuid |string |Unique identifier for the created subscription as UUID version 1.
|transportName |string |Pusher service option used for creating the subscription.
|eventNames|array|All the events for the created subscription.
|updated|string|The timestamp (in UTC) when the subscription was last updated.
|destination|string|The destination url where the Pusher service will push messages for subscribed events.
|contactEmail|string|The email used to notify any errors for subscriptions or the destination.
|status|string|The status of the created subscription.
|signingKey|string| The key used to verify that all incoming messages from Pusher service originate from Zettle. 
</details>

## Update a webhook subscription

Updates an existing webhook subscription.

```
PUT /organizations/organizationUuid}/subscriptions/{subscriptionUuid}
```

See [example use scenario 3](#_example-use-scenario-3_).

### Parameters

<details>
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li><li> Use "self" as the value. This will retrieve your organizationUuid through the authentication information in the request.</li></ul>
|subscriptionUuid |string |path |required |Unique identifier for an existing subscription as UUID version 1. 
|transportName |string |query |optional | The message option used by Pusher service. E.g. ```WEBHOOK```. You need to specify the same option that you used while creating the subscription.
|eventNames |array |query |optional | Events that you want to update on the existing subscription. The events are specified in an array.
|destination |string |query |optional | The destination url where Pusher service will push data for the updated subscription.
|contactEmail |string |query |optional | The email address used to notify in case of any errors in subscription or the destination. <br/> The email must be a valid email address and should not exceed 512 characters.
</details>


### Responses
<details>
<summary>HTTP status codes</summary>

|Status code |Description
|---- |----
|200 OK| Returned when the service updates the subscription successfully.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|405 Method Not Allowed | Returned when the ```subscriptionUuid``` is missing in the request.
|400 Bad Request| Returned when the ```eventNames``` parameter contains events that are not supported by the Pusher service.
|422 Unprocessable Entity| Returned if the ```destination``` specified in the request is empty. In case you specify the ```destination``` parameter, it has to be a valid https url.
</details>

<details>
<summary>Response attributes</summary><br/>
<p>The service returns a <code>200 OK</code> response without any content.</p>
</details>


## Delete a webhook subscription

Deletes an existing webhook subscription.

```
DELETE /organizations/{organizationUuid}/subscriptions/{subscriptionUuid}
```

See [example use scenario 4](#_example-use-scenario-4_).

### Parameters

<details>
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|organizationUuid |string |path |required |Unique identifier for your organization. You can use following options to fill in this value: <br/><ul><li> Get it by using the https://oauth.izettle.com/users/me endpoint of OAuth2 API. See [OAuth2 API](https://github.com/iZettle/api-documentation/blob/master/authorization.adoc) for more information.</li><li> Use "self" as the value. This will retrieve your organizationUuid through the authentication information in the request.</li></ul>
|subscriptionUuid |string |path |required |Unique identifier for an existing subscription as UUID version 1.
</details>


### Responses
<details>
<summary>HTTP status codes</summary>

|Status code |Description
|---- |----
|204 No Content| Returned when the service deletes the subscription successfully.
|401 Unauthorized |Returned when one of the following occurs: <br/><ul><li> The authentication information is missing in the request.</li><li>The authentication token has expired.</li><li>The authentication token is invalid.</li></ul>
|404 Not Found| Returned when the ```subscriptionUuid``` is missing in the request.
</details>

<details>
<summary>Response attributes</summary><br/>
<p>The service returns a <code>204 No Content</code> response without any content.</p>
</details>

## Supported events

<details>
<summary>You can subscribe to the following events through the service</summary>

|Event name |Required scope | Description
|---- |---- |----
|PurchaseCreated|READ:PURCHASE|Triggered when a new purchase is created
|InvoiceCreated|READ:FINANCE|Triggered when a new invoice is created
|OrganizationUpdated|READ:USERINFO|Triggered when information of an organization is updated
|OrganizationFeatureUpdated|READ:USERINFO|Triggered when an organization feature set has been updated
|PaymentInitiated|READ:FINANCE|Triggered when a new payment has been initiated
|PaymentCreated|READ:FINANCE|Triggered when a new payment gets created
|PaymentCanceled|READ:FINANCE|Triggered when a payment gets cancelled
|CardPaymentAuthorized|READ:FINANCE|Triggered when a card payments gets authorized successfully
|CardPaymentInvalid|READ:FINANCE|Triggered when a card payment is marked invalid
|ProductCreated|READ:PRODUCT|Triggered when a product gets created in the product library
|ProductUpdated|READ:PRODUCT|Triggered when a product gets updated in the product library
|ProductDeleted|READ:PRODUCT|Triggered when a product gets deleted in the product library
|InventoryBalanceChanged|READ:PRODUCT|Triggered when the balance in the inventory gets changed for a product
|InventoryTrackingStarted|READ:PRODUCT|Triggered when the inventory tracking is enabled for a product
|InventoryTrackingStopped|READ:PRODUCT|Triggered when the inventory tracking is disabled for a product
|ApplicationConnectionRemoved| any scope| Triggered when the application was disconnected from Zettle organization and the OAuth refresh token has been invalidated
|PersonalAssertionDeleted|any scope| Triggered when an API key was deleted


</details>


## Examples

### Create a subscription
_Request_ ```POST /organizations/{organizationUuid}/subscriptions```
```
{
  "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
  "transportName": "WEBHOOK",
  "eventNames": ["ProductUpdated"],
  "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
  "contactEmail": "karyamane@izettle.com"
}
```
_Response_
```
{
    "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
    "transportName": "WEBHOOK",
    "eventNames": [
        "ProductUpdated"
    ],
    "updated": "2021-03-29T16:31:47.087507Z",
    "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
    "contactEmail": "karyamane@izettle.com",
    "status": "ACTIVE",
    "signingKey": "zLzClQLQN8yfH8aEjONeXzgJRAHR0zpD7RonFCpizujCUCectBlln0vFArTbLPYa"
}
```


### Get subscriptions
_Request_ ```GET /organizations/self/subscriptions``` 

_Response_
```
[
{
        "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
        "transportName": "WEBHOOK",
        "eventNames": [
            "ProductUpdated"
        ],
        "updated": "2021-03-30T07:48:52.228Z",
        "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
        "contactEmail": "testemail@izettle.com",
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
        "contactEmail": "testemail@izettle.com",
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
        "contactEmail": "testemail@izettle.com",
        "status": "ACTIVE",
        "signingKey": "H2XDdE5REqT55rDTvAPqpA8UDY4mO3QSmcLO7h0PxvJ4qETKfx9rfzVxuKplUOYz"
    }
]
```

### Update a subscription
_Request_ ```PUT /organizations/self/subscriptions/df209936-8f31-11eb-8dcd-0242ac130003```

_Response_ <br/>
```200 OK```

### Delete a subscription

```DELETE /organizations/self/subscriptions/uuid/f02f80f8-8f35-11eb-8dcd-0242ac130003```

_Response_ <br/>
```204 No content```


## Related use scenario
<!-- One or more tasks that will be done after this one. -->
<!-- Add more use scenarios if needed. -->
[API Tutorial Template](../asciidoc-templates/api-tutorial-template.adoc)

## Related API reference
<!-- Other APIs that may be related in use scenarios. -->
[API reference Template](../api-tutorial-reference.adoc)
