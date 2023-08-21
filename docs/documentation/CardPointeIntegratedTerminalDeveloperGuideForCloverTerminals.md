# Overview

The CardPointe Integrated Terminal solution includes support for Clover Mini and Clover Flex terminals to provide a sleek and secure transaction experience for your customers. By integrating the Terminal API with your POS software, you can quickly build and deploy a complete P2PE payment solution, effectively reducing your scope of PCI compliance as a merchant or integrator.

This solution consists of the following components:

- The Terminal API, which provides your software with access to the terminal service and functions.
- Clover devices, which are EMV pre-certified and enables you to quickly achieve EMV acceptance and PCI scope reduction.
- The CardPointe Gateway, which provides a complete solution for transaction processing and reporting.
- CardSecure, which tokenizes payment card information.
- Clover terminal devices, pre-provisioned and provided for use with your merchant account.
- Your POS software, integrated with the Terminal API.

# What's New? 

## Date Updated: 10/13/2022

Our Bolt family of integrated solutions is now **CardPointe Integrated Payments**.

On Thursday, October 13th, 2022, we released an update to the CardPointe Integrated Terminal Application for Clover terminals. This update includes the following changes:

- If you are using the default device wallpaper and theme, the Bolt wallpaper and logo will be updated to **CardPointe Integrated Terminal**.
- The Bolt App, Admin Menu and other UI elements will be updated to CardPointe.
- The **Bolted** and **Unbolted** statuses will be updated to **Connected** and **Disconnected**, respectively.

Terminals will automatically download and install this update during the nightly reboot window. No merchant action is required. 

For more information on using custom branding for your Clover terminals, see Customizing the Clover Terminal.

## Date Updated: 4/12/2022 

An update to the CardPointe Integrated Terminal application for Clover terminals was released on April 14th, 2022.

This release includes internal enhancements and bug fixes.

<!-- type: row -->

<!-- type: card
title: Clover Terminal Changelog
description: Visit the Changelog for more information on recent updates to the CardPointe Integrated Terminal API and application for Clover terminals.
link:
-->

<!-- type: row-end -->

## Requirements

This solution requires:

- Your merchant account, boarded to the First Data Rapid Connect processing platform.
- Your application, integrated with the Terminal API via HTTPS.
- A Clover terminal device, pre-provisioned and provided for use with your merchant account.

For more Information on specific requirements contact isvintegrations@fiserv.com.

## Additional Resources

### Sample Postman Collection

To help you get started with your integration, we've created a sample Postman Collection that includes a template of the Terminal API service endpoints and parameters that are supported for Clover terminals. Click the following button to download the sample collection:

[Run in Postman](https://app.getpostman.com/run-collection/16c349c3713011b8c18c?action=collection%2Fimport#?env%5BBolt%20Terminal%20API%20UAT%20Environment%5D=W3sia2V5Ijoic2l0ZSIsInZhbHVlIjoie1VBVCBvciBwcm9kdWN0aW9uIHNpdGV9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ1cmwiLCJ2YWx1ZSI6Imh0dHBzOi8ve3tzaXRlfX0uY2FyZHBvaW50ZS5jb20vYXBpIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJBdXRob3JpemF0aW9uIiwidmFsdWUiOiJ7eW91ciBhdXRoIGtleX0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1lcmNoYW50SWQiLCJ2YWx1ZSI6Int5b3VyIG1lcmNoYW50SWR9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJoc24iLCJ2YWx1ZSI6Int5b3VyIGhzbn0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6IlgtQ2FyZENvbm5lY3QtU2Vzc2lvbktleSIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZX1d)

See Configuring Your Postman Environment in the general CardPointe Integrated Terminal Developer Guides for detailed information on setting up your environment variables.

### Related Documentation

This guide provides useful information specific to integrating Clover terminals. The following API references and terminal user guides provide additional helpful information: 

- See the Terminal API documentation for detailed information for integrating the Terminal API with your software.
- See the CardPointe Integrated Terminal Developer Guides for additional helpful information for developing various Terminal API workflows.
- See the CardPointe Gateway API documentation for detailed information for integrating the full capabilities of the CardPointe Gateway.
- See the Terminal User Guides for detailed information on setting up and using your Clover device.

# Integrating a Clover Terminal

If your payment acceptance solution already uses the Terminal API to integrate Ingenico terminals, you can add support for Clover terminals with some minor adjustments to your integration.

## Device Integration Details

Clover terminals offer a few additional features when compared to the Ingenico terminals. The following topics describe the new and different features that you should consider. 

### Terminal Activation Codes

When your merchants unbox a new Clover terminal device, they must enter an activation code during the initial setup process. This activation code is provided in a welcome email; however you may want to retrieve and display the activation code within your application, to improve the setup process for your merchants.

The Terminal API includes a `terminalActivationCode` endpoint, which you can use to supply the merchant ID and hardware serial number (HSN) associated with a specific terminal to retrieve the activation code for that terminal. 

The following example illustrates the `terminalActivationCode` request and response:

#### Sample terminalActivationCode Request and Response

```json
Request: 

POST /api/v3/terminalActivationCode HTTP/1.1
Host: uat.cardpointe.com:
Content-Type: application/json
X-CardConnect-SessionKey: b7964afcb63d4dbe9761667fb46e9644
Authorization: <Terminal API Key>

{
    "merchantId": "496414613885",
    "hsn": "C032UQ82940114"
}

Response:

{    
     "terminalActivationCode": "00115211"
}
```

### Merchant and Customer Modes

The CardPointe Integrated Terminal app for Clover terminals has two modes of operation: _Merchant_ Mode and _Customer_ Mode.

_Customer Mode_ is the customer-facing interface, in which the device displays an idle screen until your application sends a command to the device. This is the default mode of operation, and the device should remain in Customer Mode.

<!-- theme: danger -->
> The terminal can only receive commands when the terminal is connected and running in Customer Mode. 
>
> With the exception of the ping request, the terminal ignores commands from your software when it is in Merchant Mode. If you send a ping request while the device is in merchant mode, a "Ping received" message is displayed on the device.
>
> Other Terminal API requests that send commands to the device return the error
> `"errorMessage": "hsn: <hsn> is currently in merchant mode".`

_Merchant_ Mode is the merchant-facing administrative interface. In Merchant Mode, the merchant user can access the Admin Panel to configure and troubleshoot the application and device settings. Additionally, the user can exit the app to access the App Launcher and Settings menu to access the Android device settings. The merchant user can switch from Customer Mode to Merchant Mode by pressing all four corners of the touch screen simultaneously.

A noted, only the ping request should be used when the device is in Merchant Mode. 

### Receipt Printing

The Clover terminal includes a built-in receipt printer. To take advantage of this feature, the authCard and authManual Terminal API service endpoints now include a printReceipt parameter. See Software Integration Details for more information on integrating this feature with your software.

When a transaction is successfully authorized by the CardPointe Gateway, the authorization response includes EMV card data (if the payment was made using an EMV or contactless payment) as well as specific merchant information. Some receipt field settings must be configured for your merchant account. 

See Printing Receipts later in this guide for detailed information on the data used to print a receipt from the Clover terminal, and for guidelines on integrating a custom solution for receipt printing. Additionally, you can customize the data that you include on your receipts, depending on your business needs.

<!-- theme: danger -->
> For Clover Mini terminals, the printer is disabled when the device enters low-power mode. The Clover Mini must be connected to the hub, and the hub must be connected to a power source in order to print receipts.
>
> If your application sends a request to the printer while the terminal is in low-power mode, the response includes the following error fields:
> 
> `"errorCode": 800,`
> `"errorMessage": "Printing not supported"`

### Display Specifications

The Clover Mini features a 7″ 1280×800 touch screen display, in a landscape orientation. 

The Clover Flex features a 5" 720×1280 touch screen display in a portrait orientation. 

### LTE Connectivity

The Clover terminals support LTE connectivity, which enables merchants to fall back on the LTE wireless network to continue to process transactions in the event of a network interruption. Note that the device must have an active SIM card and wireless data plan, and merchants can manage these settings on the device.

When the terminal switches from the Wi-Fi/Ethernet network to the LTE network, the device loses the connection to the terminal service, and a new session must be established by calling the Terminal API `connect` endpoint. When you call the `connect` endpoint to establish a new connection, you can optionally include the `force` parameter with a value of `true` to terminate the existing session (and any in-flight operations) before establishing the new session.

LTE connection quality and speed can vary, and may be slower than the device's primary Ethernet or Wi-Fi connection. Therefore, your software should allow for a delay in communications with the terminal.

## Software Integration Details

The following topics describe some specific details for integrating Clover terminals and key differences from the Terminal API integration for Ingenico terminals.

> See the Terminal API Documentation for detailed information on integrating the Terminal API with your software.

### Supported Endpoints

The following table illustrates which Terminal API endpoints are supported for use with Clover terminals. See Key Differences for detailed information on updating your integration.

| Resource Name | Ingenico | Clover |
| --- | --- | --- |
| authCard | &#10003; | &#10003; |
