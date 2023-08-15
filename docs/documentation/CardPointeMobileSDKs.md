# Introduction

The CardPointe Mobile SDKs seamlessly connect your mobile applications to CardSecure to securely encrypt and tokenize customers' payment card data. Tokens and associated payment details can then be retrieved by your server application and securely transmitted to the CardPointe Gateway for authorization.

CardPointe Mobile SDKs are available for Androidtm and iOS apps, as well as server-side tool kits to help you get started with your server application.

This guide provides an overview of the Mobile SDKs. See the CardPointe Mobile SDK Developer Guides for detailed information on integrating payments with your mobile app.

# Overview

A complete mobile payment integration consists of two components:

- Tokenization is handled by the CardPointe Mobile SDK (Android or iOS) integrated with your mobile application.
- Authorization is handled by host scripts integrated with your server application.

## Tokenization (Client-side)

The CardPointe Mobile SDK installs alongside your mobile application, and uses CardSecure to tokenize and encrypt payment card data. Card data can be manually entered in the application or captured, using a supported mobile payment reader device. Payment card data is encrypted and tokenized without being exposed to your software application or server.

Additionally, tokens can be stored in customer profiles for use in subsequent transactions.

See Understanding CardSecure Tokens for detailed information on how CardSecure tokens are created and used.

## Authorization (Server-side)

The CardPointe Gateway REST clients install on your application server to integrate your solution to the CardPointe Gateway.

Using a REST client, your sever authenticates with the CardPointe Gateway, makes authorization requests using tokens retrieved from the mobile app, and handles responses from the Gateway.

See Server-side Host Scripts for information on using the CardPointe Gateway REST clients.

See the CardPointe Gateway API documentation for more information on the features and capabilities of the CardPointe Gateway.

## Tokenization and Authorization Flow

The following diagram illustrates the tokenization and payment flow using the Mobile SDK and server-side REST client.

<!-- align: center -->
![Mobile SDK Diagram](../../../../assets/images/SDK-Diagram-5-2022-09-14)
-

