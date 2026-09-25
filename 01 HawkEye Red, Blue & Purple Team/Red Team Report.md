# HawkEye Lab - Red Team Attack Analysis

## Objective
The objective of the Red Team analysis is to reconstruct the attack performed against the victim machine
and understand the attacker's techniques, infrastructure, malware delivery method, credential theft, and
data-exfiltration process.

The investigation is based on network traffic captured in a PCAP file. The traffic shows DNS requests, HTTP
communication, a malicious executable download, and SMTP communication used to exfiltrate stolen
information.

## Attack Chain
The reconstructed attack sequence is:

Malicious domain → Victim accesses domain → Malicious executable downloaded → HawkEye malware
executes → Credentials/system information collected → Data encoded → SMTP authentication → Stolen
data emailed to attacker

## Victim Identification
The most active system was identified as:
- IP: 10.4.10.132
- Hostname: Beijing-5cd1-PC
- MAC: 00:08:02:1c:47:ae
- OS: Windows 7 64-bit
The PCAP contained 4,003 packets, and the most active system exchanged approximately 2 MB of traffic.

## Initial Access / Malware Delivery
The victim queried: proforma-invoices.com

The domain resolved to: 217.182.138.150

The victim subsequently downloaded: tkraw_Protected99.exe

The HTTP User-Agent identified the victim as a Windows 7 64-bit system.

From a Red Team perspective, the malicious invoice-themed domain and executable download represent
the malware-delivery stage.

## Malware Identification
The downloaded executable had the following MD5: 71826BA081E303866CE2A2534491A2F7

The file was extracted from HTTP traffic and hashed for identification.

The decoded exfiltrated data identified the malware as:

HawkEye Keylogger – Reborn v9

The malware collected information including credentials and system information.

## Credential Theft
The malware obtained credentials stored in Google Chrome.

The investigation identified the Chrome Login Data location:

C:\Users\roman.mcguire\AppData\Local\Google\Chrome\User Data\Default\Login Data

The extracted information included Bank of America credentials.

## Data Exfiltration
The stolen information was sent through SMTP to: 23.229.162.69

The server was located in the United States according to the walkthrough's IP lookup. The SMTP
communication included authentication and subsequent transmission of the stolen information.

The malware used an email account for exfiltration, and the SMTP stream showed the recipient address
and Base64-encoded data.

The captured traffic also exposed the SMTP authentication password because the communication was not
protected by TLS.

## Exfiltration Frequency
The SMTP traffic showed repeated EHLO messages approximately every 10 minutes, indicating automated
periodic exfiltration.

## Red Team Attack Summary
| Stage | Finding |
|---|---|
| Victim | 10.4.10.132 / Beijing-5cd1-PC |
| Delivery domain | proforma-invoices.com |
| Malicious file | tkraw_Protected99.exe |
| File MD5 | 71826BA081E303866CE2A2534491A2F7 |
| Malware | HawkEye Keylogger – Reborn v9 |
| Target data | Browser credentials/system information |
| Exfiltration | SMTP |
| SMTP server | 23.229.162.69 |
| Exfiltration interval | ~10 minutes |
