# Tools Used
## 1. Wireshark
**Purpose:** Network and PCAP analysis.

Wireshark was the primary tool used throughout the investigation.

### Uses
- Packet inspection
- Capture statistics
- Ethernet endpoint analysis
- IP endpoint analysis
- DNS investigation
- HTTP investigation
- SMTP investigation
- User-Agent analysis
- HTTP file identification
- SMTP server identification
- Timeline analysis

### Example Filter
dns

Used to isolate DNS traffic.

Another filter used during the investigation:

frame.number==204

Used to inspect a specific packet.
## 2. CyberChef
**Purpose:** Decode and analyze encoded data.

CyberChef was used to decode Base64-encoded information discovered in the SMTP traffic.

### Uses
- Base64 decoding
- Analysis of encoded SMTP data
- Recovery of readable information from encoded content
- Identification of malware-related information

## 3. PowerShell
**Purpose:** File hash calculation.

PowerShell was used to calculate the MD5 hash of the extracted executable.

The identified MD5 value was:

71826BA081E303866CE2A2534491A2F7

## 4. MAC/OUI Lookup
**Purpose:** Identify the manufacturer associated with a MAC address.

The MAC address:

00:08:02:1c:47:ae

was investigated to identify its manufacturer.

The manufacturer was identified as:

Hewlett-Packard (HP)

## 5. IP Geolocation Lookup
**Purpose:** Investigate geographic and provider information associated with external IP addresses.

The suspicious IP:

217.182.138.150

was investigated and associated with Roubaix, Hauts-de-France, France, with OVH SAS identified as the ISP
in the lab walkthrough.

## 6. Web Search
**Purpose:** Supporting investigation and infrastructure research.

Web searches were used during the investigation to research information such as manufacturer/location
details and infrastructure information.

# Tool Summary
| Tool | Main Purpose |
| --- | --- |
| Wireshark | PCAP and network traffic analysis |
| CyberChef | Decoding encoded information |
|PowerShell | MD5 hash calculation |
| MAC/OUI Lookup | MAC manufacturer identification |
| IP Geolocation | IP location/provider investigation |
| Web Search | Supporting infrastructure research 
