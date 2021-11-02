# Zettle Go API Beta Documentation

Zettle provides APIs for you to integrate Zettle Go with your services.

> **Note:** The API documentation is currently in the beta phase.

Currently, Zettle provides APIs for the following markets:

-   United Kingdom
-   Sweden
-   Brazil
-   Norway
-   Denmark
-   Finland
-   Germany
-   Mexico
-   Netherlands
-   France
-   Spain
-   Italy

> **Note:** You can build integrations with Zettle Go APIs only for the supported markets, no matter where you are located.

## APIs

-   [OAuth](authorization.md)
-   [Finance](finance-api/overview.md)
-   [Purchase](purchase.adoc)
-   [Product Library](product-library.adoc)
-   [Inventory](inventory.adoc)
-   [Image](image.md)
-   [Pusher](pusher-api/)
-   [Giftcard](giftcard.adoc)

All API changes are recorded in [Changelog](CHANGELOG.adoc).

All API incidents are recorded in [Incidents](incidents.md).

For common questions about the APIs, see [Frequently Asked Questions](faq.adoc).

## Credentials

Apply for API credentials on [Zettle Developer Portal](https://developer.zettle.com/register).

## Get help
Contact our [Integrations team](mailto:api@zettle.com) for more information. 

When contacting the Integrations team, please provide the following information:

* Description about the issue. That includes:
    * What's the integration used for?
    * Which API or APIs are you using?
    * What went wrong? 

* Information about affected merchants. That includes:
    * Emails of affected merchants.
    * UUIDs and organisation UUIDs of the affected merchants.
      To fetch UUIDs and organisation UUIDs, use the `GET users/me` endpoint of the OAuth API. For more information about using the endpoint, see [Get user info](authorization.adoc/#get-user-info).
