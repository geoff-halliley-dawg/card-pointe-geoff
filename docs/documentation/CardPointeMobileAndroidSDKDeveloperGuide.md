# Introduction

The CardPointe Mobile Android SDK seamlessly connects your Android<sup>tm</sup> applications to CardSecure for tokenization of customer card data. Tokens and other associated payment details are then retrieved by your server and securely transmitted to the CardPointe Gateway for authorization, using a server-side REST client.

# What's New?

<!-- theme: warning -->
> See Getting Started, below, to download the latest version of the CardPointe Mobile Android SDK.

## Version 3.0.86

This version includes the following changes from this and previous builds:

- To improve BBPOS Chipper Mini 2 connectivity, added a check for isConnected in the BBPOS swiper listener callback. The SDK will check that the device is connected and, if so, call `onConnected` to notify the app.
- Other minor fixes and improvements.

## Version 3.0.80

This version includes the following changes from this and previous builds:

- Added `onBusy()` to the device listener. When attempting to connect to the swiper, if it is in a “busy” state and unable to initiate a new connection, the SDK will call `onBusy()`.
- The SDK will call `onError()`, if the transaction is cancelled while processing (for example, if the card is removed in the middle of an EMV transaction). This presents an error to the calling application, which can then retry the transaction.

<!-- type: row -->

<!-- type: card
title: CardPointe Mobile Andriod SDK Changelog
description: Visit the Changelog for more information on recent updates to the CardPointe Mobile Android SDK and documentation
link:
-->

<!-- type: row-end -->

# Overview

A complete mobile payment integration consists of two components:

- Tokenization is handled by the CardPointe Mobile SDK integrated with your mobile application.
- Authorization is handled by host scripts integrated with your server application.

See the CardPointe Mobile SDKs Developer Guide for detailed information on the overall solution, as well as example host scripts to help you get started with your server-side implementation.

## Supported Devices

If you are developing an application to accept card present payments, you must integrate a mobile payment reader (swiper) device with your solution. Currently, the CardPointe Mobile Android SDK includes support for the ID TECH VP3300.

The VP3300 is a Bluetooth-enabled mobile payment reader device that supports MSR (swipe) and EMV (chip) transactions. The VP3300 connects to your phone or tablet using Bluetooth 4.0, which supports Bluetooth Low Energy and automatic pairing. 

See the CardPointe Mobile Device User's Guide for more information on the ID TECH VP3300.

<!-- theme: danger -->
> The ID TECH VP3300 is only available for merchants processing on the First Data Rapid Connect platform.

> Support for NFC (contactless) transactions on the VP3300 is planned for a future update of the SDK.

# Getting Started

Download the latest version of the SDK to get started:

<!-- type: row -->

<!-- type: card
title: CardPointe Mobile Andriod SDK v3.0.86 ZIP
link:
-->

<!-- type: row-end -->

For new integrations, see the Android SDK Integration Guide, below for detailed information on integrating the SDK with your application.

For migrating an existing app to the latest version of the SDK, see the Android SDK Migration Guide, below.

<!-- theme: warning -->
> The CardPointe Mobile Android SDK includes an API reference help file. Browse to **/docs/reference api** and double-click **help-doc.html** to launch the API help.

## Requirements

Before you begin, ensure that you have the following:

- **Android App Requirements** - The CardPointe Mobile Android SDK requires the following minimum software levels:
    - Android OS version 4.4 (KitKat) or later
    - Android SDK version 7 (API level 24) or later
- **Android Studio** - The Android SDK and related documentation support the Android Studio integrated development environment.
- **CardPointe Mobile Android SDK** - See Getting Started to download the SDK.
- **Sample App** - Review the included sample applications to better understand the SDK integration.
- **CardPointe Merchant ID and API credentials** - Contact integrationdelivery@fiserv.com if you do not already have an account.
- **CardSecure Tokenization URL** - Your merchant ID must be associated with a CardSecure URL to send requests to CardSecure.
- **(Optional) Mobile Payment Reader Device** - If you are integrating a mobile payment reader (swiper) device, you must us a device that has been pre-provisioned and provided for use with your merchant account.

<!-- theme: danger -->
> If you are using the ID TECH VP3300, your merchant account must be processing on the First Data Rapid Connect platform.

- **(Optional) Android Device** - If you are using a mobile payment reader (swiper) device, you will need a physical Android device to test the integration.

## Customer and Merchant-Facing Applications

The CardPointe Mobile SDK supports both merchant and customer-facing applications. Depending on the type of application you are developing, you can include specific modules tailored to your specific audience and integration needs.

### Customer-Facing Applications

A customer user downloads and interacts with your application directly, without interacting with a merchant or participating in a card-present transaction.

### Merchant-Facing Applications

A merchant user interacts with the application as an extension of the integrated point-of-sale (POS) system, and might accept both card present and card not present payments.

### Andriod SDK Features

The following table provides an overview of the features that you might want to integrate with your application, depending on your audience:

| Functionality | User Type | Component | Description |
| --- | --- | --- | --- |
| Stored Customer Profiles | Customer | Integrated Customer Profile UI | Integrates the CardPointe Gateway Profile service, allowing customers to securely store and manage their payment accounts. <br> <br> See the CardPointe Gateway Profile service documentation for more information on the profile service. <br> <br> See Integrating the Customer Profile UI for information on adding this feature to your application. |
| Mobile Payment Reader Devices	| Merchant | Android Swiper Implementation | An integrated mobile payment reader (swiper) device used to securely capture and encrypt card data prior to tokenization. <br> <br> See Supported Devices for information on the devices that are currently supported. <br> <br> See the Android SDK Integration Guide for instructions for integrating mobile payment reader devices. |

Additionally, you can integrate the Google Pay Android API to add Google Paytm support to your app. See the Google Pay Developer Guide for more information.

# Android SDK Sample APP

Before you begin your integration, you should review the sample app, which includes demos of the features provided by the SDK.

The sample app includes the following demos:

- **Configure Swiper** - Connect to an ID TECH VP3300 device.
- **Tokenization Methods** - Tokenize card data obtained from the swiper device, or manual entry.
- **Modal Profile Flow** - Create and manage stored customer profiles using the CardPointe Gateway Profile service.
- **Stack Profile Flow** - Create and manage stored customer profiles using the CardPointe Gateway Profile service.
- **Theming** - Customize the colors used throughout the app.
- **Signature** - Capture a signature in the format required for the CardPointe Gateway Signature Capture service.

## Tokenization URL

The sample app includes an editable URL field. This URL is used to connect to CardSecure to tokenize the payment card data. Ensure that you enter the URL provided for your merchant account.

# Android SDK Integration Guide

This guide provides information for integrating the CardPointe Mobile Android SDK with your application.

<!-- theme: warning -->
> See the API reference documentation for detailed information on using the classes and methods described in this guide. Browse to **/docs/reference api** and double-click **help-doc.html** to launch the API help.

> If you already integrated an older version of the Android SDK and want to migrate to the latest version, see the Android SDK Migration Guide, below.

## Importing the SDK to Your Project

