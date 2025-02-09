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

<!-- align: center -->
![Bolt Terminal Postman](../../assets/images/Bolt_Terminal_Postman_Updated.png)

2) Click **Edit** to open the Manage Environments menu.

3) On the Manage Environments menu, enter your merchant-specific values for each variable, then click **Update**.

<!-- align: center -->
![Bolt Terminal Postman Environment](../../assets/images/Bolt_Terminal_Postman_Environment_Updated.png)

<!-- theme: warning -->
> See the [Postman user documentation](https://learning.postman.com/docs/introduction/overview/) for detailed information on using Postman to test APIs.

# Understanding the Terminal API Service Endpoints

Your client application makes requests to the Terminal API service endpoints to send commands to the connected terminal.

The Terminal API service endpoints can be categorized in three groups:

- Connectivity endpoints
- Payment and tokenization endpoints
- Operational endpoints

The following topics describe the use of these endpoints, and provide tips for using them in your integration.

## Using the Terminal API Connectivity Endpoints

You use the following endpoints to connect, disconnect, and pair terminals with your point-of-sale system:

> See the Terminal API Documentation for a complete description of each request and response.

| API Service Endpoint | Description |
| --- | --- |
| `dateTime` | You can use the `dateTime` endpoint to set the terminal's current local time. <br> <br> You should include the `dateTime` request into your terminal connection workflow to ensure that the terminal's time is accurate for transaction reporting and receipts, and to ensure that the terminal's nightly PCI reboot occurs outside of your business hours, as intended. |
| `listTerminals` | You can use the `listTerminals` endpoint to retrieve a list of terminals associated with a merchant ID. <br> <br> The `listTerminals` response returns the list of terminals to the client application, which can be used for pairing terminals with the integrated POS system. |
| `terminalDetails` | You can use the `terminalDetails` endpoint to retrieve detailed information about the terminals associated with a merchant ID. <br> <br> The `terminalDetails` response returns a list of terminal HSNs, terminal models, and supported features. This can be helpful for locations that include multiple different types of terminals (for example, both Ingenico and Clover terminals). |
| `preconnect` | You can use the `preconnect` endpoint to retrieve a two-factor authentication token, if two-factor authentication is configured for your merchant ID. <br> <br> Note that you should only integrate the `preconnect` endpoint when two-factor authentication is enabled for the merchant account. When two-factor authentication is enabled, it is **required**. in this case, your application must always call the `preconnect` endpoint before establishing a connection. Your application must retrieve the authentication token from the `preconnect` response and include it in the subsequent request to the `connect` endpoint. |
| `connect` | You can use the `connect` endpoint to establish a session between the terminal and your point-of-sale application. <br> <br> See Establishing a Session for more information and recommendations for using the `connect` endpoint. <br> <br> Additionally, see Sharing a Terminal Between POS Systems if you want to share a terminal amongst multiple point-of-sale systems or Merchant IDs. |
| `ping` | You can use the `ping` endpoint to verify that the terminal has an open session. <br> <br> Note that the **v1** `ping` endpoint does not require a session key, whereas the **v2** endpoint does. The v2 `ping` is useful for verifying that the terminal is connected and has a valid session, whereas the v1 `ping` is useful for only verifying the terminal's connection. |
| `disconnect` | You can use the `disconnect` endpoint to terminate the current session. |

## Using the Terminal API Payment and Tokenization Endpoints

You use the following endpoints to tokenize payment card data and run authorizations:

> See the Terminal API Documentation for a complete description of each request and response

| Endpoint | Environment | Description |
| --- | --- | --- |
| `readCard` | Card Present | This endpoint is used to get a token. <br> <br> The `readCard` endpoint requests card input (MSR, EMV, or NFC) and retrieves the encrypted data from the terminal. Encrypted data is sent to CardSecure for tokenization, and the token is returned to the client application to be passed in a subsequent authorization request to the CardPointe Gateway API. |
| `authCard` | Card Present | This endpoint is used to get a token and run a payment. <br> <br> The `authCard` endpoint requests card input (MSR, EMV, or NFC), and retrieves the encrypted data from the terminal. Encrypted data is sent to CardSecure for tokenization, and the token is returned to the terminal service, which initiates an authorization request to the CardPointe Gateway. |
| `readManual` | Card Not Present | This endpoint is used to get a token. <br> <br> The `readManual` endpoint requests manually-entered card input and retrieves the encrypted data from the terminal. Encrypted data is sent to CardSecure for tokenization, and the token is returned to the client application to be passed in a subsequent authorization request to the CardPointe Gateway API. |
| `authManual` | Card Not Present | This endpoint is used to get a token and run a payment. <br> <br> The `authManual` endpoint requests manually-entered card input, and retrieves the encrypted data from the terminal. Encrypted data is sent to CardSecure for tokenization, and the token is returned to the terminal service, which initiates an authorization request to the CardPointe Gateway. |
| `tip` | Card Present | This endpoint is used to prompt the user to select a tip amount prior to the authorization. <br> <br> The `tip` endpoint requests a tip amount, and retrieves the user's selection from the terminal. The `tip` request parameters allow you to configure up to three preset percentages and one custom amount, which allows the user to specify a dollar amount. <br> <br> Generally your software should call the `tip` endpoint to retrieve the tip amount to add to the transaction amount before submitting the authorization or tokenization request for the total amount. |

### Payment Workflow (authCard and authManual)

A call to the `authCard` or `authManual` endpoint captures payment card data and initiates an Authorization request to the CardPointe Gateway.

Generally, an `authCard` or `authManual` request initiates the following sequence:

1) The client application sends the request to the terminal service.
2) The terminal service sends a ping command to the terminal to verify the connection.
3) The terminal service sends a series of commands to the terminal, based on the options specified in the request.

    For example:

    - The terminal displays the transaction amount and prompts the user to confirm.
    - The terminal prompts the user to swipe/insert/tap or manually enter the card data.
    - The terminal prompts the user for a signature.

4) The captured data is passed in an authorization request to the CardPointe Gateway, which returns the authorization response details.
5) The authorization response text (resptext) displays on the terminal (for example, "Approved").

### Tokenization Workflow (readCard and readManual)

A call to the `readCard` or `readManual` endpoint captures payment card data and returns a token to be used in a subsequent Authorization request to the CardPointe Gateway. These endpoints **do not** initiate an Authorization or capture funds.

Generally, a `readCard` or `readManual` request initiates the following sequence:

1) The client application sends the request to the terminal service.
2) The terminal service sends a ping command to the terminal to verify the connection.
3) The terminal service sends a series of commands to the terminal, based on the options specified in the request.

    For example:

    - The terminal displays the transaction amount and prompts the user to confirm.
    - The terminal prompts the user to swipe/insert/tap or manually enter the card data.
    - The terminal prompts the user for a signature.

4) The terminal service returns the tokenized card number, signature, and any additional data to the client application.
5) The client application can then pass the token and cardholder data in an authorization request to the CardPointe Gateway to capture the funds for the transaction.

## Using the Terminal API Operational Endpoints

You use the following endpoints to capture information for use by your point-of-sale system:

> See the Terminal API Documentation for a complete description of each request and response.

| API Service Endpoint | Description |
| --- | --- |
| `readInput` | You can use the `readInput` endpoint to capture customer information (for example, an email address or loyalty account ID). <br> <br> See Using readInput to Capture Customer Information for more information. |
| `readSignature` | You can use the `readSignature` endpoint to capture a customer's signature. <br> <br> See Capturing and Handling Cardholder Signatures for more information. |
| `readConfirmation` | You can use the `readConfirmation` endpoint to prompt the customer to confirm the purchase amount. |
| `display` | You can use the `display` endpoint to display text on the terminal's screen. |
| `clearDisplay` | You can use the `clearDisplay` endpoint to clear the terminal's screen and return to the idle display. |
| `cancel` | You can use the `cancel` endpoint to cancel an in-flight command on the terminal. |
| `printReceipt` | On Clover terminals, you can use the `printReceipt` endpoint to reprint receipts for past transactions. See the CardPointe Integrated Terminal Developer Guide for Clover Terminals for more information. |

### Using readInput to Capture Customer Information

Calling the Terminal API `readInput` endpoint sends a command to the terminal to prompt the user to enter information. The user follows the prompt and enters information (for example, a phone number), and the information is returned to the calling application.

You might use this endpoint to gather customer contact information to store in a profile, allow the customer to enter a phone number to look up a loyalty account, or request some other information required for your business.

A request to the `readInput` endpoint requires the `format` parameter, which is used to specify the type of user input requested from the terminal.

The following table describes the supported `format` values and how each can be used:

<!-- theme: danger -->
> The following values are case-sensitive. For example, you must enter "PHONE" not "phone" or "Phone." entering an incorrect value returns the error `"errorMessage": "Invalid Format"`.

| Value | Description | Use | Examples | 
| --- | --- | --- | --- |
| `PHONE` | Phone Number Entry | Use the `PHONE` format to prompt the terminal to receive a phone number input. <br> <br> **Workflow example:** <br> <br> 1. The application sends a `readInput` request to the terminal with `"format" : "PHONE"`. <br> 2. The terminal displays a phone number input field including dashes (-). <br> 3. The user enters a phone number using the keypad. <br> 4. The response returns the phone number, including dashes to the calling application. | request: <br> <br> `"format" : "PHONE"` <br> <br> response: <br> <br> `"input" : "1-234-567-8901"` |
| `amount` | Amount Entry | 	Use the `AMOUNT` format to prompt the terminal to receive an amount input. <br> <br> **Workflow example:** <br> <br> 1. The application sends a `readInput` request to the terminal with `"format" : "AMOUNT"`. <br> 2. The terminal displays an input field with a dollar sign ($) and decimal. <br> 3. The user enters a dollar amount, using the keypad. <br> 4. The amount is returned to the calling application. <br> <br> Note that the the returned value is numeric only, with the decimal and dollar sign implied. For example, if the user enters "$123.45" at the terminal, the response returns `12345`. | request: <br> <br> `"format" : "AMOUNT"` <br> <br> response: <br> <br> `"input" : "12345"` |
| `MMYY` | Month/Year Date Entry | Use the `MMYY` format to prompt the terminal to receive a month/year date input. <br> <br> **Workflow example:** <br> <br> 1. The application sends a `readInput` request to the terminal with `"format" : "MMYY"`. <br> 2. The terminal displays an input field with 4 digits separated by a forward slash (/). <br> 3. The user enters the 2-digit month and 2-digit year, using the keypad. <br> 4. The response returns the 4-digit month and year string to the calling application in the format MMYY. <br> <br> For example, if the user enters "11/18" at the terminal, the response returns `1118`. | request: <br> <br> `"format" : "MMYY"` <br> <br> response: <br> <br> `"input" : "1222"` |
| `Nx[,y]` | Numeric Entry | Use the `Nx[,y]` format to prompt the terminal to receive a numeric string input. <br> <br> `N` specifies a numeric entry where: <br> <br> - `x` = the minimum number of digits. <br> - `y` = the maximum number of digits, up to 32. <br> <br> `y` is optional, and if `y` is not specified, then the input must match the length specified by `x`. <br> <br> For example: <br> <br> - If you specify `N5`, then the user input must be a 5-digit number, such as "19406." <br> - If you specify `N5,9`, then the user input can be a number ranging from 5 to 9 digits, such as "08081" or "080819876." <br> <br> **Workflow example:** <br> <br> 1) The application sends a `readInput` request to the terminal with `"format" : "N5"` to capture the cardholder's zip code for demographic purposes. <br> 2) The terminal displays a 5-digit input field. <br> 3) The user enters the 5-digit zip code, using the keypad. <br> 4) The response returns the 5-digit string to the calling application. | request: <br> <br> `"format" : "N5"` <br> <br> response: <br> <br> `"input" : "19406"` <br> <br> request: <br> <br> `"format" : "N5,9"` <br> <br> response: <br> <br> `"input" : "194062848"` |
| `ANx[,y]` | Alpha-Numeric Entry | Use the `ANx[,y]` format to prompt the terminal to receive an alphanumeric string input. <br> <br> `AN` specifies an alphanumeric entry where: <br> <br> - `x` = the minimum number of characters. <br> - `y` = the maximum number of characters, up to 32. <br> <br> `y` is optional, and if `y` is not specified, then the input must match the length specified by `x`. <br> <br> For example: <br> <br> - If you specify `AN8`, then the user input must be an 8-character entry, such as "A1B2C3D4." <br> - If you specify `AN1,32`, then the user input can be an entry ranging from 1 to 32 characters, such as "A" or "A123." <br> <br> **Workflow example:** <br> <br> 1. The application sends a `readInput` request to the terminal with `"format" : "AN1,20"` to capture the cardholder's driver's license number. <br> 2. The terminal displays an alphanumeric input field. <br> 3. The user enters the driver's license state and number, using the keypad. <br> 4. The response returns the alphanumeric string to the calling application. | request: <br> <br> `"format" : "AN8"` <br> <br> response: <br> <br> `"input" : "A1B2C3D4"` <br> <br> request: <br> <br> `"format" : "AN1,20"` <br> <br> response: <br> <br> `"input" : "PA22639348"` |

# Configuring Custom Themes and Logos

CardPointe Integrated Terminal devices support the ability to display a custom image, such as your merchant’s or software’s logo, and to modify the color of the header, footer, and font that displays within the user interface. This allows you to match the style of the terminal interface with your branding to create a more seamless experience for the end user.

> In addition to custom images, Clover terminals support custom background and font colors. See the CardPointe Integrated Terminal Developer Guide for Clover Terminals for more information.

## Wallpaper Image Requirements

Ensure that your image meets the following requirements:

<!-- theme: warning -->
> If you are upgrading an existing Ingenico terminal to a new Lane series device, and you use a custom wallpaper or logo images, you may need to test and adjust your images to display properly on the new device.

| Device	| Max Size	| Dimensions (X) x (Y)	| Format |
| --- | --- | --- | --- |
| Clover Flex	| 1MB	| 1720 x 1280 pixels	| JPEG or PNG |
| Clover Mini	| 1MB	| 1280 x 800 pixels	| JPEG or PNG |
| Ingenico iPP350	| 1MB	| 320 x 240 pixels | JPEG or PNG |
| Ingenico iPP320	| 1MB	| 128 x 64 pixels	| JPEG or PNG |
| Ingenico iSC Touch 250	| 1MB	| 480 x 272 pixels | JPEG or PNG |
| Ingenico iSMP4 | 1MB | 320 x 240 pixels | JPEG or PNG |
| Ingenico Lane/3000	| 1MB	| 320 x 240 pixels	| JPEG or PNG |
| Ingenico Lane/5000	| 1MB	| 480 x 320 pixels	| JPEG or PNG |
| Ingenico Lane/7000	| 1MB	| 800 x 480 pixels	| JPEG or PNG |
| Ingenico Lane/8000	| 1MB	| 800 x 480 pixels	| JPEG or PNG |

## Theme Requirements

| Attribute | Format |
| --- | --- |
| Banner/Footer Color | Hex to RGB |
| Banner/Footer Font Color | Hex to RGB |

## Customizing the Terminal Beep Settings

To experiment with the volume, pitch, and duration of the beep heard when using the device, you can temporarily adjust these settings from the Beep menu. The terminal beeps when prompting the user to tap/insert/swipe a card, and when prompting to remove. 

<!-- type: row -->
<!-- type: card
description: This feature is for testing purposes only. To permanently change your device's beep settings, note your desired settings and contact CardPointe Support.
-->

<!-- type row-end -->

1) Access the Admin Menu:
    - Press **F** and enter the default password of **CCMerchant**.
    - Press **O** (green button) to confirm the password.
2) Press **O** (green button) to select **Settings**.
3) Press the **down arrow** to scroll down and press **O** (green button) to select **Beep**.
4) Use the number pad to enter a value for **Frequency (Hz)** and press the **down arrow** to scroll down and enter values for **Volume**, **Time On (ms)**, and **Time Off (ms)**. Reference the table below for the default values and acceptable ranges.

| Parameter Name | Default | Range | Description |
| --- | --- | --- | --- |
| Frequency (Hz) | 1,000 | 20-20,000 | The frequency of the beep in Hz or vibrations per second. Controls the pitch of the beep. |
| Volume | 100 | 1-100 | Controls the volume of the beep, as a percentage. <br> <br> Low – 1-30 <br> Medium – 31-70 <br> High – 71-100 |
| Time On (ms) | 250 | 10-5,000 | Controls the length of each single beep. |
| Time Off (ms) | 250 | 10-5,000 | Controls the length of time between each beep. |

5) Press **O** (green button) when finished.
6) The terminal displays **Beep parameters have been updated** and beeps several times to confirm the new settings.

# Sharing a Terminal Between POS Systems

This guide provides information for sharing a terminal among two or more integrated point-of-sale (POS) systems using more than one merchant ID (MID).

## Overview

CardPointe Integrated Terminal devices are identified by a unique hardware serial number (HSN), which can be found on a label on the terminal, or on the terminal's display. When a terminal is provisioned, the HSN is assigned to a parent merchant ID (MID), which is used to fulfill the order and maintain a chain of custody for the terminal.

All Terminal API requests require a MID. Typically, the integrated POS software uses the MID assigned to the terminal to tokenize credit card numbers and send authorization requests.

Tokens are site-specific; however, a token can be used by any MID on the same site. Therefore, you can use the MID assigned to the terminal to tokenize a credit card using a readCard or readManual request, and then use another MID on your site to use that token to make an authorization request to the CardPointe Gateway.

The following topics describe this workflow in greater detail, including required parameters and best practices.

## Connecting to the Terminal

First, one of the integrated POS systems must connect to the terminal, as described in Connecting a Terminal. 

<!-- theme: danger -->
> When sharing a terminal, do **not** include `"force" : "true"` in the request. Doing so will terminate active (or in-flight) transactions.

#### Example Connect Request

```json
Content-Type: application/json
Authorization: ZCb8rPkZcZDVO2CIngLsFrBJgA/BYyUzIHT2zaj3NPg=

{
  "merchantId" : "1234",
  "hsn" : "99999",
  "force" : "false"
}
```

A successful response header includes a session key in the format <key value>;<expiration>. This session key value must be provided as a session key/value pair in the header of every subsequent request in the format `X-CardConnect-SessionKey: <key value>`.

#### Example Connect Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
X-Application-Context →bolt:10200
X-CardConnect-SessionKey →e3072040eb08454dac18ff67cd6c311c;expires=2019-04-22T23:49:55.684Z
```

When multiple POS systems are sharing a terminal, and `"force" : "false"`, a POS system will receive the following error response if it attempts to send a command to a terminal that is already in use:

#### Example Terminal In Use Error

```json
{
  "errorCode": 7,
  "errorMessage": "Terminal <HSN> is already in use"
}
```

<!-- theme: warning -->
> You can handle this error by displaying a message on your POS system such as "Terminal in use. Please retry in a few seconds."

## Getting a Token

Once your POS system is connected and has a valid session key, it can begin to make tokenization requests by calling the readCard endpoint for card present transactions, or readManual endpoint for card not present transactions.

These requests prompt the terminal's user to swipe/insert/tap or manually enter the payment card information.

#### Example readCard Request

```json
Content-Type: application/json
Authorization: ZCb8rPkZcZDVO2CIngLsFrBJgA/BYyUzIHT2zaj3NPg=
X-CardConnect-SessionKey: e3072040eb08454dac18ff67cd6c311c
{
  "merchantId" : "1234",
  "hsn" : "99999",
  "amount" : "25.00",  
  "includeSignature" : "false",  
  "includeAmountDisplay" : "true",
  "confirmAmount" : "true",
  "aid" : "credit"
}
```

The customer inserts the payment card at the terminal, and the Terminal API returns a token in a successful response to your POS system.

#### Example readCard Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "token" : "9445123546981111",
  "expiry" : "1220",
  "name" : "John Doe",
  "singleUseToken" : "false"
}
```

> This token is **not** directly associated with the MID used in the readCard request; therefore another MID on the same site can use this token in an authorization request to the CardPointe Gateway.

## Authorizing the Payment Using a Different MID

Once your POS system has retrieved the token, you can use the token in a request to the CardPointe Gateway API's auth endpoint.

The authorization request requires a MID; however, this MID can be any MID that shares a site with the MID that obtained the token. In this case an integrated POS system associated with another MID on the same site creates an authorization request.

#### Example auth Request using a Token

```json
PUT /cardconnect/rest/auth HTTP/1.1
Authorization: Basic aFZvgDulPzp2YXM0aK6nMAIq
Content-Type: application/json

{
  "merchid": "5678",  
  "account": "9445123546981111",
  "expiry": "1220",
  "amount": "25.00",
  "currency": "USD",
  "name": "John Doe",
  "capture": "Y"
}
```

> This transaction flow included two MIDs: one to obtain a token from the terminal, and one to submit an authorization request to the CardPointe Gateway. For reporting purposes, the MID that made the authorization request is the MID that is associated with the transaction.

# Accepting PIN Debit Cards

This guide provides information for accepting PIN debit payments.

## Merchant Requirements

To accept PIN debit transactions, your merchant account must be boarded to the **First Data Rapid Connect** processing platform.

Merchants already accepting PIN debit on the First Data North platform continue to be supported; however new or existing merchants who want to include support for PIN debit must be boarded to the Rapid Connect platform.

Contact integrationdelivery@fiserv.com to confirm your merchants' compatibility.

## Integration Requirements

- Your integration must include Ingenico terminals provisioned with the required key for processing PIN debit transactions. PIN debit is currently **not** supported for Clover terminals.
- Your application must be updated to include the following fields in the `authCard` and `readCard` requests for PIN debit transactions:
    - `"aid" : "debit"` - This allows the terminal to accept the debit application identifier (AID), when available on the customer's card.
    - `"includePIN" : "true"` - This prompts for PIN input on the terminal.
 
## Understanding EMV (Chip) and MSR (Swipe) Debit Card Handling

The parameters that your application must pass in the `readCard` or `authCard` request depend on the type of card presented in the transaction.

EMV (chip) cards are programmed with an application identifier (AID) that determines the card type. To process an EMV card as PIN debit, the card must be programmed with a debit AID, and your application must include `"aid"` : `"debit"` in the `authCard` or `readCard` request. Additionally, EMV cards are programmed with a cardholder verification method (CVM) specification, which determines if the transaction requires the cardholder's signature or PIN.

Cards that do not support debit transactions (credit-only cards) may also support PIN entry as a CVM. Including `"aid" : "debit"` in your request will ensure the cardholder is prompted for a PIN, whenever the credit or debit card supports it. However, the AID embedded in the EMV data of a credit-only card will ensure the transaction is ultimately processed as credit.

<!-- theme: danger -->
> If the card does not have a debit AID, or if you do not include the aid parameter in your request, the card is processed as credit.

Non-EMV (magstripe or MSR) cards also fall into different categories, as specified in the card's track data. For MSR debit transactions, your application **must** specify `"includePin" : "true"` in the request to prompt the cardholder for their PIN.

The following examples illustrate authCard and readCard requests including the PIN debit parameters: 

#### authCard Request Example (PIN Debit)

```json
Content-Type: application/json
X-CardConnect-SessionKey: e3072040eb049990aahgk39937cd6c311c

{
  "merchantId" : "1234",
  "hsn" : "15117SC80536933",
  "amount" : "100",
  "includeSignature" : "false",
  "includeAmountDisplay" : "false",
  "printReceipt" : "true",  
  "confirmAmount" : "true",    
  "includeAVS" : "false",
  "printReceipt" : "true",  
  "beep" : "true",
  "aid" : "debit",
  "includePIN" : "true",
  "capture" : "true",
  "orderId" : "1233200400",
}
```

#### readCard Request Example (PIN Debit)

```json
Content-Type: application/json
X-CardConnect-SessionKey: e3072040eb049990aahgk39937cd6c311c

{ 
  "merchantId" : "1234",
  "hsn" : "15117SC80536933",
  "amount" : "100",
  "aid" : "debit",
  "includePIN : "true",
  "includeSignature" : "false",  
  "includeAmountDisplay" : "true",
  "confirmAmount" : "true",
  "beep" : "true"    
}
```

## Handling PIN Prompts

You can use the readConfirmation request to prompt the customer to specify whether they intend to pay with a credit or debit card prior to initiating the readCard or authCard sequence. This can be useful to create a conditional application sequence in which the user's credit or debit selection determines the values that your application passes in the `aid` and `includePIN` parameters in the subsequent `readCard` or `authCard` request.

A call to the `readConfirmation` endpoint prompts the user to acknowledge or reply to a message on the terminal. 

You can use the `prompt` parameter to ask the user a question (for example, "Is this debit?"). If the customer selects yes at the terminal, the `readConfirmation` response includes `"confirmed" : "true"`. Your application can then call the `authCard` endpoint with `"aid" : "debit"` and `"includePIN" : "true"`.

The following examples illustrate the `readCard` request and response in this scenario:

#### readConfirmation Request Example (Pin Debit Prompt)

```json
Content-Type: application/json
X-CardConnect-SessionKey: e3072040eb049990aahgk39937cd6c311c

{ 
  "merchantId" : "1234", 
  "hsn" : "99999", 
  "prompt" : "Is this debit?",
  "beep" : true
}
```

#### readConfirmation Response Example

```json
HTTP/1.1 200 OK
Content-Type: application/json

{ "confirmed" : true }
```

# Accepting Closed Loop Gift Cards

The CardPointe platform allows merchants the flexibility to accept their own proprietary, closed loop gift cards for customer purchases. If you want to include closed loop gift cards in your accepted payment methods, see Tokenizing Closed Loop Gift Cards in the CardSecure Developer Guides for more information.

# Handling Timeouts

The following topics provide information for understanding and handling transaction timeouts.

## Understanding Timeouts

The payment flow includes requests to and responses from both the Terminal API and the CardPointe Gateway API. Both of these APIs support synchronous communication; therefore, your application must make requests and expect responses in sync with the terminal service and CardPointe Gateway service.

As a developer integrating these solutions with your point-of-sale software, it is important for you to understand how long the steps of the payment process can take so your software can interact accordingly. 

The following topics provide information to help you understand the possible timeout scenarios and how the terminal service handles each situation.

### Terminal Request Timeout (2 Minutes)

Most interactions between the terminal and your application require user input of some kind. Users might be prompted to swipe or insert a card, manually enter payment card information, or provide a signature or phone number. Because these interactions can take time to complete, the terminal service waits 1 minute and 59 seconds for a response after a request is made.

If the terminal service does not receive a response from the terminal within 2 minutes, the request times out and the Terminal API returns an HTTP 500 Error Code 1 "Terminal request timed out." Your application must resubmit the request.

### Terminal API Authorization Request Timeout (2 Minutes + 32 Seconds)

When you use the Terminal API authCard or authManual requests to authorize a payment, you must consider the individual timeout thresholds for both the Terminal API and the CardPointe Gateway API requests and responses. The Terminal API authorization requests combine the time needed for a terminal request (2 minutes) and the time it takes to handle the authorization request and response through the CardPointe Gateway (32 seconds).

After the request is sent, the terminal collects payment data from the user and sends it to the terminal service. The terminal service then makes an authorization request to the CardPointe Gateway.

The CardPointe Gateway sends the authorization request to the payment processing network and allows 31 seconds for a response. If the CardPointe Gateway does not receive a response within this limit, the request times out at 32 seconds and returns a "Timed Out" response to the Terminal API, which is included in the response. 

In some cases (for example, a network error), the terminal service might not receive a response from the CardPointe Gateway. In these cases, the terminal service automatically initiates a timeout reversal sequence to ensure that the transaction is voided. 

See Terminal API Timeout Handling, later in this guide, for more information.

### CardPointe Gateway Authorization Timeout (32 Seconds)

When you use the Terminal API readCard or readManual requests to tokenize card data for use in a CardPointe Gateway authorization request, you only need to consider the CardPoint Gateway timeout threshold. The CardPointe Gateway sends the request to the payment processing network and allows 31 seconds for a response. If the CardPointe Gateway does not receive a response within this limit, the request times out at 32 seconds and returns a "Timed Out" response. 

See the Handling Timed-Out Transactions in the CardPointe Gateway Developer Guides for information on handling Gateway-only time outs.

## Terminal API Timeout Handling

If your application uses authCard or authManual requests to authorize payments, you should allow it to handle the following scenarios:

- The application receives a "Timed out" response from the CardPointe Gateway or terminal service.
- The application receives no response.

### Gateway Timed Out Response Returned

In this case, a response, including a retrieval reference number (`retref`) for the transaction, is returned. You can use the `retref` to make an inquire request to the CardPointe Gateway to retrieve details about the authorization attempt.

#### Gateway "Timed Out" Response Example

```json
Status: 200 OK
 
{
    "amount":"685.00",
    "resptext":"Timed out",
    "setlstat":"Declined",
    "acctid":"1",
    "respcode":"62",
    "merchid":"123456789012",
    "token":"9441282699177251",
    "respproc":"PPS",
    "name":"Jane Doe",
    "currency":"USD",
    "retref":"343005123105",
    "respstat":"B",
    "account":"9419786452781111"
}
```

The response includes a response status (`repstat`) of "B," which always means "Retry." Your application should resubmit the authorization request.

<!-- theme: warning -->
> In some cases, retrying the authorization will not succeed. In the event of multiple retry failures, visit status.cardconnect.com to see if there are system-wide issues.

### No Response Returned

In this case, your application cannot determine whether or not the transaction was successful.

When the terminal service does not receive a response from the CardPointe Gateway, the terminal service uses the order ID included in the request to handle the timed out transaction. The terminal generates a unique order ID for every transaction in the format <HSN-timestamp>. Alternatively, you can specify a unique order ID in your authCard and authManual requests.

<!-- theme: danger -->
> If you include an order ID in your authCard or authManual requests you must ensure that you provide a unique value. Using duplicate order IDs can lead to the wrong transaction being voided in the event of a timeout.

The terminal service makes an inquireByOrderid request to the CardPointe Gateway to check the status of the transaction. If the CardPointe Gateway returns a response, the terminal service forwards the response to your application.

If the terminal service does not receive a response to the inquireByOrderid request, it makes three voidByOrderId requests to void the transaction and ensure that funds were not captured. If at least half of the CardPointe Gateway timeout threshold remains, the terminal service retries the authCard or authManual request. Otherwise, your application must resend the request. 

# Capturing and Handling Cardholder Signatures

Some terminals include the ability to capture a cardholder signature. This guide provides recommendations and integration details for capturing and handling signatures, if necessary for your business.

## Signature Rules and Requirements

This topic provides general best practices and integration details for capturing cardholder signature data; however, each card brand provides specific rules and requirements. You should understand and follow the signature guidelines for the card brands that you accept.

Consult the following card brand guidelines for detailed information:

- **MasterCard**: https://www.mastercard.us/content/dam/public/mastercardcom/na/global-site/documents/transaction-processing-rules.pdf
- **Visa**: https://usa.visa.com/dam/VCOM/download/about-visa/visa-rules-public.pdf

Additionally, signature requirements vary depending on the card type. For example, EMV (chip and contactless) cards do not require a signature. Ensure that you understand the requirements for accepting both EMV and MSR (magnetic-stripe) cards as determined by the card brands.

> As of April 2018, the major card brands no longer require merchants to capture cardholder signatures. 

## Cardholder Signature Guidelines

As mentioned earlier in this guide, specific rules and requirements vary by card brand and by card type. Refer to each card brand's guidance for more information.

While most transactions do not require you to capture a signature, there are some cases in which you might want to to protect your business.

Consider the following best practices:

- For EMV (chip and contactless) card transactions, no signature is required; however, you might want to capture signatures for large or high-risk transactions.
- For MSR (magnetic-stripe) card transactions, you should capture a signature for amounts over $50.

If you only want to capture signatures for MSR transactions, see Detecting MSR Transactions below for information on determining when a transaction has been processed as MSR.

## Capturing Cardholder Signatures

Capturing signatures requires a direct integration to the CardPointe Gateway API. While you can prompt for and capture a signature during the transaction, using the Terminal API, you must call the CardPointe Gateway API's signatureCapture endpoint to attach the signature to the authorization record.

### Capturing a Signature During a Transaction

To run an authorization and capture a signature at the same time, do the following:

1) Call the Terminal API authCard or authManual endpoint and include the **includeSignature = true** parameter.

   The signature prompt and capture are integrated into the command sequence that the Terminal API sends to the terminal, and the authorization response and signature data are returned to your software.
   
2) Call the CardPointe Gateway API's signatureCapture endpoint, including the `retref` and signature returned in the authorization response, to add the signature to the transaction record.

### Capturing a Signature After a Transaction

To run an authorization and capture a signature later, do the following

1) Call the Terminal API authCard or authManual endpoint and include the **includeSignature = false** parameter.

    The authorization response is returned to your software.
   
3) Call the Terminal API readSignature endpoint to prompt for and capture the cardholder's signature.

    The signature data is returned to your software.
   
4) Call the CardPointe Gateway API signatureCapture endpoint, including the `retref` and signature returned in the authorization response, to add the signature to the transaction record.

## Detecting MSR Transactions

If you only want to capture a signature for transactions using MSR (magnetic-stripe) cards, you can use the data received in the authorization response to steer the behavior of your software.

For example, do the following:

1) Call the Terminal API authCard endpoint and include the **includeSignature = false** parameter.

    The authorization response is returned to your software. If the authorization response does not include an EMV tag data array, or if **entrymode = swipe**, then the transaction was processed as MSR.
   
2) If you want to capture a signature for the transaction, do the following:

    - Call the Terminal API readSignature endpoint to prompt for and capture the cardholder's signature.

        The signature data is returned to your software.
  
    - Call the CardPointe Gateway API signatureCapture endpoint, including the retref returned in the authCard response and the signature returned in the readSignature response to add the signature to the transaction record.
  
# Printing Receipts Using Authorization Data

This guide provides information for integrators who want to use authorization response data to print receipts from an integrated POS printer.

<!-- theme: warning -->
> For information on printing receipts from a Clover terminal, see the CardPointe Integrated Terminal Developer Guide for Clover Terminals.

## Receipt Rules and Requirements

This topic provides general best practices and integration details for printing receipts and capturing cardholder signature data; however, each card brand provides specific rules and requirements. You should understand and follow the receipt guidelines for the card brands that you accept.

Consult the following card brand guidelines for detailed information:

- **MasterCard**: https://www.mastercard.us/content/dam/public/mastercardcom/na/global-site/documents/transaction-processing-rules.pdf
- **Visa**: https://usa.visa.com/dam/VCOM/download/about-visa/visa-rules-public.pdf

Additionally, receipt requirements vary depending on the card type. For example, receipts generated for EMV (chip and contactless) card transactions must include specific EMV tag data returned in the authorization response. Ensure that you understand the requirements for accepting both EMV and MSR (magnetic-stripe) cards as determined by the card brands.

## Understanding Receipt Data

When an authorization is successfully approved and processed by the CardPointe Gateway, the authorization response payload includes important transaction details that you can capture and print on a receipt.

In general, a receipt must include:

- transaction details from the authorization response
- merchant account information and additional transaction details returned in the receipt object
- EMV tag data returned in the EMV tag object, if the card used was an EMV (chip or contactless) card.

## Authorization Response Data

A successful authorization response includes the following fields. You should include the highlighted fields on your receipts.

| Field | Content | Max Length | Comments |
| --- | --- | --- | --- |
| respstat | Status | 1 | Indicates the status of the authorization request. Can be one of the following values: <br> <br> **A** - Approved <br> **B** - Retry <br> **C** - Declined |
| retref | Retrieval reference number | 12 | CardPointe retrieval reference number from authorization response |
| account | Account number | 19 | Copied from the authorization request, masked except for the last four digits. |
| token (if requested) | Token | 19 | A token that replaces the card number in capture and settlement requests if requested |
| amount | Amount | 12 | Authorized amount. Same as the request amount for most approvals. <br> The amount remaining on the card for prepaid/gift cards if partial authorization is enabled. <br> Not relevant for declines. |
| batchid | Batch ID | 12 | Automatically created and assigned unless otherwise specified. Returned for a successful authorization with capture. |
| orderid | Order ID | 19 | Order ID copied from the authorization request. |
| merchid | Merchant ID | 12 | Copied from the authorization request. <br> **Note**: _If you include the merchant ID on a receipt, mask this value, except the last four digits_. |
| respcode | Response code | - | Alpha-numeric response code that represents the description of the response |
| resptext | Response text | - | Text description of response |
| respproc | Response processor | 4 | Abbreviation that represents the platform and the processor for the transaction |
| bintype | Type of BIN | 16 | **Possible Values**: <br> <br> Corp <br> FSA+Prepaid <br> GSA+Purchase <br> Prepaid <br> Prepaid+Corp <br> Prepaid+Purchase <br> Purchase |
| entrymode | POS Entry Mode | 25 | Only returned for merchants using the First Data North and RapidConnect front end platforms. <br> **Possible Values**: <br> <br> Keyed <br> Moto <br> ECommerce <br> Recurring <br> Swipe(Non EMV) <br> DigitalWallet <br> EMVContact <br> Contactless <br> Fallback to Swipe <br> Fallback to Keyed |
| avsresp | AVS response code | 2 | Alpha-numeric AVS response. |
| cvvresp | CVV response code | 1 | Alpha-numeric CVV response. |
| authcode | Authorization code | 6 | Authorization Code from the Issuer |
| signature | Signature Bitmap | 6144 | JSON escaped, Base64 encoded, Gzipped, BMP file representing the cardholder's signature. Returned if the authorization used a token that had associated signature data or track data with embedded signature data. <br> <br> If you are integrating a custom receipt solution, you can convert this image file and print it to the receipt, if required. |
| commcard | Commercial card flag | 1 | **Y** if a Corporate or Purchase Card |
| emv | Cryptogram | - | Authorization Response Cryptogram (ARPC). This is returned only when EMV data is present within the Track Parameter. |
| emvTagData | EMV tag data | 2000 | A string of receipt and EMV tag data (when applicable) returned from the processor. <br> <br> This data returned should be presented on a receipt if applicable, and recorded with the transaction details for future reference. <br> <br> Refer to EMV Tag Data below for a list of the possible fields returned. |
| receipt | receipt data | - | An object that includes additional fields to be printed on a receipt. <br> <br> Refer to Receipt Data below for a list of the fields returned. |

## EMV Tag Data

If the card used in the authorization request was an EMV (chip or contactless) card, then the response data includes an **emvTagData** object with the following fields:

| Name | Tag | Details | Source | Format | Max Length |
| --- | --- | --- | --- | --- | --- |
| TVR (Terminal Verification Results) | 95 | Status of the different functions as seen from the terminal | Terminal | Binary | 5 |
| ARC (Authorization Response Code) | 8A | Indicates the transaction disposition of the transaction received from the issuer for online authorizations.	| Issuer/Terminal | String | 2 |
| PIN (CVM Results) | 9F34 | Indicates the results of the last CVM performed. If PIN was entered, returns "Verified by PIN"	| Terminal | String | 15 |
| Signature (CVM Results) | 9F34 | Indicates the results of the last CVM performed. If "true" then CVM supports signature and signature line may be applicable. However, card brands have moved away from requiring signature for EMV transactions. | Terminal | Boolean | 5 |
| Mode | - | Identifies the mode used to authorize (or decline) the transaction. Always "Issuer" | CardPointe Gateway | String | 6 | 
| TSI (Transaction Status Information) | 9B | Indicates the functions performed in a transaction | Terminal | Binary | 2 |
| Application Preferred Name | 9F12 | Preferred mnemonic associated with the AID. If unavailable, use Application Label. | Card | String | 16 |
| AID (Application Identifier, Terminal) | 9F06 | Identifies the application as described in ISO/IEC 7816-5	| Terminal | Binary | 16 |
| IAD (Issuer Application Data) | 9F10 | Contains proprietary application data for transmission to the issuer in an online transaction.	| Card | Binary | 32 |
| Entry method | - | Indicator identifying how the card information was obtained.	| Terminal | String | 26 |
| Application Label | 50 | Mnemonic associated with the AID according to ISO/IEC 7816-5. If unavailable, use the Application Preferred Name. | Card | String (with the special character limited to space) | 16 | 

## Receipt Data

The receipt object, included in the authorization response, provides merchant account information. The merchant account information is populated using the merchant properties configured for the MID.

Additionally, this object includes additional transaction details from the authorization response. You can optionally include a custom order note (orderNote) and item details (items), by including a userFields object in the authorization request.

You can specify the following fields in a userFields object to include an order note or item details, or to override the merchant properties:

| Field | Description |
| --- | --- |
| receiptOrderNote	| Use this field to provide a custom note to include on the receipt. |
| receiptItems	| Use this field to provide custom item descriptors to include on the receipt. |
| receiptHeader	| Use this field to override the header configured for your MID. |
| receiptFooter	| Use this field to override the footer configured for your MID. |
| receiptDba	| Use this field to override the DBA name configured for your MID. |
| receiptPhone	| Use this field to override the phone number configured for your MID. |
| receiptAddress1 |	Use this field to override the address (line 1) configured for your MID. |
| receiptAddress2	| Use this field to override the address (line 1) configured for your MID. |

Each value can be any string and the total length of user defined fields (URL/JSON-encoded) is limited to 4000 bytes. See the userFields description in the CardPointe Gateway API documentation for more information.

A successful authorization response includes a receipt object with the following fields:

| Field | Format | Description |
| --- | --- | --- |
| header | AN |	A customizable field to display an alphanumeric message. For example, a specific terms, disclosure, or cardholder agreement statement. |
| footer | AN | A customizable field to display an alphanumeric message. For example, a specific terms, disclosure, or cardholder agreement statement. |
| dba |	AN | The merchant's Doing Business As (DBA) name. |
| address1 | AN |	Line 1 of the merchant's address. |
| address2 | AN |	Line 2 of the merchant's address. |
| phone |	N	| The merchant's phone number. |
| dateTime | N | The date and time of the transaction (YYYYMMDDHHMMSS). |
| nameOnCard | A | The Cardholder's name, if included in the authorization request. |
| orderNote |	AN | A custom order note, if included in the userFields in the authorization request. |
| items |	AN | A custom item descriptor, if included in the userFields in the authorization request. |

### Printing a Receipt

To print a receipt from your custom integration, use the fields described in Understanding Receipt Data to build your receipt template.

The following example illustrates a receipt template (left) and a receipt populated with data retrieved from the authorization response (right).

<!-- align: center -->
![Clover Mini Receipt Sample](../../assets/images/Clover_Mini_Receipt_Sample.png)
