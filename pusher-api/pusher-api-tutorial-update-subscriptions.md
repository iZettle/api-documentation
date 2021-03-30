# Update event subscriptions with the pusher API
You can make changes on existing event subscriptions.

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid)
* [Step 2: Update a webhook event subscription](#step-2-update-a-webhook-event-subscription)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
None
<!-- to be continued if any -->

## Step 1: Retrieve the subscription UUID

1. Retrieve all subscriptions that you created.

    ```
    GET /organizations/self/subscriptions
    ```

2. Copy and save the UUID. It will be used for updating event subscription.


## Step 2: Update a webhook event subscription

1. Send a `PUT` request to create event subscriptions.
    
    ```
    PUT /organizations/self/subscriptions/{subscriptionUuid}
   {
     "eventNames": ["<event names>"],
     "destination": "<URL to receive events>",
     "contactEmail": "<email to receive notifications>"
   }   
    ```
  
    Where:

    * `eventNames` specifies one or more events that you want to subscribe. Event names are separated by a comma.
    * `destination` points to the URL that you want to receive events for monitoring.
    * `contactEmail` specifies an email to which you want to receive notifications.
    
    Example:
    
    The following example updates the event subscrption `ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc` and subscribes to event `ProductCreated` and `PurchaseCreated`.
    ```
        PUT /organizations/self/subscriptions/`ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc`
       {
            "eventNames": [
            "ProductCreated",
            "PurchaseCreated"
         ],
         "destination": "https://yoururl.domain",
         "contactEmail": "email_if_it_breaks@domain.com"
       }   
    ```
    
## Related task
* [Create event subscriptions with the pusher API](pusher-api-tutorial-create-subscriptions.md)
* [Delete event subscriptions with the pusher API](pusher-api-tutorial-delete-subscriptions.md)

## Related API reference
* [Pusher API reference](api-reference.md)
<!-- Add more references if needed. -->
