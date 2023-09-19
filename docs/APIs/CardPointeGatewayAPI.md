# Overview

The CardPointe Gateway API allows you to securely accept a wide-range of credit, debit, and alternative payments.  

> To get started with the CardPointe Gateway API, review the [Integration Process Overview](?path=docs/documentation/IntegrationProcessOverview.md) for helpful information for testing and integrating the API.

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](http://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 8/26/2023

An update to the CardPointe Gateway is tentatively scheduled for release to the UAT environment on 8/18/2023 and to the Production environment on 8/26/2023.

This release includes the following updates in addition to internal fixes and enhancements: 

### Original MID Requirement for Refund Requests 

Refund requests must include the Merchant ID (MID) used in the original authorization request. If the refund does not include the same MID, or a null value, then the request returns an error; for example:

```
{ 
    "respproc": "PPS", 
    "resptext": "Txn not found", 
    "retref": "198096236590", 
    "respcode": "29", 
    "respstat": "C" 
}
```

### Updated Incorrect URL Endpoint API Response 

When an incorrect endpoint URL is called a new error response is returned that displays the incorrect URL passed in the request. For example, the path below has a spelling error: 

```
{ 
    "timestamp": "2023-06-20T15:04:43.980+00:00", 
    "status": 404, 
    "error": "Not Found", 
    "path": "/cardconnect/rest/inquireByOderid" 
}
```

## Date Updated: 6/10/2023 

An update to the CardPointe Gateway was released to the UAT environment on 6/2/2023 and to the Production environment on 6/10/2023.

This update includes backend enhancements.

<!-- type: row -->

<!-- type: card
title: CardPointe Gateway API Changelog
description: Visit the Changelog for more information on recent updates to the CardPointe Gateway API and documentation
-->

<!-- type: row-end -->

# RESTful Implementation

The CardPointe Gateway API uses RESTful web services to implement create, read, update, and delete (CRUD) functions. 

- GET operations are used for inquiry requests (for example, the Inquire and Settlement Status services).
- PUT and POST operations are used to make create or update requests (for example the Authorization and Void services).
- DELETE operations are supported for the Profile service, to delete a stored profile.
- The REST Service expects all properties encoded as US ASCII Strings.

This web service uses the JSON (JavaScript Object Notation) method of encoding data for transmission via the HTTP network protocol. Most programming languages have libraries to convert an arbitrary object to and from a JSON data transfer encoding. 

> All request parameters are case-sensitive, as documented in the API.

<!-- theme: warning -->
> The CardPointe Gateway API returns all response data in JSON. Updates to the API are backwards compatible, and we reserve the right to add new properties (key/value pairs) within the JSON response.
> 
> It is a best practice to develop client applications that dynamically accept and parse all JSON response properties, rather than coding for the static position of specific properties within the response data.

# Developer Resources

The following resources are available to simplify your integration.

## Developer Guides 

Our [Developer Guides](?path=docs/getting-started.md) provide additional information for integrating the CardPointe Gateway and related products and services.

You can browse the whole library at the [Developer Guides home page](?path=docs/getting-started.md), or jump right to the [CardPointe Gateway Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) to make the most of your CardPointe Gateway API integration.

## Sample Postman Collection 

To help you get started with your integration, we created a sample Postman Collection that includes a template of the CardPointe Gateway API service endpoints.

The CardPointe Gateway API Integration collection also includes a sample Environment to help you get familiar with the API. 

Click the Run in Postman button to download the CardPointe Gateway API Integration collection:

> [Run in Postman](https://app.getpostman.com/run-collection/b88a17df4b7a2b5667d2#?env%5BCardPointe%20Gateway%20Integration%20Environment%5D=W3sia2V5Ijoic2l0ZSIsInZhbHVlIjoie1VBVCBvciBwcm9kdWN0aW9uIHNpdGV9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ1cmwiLCJ2YWx1ZSI6Imh0dHBzOi8ve3tzaXRlfX0uY2FyZGNvbm5lY3QuY29tL2NhcmRjb25uZWN0L3Jlc3QiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImNzdXJsIiwidmFsdWUiOiJodHRwczovL3t7c2l0ZX19LmNhcmRjb25uZWN0LmNvbS9jYXJkc2VjdXJlL2FwaS92MS9jY24vdG9rZW5pemUiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6IkF1dGhvcml6YXRpb24iLCJ2YWx1ZSI6IntBUEkga2V5fSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiQ29udGVudC1UeXBlIiwidmFsdWUiOiJhcHBsaWNhdGlvbi9qc29uIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJtZXJjaGlkIiwidmFsdWUiOiJ7Q2FyZENvbm5lY3QgTUlEfSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY3VycmVuY3kiLCJ2YWx1ZSI6IlVTRCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiZXhwaXJ5IiwidmFsdWUiOiJ7Q2FyZCBleHBpcmF0aW9uIG5ubm59IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJhY2NvdW50IiwidmFsdWUiOiJ7e3Rva2VufX0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InRva2VuIiwidmFsdWUiOm51bGwsImVuYWJsZWQiOnRydWV9LHsia2V5IjoicmV0cmVmIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InByb2ZpbGVpZCIsInZhbHVlIjpudWxsLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFjY3RpZCIsInZhbHVlIjpudWxsLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImJhdGNoaWQiLCJ2YWx1ZSI6bnVsbCwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJkYXRlIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfV0=)

See [Running the API in Postman](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#running-the-api-in-postman) in the [CardPointe Gateway Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for more information on using the Postman collection.

# Connecting to the Server

Your application communicates with the CardPointe Gateway RESTful web service using the following URL:

`https://<site>.cardconnect.com/cardconnect/rest/<endpoint>`

A username and password are required in the HTTP Authorization Header property in each API request using the API credentials provided for your merchant account. 

Basic Authorization is expected, using a Base64-encoded username and password string as the value. If this value is incorrect or not provided in the request header, an HTTP Exception "401:Unauthorized" is returned to the caller. 

See the [API Connectivity Guide](?path=docs/documentation/APIConnectivityGuide.md) for more information on connecting to the CardPointe Gateway and other CardPointe services.

## Testing Your API Credentials 

To test and validate your API credentials, you can make a PUT request, including the merchant ID in the body of the request, to the base URL. The Gateway verifies that the MID matches the credentials provided in the header.

For example:

```
PUT /cardconnect/rest/ Host: <site>Authorization: Basic <base64 encoded mid-level credentials>Content-Type: application/json{"merchid": "496082673888"}
```

If the credentials are valid, the response returns the message "CardConnect REST Servlet." 
