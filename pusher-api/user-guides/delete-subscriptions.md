Delete subscriptions
=====================
You can delete existing subscriptions that are no longer needed.

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid)
* [Step 2: Delete a subscription](#step-2-delete-a-subscription)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that the authorization is working.
* Make sure that the destination URL on your server is up and running.

## Step 1: Retrieve the subscription UUID

1. Retrieve all existing subscriptions.
   ```
   GET /organizations/{organizationUuid}/subscriptions
   ```
   
   Example:
   
   The following example retrieves all subscriptions for the organization with `a3931584-82b2-4873-a32f-12b254d43539` as the UUID.
   
   ```
   GET /organizations/a3931584-82b2-4873-a32f-12b254d43539/subscriptions
   ```
2. Copy and save the UUID of the subscription that you want to delete. It will be used for deleting the subscription.

## Step 2: Delete a subscription

1. Send a `DELETE` request to delete a subscription. In the request, `subscriptionsUuid` is the version 1 UUID that you retrieved in [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid).
    ```
    DELETE /organizations/{organizationUuid}/{subscriptionUuid}
    ```
       
    Example:
    
    The following example deletes the subscription `ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc`.
    ```
    DELETE /organizations/self/subscriptions/ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc
       
    ```
2. Check that the response returns with an HTTP status code `200 OK`.
    * If yes, the subscription is deleted successfully.
    * If no, update the `DELETE` request according to the error message. For more information on error messages, see [HTTP status code](../api-reference.md#deleteHttpStatusCode).
 
## Related task
* [Create subscriptions](create-subscriptions.md)
* [Update subscriptions](update-subscriptions.md)
* [View subscriptions](view-subscriptions.md)

## Related API reference
* [Pusher API reference](../api-reference.md)
<!-- Add more references if needed. -->
