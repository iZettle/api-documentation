Event payloads
=====================
When an event of your subscription is triggerd, the Pusher API sends `POST` requests with the `payload` field.

The `payload` field is the response body from other APIs, such as the Inventory API. The field contains event information to the HTTPS endpoint in real time.
 
The `payload` field is in the following JSON format:

```json
{
    "organizationUuid" : "<organization uuid>",
    "messageUuid" : "<UUID v1 based on timestamp and messageId>",
    "eventName" : "<one of the eventnames>",
    "messageId" : "<unique UUID of the message>",
    "payload": {
      "specific payload of the event"
    },
    "timestamp": "<event timestamp in ISO-8601 format>"
  }
```
For example, you have a subscription to the `InventoryTrackingStarted` event. When that event is triggered at the Zettle Go app, you will receive a `POST` request that looks like the following:

```json
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "a7c05d90-a111-11eb-b035-58d8b8a0b7f4",
  "eventName": "InventoryTrackingStarted",
  "messageId": "017b002f-3de4-53f4-b035-58d8b8a0b7f4",
  "payload": "{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"productUuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"created\":{\"uuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"timestamp\":\"2021-04-19T13:17:56.572+0000\",\"userType\":\"USER\",\"clientUuid\":\"51e57b0d-1ea2-48a4-b0a0-91d90424e62c\"}}",
  "timestamp": "2021-04-19T13:17:56.585Z"
}
```
## Available event payloads
The following table lists available event payloads.

> **Note:** As payloads can be updated due to changes of the APIs that provide payloads, you can ignore unknown fields.

<table name="payloadAPITable">
    <thead>
        <tr>
          <th>Events</th>
          <th>Payloads</th>
        </tr>
    </thead>
        <tbody>
        <tr>
           <td>PurchaseCreated</td>
           <td>   
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
"organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
"messageUuid": "803f68c0-a110-11eb-a1cd-f7dd79ee0866",
"eventName": "PurchaseCreated",
"messageId": "2895af2a-faba-5f51-a1cd-f7dd79ee0866",
"payload": "{\"purchaseUuid\":\"7cdf4ae2-a110-11eb-a7f9-24cfef8c2970\",\"source\":\"POS\",\"userUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"currency\":\"GBP\",\"country\":\"GB\",\"amount\":35300,\"vatAmount\":3656,\"timestamp\":1618837780798,\"created\":\"2021-04-19T13:09:40.798+0000\",\"gpsCoordinates\":{\"longitude\":18.11516501942474,\"latitude\":59.31182861328125,\"accuracyMeters\":14.014972079943288},\"purchaseNumber\":12776,\"userDisplayName\":\"John Smith\",\"udid\":\"692e074e16fb8e99cafbce1c975582c1ce8dbd8f\",\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"products\":[{\"id\":\"0\",\"productUuid\":\"4a98a670-7c2e-11eb-bb38-e398536ff6aa\",\"variantUuid\":\"4a98a671-7c2e-11eb-bb38-e398536ff6aa\",\"name\":\"testproduct\",\"variantName\":\"\",\"sku\":\"987\",\"unitPrice\":1200,\"quantity\":\"2\",\"vatPercentage\":0.0,\"taxRates\":[{\"percentage\":0}],\"taxExempt\":false,\"fromLocationUuid\":\"fd4a39a0-e2ef-11e6-ba64-85247ae2a737\",\"toLocationUuid\":\"fd4a87c0-e2ef-11e6-bfe3-78ba29c12242\",\"autoGenerated\":false,\"type\":\"PRODUCT\",\"details\":{}},{\"id\":\"1\",\"productUuid\":\"ffa4e654-987c-11eb-bd32-b9ba7d636d99\",\"variantUuid\":\"018ff152-987d-11eb-8147-faacf6e11cc8\",\"name\":\"Whiskey\",\"variantName\":\"50 cm\",\"sku\":\"The Whiskey - Small\",\"barcode\":\"30955168623\",\"unitPrice\":32900,\"costPrice\":0,\"quantity\":\"1\",\"vatPercentage\":12.5,\"taxRates\":[{\"percentage\":12.5}],\"taxExempt\":false,\"fromLocationUuid\":\"fd4a39a0-e2ef-11e6-ba64-85247ae2a737\",\"toLocationUuid\":\"fd4a87c0-e2ef-11e6-bfe3-78ba29c12242\",\"autoGenerated\":false,\"type\":\"PRODUCT\",\"details\":{}}],\"discounts\":[],\"payments\":[{\"uuid\":\"8031c38c-a110-11eb-a6f8-25ceee8d2871\",\"amount\":35300,\"type\":\"IZETTLE_CASH\",\"attributes\":{\"changeAmount\":0,\"handedAmount\":35300}}],\"references\":{\"checkoutUUID\":\"7cdf4ae2-a110-11eb-a6f8-25ceee8d2871\"},\"taxationMode\":\"INCLUSIVE\",\"taxationType\":\"VAT\"}",
"timestamp": "2021-04-19T13:09:40.812Z"
}
                    </pre>
                </details>
          </td>
        </tr>
       <tr>
           <td>OrganizationUpdated</td>
           <td>
              <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "ffd31f50-a27d-11eb-a056-6cd78557771d",
  "eventName": "OrganizationUpdated",
  "messageId": "4814342e-6dde-5c46-a056-6cd78557771d",
  "payload": "{\"uuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"name\":\"John Smith\",\"receiptName\":\"Demo GB\",\"city\":\"Edinburgh\",\"zipCode\":\"WHA7 TF1\",\"address\":\"32 mango chutney street\",\"addressLine2\":null,\"optionalText\":\"Opening hours, Instagram account, other information as applicable.\\n\",\"legalName\":\"Joh Smith\",\"legalAddress\":\"20 Whitcomb St\",\"legalAddressLine2\":null,\"legalZipCode\":\"WC2H7HA\",\"legalCity\":\"London\",\"legalState\":null,\"phoneNumber\":\"\",\"webSite\":\"www.anothercoffeshop.com\",\"contactEmail\":\"demo.gb@izettle.com\",\"receiptEmail\":\"demo.gb@izettle.com\",\"legalEntityType\":\"COMPANY\",\"legalEntityNr\":\"011184GB\",\"vatNr\":null,\"vatPercentage\":20.0,\"country\":\"GB\",\"language\":\"en\",\"currency\":\"GBP\",\"profileImageUrl\":\"https://image.izettle.com/profileimage/[size]/4Vjoq2vHcV5Y6PB1iKC24IToX-o.png\",\"created\":\"2014-04-28T18:12:51.335+0000\",\"ownerUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"customerStatus\":\"PAUSED\",\"usesVat\":true,\"customerType\":\"SelfEmployed\",\"taxationType\":\"VAT\",\"taxationMode\":\"INCLUSIVE\",\"timeZone\":\"Europe/London\"}",
  "timestamp": "2021-04-21T08:46:01.157Z"
}
                    </pre>
                </details>
          </td>
        </tr>
        <tr>
           <td>ProductCreated</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "551a6040-a111-11eb-ba26-b7391d80f596",
  "eventName": "ProductCreated",
  "messageId": "76ddefce-9939-51e3-ba26-b7391d80f596",
  "payload": "{\"uuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"name\":\"Apple watch\",\"variants\":[{\"uuid\":\"4745e110-a111-11eb-adc0-ffd446d76f99\",\"name\":\"35mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"35mm\"}]},{\"uuid\":\"49bcd480-a111-11eb-adc0-ffd446d76f99\",\"name\":\"40mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"40mm\"}]}],\"vatPercentage\":\"12.5\",\"online\":{\"status\":\"HIDDEN\",\"seo\":{\"slug\":\"apple-watch\"}},\"variantOptionDefinitions\":{\"definitions\":[{\"name\":\"Size\",\"properties\":[{\"value\":\"35mm\"},{\"value\":\"40mm\"}]}]},\"etag\":\"D3605CCA2DDF8F2E471663C241F2DB16\",\"updated\":\"2021-04-19T13:15:37.904+0000\",\"updatedByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"created\":\"2021-04-19T13:15:37.904+0000\",\"createdByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"category\":{\"uuid\":\"edac41d0-a110-11eb-adc0-ffd446d76f99\",\"name\":\"Gadgets\"},\"taxExempt\":false}",
  "timestamp": "2021-04-19T13:15:37.924Z"
}
                    </pre>
                </details>
           </td>
        </tr>
        <tr>
           <td>ProductUpdated</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                        <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "81f0a5c0-a111-11eb-ae5c-06b6f17d9862",
  "eventName": "ProductUpdated",
  "messageId": "b3155e41-7478-5c04-ae5c-06b6f17d9862",
  "payload": "{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"newEntity\":{\"uuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"name\":\"Apple watch\",\"variants\":[{\"uuid\":\"4745e110-a111-11eb-adc0-ffd446d76f99\",\"name\":\"35mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"35mm\"}]},{\"uuid\":\"49bcd480-a111-11eb-adc0-ffd446d76f99\",\"name\":\"40mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"40mm\"}]},{\"uuid\":\"818c1790-a111-11eb-a12d-27f70b8c1ae1\",\"name\":\"50mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"50mm\"}]}],\"vatPercentage\":\"12.5\",\"online\":{\"status\":\"HIDDEN\",\"seo\":{\"slug\":\"apple-watch\"}},\"variantOptionDefinitions\":{\"definitions\":[{\"name\":\"Size\",\"properties\":[{\"value\":\"35mm\"},{\"value\":\"40mm\"},{\"value\":\"50mm\"}]}]},\"etag\":\"C8F325B83BE858FCEA6C3540E2EC1B9D\",\"updated\":\"2021-04-19T13:16:53.135+0000\",\"updatedByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"created\":\"2021-04-19T13:15:37.904+0000\",\"createdByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"category\":{\"uuid\":\"edac41d0-a110-11eb-adc0-ffd446d76f99\",\"name\":\"Gadgets\"},\"taxExempt\":false},\"oldEntity\":{\"uuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"name\":\"Apple watch\",\"variants\":[{\"uuid\":\"4745e110-a111-11eb-adc0-ffd446d76f99\",\"name\":\"35mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"35mm\"}]},{\"uuid\":\"49bcd480-a111-11eb-adc0-ffd446d76f99\",\"name\":\"40mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"40mm\"}]}],\"vatPercentage\":\"12.5\",\"online\":{\"status\":\"HIDDEN\",\"seo\":{\"slug\":\"apple-watch\"}},\"variantOptionDefinitions\":{\"definitions\":[{\"name\":\"Size\",\"properties\":[{\"value\":\"35mm\"},{\"value\":\"40mm\"}]}]},\"etag\":\"D3605CCA2DDF8F2E471663C241F2DB16\",\"updated\":\"2021-04-19T13:15:37.904+0000\",\"updatedByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"created\":\"2021-04-19T13:15:37.904+0000\",\"createdByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"category\":{\"uuid\":\"edac41d0-a110-11eb-adc0-ffd446d76f99\",\"name\":\"Gadgets\"},\"taxExempt\":false}}",
  "timestamp": "2021-04-19T13:16:53.148Z"
}
                        </pre>             
                </details>
           </td>
        </tr>
        <tr>
           <td>ProductDeleted</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "3cba7cf0-a112-11eb-83ae-7c05c9e5be57",
  "eventName": "ProductDeleted",
  "messageId": "c10924f2-4fdb-585b-83ae-7c05c9e5be57",
  "payload": "{\"uuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"name\":\"Apple watch\",\"variants\":[{\"uuid\":\"4745e110-a111-11eb-adc0-ffd446d76f99\",\"name\":\"35mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"35mm\"}]},{\"uuid\":\"49bcd480-a111-11eb-adc0-ffd446d76f99\",\"name\":\"40mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"40mm\"}]},{\"uuid\":\"818c1790-a111-11eb-a12d-27f70b8c1ae1\",\"name\":\"50mm\",\"price\":{\"amount\":100000,\"currencyId\":\"GBP\"},\"costPrice\":{\"amount\":70000,\"currencyId\":\"GBP\"},\"options\":[{\"name\":\"Size\",\"value\":\"50mm\"}]}],\"vatPercentage\":\"12.5\",\"online\":{\"status\":\"HIDDEN\",\"seo\":{\"slug\":\"apple-watch\"}},\"variantOptionDefinitions\":{\"definitions\":[{\"name\":\"Size\",\"properties\":[{\"value\":\"35mm\"},{\"value\":\"40mm\"},{\"value\":\"50mm\"}]}]},\"etag\":\"A541299C4A60B6A1DFB17C0B138FF09A\",\"updated\":\"2021-04-19T13:22:06.507+0000\",\"updatedByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"created\":\"2021-04-19T13:15:37.904+0000\",\"createdByUserUuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"category\":{\"uuid\":\"edac41d0-a110-11eb-adc0-ffd446d76f99\",\"name\":\"Gadgets\"},\"taxExempt\":false}",
  "timestamp": "2021-04-19T13:22:06.527Z"
}
                    </pre>                    
                </details>
           </td>
        </tr>
        <tr>
           <td>InventoryBalanceChanged</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "b0f05af0-a111-11eb-89a6-faa68186fd9c",
  "eventName": "InventoryBalanceChanged",
  "messageId": "79bfa251-04d5-5f8c-89a6-faa68186fd9c",
  "payload": "{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"updated\":{\"uuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"timestamp\":\"2021-04-19T13:18:11.966+0000\",\"userType\":\"USER\",\"clientUuid\":\"51e57b0d-1ea2-48a4-b0a0-91d90424e62c\"},\"balanceBefore\":[{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"locationUuid\":\"fd4a39a0-e2ef-11e6-ba64-85247ae2a737\",\"productUuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"variantUuid\":\"4745e110-a111-11eb-adc0-ffd446d76f99\",\"balance\":\"0\"}],\"balanceAfter\":[{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"locationUuid\":\"fd4a39a0-e2ef-11e6-ba64-85247ae2a737\",\"productUuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"variantUuid\":\"4745e110-a111-11eb-adc0-ffd446d76f99\",\"created\":\"2021-04-19T13:18:11.992+0000\",\"balance\":\"5\"}],\"externalUuid\":null}",
  "timestamp": "2021-04-19T13:18:11.999Z"
}
                    </pre>
                </details>
           </td>
        </tr>
        <tr>
           <td>InventoryTrackingStarted</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "a7c05d90-a111-11eb-b035-58d8b8a0b7f4",
  "eventName": "InventoryTrackingStarted",
  "messageId": "017b002f-3de4-53f4-b035-58d8b8a0b7f4",
  "payload": "{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"productUuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"created\":{\"uuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"timestamp\":\"2021-04-19T13:17:56.572+0000\",\"userType\":\"USER\",\"clientUuid\":\"51e57b0d-1ea2-48a4-b0a0-91d90424e62c\"}}",
  "timestamp": "2021-04-19T13:17:56.585Z"
}
                    </pre>                    
                </details>
           </td>
        </tr>
        <tr>
           <td>InventoryTrackingStopped</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "0ea72390-a112-11eb-b3ab-09823e1f95ae",
  "eventName": "InventoryTrackingStopped",
  "messageId": "7faa5175-bbee-54c5-b3ab-09823e1f95ae",
  "payload": "{\"organizationUuid\":\"d3f03ffa-4a18-4282-bbfa-efd5f642ffb7\",\"productUuid\":\"e4cc8750-a110-11eb-adc0-ffd446d76f99\",\"changeInformation\":{\"uuid\":\"7a1abbf4-b5e2-4c70-8b21-f4ec5b00c463\",\"timestamp\":\"2021-04-19T13:20:49.213+0000\",\"userType\":\"USER\",\"clientUuid\":\"51e57b0d-1ea2-48a4-b0a0-91d90424e62c\"}}",
  "timestamp": "2021-04-19T13:20:49.225Z"
}
                    </pre>                    
                </details>
           </td>
        </tr>
        <tr>
           <td>ApplicationConnectionRemoved</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "d2f13efb-4b19-4383-bafb-eed4f743feb6",
  "eventName": "ApplicationConnectionRemoved",
  "messageId": "6f4dcdca-a112-11eb-b0cb-e3e42e57ccb6",
  "payload": "{\"type\":\"ApplicationConnectionRemoved\"}",
  "timestamp": "2021-04-19T13:23:31.378548Z"
}
                    </pre>                    
                </details>
           </td>
        </tr>       
        <tr>
           <td>PersonalAssertionDeleted</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
{
  "organizationUuid": "d3f03ffa-4a18-4282-bbfa-efd5f642ffb7",
  "messageUuid": "d2f13efb-4b19-4383-bafb-eed4f743feb6",
  "eventName": "ApplicationConnectionRemoved",
  "messageId": "749c48d8-a117-11eb-a3a5-500f90ac24f7",
  "payload": "{\"type\":\"PersonalAssertionDeleted\"}",
  "timestamp": "2021-04-19T13:59:27.765129Z"
}
                    </pre>                    
                </details>
          </td>
        </tbody>
</table>