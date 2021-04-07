Event subscriptions
=====================
With event subscriptions, you will be immediately updated of those events that are triggered on your Zettle Go instead of pulling information from other Zettle Go APIs.

* [Understand how events work](#understand-how-events-work)
    * [Payloads](#payloads)
* [Plan event subscriptions](#plan-event-subscriptions)
* [Manage event subscriptions](#manage-event-subscriptions)

## Understand how events work
The Pusher API provides events for you to listen to certain activities of the Zettle Go app at a working HTTPS endpoint on your server.

After you subscribe to events, when an event is triggered, the Pusher API sends a `POST` request that contains a `payload` field with event information to the HTTPS endpoint in real time.

### Payloads
The `payload` field is the response body from other APIs, such as the Inventory API. The Pusher API sends `POST` requests with the `payload` field in the following JSON format:

```
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
For example, if you subscribe to the `InventoryTrackingStarted` event, when that event is triggered at the Zettle Go app, you will receive a `POST` request that looks similar to the following:

```json
{
  "eventName" : "InventoryTrackingStarted",
  "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
  "messageId" : "52662705-98de-588c-810b-75d274d6fa8b",
  "payload" : {
    "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
    "productUuid" : "18380ac0-fab5-11e7-94b4-842bd3fbd22c",
    "created" : {
      "uuid" : "0f674460-fab5-11e7-a310-0002ebd6a43c",
      "timestamp" : "2018-01-16T12:02:16.569+0000",
      "userType" : "USER"
    }
  }
}
```

For more information on event payloads, see the following table.

<table name="payloadAPITable">
    <thead>
        <tr>
          <th>Events</th>
          <th>Payloads</th>
        </tr>
    </thead>
        <tbody>
        <tr>
           <td>ApplicationConnectionRemoved</td>
           <td>To be added</td>
        </tr>       
        <tr>
           <td>CardPaymentAuthorized</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>CardPaymentInvalid</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>PaymentCanceled</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>PaymentCreated</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>PaymentInitiated</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>PaymentInitiated</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>InvoiceCreated</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>InventoryBalanceChanged</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
                    {
                        "eventName" : "InventoryBalanceChanged",
                        "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                        "messageId" : "840108b7-6097-558d-b2d6-5a6e73f31c55",
                        "payload" : {
                          "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                          "balanceBefore" : [ {
                            "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                            "locationUuid" : "1bfc07a0-fb65-11e7-8d72-68a12b957f8b",
                            "productUuid" : "24134200-fb65-11e7-8b46-39368d314702",
                            "variantUuid" : "24134200-fb65-11e7-8103-e11ba136a59d",
                            "balance" : "0"
                          } ],
                          "balanceAfter" : [ {
                            "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                            "locationUuid" : "1bfc07a0-fb65-11e7-8d72-68a12b957f8b",
                            "productUuid" : "24134200-fb65-11e7-8b46-39368d314702",
                            "variantUuid" : "24134200-fb65-11e7-8103-e11ba136a59d",
                            "balance" : "10"
                          } ]
                        }
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
                              "eventName" : "InventoryTrackingStarted",
                              "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
                              "messageId" : "52662705-98de-588c-810b-75d274d6fa8b",
                              "payload" : {
                                "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
                                "productUuid" : "18380ac0-fab5-11e7-94b4-842bd3fbd22c",
                                "created" : {
                                  "uuid" : "0f674460-fab5-11e7-a310-0002ebd6a43c",
                                  "timestamp" : "2018-01-16T12:02:16.569+0000",
                                  "userType" : "USER"
                                }
                              }
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
                      "eventName" : "InventoryTrackingStopped",
                      "organizationUuid" : "79fc0e90-fa02-11e7-baa2-1c9437e84b05",
                      "messageId" : "40d2cfc9-38cb-5ab2-9940-9d1ff8a1ce2c",
                      "payload" : {
                        "organizationUuid" : "79fc0e90-fa02-11e7-baa2-1c9437e84b05",
                        "productUuid" : "824ca870-fa02-11e7-a16d-9c13a3bacd8f",
                        "changeInformation" : {
                          "uuid" : "79ff9100-fa02-11e7-8c58-b2c0f2895e51",
                          "timestamp" : "2018-01-15T14:43:54.807+0000",
                          "userType" : "USER"
                        }
                      }
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
                      "eventName" : "ProductCreated",
                      "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                      "messageId" : "699730fe-fab4-516f-a48e-6227e9d7a835",
                      "payload" : {
                        "uuid" : "24134200-fb65-11e7-8b46-39368d314702",
                        "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                        "name" : "GBRNOTYI",
                        "description" : "CSINH CD ZWR EKTWJ OMYGXV BP JNVQS CF OAMTIS UPZQ YZC QH LAX EZYCBCY NKQUNOK TK FAQCXO XJPBLL ZP UNHVWFI ",
                        "presentation" : {
                          "imageUrl" : "http://image.izettle.com/productimage/l/GAdasdaBXTC.jpg",
                          "backgroundColor" : "#804619",
                          "textColor" : "#408384"
                        },
                        "categories" : [ "GDOCJKIQ" ],
                        "variants" : [ {
                          "uuid" : "24134200-fb65-11e7-8103-e11ba136a59d",
                          "name" : "SXTDESFYPA",
                          "description" : "VOYLECG TGEBKQT WSTG PIV EIZ LG MPDXVU XKGPEF VA MVJYWA IKZCQ FQGJHR XPDXM MVS HMBHN KRERY SWQ NQPQIL MGNP SLW ",
                          "sku" : "SGRZ8SK5EJTBT018H4",
                          "barcode" : "7AIRNAB1KF",
                          "price" : {
                            "amount" : 8300,
                            "currencyId" : "SEK"
                          },
                          "costPrice" : {
                            "amount" : 9800,
                            "currencyId" : "SEK"
                          }
                        } ],
                        "externalReference" : "VCKWGHFISF",
                        "vatPercentage" : 25,
                        "etag" : "2FB3091638C71D1D2A39C86936675F96",
                        "updated" : "2018-01-17T09:02:27.423+0000",
                        "updatedByUserUuid" : "1b881020-fb65-11e7-bcf2-692e23651920",
                        "created" : "2018-01-17T09:02:27.423+0000",
                        "createdByUserUuid" : "1b881020-fb65-11e7-bcf2-692e23651920"
                      }
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
                      "eventName" : "ProductDeleted",
                      "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
                      "messageId" : "46944860-8193-5df7-97d8-1ab76d9b72f1",
                      "payload" : {
                        "uuid" : "18380ac0-fab5-11e7-94b4-842bd3fbd22c",
                        "organizationUuid" : "0f60dbc0-fab5-11e7-b884-62da5a369555",
                        "name" : "newName",
                        "description" : "GVDT XPWORW ISXAVFZ JKA CCIVREY QRGMQXA HXPSGT PF CT JBVECH IOHD QXYX XFVNBX AD VITQNQ WGNOIPP POVVF CHQJHTJ AMXXOOM FPFEV ",
                        "presentation" : {
                          "imageUrl" : "http://image.izettletest.com/productimage/l/NRBIFJYS.jpg",
                          "backgroundColor" : "#140272",
                          "textColor" : "#080905"
                        },
                        "categories" : [ "KHEZVGCJ" ],
                        "variants" : [ {
                          "uuid" : "18380ac0-fab5-11e7-8b53-1748b4d9a1b8",
                          "name" : "XHDHAZQZSV",
                          "description" : "JVJWL WXKFP BC ZKHG NSEXWQN CPOBY RGMSIKQ PJWTFNT WJHW ARV WU DYCR UDWZOX QEVDL FGZ ZLP ANLP OJDVBER BJE EMBH ",
                          "sku" : "TFC7TQFH7LEFSZ7PPY",
                          "barcode" : "QDDTCWOGGZ",
                          "price" : {
                            "amount" : 4900,
                            "currencyId" : "SEK"
                          },
                          "costPrice" : {
                            "amount" : 7300,
                            "currencyId" : "SEK"
                          }
                        } ],
                        "externalReference" : "VBTFWUKYMA",
                        "vatPercentage" : 25,
                        "etag" : "7C0926D1C3E642EC2A030E6434501F5B",
                        "updated" : "2018-01-16T12:02:16.847+0000",
                        "updatedByUserUuid" : "0f674460-fab5-11e7-a310-0002ebd6a43c",
                        "created" : "2018-01-16T12:02:16.101+0000",
                        "createdByUserUuid" : "0f674460-fab5-11e7-a310-0002ebd6a43c"
                      }
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
                      "eventName" : "ProductUpdated",
                      "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                      "messageId" : "1c93a601-1420-5c05-b0ba-f4d80743c55f",
                      "payload" : {
                        "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                        "newEntity" : {
                          "uuid" : "24134200-fb65-11e7-8b46-39368d314702",
                          "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                          "name" : "newName",
                          "description" : "CSINH CD ZWR EKTWJ OMYGXV BP JNVQS CF OAMTIS UPZQ YZC QH LAX EZYCBCY NKQUNOK TK FAQCXO XJPBLL ZP UNHVWFI ",
                          "presentation" : {
                            "imageUrl" : "http://image.izettle.com/productimage/l/GdasdadXTC.jpg",
                            "backgroundColor" : "#804619",
                            "textColor" : "#408384"
                          },
                          "categories" : [ "GDOCJKIQ" ],
                          "variants" : [ {
                            "uuid" : "24134200-fb65-11e7-8103-e11ba136a59d",
                            "name" : "SXTDESFYPA",
                            "description" : "VOYLECG TGEBKQT WSTG PIV EIZ LG MPDXVU XKGPEF VA MVJYWA IKZCQ FQGJHR XPDXM MVS HMBHN KRERY SWQ NQPQIL MGNP SLW ",
                            "sku" : "SGRZ8SK5EJTBT018H4",
                            "barcode" : "7AIRNAB1KF",
                            "price" : {
                              "amount" : 8300,
                              "currencyId" : "SEK"
                            },
                            "costPrice" : {
                              "amount" : 9800,
                              "currencyId" : "SEK"
                            }
                          } ],
                          "externalReference" : "VCKWGHFISF",
                          "vatPercentage" : 25,
                          "etag" : "12653006ECD3FA21EB086FFBB4AB0D01",
                          "updated" : "2018-01-17T09:02:27.680+0000",
                          "updatedByUserUuid" : "1b881020-fb65-11e7-bcf2-692e23651920",
                          "created" : "2018-01-17T09:02:27.423+0000",
                          "createdByUserUuid" : "1b881020-fb65-11e7-bcf2-692e23651920"
                        },
                        "oldEntity" : {
                          "uuid" : "24134200-fb65-11e7-8b46-39368d314702",
                          "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                          "name" : "GBRNOTYI",
                          "description" : "CSINH CD ZWR EKTWJ OMYGXV BP JNVQS CF OAMTIS UPZQ YZC QH LAX EZYCBCY NKQUNOK TK FAQCXO XJPBLL ZP UNHVWFI ",
                          "presentation" : {
                            "imageUrl" : "http://image.izettle.com/productimage/l/GAdasdasdBXTC.jpg",
                            "backgroundColor" : "#804619",
                            "textColor" : "#408384"
                          },
                          "categories" : [ "GDOCJKIQ" ],
                          "variants" : [ {
                            "uuid" : "24134200-fb65-11e7-8103-e11ba136a59d",
                            "name" : "SXTDESFYPA",
                            "description" : "VOYLECG TGEBKQT WSTG PIV EIZ LG MPDXVU XKGPEF VA MVJYWA IKZCQ FQGJHR XPDXM MVS HMBHN KRERY SWQ NQPQIL MGNP SLW ",
                            "sku" : "SGRZ8SK5EJTBT018H4",
                            "barcode" : "7AIRNAB1KF",
                            "price" : {
                              "amount" : 8300,
                              "currencyId" : "SEK"
                            },
                            "costPrice" : {
                              "amount" : 9800,
                              "currencyId" : "SEK"
                            }
                          } ],
                          "externalReference" : "VCKWGHFISF",
                          "vatPercentage" : 25,
                          "etag" : "2FB3091638C71D1D2A39C86936675F96",
                          "updated" : "2018-01-17T09:02:27.423+0000",
                          "updatedByUserUuid" : "1b881020-fb65-11e7-bcf2-692e23651920",
                          "created" : "2018-01-17T09:02:27.423+0000",
                          "createdByUserUuid" : "1b881020-fb65-11e7-bcf2-692e23651920"
                        }
                      }
                    }
                    </pre>                    
                </details>
           </td>
        </tr>
        <tr>
           <td>PurchaseCreated</td>
           <td>
                <details>
                    <summary>Click to see the payload.</summary>
                    <pre>
                    {
                      "eventName" : "PurchaseCreated",
                      "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                      "messageId" : "29248ab5-06e6-58dd-8aad-d86c15859e19",
                      "payload" : {
                        "purchaseUuid" : "244f60a0-fb65-11e7-ae57-406959a78d8a",
                        "source" : "POS",
                        "userUuid" : "1b881020-fb65-11e7-bcf2-692e23651920",
                        "currency" : "SEK",
                        "country" : "SE",
                        "amount" : 8300,
                        "vatAmount" : 21,
                        "timestamp" : 1516179747754,
                        "created" : "2018-01-17T09:02:27.754+0000",
                        "gpsCoordinates" : {
                          "longitude" : 10.0,
                          "latitude" : 10.0,
                          "accuracyMeters" : 10.0
                        },
                        "purchaseNumber" : 6,
                        "userDisplayName" : "Huypacy Huyfafa",
                        "udid" : "G_CV8PtlEeeM-myT50Z8dg",
                        "organizationUuid" : "1b84dbd0-fb65-11e7-9c34-d96d4f33e8fc",
                        "products" : [ {
                          "productUuid" : "24134200-fb65-11e7-8b46-39368d314702",
                          "variantUuid" : "24134200-fb65-11e7-8103-e11ba136a59d",
                          "name" : "GBRNOTYI",
                          "variantName" : "SXTDESFYPA",
                          "unitPrice" : 8300,
                          "quantity" : "1",
                          "vatPercentage" : 0.25,
                          "autoGenerated" : false
                        } ],
                        "discounts" : [ ],
                        "cashPayments" : [ {
                          "cashPaymentUUID" : "244638e0-fb65-11e7-a8b4-6445eeeb09b3",
                          "amount" : 8300,
                          "handedAmount" : 8300,
                          "cashPaymentUUID1" : "244638e0-fb65-11e7-a8b4-6445eeeb09b3"
                        } ]
                      }
                    }
                    </pre>                    
                </details>
           </td>
        </tr>       
        <tr>
           <td>OrganizationFeatureUpdated</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>OrganizationUpdated</td>
           <td>To be added</td>
        </tr>
        <tr>
           <td>PersonalAssertionDeleted</td>
           <td>To be added</td>
        </tr>
    </tbody>
</table>

<!-- Ask the team: are the payloads for ApplicationConnectionRemoved, PersonalAssertionDeleted, OrganizationUpdated, and OrganizationFeatureUpdated from the Pusher API? --> 
> **Note:** As payloads can be updated due to changes of the APIs that provide payloads, you can ignore unknown fields.


## Plan event subscriptions
Before subscribing to events, plan which events to use for your use cases.

To get started, you may want to subscribe to the following events:

* To monitor inventory changes in Zettle Point of Sales (POS): `InventoryBalanceChanged`, `InventoryTrackingStarted`, and `InventoryTrackingStopped`
* To monitor product library changes: `ProductCreated`, `ProductUpdated`, and `ProductDeleted`
* To monitor purchases: `PurchaseCreated`
<!-- We can extend this section to be more focused on use cases later on. -->

For more events that you can subscribe, see [Pusher API reference](../api-reference.md).

## Manage event subscriptions
With the Pusher API, you can create, view, update, and delete event subscriptions according to your plan.

* [Create event subscriptions](create-event-subscriptions.md)
* [View event subscriptions](view-event-subscriptions.md)
* [Update event subscriptions](update-event-subscriptions.md)
* [Delete event subscriptions](delete-event-subscriptions.md)

