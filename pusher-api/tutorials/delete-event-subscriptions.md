Delete event subscriptions
=====================
You can delete existing event subscriptions that are no longer needed.

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid)
* [Step 2: Delete an event subscription](#step-2-delete-an-event-subscription)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that the authorization is working.
* Make sure that the destination URL on your server is working.
<!-- To Ketkee, if a failing webhook is caused by a faulty destination URL, how can the error be fixed if we cannot PUT with the correct URL? -->

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
2. Copy and save the UUID of the event subscription that you want to delete. It will be used for deleting the event subscription.

## Step 2: Delete an event subscription

1. Send a `DELETE` request to delete an event subscription. In the request, `subscriptionsUuid` is the version 1 UUID that you retrieved in [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid).

    ```
    DELETE /organizations/self/{subscriptionUuid}
    ```
   or
    
    ```
    DELETE /organizations/{organizationUuid}/{subscriptionUuid}
    ```
       
    Example:
    
    The following example deletes the event subscription `ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc`.
    ```
        DELETE /organizations/self/subscriptions/ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc
       
    ```
2. Check that the response returns with an HTTP status code `200 OK`.
    * If yes, the subscription is deleted successfully.
    * If no, update the `DELETE` request according to the error message.
 
## Related task
* [Create event subscriptions](create-event-subscriptions.md)
* [Update event subscriptions](update-event-subscriptions.md)
* [View event subscriptions](view-event-subscriptions.md)

## Related API reference
* [Pusher API reference](../api-reference.md)
<!-- Add more references if needed. -->