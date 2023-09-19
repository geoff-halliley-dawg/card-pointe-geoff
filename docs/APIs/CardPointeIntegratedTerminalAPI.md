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

