Create an API key
======

An API key is used in the authorisation assertion grant flow for an app. It must be created by a Zettle merchant in their my.zettle.com.

If you're developer of a self-hosted app, share this guide with the merchant that will use the app.

* [Create an API key with a link from the developer](#create-an-api-key-with-a-link-from-the-developer)
* [Create an API with OAuth scopes from the developer](#create-an-api-key-with-oauth-scopes-from-the-developer)

## Prerequisites
* You are a Zettle merchant and have an account at my.zettle.com.
* You have a link or OAuth scopes from the developer of the app that you will use.  The OAuth scopes are permissions that you give to the developer to access your Zettle account data. 

## Create an API key with a link from the developer

1. Open the link from the developer.

2. Enter your Zettle account password.
   
3. Review the information and click **Create key**. A client ID will be created together with the API key.
       
   <img id="create-API-keys-from-the-deep-link" src="../../../images/create-API-keys-from-the-deep-link.png" alt="This screenshot shows the dialog that is started after clicking a deep link for creating API keys. You can find the Create key button in the lower right corner of the dialog.">       
    
4. On the Create API key page, click **Copy key**. Keep the API key and the client ID as secret somewhere safe.
       
   <img id="copy-key" src="../../../images/copy-key.png" alt="This screenshot shows the dialog where you can copy the API key and save it for later use. You can find the Copy key button in the lower right corner of the dialog.">
    
5. Share the API key and client ID with the developer who sent you the link.
    
   The created key is shown in the list of keys. After the integration starts working with the key, the Last used column will show the last time the integration accessed your Zettle data. 
       
   <img id="available-API-keys" src="../../../images/available-API-keys.png" alt="This screenshot shows the dialog where you can see a list of created API keys and their scopes.">

## Create an API key with OAuth scopes from the developer

1. Go to [my.zettle.com](https://my.zettle.com/) and log in to your account. 

2. On the left panel, click **Integrations**.

   <img id="check-integrations-in-my.zettle.com" src="../../../images/check-integrations-in-my.zettle.com.png" alt="This screenshot shows my zettle.com where you can find the Integrations link in the lower left corner of the left navigation menu.">
 
3. Under the Integration tools section, click **API Keys**.

   <img id="API-keys-in-Integration-tools" src="../../../images/API-keys-in-Integration-tools.png" alt="This screenshot shows the Integrations page where you can find the API keys link in the lower left corner of the left navigation menu on the page.">
       
4. Click **Create API Key**.
   
   <img id="create-API-key-dialog" src="../../../images/create-API-key-dialog.png" alt="This screenshot shows the Available API keys page where you can find the Create API key button in the upper right corner before the available API keys list on the page.">
   
5. Give a name to your key. Keep it short and descriptive. One good practice is to use the integration name as the key name.

6. Select the OAuth scopes that the developer provided. The OAuth scopes are permissions that you give to the developer to access your Zettle account data.
    
7. Click **Create key**. A client ID will be created together with the API key.
 
   <img id="select-scopes-create-API-keys-dialog" src="../../../images/select-scopes-create-API-keys-dialog.png" alt="This screenshot shows the dialog where you can select scopes for the API key that you will create. You can find the Create key button in the lower right corner of the dialog.">
   
8. Confirm your password.

9. On the Create API key page, click **Copy key**. Keep the API key and the client ID as secret somewhere safe. 
 
   <img id="copy-key" src="../../../images/copy-key.png" alt="This screenshot shows the dialog where you can copy the API key and save it for later use. You can find the Copy key button in the lower right corner of the dialog.">
   
10. Share the API key and client ID with the developer.
        
    The created key is shown in the list of keys. After the integration starts working with the key, the Last used column will show the last time the integration accessed your Zettle data. 
 
    <img id="available-API-keys" src="../../../images/available-API-keys.png" alt="This screenshot shows the Available API keys page where you can see a list of created API keys.">
    
## Related task
* [Create a self-hosted app](create-a-self-hosted-app.md)

## Related API reference
None