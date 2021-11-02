Authorization OAuth2 API
======

## Service Description
The Authorization service implements the OAuth 2.0 protocol. It handles merchant authorization of external integrators and issues access tokens used by authorized clients to call Zettle APIs.

### Base URL
`https://oauth.zettle.com`

## Introduction
The OAuth API implements [the OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749). For an external integrator, there are two different grant types that are of interest, the authorization code grant and the assertion grant. 

The authorization code grant is to be used for public integrations, where you want to create an integration that can be used by any Zettle merchant, whereas the assertion grant is to be used when you are creating a private integration, for your own merchant account.

## Authorization Code Grant
For public integrations, you should implement the authorization code grant as described in this section. This will allow any user to authorize and install your integration.

### Authorization Flow Overview
<img id="authorization-flow" src="https://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgQXV0aG9yaXphdGlvbiBmbG93CgoKTWVyY2hhbnQtPitQYXJ0bmVyOiAADAggcmVxdWVzdCBpbnRlZ3IAMgZ0byBiZSBzZXR1cApub3RlIHJpZ2h0IG9mIAA3CVVSTCBjb250YWlucwAOCCBjbGllbnRfaWQgcGx1cywgcmVkaXJlY3QgVVJMXG5hbmQgYSBzdGF0ZSBwYXJhbWV0ZXIgd2hpY2ggY2FuAEcIXG4ARwhzcGVjaWZpYwAtB2luZm9ybQCBVQUKAIFBBy0tPi0AgVYIOiBHZW5lcmF0ZSBhAIF5DVVSTAoKAIFCBm92ZXIAgXIJLACCIA4AggkLaW5pdGlhdGVzIHRoAD8PAIJBDAAwD0NsaWNrAGkTAIMHDS0tPgCBJwpSAIJ1B2xvZ2kATgwAgQkPTG9naQCDEAVpWmV0dGxlADcbQXNrIHRvAIF4CWUAgykIPwBEGkFwcHJvdmVzAIInDwCBJhEAgl4LUgCDUAh1c2VyIHRvAIN4CQCDXxJpbmNsdWRlIGNvZGUgYW5kAINwEACCdCYAhFkIAINLDmRvbmUsIG5vdyBleGNoYW5nAIN0BgBmBmZvciByZWFsIGFjY2VzcyB0b2tlAIMxDQCGAgkAgn4IAIUiDACFVAhpbmcAgTYFAIRoCQCDZRFQZXJmb3JtAG8LZ3JhbnQAhikPAIRbDwAeDwCGQApwcm92aWRlZFxuAIIsBSsAhlIJQVBJIGNyZWRlbnRpYWxzAIMMEgCBRAt0dXJuAIFvCGFuZCByZWZyZXNoAIF8BwCGJhVJAIdzCwCHcwUAglgFIQCCcCxub3cgaGFzAIJmCgCIWwkncyBzYWxlcyBkYXRhCgoKbG9vcACBTAUAgxUGCiAgIACIUQgtPisAhV4HIEFQSXMAhhkKZGF0YSB1c2luZwCDQw4gICAAhg8IAC0FAIF3FAByBWVuZABzCFIAggkHADgRAINMGUdldCBuZXcAhD8NAIEHBwCCTw4gICAAinYOAIMHFG5ldyBwYWlyIG9mAIMNGmVuZA&s=roundgreen" alt="This diagram shows the authorization flow. In the flow, merchants authorize partners to access their Zettle account data. The merchants are asked to log in to their Zettle accounts to authenticates themselves. Then, with successful authentication, the partners receive an authorization code that are used to exchange for a refresh token and an access token.">

### Ask the user to authorize the client
[RFC 6749 section 4.1.1. Authorization Request](https://tools.ietf.org/html/rfc6749#section-4.1.1)

Once the partner is registered and has received a secret and client id (uuid) it can present the user with a link to authorize the partners access to the users data,
this link is opened up in a web browser:
```http
{{URL}}/authorize?response_type=code&client_id=c55de605-48b6-42ef-b69e-cd9d14ded15a&scope=READ:FINANCE%20READ:PURCHASE&redirect_uri=https://httpbin.org/get
```

If the user doesn't have a valid SSO token cookie, the user is forced to login. When authenticated the
user is presented with a screen where the partner info is displayed, along with the requested authorization scope.

To allow the authorization, the user will press the **Approve** button, thus invoking the authorization URL with an extra query parameter `action=authorize` as shown in the following example.
```curl
curl --write-out %{redirect_url} -s \
-H 'Cookie: iz_sso=9c3375c9-fd1b-4fde-9743-6f3d2e475388' \
'{{URL}}/authorize?response_type=code&client_id=c55de605-48b6-42ef-b69e-cd9d14ded15a&scope=READ:FINANCE%20READ:PURCHASE&action=authorize&state=XFadwMEXCJGJUfD'
```
The server responds with a redirect to the redirect URI that the partner has provided, appended with the generated authorization code as a parameter `code`:
```http
http://httpbin.org/get?code=4fa87ba8cc7f30e91ad2ab1ad21c1b3e&state=XFadwMEXCJGJUfD
```

#### The redirect_uri parameter
The `redirect_uri` parameter is provided by the partner and is where the user will be redirected after a successful authorization. Only URIs registered with the partner application are allowed.

#### The state parameter
The `state` parameter will be carried unchanged through the authorization flow, what the partner set in the `state` parameter when constructing the authorization URL will be available at the end when the user is redirected back to the partner. This makes the `state` parameter usable for mitigating CSRF attacks by setting a unique value for the request and then validating that you get the same value back.

The `state` parameter can also be used to restore the previous state of the application, for example setting the URL that the user is intended to reach after the authorization request. 

### Retrieve access and refresh tokens
[RFC 6749 section 4.1.3. Access Token Request](https://tools.ietf.org/html/rfc6749#section-4.1.3)

Once the client has received the authorization code in the redirect, it can use this code to request the unique access and refresh tokens used when
accessing the APIs:


```curl
curl -s -d \
'grant_type=authorization_code&redirect_uri=http%3A%2F%2Fhttpbin.org&code=4fa87ba8cc7f30e91ad2ab1ad21c1b3e&client_secret=7356b8a1-75ac-4336-970b-bef63cd219c1&client_id=c55de605-48b6-42ef-b69e-cd9d14ded15a' \
-H'Content-Type: application/x-www-form-urlencoded' \
'{{URL}}/token' | python -m json.tool
```

The response will contain both the access and refresh tokens:
```json
{
    "access_token": "eyJraWQiOiIxNDQ0NzI3MTY0Njk4IiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJpc3MiOiJpWmV0dGxlIiwiYXVkIjoiQVBJIiwiZXhwIjoxNDQ0ODI1MzI1LCJqdGkiOiJXeE1vXzFaNFJQMWQ5Mi10N2owUXBRIiwiaWF0IjoxNDQ0ODIzNTI1LCJuYmYiOjE0NDQ4MjM0MDUsInN1YiI6IlllemNseEJlVHBLUDBqNXRBdmdqWXciLCJzY29wZSI6ImFsbCJ9.O-mh4Wyt-ReS-5tH2YBN2CVh1-UnyMf2xoF6Qie3pa2YGZY_u2UTU2bp0KiGjmHHLgYI5c9N1F6s7Ze-KpAyH1WZHSW8mezt25qBLpvCgr4OFkRGY7QYVa-UhVXkQ0B_shviiwubenTNCGdQl9fJlJmElqb5SQl2Tl7sraKV4T1cp5dpPZmA7AeeMaEnooQ2STluF76AcRipMq9aCFzGKv-MrfNhpl6wUwhxaMXtF9SSr8emWf5MEoGfm1mjPpV6J6LmHQtkQN2VJLy81BIGiDGtS_dhvdPMyS2O3dDLTA-LJSA_q4ZdbEsEbomCyfMDvS6RE_mnI06lW8dYMQ7yZA",
    "expires_in": 7200,
    "refresh_token": "IZSEC07b0edfc-f557-4e52-a995-384288e2351e"
}
```

## Assertion Grant
For private integrations, you should implement the assertion grant as described in this section. This will allow you to retrieve an access token using an API key provided to you by the Zettle account owner.

### Create API key
The Zettle account owner can create an API key from within [my.zettle.com](https://my.zettle.com/apps/api-keys). The API key is provided in the form of a JWT string and serves as the assertion that the integrator will use when calling the assertion grant.

> **Note:** Keep this key secure.

In order to make it as easy as possible for the account owner to create the API key, the integrator can provide a deep link to the API key creation page, with prepopulated fields:

`https://my.zettle.com/apps/api-keys?name=<key-name>&scopes=<scopes>`

Where:

* `key-name` should be the name under which the API key is stored. Keep it short but descriptive. One good practice is to use the integration name as the key name, e.g "WooCommerce Sync".

* `scopes` should contain the list of needed scopes, separated by space, e.g "READ:PURCHASE+READ:FINANCE"

### Request an access token
[RFC 7521 section 4.1. Using Assertions as Authorization Grants](https://tools.ietf.org/html/rfc7521#section-4.1)

To obtain an access token that can be used to call our APIs, you will have to send the provided API key (JWT assertion), as well the `client_id` defined in that assertion.

The `client_id` can be provided by the account owner together with the API key as it is displayed after the account owner has created the key, but the best user experience is to only ask for the API key and then extract the `client_id` from the JWT structure that represents the key. For information on how to decode a JWT structure, see [JWT](https://jwt.io/).

The `grant_type` should be set to `urn:ietf:params:oauth:grant-type:jwt-bearer`, indicating that this is a JWT assertion.

```curl
curl -s -d \
'grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer&client_id=c55de605-48b6-42ef-b69e-cd9d14ded15a&assertion=eyJraWQiOiIwIiwidHlwIjoiS....' \
-H 'Content-Type: application/x-www-form-urlencoded' \
'https://oauth.zettle.com/token'
```

You will get a normal access token back, which you can then start using. There is no refresh token provided in the response for this grant. To get a new access token once the previous one has expired, you will just call the assertion grant again with the same API key.

### Optional tracking when using assertions
Since an assertion is not connected to an OAuth application, the usage cannot be tracked by Zettle in the same way as for normal OAuth based integrations. This can be a problem in cases where the developer of an integration and Zettle has a partnership and where the usage of that integration should be attributed to the developer.

To solve this, we provide two tracking entry points. They both require the developer to register the integration as a normal OAuth application at [Zettle Developer Portal](https://developer.zettle.com).

When requesting an access token using an assertion, a developer that wants to be attributed should use the `client_id` of the created OAuth application instead of the `client_id` of the assertion itself in the `/token` request.

When performing API calls, a custom header `X-iZettle-Application-Id` should be set, with the `client_id` of the application.

These two measures will ensure that the usage of the integration can be attributed to the integrator.

## Refreshing an access token
[RFC 6749 section 6. Refreshing an Access Token](https://tools.ietf.org/html/rfc6749#section-6)

The access token has a lifetime of 7200 seconds (120 minutes). Once it has expired, the client needs to request a new access token using the refresh token:
```curl
curl -s -d \
'grant_type=refresh_token&client_secret=7356b8a1-75ac-4336-970b-bef63cd219c1&client_id=c55de605-48b6-42ef-b69e-cd9d14ded15a&refresh_token=07b0edfc-f557-4e52-a995-384288e2351e' \
-H'Content-Type: application/x-www-form-urlencoded' \
'{{URL}}/token' | python -m json.tool
```

The response has the same format as the request:
<!-- what format -->
```json
{
    "access_token": "eyJraWQiOiIxNDQ0NzI3MTY0Njk4IiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJpc3MiOiJpWmV0dGxlIiwiYXVkIjoiQVBJIiwiZXhwIjoxNDQ0ODI1NTk5LCJqdGkiOiJzRXlEQ2JOS1d1dWhqN2FadGxibnJnIiwiaWF0IjoxNDQ0ODIzNzk5LCJuYmYiOjE0NDQ4MjM2NzksInN1YiI6IlllemNseEJlVHBLUDBqNXRBdmdqWXciLCJzY29wZSI6ImFsbCJ9.RtbbSu68fMMGssQHIhdLF6Sa4nFeBkMDSQkDsVYxaKa0jMqd6i6Dl9W1C4XJdnNdNiuke6fG5dGGSB6yR6mx5qXJcEBl8bwUTp7r1jX3n9WbgXHQtwCiSx5J3wMrE3RIEGHqSeD0DkQDLaKLqlb12H1DUMK4wTFL3_KxtYqP_dEijOPtV9gN7EkZUIitWqMa3DOR2IqszldrcUXIVPkp_DRWtjvBSCsgglQFGgjyblpOQJM5CR64aD1CgyOSE6JAMWHBhbB7j7gB6DALHLh82twU9camEkCFKKra4n1Zj6mHF9DMSwccH7lpdjjSKPEUujyKCaLQRn82AH0Q8vSlKg",
    "expires_in": 7200,
    "refresh_token": "IZSEC07b0edfc-f557-4e52-a995-384288e2351e"
}
```

> **Note:** Rotation of refresh tokens may be enabled at any given time. That is, when you get a new access token, you might also get a new refresh token (invalidating any previously issued refresh token). Therefore always make sure to pick up the refresh token you get in the response and use that for the next refresh request, instead of reusing the current refresh token.

## Errors
[RFC 6749 section 5.2. Error Response](https://tools.ietf.org/html/rfc6749#section-5.2)

The Authorization OAuth2 API will return errors when retrieving and refreshing access tokens as specified in the RFC, for example:

```json
HTTP 400 Bad Request
{
    "error": "invalid_grant",
    "error_description": "invalid refresh_token"
}
```

## Include the access token in an API request
Access tokens should be included as follows:
```http
 GET /resource HTTP/1.1
 Host: server.example.com
 Authorization: Bearer eyJraWQiOiIxNDQ0NzI3MTY0Njk4IiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ
```
This is described in detail in [RFC 6750 section 2.1](https://tools.ietf.org/html/rfc6750#section-2.1).

### Errors
When using an expired or invalid access token in an API request, the resource service will return a `HTTP 401 Unauthorized` response, with the error set in the HTTP header `WWW-Authenticate`, similar to this:
```json
Bearer error="invalid_request", error_description="ACCESS_TOKEN_EXPIRED"
```

> **Note:** Not all services are not currently using the error code `ACCESS_TOKEN_EXPIRED` as the content of the `error_description` field, but rather the clear text value "The access token has expired". This will change over time and eventually all services will use the `ACCESS_TOKEN_EXPIRED`.

## Utility Endpoints

### Get user info

`GET users/self`

#### Example response
```json
{
    "uuid": "de305d54-75b4-431b-adb2-eb6b9e546014",
    "organizationUuid": "ab305d54-75b4-431b-adb2-eb6b9e546013"
}
```

### Disconnect application from organization
An external integration that want to disconnect its connection to an Zettle organization can do so by calling the following endpoint, providing the issued access token for that organization. This can be useful when a user disconnects outside of Zettle, to clean up registered webhooks and remove access for the external integration.

```
DELETE /application-connections/self
Authorization: Bearer <access token>
```

#### Response
This endpoint will respond with a `204 No Content` HTTP code.
