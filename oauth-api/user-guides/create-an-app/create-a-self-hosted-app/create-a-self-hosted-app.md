Create a self-hosted app
===
A self-hosted app is hosted by one or more merchants individually. To authorise, the app uses the authorisation assertion grant.

In an authorisation assertion grant flow, you use a JSON web token (JWT) assertion to request an access token from the authorisation server. The JWT assertion is used as an API key. An API key is created by a merchant (Zettle merchant account owner). It's valid until the merchant revokes it.
<!-- add assertion grant in glossary -->

To authorise your app using assertion grant, you need to ask the merchant (Zettle merchant account owner) to create an API key at my.zettle.com. You can provide the merchant a deep link that directs them to the API creation page or steps to create an API key.

> **Note:** If you want to build a partner-hosted app that will be hosted by you as an integrator, [create a partner-hosted app](../create-a-partner-hosted-app.md).

* [Prerequisites](#prerequisites)
* [Step 1: Get an API key from a Zettle merchant](#step-1-get-an-api-key-from-a-zettle-merchant)
* [Step 2 (Optional): Prepare for app tracking](#step-2-optional-prepare-for-app-tracking)  
* [Next task](#next-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* You have an account for the [Developer Portal](https://developer.zettle.com/). If you don't have an account, [sign up for a developer account](../../../../get-started/user-guides/sign-up-for-a-developer-account.md).

## Step 1: Get an API key from a Zettle merchant
An API key must be created by the Zettle merchant that will use your app.
 
1. Ask the merchant to create an API key in one of the following ways:
   * Provide the merchant a deep link with pre-populated fields and [the instructions to create an API key with the deep link](create-an-api-key.md#create-an-api-key-with-a-deep-link-from-the-developer).
      ```
      https://my.zettle.com/apps/api-keys?name=<key-name>&scopes=<scopes>
      ```
      Where:
      * `<key-name>` should be the name under which the API key is stored. Keep it short and descriptive. One good practice is to use the integration name as the key name, for example, `WooCommerce`.
      * `<scopes>` should contain the list of needed OAuth scopes separated by a space, for example, `READ:PURCHASE%20READ:FINANCE`.
      
      Example:
      ```
      https://my.zettle.com/apps/api-keys?name=WooCommerce&scopes=READ:PURCHASE%20READ:FINANCE
      ```
   * Provide the merchant OAuth scopes and [the instructions to create an API key with the OAuth scopes](create-an-api-key.md#create-an-api-key-with-oauth-scopes-from-the-developer).
   
2. Ask the merchant to share the created API key and client ID with you.

## Step 2 (Optional): Prepare for app tracking
App tracking is optional. To track a self-hosted app, you need to use the following when setting up the authorisation assertion grant flow for the app:

* The API key from the merchant
* The client ID of a partner-hosted app

> **Note:** The client ID that you receive from the merchant cannot be used to track a self-hosted app. If you want to track a self-hosted app, you need to use the client ID of a partner-hosted app instead of the client ID from the merchant.

To prepare for tracking a self-hosted app:
1. [Create a partner-hosting app](../create-a-partner-hosted-app.md).
2. When setting up the authorisation assertion grant flow, make sure to use the client ID of the partner-hosted app instead of the client ID from the merchant. 

## Next task
* [Set up authorisation assertion grant](../../set-up-app-authorisation/set-up-authorisation-assertion-grant.md)

## Related API reference
* [OAuth2 API Reference](../../../../authorization.md)