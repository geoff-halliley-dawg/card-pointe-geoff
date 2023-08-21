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

