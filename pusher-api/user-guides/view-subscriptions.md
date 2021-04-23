View subscriptions
=====================
You can view existing subscriptions.

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve the subscription UUID](#step-1-retrieve-the-subscription-uuid)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that the authorization is working.
<!-- to be continued if any -->

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

## Related task
* [Create subscriptions](create-subscriptions.md)
* [Delete subscriptions](delete-subscriptions.md)
* [Update subscriptions](update-subscriptions.md)

## Related API reference
* [Pusher API reference](../api-reference.md)
<!-- Add more references if needed. -->
