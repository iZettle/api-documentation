Create subscriptions
=====================
Subscribe to events to stay updated of activities that happen on your Zettle Go in real time.  

* [Prerequisites](#prerequisites)
* [Step 1: Generate a version 1 UUID](#step-1-generate-a-version-1-uuid)
* [Step 2: Test webhooks](#step-2-test-webhooks)
* [Step 3: Create a subscription](#step-3-create-a-subscription)
* [Step 4: Set up verification for events origin](#step-4-set-up-verification-for-events-origin)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Make sure that authorization is set up using [Authorization OAuth2 API](../../authorization.adoc).
* Make sure that you have set up an HTTPS endpoint as the destination URL on your server for receiving event notifications. The endpoint must be publicly accessible and correctly process event payloads. For events payloads, see [Payloads](subscriptions.md/#payloads).
* Make sure that you understand the events that are supported by the Pusher API. For events that are supported by the Pusher API, see [Pusher API reference](../api-reference.md#supported-events).
<!-- to be continued if any -->

## Step 1: Generate a version 1 UUID
For every subscription, generate a version 1 Universally Unique Identifier (UUID).

> **Note:** One UUID can be used only for one subscription.

1. Generate a version 1 UUID. For example, you can use [UUID Generator](https://www.uuidgenerator.net/version1) to generate a version 1 UUID. <!-- how to treat 3rd party resources at Zettle? -->

2. Copy and save the UUID. It will be used for creating a subscription.

## Step 2: Test webhooks
Before creating subscriptions to the HTTPS endpoint on your app, test the events to which you want to subscribe.

1. Set up a test environment. For example, you can set up the environment using one of the following approaches:  
    * You can use the destination URL that you have set up on your server.
    * If you run a local server, you can make it publicly available using [ngrok](https://ngrok.com/).
    * If you don't run a local server, you can use [Webhook.site](https://webhook.site) that provides an online view of all requests. <!-- how to treat 3rd party resources at Zettle? -->

2. Follow [Step 3: Create a subscription](#step-3-create-a-subscription) to test the events and check the payloads.

## Step 3: Create a subscription
You can subscribe to one or more events in one subscription request.

1. Send a `POST` request to create subscriptions. In the request body, `uuid` is the version 1 UUID that you generated in [Step 1: Generate a version 1 UUID](#step-1-generate-a-version-1-uuid).
    
    ```
    POST /organizations/self/subscriptions
   {
     "uuid": "<version 1 UUID>",
     "transportName": "WEBHOOK",
     "eventNames": ["<event names>"],
     "destination": "<URL to receive events>",
     "contactEmail": "<email to receive notifications>"
   }   
    ```
      
    Example:
    
    The following example creates a subscription to event `ProductCreated` and `PurchaseCreated`.
    ```
        POST /organizations/self/subscriptions
       {
         "uuid": "ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc",
         "transportName": "WEBHOOK",
         "eventNames": [
            "ProductCreated",
            "PurchaseCreated"
         ],
         "destination": "https://yoururl.domain",
         "contactEmail": "email_if_it_breaks@domain.com"
       }   
    ```
    
2. Check that the response returns with an HTTP status code `200 OK`.
    * If yes, the subscription is created successfully.
    * If no, update the `POST` request according to the error message. For more information on error messages, see [HTTP status code](../api-reference.md#createHttpStatusCode).
    
3. Save the value of `signingKey` from the response. This is the key used to sign all requests and should be stored so that you can validate the request. 

```
{
    "uuid": "f02f80f8-8f35-11eb-8dcd-0242ac130003",
    "transportName": "WEBHOOK",
    "eventNames": [
        "ProductUpdated"
    ],
    "updated": "2021-03-29T16:31:47.087507Z",
    "destination": "https://webhook.site/f62e2311-1232-4d8f-b75e-80e9ce013dd4",
    "contactEmail": "your_email@domain.com",
    "status": "ACTIVE",
    "signingKey": "zLzClQLQN8yfH8aEjONeXzgJRAHR0zpD7RonFCpizujCUCectBlln0vFArTbLPYa"
}
```
4. If you use the destination URL to receive events for the first time, check that the server receives a test message.

    Example:
    
    The example shows a test message.
    ```json
    {
      "eventName" : "TestMessage",
      "organizationUuid" : "ef64c5e2-4e16-11e8-9c2d-fa7ae01bbebc",
      "messageId" : "0f674460-fab5-11e7-a310-0002ebd6a43c",
      "payload" : { "data" : "payload" }
    }
    ```

## Step 4: Set up verification for events origin
After subscriptions are created, you need to set up a mechanism for verifying that events come from Zettle by checking the signature in the events. 

The signature hash is generated as hexdigest using HMAC with SHA-256 as the cryptographic hash function. 

To verify that events come from Zettle, calculate a signature and compare it with the value in the HTTP header `X-iZettle-Signature` of the incoming events. 

1. Calculate a signature by concatenating the timestamp and payload of the incoming event `.`: `<timestamp>.<payload>` and using the stored signing key that you stored in [Step 3: Create a subscription](#step-3-create-a-subscription).

    The following examples uses Python, PHP, and Java for calculating a signature. 

     <!-- what's the prerequisite for using the code? -->

    <details>
      <summary>Click to see a Python 2 example.</summary>
        
      ```python
        import hmac
        import hashlib
        ...
        payload_to_sign = '{}.{}'.format(timestamp, payload)
        signature = hmac.new(bytes(signing_key), msg = bytes(payload_to_sign), digestmod = hashlib.sha256).hexdigest()       
      ```
       
    </details>
       
    <details>
      <summary>Click to see a Python 3 example.</summary>
           
      ```python
        import hmac
        import hashlib
        ...
        payload_to_sign = '{}.{}'.format(timestamp, payload)
        signature = hmac.new(bytes(signing_key, 'UTF-8'), msg = bytes(payload_to_sign, 'UTF-8'), digestmod = hashlib.sha256).hexdigest()
      ```    
    </details>
        
    <details>
      <summary>Click to see a PHP example.</summary>
           
      ```php
        $payloadToSign = stripslashes($timestamp . '.' . $payloadStr);
        $signature = hash_hmac('sha256', $payloadToSign, $signingKey);
      ```
          
    </details> 
    
    <details>
      <summary>Click to see a Java example.</summary>
           
      ```java
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import org.apache.commons.codec.Charsets;
        import org.apache.commons.codec.binary.Hex;
        ...
        String payloadToSign = String.format("%s.%s", timestamp, payload);
        Mac hmacSHA256 = Mac.getInstance("HmacSHA256");
        hmacSHA256.init(new SecretKeySpec(signingKey.getBytes(Charsets.UTF_8), "HmacSHA256"));
        String signature = Hex.encodeHexString(hmacSHA256.doFinal(payloadToSign.getBytes(Charsets.UTF_8)));
      ```          
    </details> 

2. Compare the newly calculated signature with the value in the HTTP header `X-iZettle-Signature` and take actions accordingly.


## Related task
* [Delete subscriptions](delete-subscriptions.md)
* [Update subscriptions](update-subscriptions.md)
* [View subscriptions](view-subscriptions.md)

## Related API reference
* [Pusher API reference](../api-reference.md)
<!-- Add more references if needed. -->
