# HawkEye Lab — Purple Team Handover Report

## Executive Summary
The HawkEye investigation identified a compromised Windows endpoint that downloaded a malicious
executable from a suspicious domain. The downloaded malware was identified as HawkEye Keylogger –
Reborn v9. The malware collected sensitive information and periodically exfiltrated the collected data
through an external SMTP server.
<img width="1672" height="941" alt="22 Which malware variant exfiltrated the data" src="https://github.com/user-attachments/assets/5b02406c-33de-4baa-ae00-bf762720cf6f" />

The purpose of the Purple Team analysis is to connect the observed attack techniques with corresponding
defensive detections and response actions.

## Attack vs Defence Handover
| Attack Activity | Evidence | Blue Team Detection | Defensive Action |
|---|---|---|---|
| Suspicious DNS lookup | proforma-invoices.com | DNS monitoring | Investigate/block malicious domain |
| Malware download | tkraw_Protected99.exe | HTTP monitoring | Block/quarantine file |
| Malware identification | MD5 hash | EDR/hash detection | Search environment for same hash |
| Credential theft | Chrome Login Data | Endpoint monitoring | Investigate browser credential access |
| Public IP discovery | bot.whatismyipaddress.com | DNS/HTTP anomaly detection | Investigate endpoint behavior |
| SMTP connection | 23.229.162.69 | Outbound SMTP monitoring | Restrict/block suspicious destination |
| SMTP authentication | AUTH LOGIN/PLAIN | Network monitoring | Alert on unusual endpoint SMTP authentication |
| Encoded stolen data | Base64 data | Content/network analysis | Investigate encoded outbound data |
| Periodic exfiltration | ~10-minute intervals | Behavioral detection | Create periodicbeacon/ exfiltration alert |

This table is the core of your Purple Team handover.

## Attack Timeline
You can put this timeline into the report:
### 1. DNS Resolution
Victim 10.4.10.132 queried proforma-invoices.com.
↓
### 2. Malicious Infrastructure
Domain resolved to 217.182.138.150.
↓
### 3. Malware Download
Victim downloaded tkraw_Protected99.exe.
↓
### 4. Malware
The investigation identified HawkEye Keylogger – Reborn v9.
↓
### 5. Information Collection
The malware collected credentials and system information.
↓
### 6. External IP Discovery
The victim contacted bot.whatismyipaddress.com.
↓
### 7. SMTP Authentication
The malware authenticated to an external mail server.
↓
### 8. Data Exfiltration
Stolen information was transmitted through SMTP.
↓
### 9. Periodic Exfiltration
The activity occurred approximately every 10 minutes.

## Key IOCs for Blue Team Handover
### Domain
proforma-invoices.com
### IP addresses
217.182.138.150

23.229.162.69

173.66.146.112
### Host
10.4.10.132

Beijing-5cd1-PC
### File
tkraw_Protected99.exe
### MD5
71826BA081E303866CE2A2534491A2F7
### Malware
#### HawkEye Keylogger – Reborn v9
These findings are directly supported by the DNS, HTTP, SMTP and malware-analysis sections of the
walkthrough.
<table>
  <tr>
    <td><img width="1917" height="1078" alt="11 What is the IP of the domain in the previous question" src="https://github.com/user-attachments/assets/032ccdd5-266f-47d7-9c35-ec5b58a414b3" /></td>
    <td><img width="1907" height="1068" alt="14 What is the name of the malicious file downloaded by the accountant" src="https://github.com/user-attachments/assets/31d555ec-4a0e-46dc-9cfe-e868e25e92d5" /></td>
  </tr>
</table>
<img width="1672" height="941" alt="22 Which malware variant exfiltrated the data" src="https://github.com/user-attachments/assets/4355b337-5aa8-4289-9843-77f2621c5e3e" />

## Detection Opportunities
The Purple Team should recommend detection rules around:
### DNS
Alert when internal endpoints communicate with known/suspicious malware-delivery domains.
### HTTP
Alert when a workstation downloads executable files from suspicious external domains.
### Endpoint
Monitor suspicious executable execution and browser credential-store access.
### SMTP
Alert when normal user endpoints initiate external SMTP connections.
### Data Exfiltration
Detect repeated outbound communication at regular intervals, especially when associated with unusual
destinations.
### Credential Protection
Monitor access to browser credential databases and protect sensitive accounts with MFA.

## Purple Team Conclusion
The HawkEye investigation demonstrates how an attacker can use a malicious download to compromise an
endpoint, collect sensitive information, and exfiltrate the information through email.
The Red Team perspective identifies the attack chain and attacker infrastructure, while the Blue Team
perspective converts those observations into detection, containment and prevention opportunities. The
Purple Team handover connects both sides by turning each attack technique into an actionable defensive
control.
