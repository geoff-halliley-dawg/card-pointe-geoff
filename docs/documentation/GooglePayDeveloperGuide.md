# Overview

This guide provides information to help you quickly integrate Google Pay<sup>tm</sup> acceptance into your Androidtm or web-based application. Adding Google Pay to your payment methods allows customers to use their stored Google Pay data to quickly and securely pay for goods and services.

# How it Works

By integrating Google Pay acceptance and CardSecure tokenization into your app, you enable your customers to pay quickly and securely using the payment data stored in a Google Pay wallet. CardSecure handles the decryption and tokenization of the customer's payment data (the Google Pay response object). Integrating Google Pay enables you to provide a quick, seamless checkout experience to millions of Google Pay users.

All transactions are available in CardPointe for reporting. Additionally, integrating the CardPointe Gateway API allows you to take advantage of the complete set of transaction and reporting features of the CardPointe Gateway.

## Developing your App

First, you build or upgrade an application, using the Google Pay API to integrate Google Pay acceptance. During that process, you configure some key parameters to integrate CardSecure's tokenization method. See Integrating the Google Pay API later in this guide for more information. When your development is complete, you certify the app with Google to make it available to your customers.

## Payment Process

1) When a customer selects Google Pay as their payment method, your application requests that customer's encrypted payment card or wallet information.
2) Your app then passes that encrypted data to CardSecure, which decrypts and tokenizes the data.
3) Your app receives the tokenized data, and then passes it to the CardPointe Gateway in an authorization request.
4) Your app then receives a response from the CardPointe Gateway, completing the transaction.

## Testing your Integration

To test your Google Pay integration, you must enroll at least one valid payment card in your Google Pay wallet. When you use your wallet to generate a payload in the Google Pay test environment, Google returns dummy data in place of the actual card data. You then send this dummy data to CardSecure to test your integration.

# Requirements

To integrate Google Pay acceptance, you need:

- An Android or web application that integrates the following APIs:
  - Google Pay API - to request and retrieve customer payment information.
  - CardSecure API - to decrypt and tokenize Google Pay data.
  - CardPointe Gateway API - to process payments using tokens.
- For an Android application, an Android device with Google Play services version 16.0.0 or greater installed.
- A CardPointe merchant account configured to accept Google Pay payments.

**Notes:**
Google Pay acceptance is only supported for merchants boarded to the First Data Rapid Connect platform.
Supported card networks and authorization methods are dependent on your CardPointe merchant account configuration.

> By integrating the Google Pay API, you accept the Google Pay API Terms of Service.

## Integrating Google Pay for Android Applications

To integrate Google Pay acceptance into your application, perform the steps provided in the Google Pay Android API documentation, using the following supplemental information to integrate the Google Pay API and CardSecure with your app:

1) When you choose a payment tokenization method, do the following to connect to CardSecure:
    - For the **Gateway** parameter replace `example` with `cardconnect`.
    - For the **GatewayMerchantID** parameter, replace `exampleGatewayMerchantId` with your CardPointe merchant ID (MID).
3) When you define supported payment card networks, do the following:
    - For **getAllowedCardNetworks**, specify the card types that your merchant account is configured to accept.
    - For **getAllowedAuthCardMethods**, specify both `PAN_ONLY` and `CRYPTOGRAM_3DS`.
4) When you describe your allowed payment methods, do the following:
    - For **getBaseCardPaymentMethod**, specify
      `cardPaymentMethod.put("tokenizationSpecification", getTokenizationSpecification());`.
5) Configure your app to handle the response object. See Tokenizing Google Pay Data for more information.

## Integrating Google Pay with a Web Application

To integrate Google Pay acceptance into your application, perform the steps provided in the Google Pay web API documentation, using the following supplemental information to integrate the Google Pay API and CardSecure with your app:

To integrate Google Pay acceptance into your application, perform the steps provided in the Google Pay Android API documentation, using the following supplemental information to integrate the Google Pay API and CardSecure with your app:

1) When you choose a payment tokenization method, do the following to connect to CardSecure:
    - For the **gateway** parameter replace `example` with `cardconnect`.
    - For the **gatewayMerchantID** parameter, replace `exampleGatewayMerchantId` with your CardPointe merchant ID (MID).
  
      >The Google Pay Web API includes a `MerchantInfo` object with an additional `MerchantID` parameter used to specify a Google Pay merchant identifier generated by Google when you create your Google Pay merchant account. See the Google Pay Web API documentation for more information.
2) When you define supported payment card networks, do the following:
    - For **getAllowedCardNetworks**, specify the card types that your merchant account is configured to accept.
    - For **getAllowedAuthCardMethods**, specify both `PAN_ONLY` and `CRYPTOGRAM_3DS`.
3) When you describe your allowed payment methods, do the following:
    - For **cardPaymentMethod**, specify the tokenizationSpecification as follows:
    ```json
const cardPaymentMethod = Object.assign(
  {tokenizationSpecification: tokenizationSpecification},
  baseCardPaymentMethod
);
    ```
