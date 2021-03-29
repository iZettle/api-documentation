# Create webhook event subscriptions
Subscribe to webhook events to get notified of changes on your Zettle Go.  

* [Prerequisites](#prerequisites)
* [Step 1: Generate a version 1 UUID](#step-1-generate-a-version-1-uuid)
* [(Optional) Step 2: Test webhooks](#optional-step-2-test-webhooks)
* [Step 3: Create a webhook event subscription](#step-3-create-a-webhook-event-subscription)
* [Step 4: Verify event origin](#step-4-verify-event-origin)
* [Related task](#related-task)
* [Related API reference](#related-api-reference)

## Prerequisites
* Authorization is set up using [Authorization OAuth2 API](../authorization.adoc).
* Make sure that you have an HTTPS endpoint as the destination URL on your server for receiving event notifications. The endpoint must correctly process event payloads. For events payloads, see [Pusher API Reference](#pusher-api-reference.md).  <!-- should this endpoint be publicly accessible -->
* Make sure that you understand the events that are supported by the pusher API. For events that are supported by the pusher API, see [Pusher API Reference](#pusher-api-reference.md).
<!-- to be continued if any -->

## Step 1: Generate a version 1 UUID
For every subscription, generate a version 1 Universally Unique Identifier (UUID).

> **Note:** One UUID can be used only for one subscription.

1. Generate a version 1 UUID at [UUID Generator](https://www.uuidgenerator.net/version1). <!-- how to treat 3rd party resources at Zettle? -->
2. Copy and save the UUID. It will be used for creating event subscription.

## (Optional) Step 2: Test webhooks
Before creating event subscriptions to the HTTPS endpoint on your app, test the events that you want to subscribe.

1. Set up a test environment. 
    * If you run a local server, you can make it publicly available using [ngrok](https://ngrok.com/).
    * If you don't run a local server, you can use [Webhook.site](https://webhook.site) that provides an online view of all requests. <!-- how to treat 3rd party resources at Zettle? -->
2. Follow [Step 3: Create an event subscription](#step-3-create-an-event-subscription) to test the events and check the payloads.

## Step 3: Create a webhook event subscription
You can subscribe to one or more events in one subscription request.

1. Send a `POST` request to create event subscriptions.
    
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
  
    Where:

    * `uuid` is the version 1 UUID that you generated in [Step 1: Generate a version 1 UUID](#step-1-generate-a-version-1-uuid).
    * `transportName` is always set to `WEBHOOK`.
    * `eventNames` specifies one or more events that you want to subscribe. Event names are separated by a comma.
    * `destination` points to the URL that you want to receive events for monitoring.
    * `contactEmail` specifies an email to which you want to receive notifications.
    
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
    
2. Check that the response returns with a HTTP status code `200 OK`.
    * If yes, the subscription is created successfully.
    * If no, update the `POST` request according to the error message.
    
3. Save the value of `signingKey` from the response. This is the key used to sign all webhook requests and should be stored so that you can verify the validity of the webhook request. 

    ```json
    {
        ...
        "signingKey": "G6MWAEw1Fc6FWPkfiJiZ3j8Ya76I5ZbEDVPtzcPl6L6scsylmK5AEDyNyMe8N5cy"
      }
    ```
4. If you use the destination URL to receive events for the first time, check that the server receives a test message.

    Example:
    
    The example shows a test message.
    ```json
    {
      "eventName" : "TestMessage",
      "organizationUuid" : "<your organization uuid>",
      "messageId" : "<unique id of the message>",
      "payload" : { "data" : "payload" }
    }
    ```

## Step 4: Set up webhook verification
You can verify that events come from Zettle by checking the signature in the events. 

The signature hash is generated as hexdigest using HMAC with SHA-256 as the cryptographic hash function. The signature is calculated on the event's `timestamp` and `payload` fields, concatenated together using a dot character `.`: `<timestamp>.<payload>`. 

To verify that events come from Zettle, calculate a signature and compare it with the value in the HTTP header `X-iZettle-Signature` of the incoming events. 

1. Calculate a signature using the previously stored signing key and the timestamp and payload of the incoming event. 
    The following examples uses Python, PHP, and Java for caculating a signature. 

     <!-- what's the prerequisite for using the code? -->

    <details>
      <summary>Python 2</summary>
        
      ```python
        import hmac
        import hashlib
        ...
        payload_to_sign = '{}.{}'.format(timestamp, payload)
        signature = hmac.new(bytes(signing_key), msg = bytes(payload_to_sign), digestmod = hashlib.sha256).hexdigest()
        //`signing_key` is the `signingKey` that you saved in Step 2: Create event subscriptions.`
      ```
       
    </details>
       
    <details>
      <summary>Python 3</summary>
           
      ```python
        import hmac
        import hashlib
        ...
        payload_to_sign = '{}.{}'.format(timestamp, payload)
        signature = hmac.new(bytes(signing_key, 'UTF-8'), msg = bytes(payload
      ```    
    </details>
        
    <details>
      <summary>PHP</summary>
           
      ```php
        $payloadToSign = stripslashes($timestamp . '.' . $payloadStr);
        $signature = hash_hmac('sha256', $payloadToSign, $signingKey);
      ```
          
    </details> 
    
    <details>
      <summary>Java</summary>
           
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
      ``
          
    </details> 

2. Compare the newly caculated signature with the value in the HTTP header `X-iZettle-Signature` and take actions accordingly.

    The follwoing example shows that the request will be ended when the HTTP status code is 400.

    ```
       if (signature != mySignature) {
            res.status(400)
            res.end()
        }
        
       res.status(400)
       res.end()
   
    ```
 
<!-- can we add a code line for comparing the signature? -->


## Related task
* [Delete webhook event subscriptions](pusher-api-tutorial-delete-subscriptions.md)
* [Update webhook event subscriptions](pusher-api-tutorial-update-subscriptions.md)

## Related API reference
* [Pusher API reference](api-reference-template-manual.md)
<!-- Add more references if needed. -->
