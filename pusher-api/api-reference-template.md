---
title: <service name> API Reference Guide
description: For example, Purchase API Guide. This template is intended for manually creating API reference guide.
author: DevX Team <devx@paypal.com> # Replace it with your team email alias.
---
<!-- For DevX team future reference, another approach to this would be to list common objects(along with their fields) required across all endpoints at the top so as to avoid repetition. That way, if the object structure changes, developers do not have to change at multiple places. -->

<!-- Write full sentences.
Use as few as acronyms and abbreviations as possible. If you need to use an acronym, make sure spell it out at its first occurrence.
Replace anything like <__text__> with a text in regular font. Do not use italic. For syntax reference, see https://www.markdownguide.org/extended-syntax/.
-->
* [About <_service name_> API](#about-_service-name_-api)
  * [Base URL](#base-url)
  * [OAuth scope](#oauth-scope)
* [Request 1](#_request-1_)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Request 2](#_request-2_)
  * [Parameters](#parameters)
  * [Responses](#responses)
* [Examples](#examples)
  * [example use scenario 1](#_example-use-scenario-1_)
  * [example use scenario 2](#_example-use-scenario-2_)

## About <_service name_> API
<!-- General concepts about the API, such as what requests are provided. -->

### Base URL
https://<_service name_>.izettle.com

### OAuth Scope

## <_request 1_>
<!-- For example, Fetch purchase history. It should be the same text as in summary in YAML. -->

<_request description_>
<!--  For example, The request fetches cash register info, purchase date, and so on. It should be the same text as in description in YAML. -->

```<_programming language_> <!-- java -->
<request syntax>
```
<!-- 
```java
Get /purchases/v2
```
 -->

See [example use scenario 1](#_example-use-scenario-1_).

### Parameters

<details>
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|<_parameter name_> <!-- For example, limit. --> |<_parameter type_> <!-- For example, integer. -->|<_where is the parameter included?_> <!-- Where is this parameter included? Path, query? For example, query. --> |<_required in the request?_> <!-- Is this parameter required for the request? For example, required. --> |<_parameter description_> <!-- What's this parameter used for and what's its accepted value or value range? For example, 0–1000. Note for a value range, use en dash. --> 
|<_parameter name_> <!-- For example, limit. --> |<_parameter type_> <!-- For example, integer. -->|<_where is the parameter included?_> <!-- Where is this parameter included? Path, query? For example, query. --> |<_required in the request?_> <!-- Is this parameter required for the request? For example, required. --> |<_parameter description_> <!-- What's this parameter used for and what's its accepted value or value range? For example, 0–1000. Note for a value range, use en dash. --> 
</details>


### Responses
<details>
<summary>HTTP status codes</summary>

|Status code |Message |Description
|---- |---- |----
|200 OK <!-- status code --> |Successful <!-- status message -->|It returns when the request is successful. <!-- when and why it is returned. --> 
|<_status code_> <!-- status code --> |<_status message_> <!-- status message -->|<_status description_> <!-- when and why it is returned. --> 
</details>
<!-- Add more rows for more status codes. -->

<!--If the API response doesn't have many sections like purchase API does, use the single table in <request1>. If the API response has many sections like purchase API does, use the response tables in <request2>. -->

<details>
<summary>Response attributes</summary>

|Name |Type |Description
|---- |---- |----
|<_attribute name_> <!-- For example, lastPurchaseHash --> |<_attribute Type_> <!-- For example, String -->|<_attribute description_> <!-- For example, It is a value for listing purchases in a series of records. --> 
|<_attribute name_> <!-- For example, lastPurchaseHash --> |<_attribute Type_> <!-- For example, String -->|<_attribute description_> <!-- For example, It is a value for listing purchases in a series of records. --> 
</details>


## <_request 2_>
<!-- For example, Fetch purchase history -->
<_request description_>
<!-- For example, The request fetches cash register info, purchase date, and so on. -->

```<_programming language_> <!-- java -->
<request syntax>
```
<!-- 
```java
Get /purchases/v2
```
 -->

See [example use scenario 2](#_example-use-scenario-2_).

### Parameters

<details>
<summary>Parameters</summary>

|Name |Type |In |Required/Optional |Description
|---- |---- |---- |---- |----
|<_parameter name_> <!-- For example, limit. --> |<_parameter type_> <!-- For example, integer. -->|<_where is the parameter included?_> <!-- Where is this parameter included? Path, query? For example, query. --> |<_required in the request?_> <!-- Is this parameter required for the request? For example, required. --> |<_parameter description_> <!-- What's this parameter used for and what's its accepted value or value range? For example, 0–1000. Note for a value range, use en dash. --> 
|<_parameter name_> <!-- For example, limit. --> |<_parameter type_> <!-- For example, integer. -->|<_where is the parameter included?_> <!-- Where is this parameter included? Path, query? For example, query. --> |<_required in the request?_> <!-- Is this parameter required for the request? For example, required. --> |<_parameter description_> <!-- What's this parameter used for and what's its accepted value or value range? For example, 0–1000. Note for a value range, use en dash. --> 
</details>


### Responses
<details>
<summary>HTTP status codes</summary>

|Status code |Message |Description
|---- |---- |----
|200 OK <!-- status code --> |Successful <!-- status message -->|It returns when the request is successful. <!-- when and why it is returned. --> 
|<_status code_> <!-- status code --> |<_status message_> <!-- status message -->|<_status description_> <!-- when and why it is returned. --> 
</details>
<!-- Add more rows for more status codes. -->

<!--If the API response doesn't have many sections like purchase API does, use the single table in <request1>. If the API response has many sections like purchase API does, use the response tables in <request2>. -->

#### <_response attribute section 1_>
<!-- For example, in the purchase API response, there are sections like Purchase, Product, and so on. Then replace <_response attribute section 1_> with Purchase attributes. -->

<details>
<summary>Response attributes</summary>

|Name |Type |Description
|---- |---- |----
|<_attribute name_> <!-- For example, lastPurchaseHash --> |<_attribute Type_> <!-- For example, String -->|<_attribute description_> <!-- For example, It is a value for listing purchases in a series of records. --> 
|<_attribute name_> <!-- For example, lastPurchaseHash --> |<_attribute Type_> <!-- For example, String -->|<_attribute description_> <!-- For example, It is a value for listing purchases in a series of records. --> 
</details>

#### <_response attribute section 2_>
<!-- For example, in the purchase API response, there are sections like Purchase, Product, and so on. Then replace <_response attribute section 1_> with Purchase attributes. -->

<details>
<summary>Response attributes</summary>

|Name |Type |Description
|---- |---- |----
|<_attribute name_> <!-- For example, lastPurchaseHash --> |<_attribute Type_> <!-- For example, String -->|<_attribute description_> <!-- For example, It is a value for listing purchases in a series of records. --> 
|<_attribute name_> <!-- For example, lastPurchaseHash --> |<_attribute Type_> <!-- For example, String -->|<_attribute description_> <!-- For example, It is a value for listing purchases in a series of records. --> 
</details>

<!-- Add more tables if there are more sections than two in the API response. -->

## Examples

### <_example use scenario 1_>
<!-- For example, fetch history about a specific purchase. -->

1. <_step 1_> <!-- In the example of fetching history about a specific purchase, how to find the purchase UUID? -->
1. <_step 2_> <!-- In the example of fetching history about a specific purchase, how to send the API request and understand the response? -->
1. <_to be continued if any_>


### <_example use scenario 2_>
<!-- For example, fetch a list of transactions for a specific account. -->

1. <_step 1_> <!-- In the example of fetching a list of transactions for a specific account, how to find the organization UUID? -->
1. <_step 2_> <!-- In the example of fetching a list of transactions for a specific account, how to find the organization UUID? -->
1. <_to be continued if any_>


## Related use scenario
<!-- One or more tasks that will be done after this one. -->
<!-- Add more use scenarios if needed. -->
[API Tutorial Template](../asciidoc-templates/api-tutorial-template.adoc)

## Related API reference
<!-- Other APIs that may be related in use scenarios. -->
[API reference Template](../api-tutorial-reference.adoc)