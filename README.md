# Zettle Go API Beta Documentation

Zettle provides APIs for you to integrate Zettle with your services.

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

> **Note:** You can build integrations with Zettle APIs only for the supported markets, no matter where you are located.

## APIs

-   [OAuth](authorization.md)
-   [Finance](finance-api/overview.md)
-   [Purchase](purchase.adoc)
-   [Product Library](product-library.adoc)
-   [Inventory](inventory-api/overview.md)
-   [Image](image.md)
-   [Pusher](pusher-api/overview.md)
-   [Giftcard](giftcard.md)

All API changes are recorded in [Changelog](CHANGELOG.adoc).

All API incidents are recorded in [Incidents](incidents.md).

For common questions about the APIs, see [Frequently Asked Questions](faq.adoc).

## Get started
Before you start to build an app with Zettle, you need to [sign up for a developer account at Zettle Developer Portal](get-started/user-guides/sign-up-for-a-developer-account.md).

When you have a developer account, choose one of the following app types based on your needs:
* To build a self-hosted app that is hosted by merchants individually, [create a self-hosted app](oauth-api/user-guides/create-an-app/create-a-self-hosted-app/create-a-self-hosted-app.md).
* To build a partner-hosted app that is hosted by you as an integrator, [create a partner-hosted app](oauth-api/user-guides/create-an-app/create-a-partner-hosted-app.md).

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
      To fetch UUIDs and organisation UUIDs, use the `GET users/self` endpoint of the OAuth API. For more information about using the endpoint, see [Get user info](authorization.md/#get-user-info).
