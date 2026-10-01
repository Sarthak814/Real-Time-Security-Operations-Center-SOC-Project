# Indicators of Compromise (IOCs)

## Network Indicators
Type Indicator Description
Internal IP 10.4.10.132 Most active internal host
DNS Server 10.4.10.4 DNS server used by the investigated host
Suspicious IP 217.182.138.150 IP address associated with the suspicious domain
SMTP Server 23.229.162.69 SMTP infrastructure observed during the investigation
Victim Public IP 173.66.146.112 Public IP identified through HTTP traffic

## Domain Indicators
Type Indicator Description
Suspicious Domain
proformainvoices.
com
Domain identified through DNS investigation

## File Indicators
Type Indicator Description
Filename tkraw_Protected99.exe Executable downloaded through HTTP
MD5 71826BA081E303866CE2A2534491A2F7 MD5 hash of the extracted executable

## Email Indicators
Type Indicator Description
Email Address sales.del@macwinlogistics.in Email address observed in SMTP communication

## Malware
Type Indicator
Malware HawkEye Keylogger – Reborn v9

## Important Note
The credentials discovered during the lab are intentionally not published in this public IOC document.
