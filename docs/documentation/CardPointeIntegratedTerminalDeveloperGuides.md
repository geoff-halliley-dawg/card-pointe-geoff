<!-- type :row -->

<!-- type: card
description: The following guides provide best practices and other supplemental information for integrating the CardPointe Integrated Terminal API.
-->

<!-- type: row-end -->

# Understanding the Terminal API Application Workflow

The CardPointe Integrated Terminal API allows developers the flexibility to accommodate a wide variety of business needs and specific use cases. Regardless of the intricacies of each implementation, a Terminal API integration generally involves the following workflow:

1) Connecting a Terminal
2) Connecting your Client
3) Establishing a Session
4) Setting the Terminal's Time
5) Getting a Token and Running a Payment

<!-- theme: warning -->
> See the API Connectivity Guide for general information on connecting to the Terminal API and related services.

## Connecting a Terminal

Once a terminal is provisioned and delivered to a client, it must be connected to the client's network using either an Ethernet or Wi-Fi connection.

> If your site's network configuration includes firewall rules to restrict traffic, you should allow outbound traffic to the following IP address ranges to ensure that your terminals and software can communicate with the required servers:
>
> - 198.62.138.0/24
> - 206.201.63.0/24
> 
> See Allowing CardPointe Integrated Terminal Network Connections for more detailed information.

The terminal displays either **Connected** or **Disconnected** depending on whether it has established an internet connection and made contact with the terminal service.

Once the terminal has established a **Connected** status, it can receive requests from your application.

## Connecting your Client

The Terminal API REST Web Service base URL includes a protocol, host, port and servlet specification. For example:

`https://<site>.cardpointe.com/api/v3`

An API key is required as the HTTP authorization header in each POST request. The API key is provided by an Integrated Solutions specialist and can be used for one merchant ID (MID) or a group of MIDs.

The following example shows an API key supplied as a basic authorization header:

`Authorization: QWxhZGluOnNlc2FtIG9wZW4sjajlkHJApa=`

If the API key is missing or incorrect in the request, an HTTP Exception "401:Unauthorized" is returned to the calling application.

<!-- theme: warning -->
> It is strongly recommended that you maintain your API key on an application server, rather than in the client application, to ensure that the key value is secure. This requires that all requests and responses are managed by the server, not the client.

## Establishing a Session

In addition to connecting to the server, the terminal also connects to your point-of-sale system. This connection is referred to as a session. You establish a session between your POS system and the terminal by calling the connect endpoint.

A successful call to the connect endpoint returns a custom HTTP header, called X-CardConnect-SessionKey. The returned value includes the session key value and an expiration date and time.

For example:

`X-CardConnect-SessionKey →<key value>;expires=<date:time>.`

This session key **must** be included in the header of each Terminal API requests. Requests without a session key return the following error:

`"errorCode": 1, "errorMessage": "SessionKey header is required"`

A session key is valid for **10 minutes** after it is generated. Requests including an expired session key return the following error:

`"errorCode": 1, "errorMessage": "Session key for hsn <terminal HSN> was not valid"`

In this case, send a connect request including "force" : `"true" to forcibly` terminate the invalid session and retrieve a new session key.

## Setting the Terminal's Time

You should include the dateTime request into your terminal connection workflow to ensure that the terminal's time is accurate for transaction reporting and receipts, and to ensure that the terminal's nightly PCI reboot occurs outside of your business hours, as intended.

## Getting a Token and Running a Payment

The Terminal API provides the following methods for either tokenizing payment card data or getting a token and running a payment:

> Regardless of which method you implement, tokenization is handled by CardSecure, and the payment authorization is handled by the CardPointe Gateway.

- If you use the Terminal API readCard or readManual endpoint to get a token, you must then pass that token in an authorization request from your client application, using the CardPointe Gateway API.
- If you use the Terminal API authCard or authManual endpoint to get a token, the terminal passes the token in an authorization request to the CardPointe Gateway.

In addition to handling payment requests, the CardPointe Gateway also provides methods for voiding and refunding transactions, and for gathering reporting data. Integrate your application using the CardPointe Gateway API to take full advantage of these and other features offered by the CardPointe Gateway. For more information, see the CardPointe Gateway API documentation.

# Running the API in Postman

To help you get started with your integration, we created a sample Postman Collection that includes a template of the Terminal API service endpoints.

[Run in Postman](https://app.getpostman.com/run-collection/78bf7730d5cf55a3080f?action=collection%2Fimport#?env%5BBolt%20Terminal%20API%20UAT%20Environment%5D=W3sia2V5Ijoic2l0ZSIsInZhbHVlIjoie1VBVCBvciBwcm9kdWN0aW9uIHNpdGV9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ1cmwiLCJ2YWx1ZSI6Imh0dHBzOi8ve3tzaXRlfX0uY2FyZHBvaW50ZS5jb20vYXBpIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJBdXRob3JpemF0aW9uIiwidmFsdWUiOiJ7eW91ciBhdXRoIGtleX0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1lcmNoYW50SWQiLCJ2YWx1ZSI6Int5b3VyIG1lcmNoYW50SWR9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJoc24iLCJ2YWx1ZSI6Int5b3VyIGhzbn0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6IlgtQ2FyZENvbm5lY3QtU2Vzc2lvbktleSIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZX1d)

## Configuring Your Postman Environment

The Terminal API service templates in this collection use Environment variables to simplify your integration. Environment variables allow you to autofill select fields with pre-configured values. For example, instead of manually specifying your merchant ID in the body of each request, you can set the `{{merchantid}}` variable to your specific MID.

Once you have received a CardPointe Integrated Terminal device and API key, you can configure the following variables to auto-fill your merchant-specific data in requests to the Terminal API:

- **site** - Set this value to the UAT or production host that you received (for example, `bolt-uat`). The `{{site}}` variable is required to complete the URL.
- **url** - Set this value to the root URL that you received . The `{{url}}` variable is used to set the base url (host and port) for the REST web services.
- **Authorization** - Set this value to the API key that you received. The `{{Authorization}}` variable is used in the header of every request.
- **merchantId** - Set this value to your CardPointe merchant ID. The `{{merchantid}}` variable is used in the body of most requests.
- **hsn** - Set this value to the hardware serial number (HSN) for your terminal. The `{{HSN}}` variable is used in the body of most requests.
- **X-CardConnect-SessionKey** - Set this value to a valid session key. Session keys are returned by successful requests to the connect endpoint. The `{{X-CardConnect-SessionKey}}` variable is required in the header of some requests.

To configure environment variables, do the following in Postman:

1) Select the **Terminal API UAT Environment**, then click the eye icon to open the environment.

