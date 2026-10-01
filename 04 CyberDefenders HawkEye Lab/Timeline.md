# HawkEye Attack Timeline
## Overview
The following timeline reconstructs the major stages identified during the HawkEye network-forensics
investigation.

| Stage | Event | Evidence |
| --- | --- | --- |
| 1 | PCAP capture begins | Wireshark
| 2 | Internal systems identified | Wireshark Endpoints
| 3 | 10.4.10.132 identified as the most active host | Network analysis
| 4 | Hostname identified as Beijing-5cd1-PC | DHCP traffic
| 5 | DNS server identified as 10.4.10.4 | DNS traffic
| 6 | Host queries proforma-invoices.com | DNS query
| 7 | Domain resolves to 217.182.138.150 | DNS response
| 8 | HTTP communication with external infrastructure observed | HTTP traffic
| 9 | tkraw_Protected99.exe downloaded | HTTP traffic
| 10 | MD5 hash calculated | PowerShell
| 11 | LiteSpeed identified as web server software | HTTP headers
| 12 | Victim public IP identified as 173.66.146.112 | HTTP response
| 13 | SMTP communication identified | SMTP traffic
| 14 | SMTP server identified as 23.229.162.69 | SMTP traffic
| 15 | Exim 4.91 identified | SMTP banner
| 16 | Email communication identified | SMTP commands
| 17 | Base64-encoded information identified | SMTP data
| 18 | Encoded information decoded | CyberChef
| 19 | HawkEye Keylogger – Reborn v9 identified | Decoded data
| 20 | Evidence of credential collection identified | Decoded data
| 21 | Repeated SMTP activity analyzed | Wireshark
| 22 | Exfiltration interval estimated at approximately 10 minutes | SMTP timestamps

## Capture Information
First packet: 2019-04-10 20:37:07 UTC

Capture duration: 01:03:41

Total packets: 4003

## Exfiltration
Analysis of repeated SMTP activity indicated that information was exfiltrated approximately every 10
minutes.
