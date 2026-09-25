# HawkEye Lab — Blue Team Detection & Response Report

## Objective
The Blue Team's objective is to identify malicious activity in the network capture, determine the
compromised host, identify indicators of compromise, understand the attacker's behavior, and
recommend detection and containment measures.

## Detection
The following activities should be considered suspicious:
1. DNS request for proforma-invoices.com
2. HTTP download of an executable from the suspicious domain
3. Download of tkraw_Protected99.exe
4. Communication with external infrastructure
5. Requests to bot.whatismyipaddress.com
6. Outbound SMTP communication from the victim
7. SMTP authentication from the endpoint
8. Base64-encoded data transmitted through email
9. Repeated SMTP communication approximately every 10 minutes

The PCAP shows that the victim contacted the public-IP lookup service after downloading the malicious
file.
<img width="1907" height="1068" alt="17 What is the public IP of the victim&#39;s computer" src="https://github.com/user-attachments/assets/7098d4ad-3d58-4ecd-95a9-7dfdf1bff7ee" />


## Indicators of Compromise
### Host Indicators
• 10.4.10.132
• Beijing-5cd1-PC
• Windows 7 64-bit
### Network Indicators
• proforma-invoices.com
• 217.182.138.150
• 23.229.162.69
• bot.whatismyipaddress.com
### File Indicator
• tkraw_Protected99.exe
• MD5: 71826BA081E303866CE2A2534491A2F7

## Investigation Process
A Blue Team analyst could investigate the incident in this order:
### Step 1 — Identify active hosts
Use Wireshark's Statistics → Endpoints → IPv4 to identify the most active systems.
### Step 2 — Investigate DNS
Filter DNS traffic and identify suspicious domains.
### Step 3 — Investigate HTTP
Search for executable downloads and inspect HTTP streams.
### Step 4 — Extract the suspicious file
Export the HTTP object and calculate its hash.
### Step 5 — Investigate SMTP
Filter SMTP traffic and inspect authentication and email transmission.
### Step 6 — Decode suspicious data
Use CyberChef to decode Base64 content and determine what information was stolen.

The walkthrough specifically demonstrates Wireshark analysis of TCP, DNS, HTTP and SMTP traffic and
CyberChef decoding of encoded information.

## Containment
A Blue Team response should include:
- Isolate the affected endpoint.
- Block the malicious domain.
- Block identified malicious/suspicious IP addresses after validation.
- Block the malicious file hash through endpoint security controls where supported.
- Investigate other systems for the same indicators.
- Reset compromised credentials.
- Review browser-stored credentials.
- Investigate outbound SMTP traffic.

## Prevention
Recommended defensive improvements:
- Upgrade unsupported operating systems.
- Implement endpoint detection and response.
- Monitor unusual outbound SMTP traffic.
- Restrict direct SMTP connections from user endpoints.
- Use email security controls for malicious attachments and links.
- Monitor DNS requests for newly registered/suspicious domains.
- Enable MFA for important accounts.
- Avoid storing sensitive credentials directly in browsers where organizational policy requires
stronger controls.

The lab identifies the victim as Windows 7, and the walkthrough notes that the OS had reached end-of-life.

## Blue Team Conclusion
The network capture provides evidence of a compromised Windows endpoint that downloaded a
suspicious executable and subsequently transmitted stolen information through SMTP. The repeated
outbound SMTP activity provides a useful behavioral detection opportunity because the exfiltration
occurred at regular intervals.
