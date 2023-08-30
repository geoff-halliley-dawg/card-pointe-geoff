<!-- type: tab -->

<!-- type: card
description: This solution is currently in development and the information in this guide is subject to change.
-->

<!-- type: row-end -->

# Overview

The CardPointe Unattended solution extends CardPointe Integrated Payments to unattended kiosk applications.

The CardPointe Unattended solution includes the following components:

- **Unattended card reader device** - The unattended device accepts EMV (chip), NFC (contactless), and MSR (magnetic stripe) payments, and connects via serial/USB to the host system and your client application.
- **Payment Application Engine** - The Payment Application Engine (PAE) provides the interface between your client application and the unattended device. Your application sends commands to the device to initiate the card reader, and securely capture the card data.
- **Web Payment Application** - The web payment application retrieves the encrypted card data from the device and initiates an authorization request to the CardPointe Gateway, via HTTP/Ethernet.
- **App Loader** - The app loader runs on the unattended device to check for and automatically download and install updates to the firmware, PAE, and SDK installed on the device.
- **CardPointe Gateway API** - The CardPointe Gateway API provides methods for managing and reporting on your transactions. Your application uses the CardPointe Gateway API to void, refund, or check the status of a transaction.

> Similarly to the CardPointe Integrated Terminal solution, your application **must** integrate the CardPointe Gateway API to manage transactions and perform other business functions. See the CardPointe Gateway API documentation for detailed information on integrating the CardPointe Gateway API.

## Features

The CardPointe Unattended solution offers a simple integration for retrieving encrypted payment card data from an unattended kiosk, tokenizing that data, and using the token to process a payment or refund on the CardPointe Gateway.

If you are currently integrating the CardPointe Integrated Terminal API for an attended payment solution, the CardPointe Unattended payment process is similar to a simplified authCard process.

For additional transaction management and reporting features, your application **must** use the CardPointe Gateway API to communicate directly with the CardPointe Gateway. See the CardPointe Gateway API for detailed information.

See Understanding Payment Flows, later in this guide for more information on making payment requests.

## Requirements

The unattended device requires the following connections:

- A Serial/USB connection to your application.
- An Ethernet connection to validate the device's configuration and to send payment requests to the CardPointe Gateway.
- A power connection using the included AC adapter. 

Your client application must:

- Run on a host system within the unattended kiosk.
- Send ASCII command strings to the unattended device over the Serial/USB connection.
- Send RESTful HTTP requests (for example, to void a transaction) to the CardPointe Gateway over a network (Ethernet or WiFi) connection.
- Retain the `processorReference` (the `retref`, or retrieval reference number, provided by the CardPointe Gateway and returned in the DPT response) in order to look up and manage, refund, or void payments.

<!-- theme: warning -->
> It is strongly recommended that you store the `processorReference` in a database table, which your application can use to manage transactions.

# Developer Resources

The following resources are available to help you integrate the CardPointe Unattended solution with your application.

## RS-232 Serial Drivers

Most workstations should support the serial-to-USB connection natively, without any additional drivers. If your workstation does not recognize the connected device, you may need to install a driver.

Visit the following site to download the necessary drivers for your operating system: 

https://www.prolific.com.tw/US/ShowProduct.aspx?pcid=41&showlevel=0017-0037-0041

## Serial Terminal Applications

To test the device connection and to send and receive data, you can use a serial terminal application.

The following serial terminal applications have been tested for use with the unattended device:

- For macOS: https://www.macupdate.com/app/mac/31352/coolterm
- For Windows: https://www.microridge.com/comtestserial.htm

Before you begin, note the name or number of the port that the device is connected to on your workstation. 

Additionally, configure the port configuration settings as follows:

- **Baudrate**: 115200
- **Data Bits**: 8
- **Parity**: None
- **Stop Bits**: 1

## Sample Checksum Calculator

As described in the CardPointe Unattended API, every command and response must include a checksum value. The checksum is derived from the CRC-16 decimal calculation of the ASCII command string up to and including the delimiter preceding the checksum field.

For testing, you can find a simple checksum calculator at crccalc.com.

<!-- theme: warning -->
> If using crccalc.com, ensure that you set the **Input type** to **ASCII**, **Output type** to **Decimal**, and click the **Calc CRC-16** button. The correct checksum value is displayed in the top-left corner of the results table (using the **CRC-16/CCITT-FALSE** algorithm).

Additionally, you can integrate the following sample code with your application to calculate checksums:

#### Sample Checksum Calculator Code

```C++
const int CRCTable[] =
{
0x0000, 0x1021, 0x2042, 0x3063, 0x4084, 0x50A5, 0x60C6, 0x70E7, 0x8108, 0x9129, 0xA14A, 0xB16B, 0xC18C, 0xD1AD, 0xE1CE, 0xF1EF, 0x1231, 0x0210, 0x3273, 0x2252, 0x52B5, 0x4294, 0x72F7, 0x62D6, 0x9339, 0x8318, 0xB37B, 0xA35A, 0xD3BD, 0xC39C, 0xF3FF, 0xE3DE, 0x2462, 0x3443, 0x0420, 0x1401, 0x64E6, 0x74C7, 0x44A4, 0x5485, 0xA56A, 0xB54B, 0x8528, 0x9509, 0xE5EE, 0xF5CF, 0xC5AC, 0xD58D, 0x3653, 0x2672, 0x1611, 0x0630, 0x76D7, 0x66F6, 0x5695, 0x46B4, 0xB75B, 0xA77A, 0x9719, 0x8738, 0xF7DF, 0xE7FE, 0xD79D, 0xC7BC, 0x48C4, 0x58E5, 0x6886, 0x78A7, 0x0840, 0x1861, 0x2802, 0x3823, 0xC9CC, 0xD9ED, 0xE98E, 0xF9AF, 0x8948, 0x9969, 0xA90A, 0xB92B, 0x5AF5, 0x4AD4, 0x7AB7, 0x6A96, 0x1A71, 0x0A50, 0x3A33, 0x2A12, 0xDBFD, 0xCBDC, 0xFBBF, 0xEB9E, 0x9B79, 0x8B58, 0xBB3B, 0xAB1A, 0x6CA6, 0x7C87, 0x4CE4, 0x5CC5, 0x2C22, 0x3C03, 0x0C60, 0x1C41, 0xEDAE, 0xFD8F, 0xCDEC, 0xDDCD, 0xAD2A, 0xBD0B, 0x8D68, 0x9D49, 0x7E97, 0x6EB6, 0x5ED5, 0x4EF4, 0x3E13, 0x2E32, 0x1E51, 0x0E70, 0xFF9F, 0xEFBE, 0xDFDD, 0xCFFC, 0xBF1B, 0xAF3A, 0x9F59, 0x8F78, 0x9188, 0x81A9, 0xB1CA, 0xA1EB, 0xD10C, 0xC12D, 0xF14E, 0xE16F, 0x1080, 0x00A1, 0x30C2, 0x20E3, 0x5004, 0x4025, 0x7046, 0x6067, 0x83B9, 0x9398, 0xA3FB, 0xB3DA, 0xC33D, 0xD31C, 0xE37F, 0xF35E, 0x02B1, 0x1290, 0x22F3, 0x32D2, 0x4235, 0x5214, 0x6277, 0x7256, 0xB5EA, 0xA5CB, 0x95A8, 0x8589, 0xF56E, 0xE54F, 0xD52C, 0xC50D, 0x34E2, 0x24C3, 0x14A0, 0x0481, 0x7466, 0x6447, 0x5424, 0x4405, 0xA7DB, 0xB7FA, 0x8799, 0x97B8, 0xE75F, 0xF77E, 0xC71D, 0xD73C, 0x26D3, 0x36F2, 0x0691, 0x16B0, 0x6657, 0x7676, 0x4615, 0x5634, 0xD94C, 0xC96D, 0xF90E, 0xE92F, 0x99C8, 0x89E9, 0xB98A, 0xA9AB, 0x5844, 0x4865, 0x7806, 0x6827, 0x18C0, 0x08E1, 0x3882, 0x28A3, 0xCB7D, 0xDB5C, 0xEB3F, 0xFB1E, 0x8BF9, 0x9BD8, 0xABBB, 0xBB9A, 0x4A75, 0x5A54, 0x6A37, 0x7A16, 0x0AF1, 0x1AD0, 0x2AB3, 0x3A92, 0xFD2E, 0xED0F, 0xDD6C, 0xCD4D, 0xBDAA, 0xAD8B, 0x9DE8, 0x8DC9, 0x7C26, 0x6C07, 0x5C64, 0x4C45, 0x3CA2, 0x2C83, 0x1CE0, 0x0CC1, 0xEF1F, 0xFF3E, 0xCF5D, 0xDF7C, 0xAF9B, 0xBFBA, 0x8FD9, 0x9FF8, 0x6E17, 0x7E36, 0x4E55, 0x5E74, 0x2E93, 0x3EB2, 0x0ED1, 0x1EF0
};
int CalcCRC(const char buffer[], unsigned int len)
{
    int crc = 0xffff;
    for (int i = 0; i < len; i++)
    {
        crc = CRCTable[(((crc >> 8)) ^ (buffer[i] & 0xff))] ^ (((crc << 8) & 0xffff));
    }
return crc; }
```

# Understanding Payment Flows

