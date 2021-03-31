Update webhook event subscriptions
=====================
You can make changes on existing event subscriptions.

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid)
* [Step 2: Update an event subscription](#step-2-update-an-event-subscription)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that the authorization is working.
* Make sure that the destination URL on your server is working.
<!-- To Ketkee, if a failing webhook is caused by a faulty destination URL, how can the error be fixed if we cannot PUT with the correct URL? -->
<!-- to be continued if any -->

## Step 1: Retrieve the subscription UUID

1. Retrieve all existing subscriptions.

    ```
    GET /organizations/self/subscriptions
    ```
   or
   
   ```
   GET /organizations/{organizationUuid}/subscriptions
   ```
   
   Example:
   
   The following example retrieves all subscriptions for the organization with `a3931584-82b2-4873-a32f-12b254d43539` as the UUID.
   
   ```
   GET /organizations/a3931584-82b2-4873-a32f-12b254d43539/subscriptions
   ```
   

2. Copy and save the UUID of the event subscription that you want to update. It will be used for updating event subscription.


## Step 2: Update an event subscription

1. Send a `PUT` request to update an event subscription. In the request, `subscriptionUuid` is the version 1 UUID that you retrieved in [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid).
    
    ```
    PUT /organizations/self/subscriptions/{subscriptionUuid}
   {
     "eventNames": ["<event names>"],
     "destination": "<URL to receive events>",
     "contactEmail": "<email to receive notifications>"
   }   
    ```
   or
   ```
       PUT /organizations/{organizationUuid}/subscriptions/{subscriptionUuid}
      {
        "eventNames": ["<event names>"],
        "destination": "<URL to receive events>",
        "contactEmail": "<email to receive notifications>"
      }   
   ```
  
    Example:
    
    The following example updates the event subscription `ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc` and subscribes to event `ProductCreated` and `PurchaseCreated`.
    ```
        PUT /organizations/self/subscriptions/ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc
       {
            "eventNames": [
            "ProductCreated",
            "PurchaseCreated"
         ],
         "destination": "https://yoururl.domain",
         "contactEmail": "email_if_it_breaks@domain.com"
       }   
    ```
2. Check that the response returns with an HTTP status code `200 OK`.
    * If yes, the subscription is updated successfully.
    * If no, update the `PUT` request according to the error message.
  
## Related task
* [Create event subscriptions](create-event-subscriptions.md)
* [Delete event subscriptions](delete-event-subscriptions.md)
* [View event subscriptions](view-event-subscriptions.md)
## Related API reference
* [Pusher API reference](../api-reference.md)
<!-- Add more references if needed. -->
