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
<table>
  <tr>
<td><img width="1907" height="1068" alt="04 What is the most active computer at the link level" src="https://github.com/user-attachments/assets/2d01bc99-4031-4565-a869-e40187f7ff16" /></td>
<td><img width="1917" height="1078" alt="08 What is the name of the most active computer at the network level" src="https://github.com/user-attachments/assets/60298ce2-449b-45a3-a431-c7aa264a2dfe" /></td>
    </tr>
  </table>

## Initial Access / Malware Delivery
The victim queried: proforma-invoices.com

The domain resolved to: 217.182.138.150

The victim subsequently downloaded: tkraw_Protected99.exe

The HTTP User-Agent identified the victim as a Windows 7 64-bit system.
<table>
  <tr>
<td><img width="1916" height="1077" alt="10 What domain is the victim asking about in packet 204" src="https://github.com/user-attachments/assets/a1dea03e-5007-4671-9855-c5df57e0d175" /></td>
<td><img width="1907" height="1062" alt="13 What operating system does the victim&#39;s computer run" src="https://github.com/user-attachments/assets/7fba8021-12db-45be-bfdf-263fb79e1906" /></td>
    </tr>
  </table>
From a Red Team perspective, the malicious invoice-themed domain and executable download represent
the malware-delivery stage.

## Malware Identification
The downloaded executable had the following MD5: 71826BA081E303866CE2A2534491A2F7

The file was extracted from HTTP traffic and hashed for identification.

The decoded exfiltrated data identified the malware as:

HawkEye Keylogger – Reborn v9

The malware collected information including credentials and system information.
<table>
  <tr>
    <td><img width="1672" height="941" alt="22 Which malware variant exfiltrated the data" src="https://github.com/user-attachments/assets/df5a5357-0153-41fa-9ae9-2aac0c19a700" /></td>
    <td><img width="1907" height="1068" alt="14 What is the name of the malicious file downloaded by the accountant" src="https://github.com/user-attachments/assets/abc84d3f-e80e-479a-b329-4317745c83e1" /></td>
    </tr>
</table>

## Credential Theft
The malware obtained credentials stored in Google Chrome.

The investigation identified the Chrome Login Data location:

C:\Users\roman.mcguire\AppData\Local\Google\Chrome\User Data\Default\Login Data

The extracted information included Bank of America credentials.
<img width="1672" height="941" alt="23 What are the bankofamerica access credentials" src="https://github.com/user-attachments/assets/59a37331-2a3a-4a18-8790-d8305047ba54" />


## Data Exfiltration
The stolen information was sent through SMTP to: 23.229.162.69

The server was located in the United States according to the walkthrough's IP lookup. The SMTP
communication included authentication and subsequent transmission of the stolen information.

The malware used an email account for exfiltration, and the SMTP stream showed the recipient address
and Base64-encoded data.

The captured traffic also exposed the SMTP authentication password because the communication was not
protected by TLS.
<table>
  <tr>
    <td><img width="1906" height="1068" alt="18 In which country is the email server to which the stolen information is sent" src="https://github.com/user-attachments/assets/a4b31d48-d4d7-4db6-8c7d-6201f294c5d5" /></td>
    <td><img width="1672" height="941" alt="20 To which email account is the stolen information sent" src="https://github.com/user-attachments/assets/a83ebece-4f55-4f77-b3df-35296db4a889" /></td>
    <td><img width="1676" height="939" alt="21 What is the password used by the malware to send the email" src="https://github.com/user-attachments/assets/6bb933c5-a44e-46c1-8f21-b8592cbfdae4" /></td>
    </tr>
</table>


## Exfiltration Frequency
The SMTP traffic showed repeated EHLO messages approximately every 10 minutes, indicating automated
periodic exfiltration.
<img width="1907" height="1068" alt="24 Every how many minutes does the collected data get exfiltrated" src="https://github.com/user-attachments/assets/85225132-0e89-4b87-a4ec-f510e06ad1d4" />


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
