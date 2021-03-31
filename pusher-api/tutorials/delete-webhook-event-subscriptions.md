Delete webhook event subscriptions
=====================
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

2. Copy and save the UUID of the event subscription that you want to delete. It will be used for deleting the event subscription.

## Step 2: Delete a webhook event subscription

1. Send a `DELETE` request to create event subscriptions. In the request, `subscriptionsUuid` is the version 1 UUID that you retrieved in [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid).
    
    ```
    DELETE /organizations/{organizationUuid}/{subscriptionsUuid}
    ```
       
    Example:
    
    The following example deletes the event subscription `ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc`.
    ```
        DELETE /organizations/self/subscriptions/`ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc`
       
    ```
    
## Related task
* [Create webhook event subscriptions](create-webhook-event-subscriptions.md)
* [Update webhook event subscriptions](update-webhook-event-subscriptions.md)

## Related API reference
* [Pusher API reference](../api-reference.md)
<!-- Add more references if needed. -->
