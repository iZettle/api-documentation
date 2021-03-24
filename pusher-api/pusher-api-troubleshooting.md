# Troubleshoot Errors in the pusher API
* [Failing webhooks](#failing-webhooks)
    * [Root cause](#root-cause)
    * [Fix 404 DESTINATION_RESPONDED_WITH_ERROR_CODE](#fix-404-destination_responded_with_error_code)
    * [Fix 400 DESTINATION_NOT_ACCESSIBLE](#fix-400-destination_not_accessible)
* [Related API reference](#related-api-reference)

## Failing webhooks
When the webhook destination URL does not reply with a successful HTTP status code (2xx), the pusher API will mark the webhook as failing. 

The pusher API will email you about failing webhooks at the email address in the event subscription.
<!-- when does the email be sent? At the very first failure?-->

The pusher API will also re-send event notifications to the destination URL with attempts in the following interval and sequence:
1. Every 60 seconds for 10 attempts in total
2. Every 600 seconds for nine attempts in total
3. Every hour in the next 70 hours

During the attempts, when the destination URL starts responding with a successful status code, the subscription will be marked as active again.

After all attempts, if the destination URL still doesn't reply with a 2xx code, the event subscription will be deleted.

### Root cause
Failing webhooks can be caused that the HTTPS endpoint is faulty. One of the following error codes may return:

* `HTTP 400 DESTINATION_NOT_ACCESSIBLE`  
 
* `HTTP 404 DESTINATION_RESPONDED_WITH_ERROR_CODE`

### Fix 404 DESTINATION_RESPONDED_WITH_ERROR_CODE
This error usually returns when the destination URL is wrong.

1. Retrieve subscriptions.

    ```
    GET /organizations/self/subscriptions
    ```
 
2. Check that the event subscription with the `failing` status has the correct destination URL in the response.

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
    
3. Does the failing event subscription have the correct destination URL?
    * Yes: Check the destination URL is up and running by following [Fix 400 DESTINATION_NOT_ACCESSIBLE](#fix-400-destination_not_accessible).
    * No: [Update event subscriptions with the pusher API](pusher-api-tutorial-update-subscriptions.md).
 
    
### Fix 400 DESTINATION_NOT_ACCESSIBLE
This error usually returns when the destination URL is not up and running.

1. If you receive the error while testing webhooks, take one of the following actions: 

    * Restart your local server, if you use a local test environment. 
    * Regenerate a URL, if you use an online server such as webhook.site.
    
2. If you receive the error in a production environment, make sure that the destination URL is up and running.

3. Is HTTP status code 2xx returned?
    * Yes: The error is fixed.
    * No: Contact our [Integrations team](mailto:api@zettle.com) for help.
    <!--If still no, does it mean that subscription itself can be faulty? Or should the integrators contact technical partner support? -->    

## Related API reference
* [API Reference Template](api-reference-template-manual.md)
<!-- Add more references if needed. -->
