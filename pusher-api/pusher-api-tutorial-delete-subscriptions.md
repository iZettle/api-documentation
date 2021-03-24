# Delete webhook event subscriptions
You can delete event subscriptions that are no longer needed.

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid)
* [Step 2: Delete a webhook event subscription](#step-2-delete-a-webhook-event-subscription)
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

## Step 2: Delete a webhook event subscription
=======

1. Send a `DELETE` request to create event subscriptions.
    
    ```
    DELETE /organizations/{organizationUuid}/{subscriptionsUuid}
    ```
  
    Where:

    * `subscriptionsUuid` is the version 1 UUID that you generated in [Step 1: Generate a version 1 UUID](#step-1-generate-a-version-1-uuid).
    
    Example:
    
    The following example updates the event subscrption `ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc` and subscribes to event `ProductCreated` and `PurchaseCreated`.
    ```
        DELETE /organizations/self/subscriptions/`ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc`
       
    ```
    
## Related task
* [Create webhook event subscriptions](pusher-api-tutorial-create-subscriptions.md)
* [Update webhook event subscriptions](pusher-api-tutorial-update-subscriptions.md)

## Related API reference
* [Pusher API reference](api-reference-template-manual.md)
<!-- Add more references if needed. -->
