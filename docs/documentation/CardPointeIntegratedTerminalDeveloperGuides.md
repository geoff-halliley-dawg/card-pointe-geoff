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
> 198.62.138.0/24
> 206.201.63.0/24
> See Allowing CardPointe Integrated Terminal Network Connections for more detailed information.

The terminal displays either Connected or Disconnected depending on whether it has established an internet connection and made contact with the terminal service.

Once the terminal has established a Connected status, it can receive requests from your application.
