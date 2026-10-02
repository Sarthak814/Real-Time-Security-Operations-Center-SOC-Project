# HawkEye Network Forensics Investigation Report

## 1. Executive Summary
The HawkEye lab is a network-forensics investigation involving a PCAP capture containing network activity
associated with an information-stealing malware infection.

The investigation focused on analyzing network traffic to identify the affected system, suspicious DNS
activity, malicious infrastructure, a downloaded executable, SMTP communication, encoded data, and
evidence of data exfiltration.

Wireshark was used as the primary network-analysis tool to examine DNS, HTTP, SMTP, TCP, endpoint, and
packet information. CyberChef was used to decode Base64-encoded data. PowerShell was used to
calculate the MD5 hash of an extracted executable.

The investigation identified a suspicious domain, its associated IP address, a downloaded executable
named tkraw_Protected99.exe, the executable's MD5 hash, SMTP infrastructure, an email account
used for exfiltration, and evidence identifying the malware as HawkEye Keylogger – Reborn v9.

The official walkthrough describes the objective as reconstructing the attack timeline, identifying indicators
of compromise, and determining how the malware collected and exfiltrated sensitive information.

## 2. Lab Overview
**Lab:** HawkEye

**Platform:** CyberDefenders

**Category:** Network Forensics / Blue Team

**Primary Evidence:** PCAP network capture

**Primary Tool:** Wireshark

The investigation analyzes captured network traffic generated during a suspected malware infection.

The traffic contains DNS requests, HTTP communication, file downloads, SMTP communication, and
encoded data.

## 3. Investigation Objectives
The main objectives of the investigation were:
1. Determine the size and duration of the PCAP.
2. Identify the systems involved in the communication.
3. Identify the most active host.
4. Identify the DNS server and suspicious DNS requests.
5. Investigate suspicious domains and IP addresses.
6. Identify downloaded malicious files.
7. Calculate and document the file hash.
8. Identify the web server involved in malware delivery.
9. Identify the victim's public IP address.
10. Investigate SMTP communication.
11. Identify the email infrastructure used for communication.
12. Decode encoded information.
13. Identify the malware variant.
14. Identify evidence of stolen information.
15. Determine the approximate frequency of data exfiltration.

## 4. Investigation Environment
### Evidence
The investigation was performed using the HawkEye PCAP supplied with the CyberDefenders lab.
### Tools
- Wireshark
- CyberChef
- PowerShell
- MAC/OUI lookup
- IP geolocation lookup
- Web search

## 5. Investigation Methodology
The investigation followed a network-forensics workflow:

PCAP

↓

Capture Statistics

↓

Network Endpoints

↓

DNS Investigation

↓

Suspicious Domain

↓

IP Investigation

↓

HTTP Investigation

↓

Malware Download

↓

File Hash

↓

SMTP Investigation

↓

Encoded Data

↓

CyberChef Decoding

↓

Malware Identification

↓

Data Exfiltration Analysis

6. Step-by-Step Investigation
6.1 PCAP Packet Count
Objective
Determine the total number of packets contained in the capture.
Tool
Wireshark
Method
The packet count was checked using the Wireshark interface and capture statistics.
Finding
The PCAP contains:
4,003 packets
Evidence
Screenshot:
01-packet-count.png
6.2 First Packet Timestamp
Objective
Determine when the network capture began.
Tool
Wireshark
Method
Wireshark's Capture File Properties were inspected.
Finding
The first packet was captured at:
2019-04-10 20:37:07 UTC
Evidence
02-first-packet.png
6.3 Capture Duration
Objective
Determine how long the network capture lasted.
Tool
Wireshark
Finding
The capture duration was:
01:03:41
Evidence
03-capture-duration.png
6.4 Most Active MAC Address
Objective
Identify the most active link-layer computer.
Tool
Wireshark
Method
Wireshark:
Statistics → Endpoints → Ethernet
was used to examine Ethernet endpoints.
Finding
The most active MAC address was:
00:08:02:1c:47:ae
The endpoint accounted for the complete packet activity recorded in the Ethernet statistics.
Evidence
04-most-active-mac.png
6.5 MAC Address Manufacturer
Objective
Identify the manufacturer associated with the MAC address.
Tool
MAC/OUI lookup
Finding
The MAC address:
00:08:02:1c:47:ae
was associated with:
Hewlett-Packard (HP)
Evidence
05-nic-manufacturer.png
6.6 Manufacturer Headquarters
Objective
Identify the headquarters location of the identified manufacturer.
Finding
The official walkthrough identifies HP's headquarters as:
Palo Alto, California, USA
Evidence
06-hp-headquarters.png
6.7 Internal IPv4 Hosts
Objective
Identify the private IPv4 addresses communicating in the capture.
Finding
The following private IPv4 addresses were identified:
10.4.10.2
10.4.10.4
10.4.10.132
10.4.10.255
The address 10.4.10.255 represents a broadcast address and was therefore excluded as an individual
computer.
The three identified computers were:
10.4.10.2
10.4.10.4
10.4.10.132
Evidence
07-internal-hosts.png
6.8 Most Active Network Host
Objective
Identify the most active internal computer.
Tool
Wireshark
Finding
The most active internal host was:
10.4.10.132
DHCP information revealed the hostname:
Beijing-5cd1-PC
Evidence
08-victim-hostname.png
6.9 DNS Server
Objective
Identify the DNS server used by the investigated host.
Tool
Wireshark
Filter
dns
Finding
The DNS server was:
10.4.10.4
The investigated host 10.4.10.132 generated DNS requests to this server.
Evidence
09-dns-server.png
6.10 Suspicious DNS Query
Objective
Identify the domain queried by the investigated system.
Tool
Wireshark
Filter
frame.number==204
Finding
The DNS query was:
proforma-invoices.com
This domain became an important indicator for further investigation.
Evidence
10-dns-query.png
6.11 Domain IP Address
Objective
Determine the IP address associated with the suspicious domain.
Tool
Wireshark
Method
The DNS response containing the A record was examined.
Finding
The domain resolved to:
217.182.138.150
Evidence
11-domain-ip.png
6.12 IP Geolocation
Objective
Investigate the geographic information associated with the suspicious IP address.
Finding
The IP address:
217.182.138.150
was identified in:
Roubaix, Hauts-de-France, France
The ISP identified in the walkthrough was:
OVH SAS
Evidence
12-ip-geolocation.png
6.13 HTTP User-Agent
Objective
Identify information about the software and operating system generating HTTP requests.
Tool
Wireshark
Finding
The HTTP User-Agent identified the system as using Internet Explorer and Windows NT 6.1.
Windows NT 6.1 corresponds to Windows 7.
The User-Agent also contained the WOW64 indicator.
Evidence
13-user-agent.png
6.14 Malicious File
Objective
Identify the executable downloaded during the HTTP communication.
Tool
Wireshark
Finding
The suspicious executable was:
tkraw_Protected99.exe
Evidence
14-malicious-file.png
6.15 File Hash
Objective
Generate a hash that can uniquely identify the extracted executable.
Tools
• Wireshark
• PowerShell
Method
The executable was extracted from the HTTP traffic and its MD5 hash was calculated.
Finding
MD5:
71826BA081E303866CE2A2534491A2F7
Evidence
15-file-hash.png
6.16 Web Server Software
Objective
Identify the web server software responsible for hosting the downloaded file.
Tool
Wireshark
Method
The HTTP response headers were inspected.
Finding
The server software was:
LiteSpeed
Evidence
16-web-server.png
6.17 Victim Public IP
Objective
Determine the public IP address associated with the victim system.
Tool
Wireshark
Method
HTTP traffic involving:
bot.whatismyipaddress.com
was examined.
The server response returned the public IP address.
Finding
173.66.146.112
Evidence
17-public-ip.png
6.18 SMTP Server
Objective
Identify the SMTP server involved in the email communication.
Tool
Wireshark
Finding
The SMTP server IP address was:
23.229.162.69
The walkthrough identifies the location as Tempe, Arizona, USA, and the provider as GoDaddy.com LLC.
Evidence
18-email-server.png
6.19 SMTP Server Software
Objective
Identify the SMTP server software.
Tool
Wireshark
Method
The SMTP server banner was examined.
Finding
The server was running:
Exim 4.91
The server banner included:
p3plcpnl04313.prod.phx3.secureserver.net
Evidence
19-mail-server-software.png
6.20 Email Communication
Objective
Identify the email addresses involved in the SMTP communication.
Tool
Wireshark
Method
SMTP MAIL FROM and RCPT TO commands were examined.
Finding
The same email address was used as the sender and recipient:
sales.del@macwinlogistics.in
The SMTP data also contained Base64-encoded information.
Evidence
20-email-address.png
6.21 Decoding the SMTP Password
Objective
Decode Base64-encoded authentication information.
Tool
CyberChef
Method
The encoded value observed in the SMTP communication was decoded using CyberChef.
Finding
The decoded value revealed an SMTP password.
Note: The actual credential is intentionally not reproduced in this public GitHub report.
Evidence
21-password-decoding.png
6.22 Malware Identification
Objective
Identify the malware responsible for the observed activity.
Tool
CyberChef
Method
Encoded exfiltrated data was decoded and analyzed.
Finding
The decoded data identified the malware as:
HawkEye Keylogger – Reborn v9
The walkthrough describes the malware as an information stealer/keylogger capable of collecting
information such as keystrokes, clipboard data, browser credentials, and email credentials.
Evidence
22-malware-variant.png
6.23 Stolen Credential Evidence
Objective
Identify evidence of credentials collected by the malware.
Finding
The decoded data contained evidence of credentials retrieved from Chrome's Login Data database.
The walkthrough identifies a Bank of America credential entry associated with:
C:\Users\roman.mcguire\AppData\Local\Google\Chrome\User Data\Default\Login Data
Security Note
Actual credential values should not be published in a public GitHub repository. If the internship requires
the exact values, they should be kept in a private evidence file rather than the public repository.
Evidence
23-stolen-credentials.png
6.24 Data Exfiltration Frequency
Objective
Determine how frequently stolen information was exfiltrated.
Tool
Wireshark
Method
Repeated SMTP EHLO timestamps were compared.
Finding
The exfiltration activity occurred approximately every:
10 minutes
Evidence
24-exfiltration-frequency.png
7. Attack Timeline
The investigation can be summarized as:
PCAP Capture
↓
Internal Host Identified
↓
10.4.10.132 identified as active host
↓
DNS request generated
↓
proforma-invoices.com queried
↓
Domain resolved to 217.182.138.150
↓
HTTP communication established
↓
tkraw_Protected99.exe downloaded
↓
Executable hash calculated
↓
Victim public IP identified
↓
SMTP communication observed
↓
Encoded data transmitted
↓
Data decoded using CyberChef
↓
HawkEye Keylogger – Reborn v9 identified
↓
Stolen information identified
↓
Repeated SMTP exfiltration observed
8. Indicators of Compromise
The main indicators identified during the investigation include:
• proforma-invoices.com
• 217.182.138.150
• tkraw_Protected99.exe
• 71826BA081E303866CE2A2534491A2F7
• 23.229.162.69
• sales.del@macwinlogistics.in
Additional network information is documented separately in the IOC file.
9. MITRE ATT&CK Mapping
The investigation provides evidence consistent with several ATT&CK concepts.
Observed Activity Possible ATT&CK Technique
Malicious executable delivered through HTTP T1105 – Ingress Tool Transfer
Collection of browser credentials T1555.003 – Credentials from Web Browsers
Keylogging behavior T1056.001 – Keylogging
Exfiltration through email T1048 – Exfiltration Over Alternative Protocol
Communication with external infrastructure Command and Control activity
These mappings should be treated as analytical mappings based on the observed lab behavior rather than
as a claim that every technique was independently confirmed from the PCAP.
10. Key Findings
The investigation identified:
1. A PCAP containing 4,003 packets.
2. An active internal host at 10.4.10.132.
3. Hostname information associated with Beijing-5cd1-PC.
4. DNS communication with 10.4.10.4.
5. A suspicious DNS query for proforma-invoices.com.
6. The domain resolving to 217.182.138.150.
7. A suspicious executable named tkraw_Protected99.exe.
8. The executable MD5 hash 71826BA081E303866CE2A2534491A2F7.
9. A LiteSpeed web server hosting the downloaded file.
10. A victim public IP of 173.66.146.112.
11. SMTP communication with 23.229.162.69.
12. An Exim 4.91 SMTP server.
13. Encoded data transmitted through SMTP.
14. Evidence identifying HawkEye Keylogger – Reborn v9.
15. Evidence of credential collection.
16. Approximately 10-minute exfiltration intervals.
11. Security Observations
The investigation demonstrates how a network capture can reveal multiple stages of a malware infection.
DNS traffic provided the initial suspicious domain.
HTTP traffic provided evidence of the malicious file download and server infrastructure.
SMTP traffic provided evidence of command/data communication and exfiltration.
Encoded SMTP content required additional analysis with CyberChef before the stolen information and
malware identity could be understood.
This demonstrates the importance of correlating multiple network protocols rather than investigating
individual packets in isolation.
12. Conclusion
The HawkEye investigation demonstrated a complete network-forensics workflow beginning with PCAP
analysis and progressing through host identification, DNS investigation, HTTP analysis, malware-file
extraction, hash generation, SMTP analysis, encoded-data decoding, malware identification, and
exfiltration analysis.
The investigation identified network, file, domain, email, and malware-related indicators associated with
the HawkEye infection.
The results provide a practical example of how tools such as Wireshark, CyberChef, and PowerShell can be
combined during a cybersecurity investigation to reconstruct malicious activity from network evidence.
