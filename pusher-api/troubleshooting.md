# Troubleshoot errors in the Pusher API
* [Failing webhooks](#failing-webhooks)
    * [Retry policy](#retry-policy)
    * [Root cause](#root-cause)
    * [Fix 500 DESTINATION_RESPONDED_WITH_ERROR_CODE](#fix-500-destination_responded_with_error_code)
    * [Fix 400 DESTINATION_NOT_ACCESSIBLE](#fix-400-destination_not_accessible)
* [Related API reference](#related-api-reference)

## Failing webhooks
When the destination URL does not reply with a successful HTTP status code (2xx), the Pusher API will mark the webhook as failing. Also, the Pusher API will notify you of the failing webhook at the email address that is used in the subscription.

### Retry policy
The Pusher API will also re-send event notifications to the destination URL with retry attempts in the following interval and sequence:
1. One retry attempt every 60 seconds for 10 attempts in total
2. One retry attempt every 600 seconds for nine attempts in total
3. One retry attempt every hour in the next 70 hours

During the retry, when the destination URL starts responding with a successful status code, the subscription will be marked as active again.

After the retry, if the destination URL still doesn't reply with a 2xx code, the subscription will be deleted. You will need to create the subscription again.

### Root cause
Failing webhooks may be caused by faulty destination URLs. One of the following error codes may return:

* `HTTP 400 DESTINATION_NOT_ACCESSIBLE`  
 
* `HTTP 500 DESTINATION_RESPONDED_WITH_ERROR_CODE`

### Fix 500 DESTINATION_RESPONDED_WITH_ERROR_CODE
This error usually returns when the destination URL is wrong.

1. Check your local logs and make sure that your service is not failing.
2. Retrieve all existing subscriptions.
       
    ```
    GET /organizations/{organizationUuid}/subscriptions
    ```
   
   Example:
      
      The following example retrieves all subscriptions for the organization with `a3931584-82b2-4873-a32f-12b254d43539` as the UUID.
      
      ```
      GET /organizations/a3931584-82b2-4873-a32f-12b254d43539/subscriptions
      ```
   
 
3. Check that the subscription with the `FAILING` status has the correct destination URL in the response.

    ```
    {
            "uuid": "bc281bd9-bdb0-1011-ad31-6744c6f2972c",
            "transportName": "WEBHOOK",
            "eventNames": [
                "InventoryBalanceChanged"
            ],
            "updated": "2021-03-24T13:43:21.508Z",
            "destination": "https://webhook.site/187c8626-f79c-4b35-8643-9db33d487a34",
            "contactEmail": "187c8626-f79c-4b35-8643-9db33d487a34@email.webhook.site",
            "status": "FAILING",
            "signingKey": "69ZZkaQSdfyb6hwb4rED03rOjiHwGYEh4sORnHbK8hWuxekGP3hBgw95ZFv4FAH6"
        },
    ```
    
4. Does the failing subscription have the correct destination URL?
    * Yes: Check the destination URL is up and running by following [Fix 400 DESTINATION_NOT_ACCESSIBLE](#fix-400-destination_not_accessible).
    * No: [Update subscriptions](user-guides/update-subscriptions.md).
 
    
### Fix 400 DESTINATION_NOT_ACCESSIBLE
This error usually returns when the destination URL is not up and running. Check your local logs and make sure that your service is not failing.

1. Check your local logs and make sure that your service is not failing.
2. If you receive the error while testing webhooks, take one of the following actions: 

    * Restart your local server, if you use a local test environment. 
    * Regenerate a URL, if you use an online server such as webhook.site.
    
3. If you receive the error in a production environment, make sure that the destination URL is up and running.

4. Is HTTP status code 2xx returned?
    * Yes: The error is fixed.
    * No: Contact our [Integrations team](mailto:api@zettle.com) for help.
    <!--If still no, does it mean that subscription itself can be faulty? Or should the integrators contact technical partner support? -->    

## Related API reference
* [Pusher API reference](api-reference.md)
<!-- Add more references if needed. -->
