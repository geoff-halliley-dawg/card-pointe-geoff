<!-- type: row -->

<!-- type: card
description: The following guide provides best practices and other supplemental information for integrating the CoPilot API.

-->

<!-- type: row-end -->

# Using the CoPilot API to Submit a Merchant Application 

## Overview 

The Copilot API is an interface into the CoPilot merchant management platform. You can use the CoPilot API to create and update merchant accounts programmatically, instead of using the CoPilot web interface. 

> Applications generated using the CoPilot API cannot be white-labeled. The merchant application contains CardPointe and CardConnect branding as an indicator that the merchant account is independent of the ISV partner’s software, as well as containing CardConnect's Terms and Conditions that must be reviewed and accepted in order to create the account.

### Merchant Creation Workflow 

The following process outlines the typical workflow for using the CoPilot API to create a merchant account and send the digital application to the owner for review and signing:

1) Submit a request to the Token endpoint to obtain a bearer token needed for authentication.
2) Submit a `POST` request to the Merchant endpoint to create a new merchant account.
3) Store the CoPilot Merchant ID returned in the response, as it is needed in subsequent calls.
4) Submit a `PUT` request to the Signature endpoint to receive the link to the digital application.

Provide the digital application URL to the merchant so that they may review and sign the application digitally using their web browser. Once signed, the account then automatically progresses through the remaining underwriting and boarding procedures.

### Running the API in Postman 

To help you get started with your integration, we have created a Postman Collection that includes templates for all of the requests documented in the CoPilot API. 

The Postman Collection also includes a sample Environment containing variables for fields that are required in many of the API requests. See Configuring Your Postman Environment below for more information. 

Click the button below to download the CoPilot API Postman collection:

[Run in Postman](https://app.getpostman.com/run-collection/123cd242a1b83c941022?action=collection%2Fimport#?env%5BCoPilot%20API%20(UAT)%5D=W3sia2V5IjoidG9rZW4tdXJsIiwidmFsdWUiOiJodHRwczovL2FjY291bnRzdWF0LmNhcmRjb25uZWN0LmNvbS9hdXRoL3JlYWxtcy9jYXJkY29ubmVjdC9wcm90b2NvbC9vcGVuaWQtY29ubmVjdC90b2tlbiIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoidXNlcm5hbWUiLCJ2YWx1ZSI6IklOU0VSVF9VU0VSTkFNRSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoicGFzc3dvcmQiLCJ2YWx1ZSI6IklOU0VSVF9QQVNTV09SRCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY2xpZW50LWlkIiwidmFsdWUiOiJJTlNFUlRfQ0xJRU5UX0lEIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJjbGllbnQtc2VjcmV0IiwidmFsdWUiOiJJTlNFUlRfQ0xJRU5UX1NFQ1JFVCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYWNjZXNzLXRva2VuIiwidmFsdWUiOiJTRVRfQllfVEVTVFNfVEFCX09GX1RPS0VOX1JFUVVFU1QiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFwaS11cmwiLCJ2YWx1ZSI6Imh0dHBzOi8vYXBpLXVhdC5jYXJkY29ubmVjdC5jb20iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFwaS12ZXJzaW9uIiwidmFsdWUiOiIxLjAiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1lcmNoYW50LWlkIiwidmFsdWUiOiJJTlNFUlRfQ09QSUxPVF9NRVJDSEFOVF9JRCIsImVuYWJsZWQiOnRydWV9LHsia2V5Ijoic2FsZXMtY29kZSIsInZhbHVlIjoiSU5TRVJUX1NBTEVTX0NPREUiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im9yZGVyLWlkIiwidmFsdWUiOiJJTlNFUlRfT1JERVJfSUQiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1pZCIsInZhbHVlIjoiSU5TRVJUX0ZST05URU5EX01JRCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYmlsbGluZy1wbGFuLWlkIiwidmFsdWUiOiJJTlNFUlRfQklMTElOR19QTEFOX0lEIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJhcHBsaWNhdGlvbi10ZW1wbGF0ZS1pZCIsInZhbHVlIjoiSU5TRVJUX1RFTVBMQVRFX0lEIiwiZW5hYmxlZCI6dHJ1ZX1d)

### Configuring Your Postman Environment 

Included with the collection is a Postman Environment containing variables where you can pre-fill fields that are required in many of the API requests. 

As an initial step in configuring your environment variables, enter your authentication credentials in the existing environment variables and send a request to the Token endpoint. The Bearer token returned is automatically stored to the `access-token` environment variable and used to authenticate your subsequent API requests.

