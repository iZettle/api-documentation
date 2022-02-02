Set up authorisation code grant
===
To build partner-hosted apps with APIs, you need to set up the authorisation code grant flow using a client ID and client secret.

* [Prerequisites](#prerequisites)
* [Step 1: Initiate the authorisation flow in a browser](#step-1-initiate-the-authorisation-flow-in-a-browser)
* [Step 2: Retrieve access token and refresh token](#step-2-retrieve-access-token-and-refresh-token)
* [Step 3: Generate a new access token from the refresh token](#step-3-generate-a-new-access-token-from-the-refresh-token)
* [Previous task](#previous-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* You have an account for the [Developer Portal](https://developer.zettle.com/). If you don't have an account, [sign up for a developer account](../../../get-started/user-guides/sign-up-for-a-developer-account.md).
* You have credentials for the app. If you don't have any, [create a partner-hosted app](../create-an-app/create-a-partner-hosted-app.md).

## Step 1: Initiate the authorisation flow in a browser
To access the merchant's Zettle account data, redirect the merchant to the Zettle authorisation flow. The merchant will authorise the access request from the app in the authorisation flow.

1. Present the following URL in a browser to redirect the merchant to Zettle authorisation flow. The `redirect_uri` must be the same as the one you used for creating the partner-hosted app.
     
   ```
   https://oauth.zettle.com/authorize?response_type=code&scope={oauth_scope}
   &client_id={client_ID}&redirect_uri={redirect_uri}&state={state_value}
   ```

   Example:
   
   The following example initiating URL asks the merchant to authorise the app with read and write access to the merchant's product library. 
   
   Request   
   ```

   https://oauth.zettle.com/authorize?response_type=code&scope=READ:PRODUC
   T%20WRITE:PRODUCT&client_id=6adde977-c34d-4de1-99b2-
   f6ed3e65431a&redirect_uri=https%3A%2F%2Fwww.example.com%2Fzettle%2Fretu
   rn&state=abc123678
   ```
      
2. After the merchant authenticates themselves and authorises the app to access their data, they will get redirected by the authorisation server to `redirect_uri` together with the authorisation code.

   ```
   https://www.example.com/get?code=4fa87ba8cc7f30e91ad2ab1ad21c1b3e&state=abc123678
   ```
## Step 2: Retrieve access token and refresh token
Use the temporary authorisation code to retrieve a short-lived access token for the first time. The access token is used to get access to the merchant's Zettle account data. When it's retrieved, a long-lived refresh token is also returned. The refresh token is used to generate a new access token when it expires.

> **Note:** You can use the authorisation code to retrieve the access token and refresh token for only one time, as the authorisation code is disposable and can only be used once.

1. Retrieve access token and refresh token using the authorisation code.

   ```
   curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=authorization_code&redirect_uri={redirect_uri}&code={authorisation_code}&client_secret={client_secret}&client_id={client_ID}" https://oauth.zettle.com/token
   ```

   Example:
   
   The following example retrieves access token and refresh token using the authorisation code. The access token is valid for 7200 seconds and the refresh token is valid for 180 days.

   Request   
   ```
   curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=authorization_code&code=4fa87ba8cc7f30e91ad2ab1ad21c1b3e&client_secret=7356b8a1-75ac-4336-970b-bef63cd219c1&client_id=c55de605-48b6-42ef-b69e-cd9d14ded15a&redirect_uri=https%3A%2F%2Fwww.example.com%2Fzettle%2Freturn" https://oauth.zettle.com/token
   ```
   Response         
   ```json
       {
           "access_token": "eyJraWQiOiIxNDQ0NzI3MTY0Njk4Iiwi...yZA",
           "refresh_token": "IZSEC07b0edfc-f557-4e52-a995-384288e2351e",
           "expires_in": 7200
       }
   ```
2. Save the refresh token for generating a new access token when it expires.

## Step 3: Generate a new access token from the refresh token
An access token is valid only for 7200 seconds. When it expires, use the refresh token to generate a new access token. 

> **Note:** Everytime a new access token is generated, a new refresh token is also generated. Even though a refresh token is valid for 180 days, it's recommended to use the new refresh token in the next request for generating a new access token.

1. Use the refresh token to generate a new access token.
     
   ```
   curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=refresh_token&refresh_token={refresh_token}&client_secret={client_secret}&client_id={client_ID}" https://oauth.zettle.com/token
   ```
   Example:
   
   The following example retrieves a new access token using the refresh token. The access token is valid for 7200 seconds. 
   
   Request   
        
   ```
   curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=refresh_token&refresh_token=IZSEC90590831-93a5-4289-ee37-a17824c0fea1&client_secret=IZSECe8b27fdb-faea-46e1-a886-6eded9f84f72}&client_id=6adde977-c34d-4de1-99b2-f6ed3e65431a" https://oauth.zettle.com/token
   ```
   Response         
   ```json
       {
           "access_token": "eyJraWQiOiIxN...R5Y6FDNTva7esJ5Q",
           "refresh_token": "IZSEC07b0edfc-f557-4e52-a995-384288e2351e",
           "expires_in": 7200
       }
   ```

2. Save the new refresh token.
3. In the next request for generating a new access token, use the new refresh token instead of reusing the current refresh token.

 
## Previous task
* [Create a partner-hosted app](../create-an-app/create-a-partner-hosted-app.md)

## Related API reference
* [OAuth2 API Reference](../../../authorization.md)