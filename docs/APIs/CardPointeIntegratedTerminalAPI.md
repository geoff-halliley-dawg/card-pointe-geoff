> The Bolt Terminal API is now the CardPointe Integrated Terminal API.

# CardPointe Integrated Terminal API Overview

The CardPointe Integrated Terminal API streamlines the integration of our P2PE-validated, card-present payment acceptance solution with your point-of-sale (POS) application. Integrating the Terminal API with your application, allows you to easily and securely accept card-present payments within your POS environment. 

The CardPointe Integrated Terminal solution consists of the following components:

- The Terminal API, which provides your software with access to the terminal service.
- CardPointe Integrated Terminals, pre-provisioned for your merchant account. 
- The [CardPointe Gateway](?path=docs/APIs/CardPointeGatewayAPI.md), which provides a complete solution for transaction processing and reporting. 
- [CardSecure](?path/docs/documentation/CardSecure.md), which tokenizes payment card information.
- Your point-of-sale (POS) software, integrated with the Terminal API.

> This API reference describes the complete Terminal API and all supported features and operations. Some features are specific to either Ingenico or Clover terminals, as noted throughout the API. See the [CardPointe Integrated Terminal Developer Guide for Clover Terminals](?path/docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md) for detailed information on key differences and additional information specific to integrating Clover terminals.

<!-- theme: warning -->
> The CardPointe Gateway API provides additional capabilities not offered by the Terminal API, including the ability to void or inquire on past transactions. To take full advantage of these features, you must integrate the CardPointe Gateway API into your payment acceptance solution. See the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) for information on integrating these features and other advanced capabilities.

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](http://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 8/16/2022 

Our Bolt family of integrated solutions is becoming **CardPointe Integrated Payments**.

Over the coming weeks, your terminals will receive an update with minor changes, including an updated default theme and branding for terminals that are not configured with custom branding.

Terminals will update automatically during the nightly reboot window; therefore, we ask that you leave your terminals powered on and connected to your network overnight. Once your terminal is updated, you may notice the following changes:

- If you are using the default device wallpaper and theme, the Bolt wallpaper and logo will change to **CardPointe Integrated Terminal**.
- The **Bolted** and **Unbolted** statuses will change to **Connected** and **Disconnected**, respectively.

<!-- type: row -->

<!-- type: card
title: CardPointe Integrated Terminal API Changelog
description: Visit the Changelog for more information on recent updates to the CardPointe Integrated Terminal API
-->

<!-- type: row-end -->

# Getting Started

See the CardPointe Integrated Terminal Developer Guides for information to help you get started, including guides for [understanding the Terminal API application workflow](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#understanding-the-terminal-api-application-workflow) and [understanding the Terminal API service endpoints](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#understanding-the-terminal-api-service-endpoints).

Additionally, see the [API Connectivity Guide](?path=docs/documentation/APIConnectivityGuide.md) for information on API connectivity.

## RESTful Implementation

The Terminal API is a RESTful web service, which uses the JSON (JavaScript Object Notation) method of encoding data for transmission via the HTTP network protocol. Most programming languages have libraries to convert an arbitrary object to and from a JSON data transfer encoding. See the [API Basics and Best Practices Guide](?path=docs/documentation/APIBasicsAndBestPractices.md) for more information on these concepts.

The Terminal API provides a library of POST operations used to submit HTTP requests to the web service. The Terminal service expects that ALL properties are encoded as US ASCII strings. Responses are returned in JSON objects. See [Service Endpoints](#service-endpoints) for detailed information on the supported request types and corresponding responses.

<!-- theme: danger -->
> Changes to the Terminal API are designed to retain backwards-compatibility. The JSON standard does not assign any significance to the ordering of key/value pairs, and this API does not guarantee the order of key/value pairs in response messages. See the [API Basics and Best Practices Guide](?path=docs/documentation/APIBasicsAndBestPractices.md) for recommendations for retaining backwards compatibility in your application.

## Requirements

Integrating the Terminal API requires: 

- Your application, which calls the Terminal service endpoints via HTTPS. 
- Pre-provisioned terminal devices.

Additionally, integrating EMV acceptance requires your merchant account to be on a supported processing platform. 

For more information on specific requirements, contact integrationdelivery@fiserv.com.

## Additional Resources

In addition to this API documentation, the following additional resources are available to help you get started.

### Developer Guides 

Browse the [Developer Guides](?path=docs/getting-started.md) library for general information on integrating our products and services.

The following guides provide helpful information for developing your integration:

<!-- type: row -->

<!-- type: card
title: CardPointe Integrated Terminal Developer Guides
description: Provides best practices and supplemental information for integrating the Terminal API with your point-of-sale application
link: ?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md
-->

<!-- type: card
title: CardPointe Integrated Terminal Developer Guide for Clover Terminals
description: Provides specific details for integrating Clover terminals with a new or existing CardPointe Integrated Terminal solution
link: ?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md
-->

<!-- type: row-end -->

Additionally, review the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) documentation for more information on the additional transaction processing and reporting capabilities offered by the CardPointe Gateway, for example the ability to void and inquire on transactions.

### Run in Postman

To help you get started with your integration, we created a Terminal API Postman collection that includes a template of the Terminal API service endpoints. Click the following button to download the collection.

> [Run in Postman](https://app.getpostman.com/run-collection/78bf7730d5cf55a3080f?action=collection%2Fimport#?env%5BBolt%20Terminal%20API%20UAT%20Environment%5D=W3sia2V5Ijoic2l0ZSIsInZhbHVlIjoie1VBVCBvciBwcm9kdWN0aW9uIHNpdGV9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ1cmwiLCJ2YWx1ZSI6Imh0dHBzOi8ve3tzaXRlfX0uY2FyZHBvaW50ZS5jb20vYXBpIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJBdXRob3JpemF0aW9uIiwidmFsdWUiOiJ7eW91ciBhdXRoIGtleX0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1lcmNoYW50SWQiLCJ2YWx1ZSI6Int5b3VyIG1lcmNoYW50SWR9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJoc24iLCJ2YWx1ZSI6Int5b3VyIGhzbn0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6IlgtQ2FyZENvbm5lY3QtU2Vzc2lvbktleSIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZX1d)

See [Running the API in Postman](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#running-the-api-in-postman) in the [CardPointe Integrated Terminal Developer Guides](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md) for information on using the Postman collection.

# Service Endpoints
