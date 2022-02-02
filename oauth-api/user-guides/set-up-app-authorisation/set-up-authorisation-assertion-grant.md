Set up authorisation assertion grant
===

To build self-hosted apps with APIs, you need to set up authorisation assertion grant for the app using an API key. An API key is a JSON web token (JWT) assertion. 

* [Prerequisites](#prerequisites)
* [Step 1: Retrieve an access token from an API key](#step-1-retrieve-an-access-token-from-an-api-key)
* [Step 2: Generate a new access token from the API key](#step-2-generate-a-new-access-token-from-the-api-key)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* You have an account for the [Developer Portal](https://developer.zettle.com/). If you don't have an account, [sign up for a developer account](../../../get-started/sign-up-for-a-developer-account.md).

* You have an API key and a client ID for the app. If you don't have any, [create a self-hosted app](../create-an-app/create-a-self-hosted-app).

## Step 1: Retrieve an access token from an API key
To retrieve a short-lived access token from the API key, include the API key and the client ID in the following request.
> **Note:** If you plan to track the app later on, [use the client ID of a partner-hosting app instead of the client ID from the merchant](../create-an-app/create-a-self-hosted-app/create-a-self-hosted-app.md#step-2-optional-prepare-for-app-tracking).
   
   ```
    curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer&client_id={client_ID}&assertion={API_key}" https://oauth.zettle.com/token
   ```

   Example:
   
   The following example retrieves an access token using the assertion grant flow. The response value `expires_in` is the remaining lifetime of the access token in seconds. The access token is valid for 7200 seconds.
   
   Request   
   ```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer&client_id=6adde977-c34d-4de1-99b2-f6ed3e65431a&assertion=eyJraWQiOiIwIiwidHlwIjoiSl...y9V15QKjn4ZgKRumYb_ikw" https://oauth.zettle.com/token
   ```
   Response         
   ```json
       {
           "access_token": "eyJraWQiOiIxNDQ0NzI3MTY0Njk4Iiwi...yZA",
           "expires_in": 7200
       }
   ```

> **Note:** If the API key is invalid or revoked by the merchant (Zettle account owner), the response returns error `invalid_grant`. You need to [get a new API key from the merchant](../create-an-app/create-a-self-hosted-app/create-a-self-hosted-app.md#step-1-get-an-api-key-from-a-zettle-merchant).


## Step 2: Generate a new access token from the API key
An access token is valid only for 7200 seconds. Use the same API key to generate a new access token, as in [Step 1: Retrieve an access token from an API key](#step-1-retrieve-an-access-token-from-an-api-key). 
 

## Previous task
* [Create a self-hosted app](../create-an-app/create-a-self-hosted-app)

## Related API reference
* [OAuth2 API Reference](../../../authorization.md)
