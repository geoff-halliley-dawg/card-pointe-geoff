<!-- type: row -->

<!-- type: card
description: The following guides provide best practices and other supplemental information for integrating the CardPointe Gateway API.

-->

<!-- type: row-end -->

# Running the API in Postman

To help you get started with your integration, you can use the sample CardPointe Gateway API Integration Postman Collection, which includes a template of the API service endpoints.

The CardPointe Gateway API Integration collection also includes a sample Environment to help you get familiar with the API. See Configuring Your Postman Environment, below, for more information.

Click the button below to download the CardPointe Gateway API Integration collection:

[Run in Postman](https://app.getpostman.com/run-collection/b88a17df4b7a2b5667d2#?env%5BCardPointe%20Gateway%20Integration%20Environment%5D=W3sia2V5Ijoic2l0ZSIsInZhbHVlIjoie1VBVCBvciBwcm9kdWN0aW9uIHNpdGV9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ1cmwiLCJ2YWx1ZSI6Imh0dHBzOi8ve3tzaXRlfX0uY2FyZGNvbm5lY3QuY29tL2NhcmRjb25uZWN0L3Jlc3QiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImNzdXJsIiwidmFsdWUiOiJodHRwczovL3t7c2l0ZX19LmNhcmRjb25uZWN0LmNvbS9jYXJkc2VjdXJlL2FwaS92MS9jY24vdG9rZW5pemUiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6IkF1dGhvcml6YXRpb24iLCJ2YWx1ZSI6IntBUEkga2V5fSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiQ29udGVudC1UeXBlIiwidmFsdWUiOiJhcHBsaWNhdGlvbi9qc29uIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJtZXJjaGlkIiwidmFsdWUiOiJ7Q2FyZENvbm5lY3QgTUlEfSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY3VycmVuY3kiLCJ2YWx1ZSI6IlVTRCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiZXhwaXJ5IiwidmFsdWUiOiJ7Q2FyZCBleHBpcmF0aW9uIG5ubm59IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJhY2NvdW50IiwidmFsdWUiOiJ7e3Rva2VufX0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InRva2VuIiwidmFsdWUiOm51bGwsImVuYWJsZWQiOnRydWV9LHsia2V5IjoicmV0cmVmIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InByb2ZpbGVpZCIsInZhbHVlIjpudWxsLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFjY3RpZCIsInZhbHVlIjpudWxsLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImJhdGNoaWQiLCJ2YWx1ZSI6bnVsbCwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJkYXRlIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfV0=)

## Configuring Your Postman Environment

Environment variables allow you to autofill select fields with pre-configured values. For example, instead of manually specifying your merchant ID in the body of each request, you can set the `{{merchantid}}` variable to your specific MID.

Once you have received API credentials , you can configure the following variables to auto-fill your merchant-specific data in requests to the CardPointe Gateway API:

- **site** - Set this value to the site for the test or production environment.
- **url** - The `{{url}}` variable is used to set the base url (`https://{{site}}.cardconnect.com/cardconnect/rest/`) for the CardPointe Gateway RESTful web services. `{{site}}` is populated with the value you set for that variable.
- **csurl** - The `{{csurl}}` variable is used to set the url  (`https://{{site}}.cardconnect.com/cardsecure/api/v1/ccn/tokenize`) for the CardSecure tokenize REST web service. `{{site}}` is populated with the value you set for that variable.
- **Authorization** - Set this value to the Base64-encoded API credentials that you received. The `{{Authorization}}` variable is used in the header of every request.
- **merchid** - Set this value to your merchant ID (MID). The `{{merchid}}` variable is used in the body of most requests.

These variables are required in the header and body of most requests. The sample environment includes additional variables that you can configure when testing your integration.

Additionally, the sample collection includes test scripts, which gather specific values (such as `token`) from the response body, and dynamically update the corresponding environment variable.

To configure environment variables, do the following in Postman:

1) Click the gear icon to open the Manage Environments menu.

<!-- align: center -->
![CCG Postman Sample](../../assets/images/CCG_Postman_Sample_2.png)

2) On the Manage Environments menu, enter your merchant-specific values for each variable.

<!-- align: center -->
![Gateway PM Environment](../../assets/images/Gateway-PM-Environment.png)

3) Click **Update**

<!-- theme: warning -->
> See the [Postman user documentation](https://learning.postman.com/docs/introduction/overview/) for detailed information on using Postman to test APIs.

# Testing Your Integration

This guide provides information to help you develop and test your integrated application. Whether you are developing a new application, or maintaining an existing one, you should incorporate continuous testing in your SDLC.

<!-- theme: warning -->
> As we continue to update and improve the CardPointe Gateway, you should regularly schedule regression testing to ensure that your application utilizes and is compatible with applicable changes to the Gateway.

## Understanding the UAT Environment

You use the UAT (user acceptance testing) sandbox environment to test and validate your application. When you begin your application development and integration, you connect to the UAT instance of the CardPointe Gateway.

To connect to the UAT environment, your application uses the following URL:

`https://<site>-uat.cardconnect.com/cardconnect/rest/<endpoint>`

where <site> is the site name provided to you, and <endpoint> is a CardPointe Gateway service endpoint.

The UAT environment includes emulators that simulate the payment processing activities that occur in production. In this environment, you test with dummy data that is never sent to the payment processor. You should use test card numbers (for example, 4111 1111 1111 1111 or 4444 3333 2222 1111) and physical test cards. 

## UAT Request Rate Limiting

In the UAT environment, requests to the following endpoints are rate-limited: 

- `funding`

- `inquire`

- `profile`

- `settlestat`

Requests to these UAT endpoints are limited to **20 transactions per minute** (TPM), by IP address.

Responses from these endpoints in the UAT environment include the following rate-limit header fields:

`X-Rate-Limit-Retry-After-Seconds:` - Returned for unsuccessful `HTTP 429 Too Many Requests` responses when the limit has been reached. Specifies the seconds remaining before the limit resets.
`X-Rate-Limit-Remaining:` - Returned for successful `HTTP 200 OK` responses. Specifies the number of requests available before the limit is reached.

## Understanding UAT Responses

Data that you transmit to the UAT environment is never sent to the payment processing networks; instead, the CardPointe Gateway communicates with an emulator that simulates the payment processor that your merchant ID uses to process payments. The emulator mimics the behavior of the given processing host, and returns a response similar to what you would receive for a live transaction in the production environment.

CardPointe Gateway API responses returned in the UAT environment include fields and arrays in a randomized order. Additionally, UAT responses include dummy fields, arrays, and values. This is intended to help clients develop integrated software that dynamically parses the response data, rather than expecting fields to be present in static positions within the response object. 

<!-- theme: warning -->
> See Ensuring Backwards Compatibility in the API Basics and Best Practices Guide for more information.

Some specific situations, such as a network timeout and specific decline scenarios, require specific input to initiate. See Test Cases, below, for more information on these specific scenarios.

See Gateway Response Codes for a complete list of all possible response codes for the CardPointe Gateway and each processor.

## Getting Started

To get started, contact integrationdelivery@fiserv.com to request the following test account details:

- **UAT Merchant ID (MID)** - A UAT test MID that you will use to authenticate requests and access the CardPointe dashboard for reporting.
- **UAT API Credentials** - A set of API credentials provisioned for your UAT MID, which you will use to authenticate your API requests.
- **UAT API URL** - A UAT CardPointe Gateway API URL that you will use to test your API requests.

<!-- theme: warning -->
> Once your integration has been validated for production use, you will receive unique credentials for use in the production environment. See the [Integration Process Overview](path?=../../../../docs/documentation/IntegrationProcessOverview.md) for more information.
