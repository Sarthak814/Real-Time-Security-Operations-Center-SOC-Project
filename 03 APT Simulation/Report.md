# Simulating and Responding to a Multi-Stage APT Attack Using Open-Source SOC Tools

## 1. Introduction
An Advanced Persistent Threat (APT) is a sophisticated cyberattack in which an attacker attempts to gainunauthorized access to a computer system or network, maintain access for an extended period, movebetween systems, and steal sensitive information.

APT attacks typically involve multiple stages, including initial access, persistence, privilege escalation, lateral movement, command and control, and data exfiltration. Detecting such attacks requires continuous monitoring, log analysis, threat intelligence, and coordinated incident response.

This project presents a simulated multi-stage APT attack and a corresponding defensive investigation using open-source Security Operations Center (SOC) tools. The exercise covers attack simulation, security event detection, forensic investigation, threat intelligence, and mitigation.

The proposed environment uses Security Onion, Wazuh, Kibana, TheHive, Cortex, MISP, YARA, and Sigma to demonstrate how security teams can investigate and respond to a coordinated cyberattack.

## 2. Objectives
The main objectives of this project are:

- To understand the stages of a multi-stage APT attack.
- To simulate common attacker behaviors in an authorized laboratory environment.
- To identify suspicious activities using security monitoring tools.
- To analyze logs and correlate security events from multiple sources.
- To investigate suspicious files, IP addresses, and URLs.
- To reconstruct an attack timeline using collected evidence.
- To identify Indicators of Compromise (IoCs).
- To develop detection rules using YARA and Sigma.
- To share threat intelligence using MISP.
- To prepare an incident response and mitigation plan.

## 3. Project Scope
This project focuses on the detection and investigation of simulated APT activity in a controlled laboratory environment. It demonstrates the workflow that a security analyst can use to identify suspicious behavior, investigate security alerts, document evidence, and recommend remediation.

The exercise covers the following attack stages:

1. Initial access.
2. Persistence establishment.
3. Privilege escalation.
4. Lateral movement.
5. Data exfiltration.
6. Detection and investigation.
7. Threat intelligence and mitigation.

All attack activities are intended for an isolated, authorized test environment. No real organizational systems or sensitive data are required.

## 4. Tools and Technologies
### 4.1 Security Onion
Security Onion is an open-source security monitoring platform used for network security monitoring, threat hunting, and investigation. It integrates security tools to help analysts identify suspicious network traffic and investigate potential attacks.

**Role in the project:**

- Monitor network traffic.
- Identify suspicious communication patterns.
- Investigate potential command-and-control activity.
- Review network security alerts.
- Support threat hunting and incident investigation.

### 4.2 Wazuh
Wazuh is an open-source security monitoring platform that provides endpoint monitoring, log analysis, threat detection, file integrity monitoring, and security alerting.

**Role in the project:**

- Collect endpoint security logs.
- Detect suspicious authentication attempts.
- Monitor changes to important files.
- Identify suspicious process execution.
- Detect potentially unauthorized scheduled tasks.
- Generate alerts for suspicious system activities.

### 4.3 Kibana

Kibana provides search, filtering, visualization, and investigation capabilities for data stored in Elasticsearch.

**Role in the project:**

- Search collected security logs.
- Filter events by IP address, timestamp, and event type.
- Correlate authentication and endpoint events.
- Investigate suspicious network connections.
- Visualize security events and construct timelines.

### 4.4 TheHive
TheHive is an incident response platform that helps security teams manage alerts, cases, observables, and investigation activities.

**Role in the project:**

- Create an incident investigation case.
- Record suspicious indicators.
- Assign investigation tasks.
- Maintain investigation notes.
- Document findings and response actions.

### 4.5 Cortex
Cortex supports automated analysis of observables through analyzers and responders, depending on the installed integrations and configuration.

**Role in the project:**

- Analyze suspicious IP addresses.
- Examine URLs and file hashes.
- Enrich investigation data with available threat intelligence.
- Support analyst decisions during incident response.

### 4.6 MISP
MISP is an open-source threat intelligence platform used to collect, organize, correlate, and share threat intelligence.

**Role in the project:**

- Record Indicators of Compromise.
- Organize related threat intelligence.
- Associate indicators with an incident.
- Share relevant indicators with authorized security teams.

### 4.7 YARA
YARA is a pattern-matching tool used to identify files based on strings, byte sequences, and other defined characteristics.

**Role in the project:**

- Create rules for identifying known suspicious file patterns.
- Support malware triage.
- Help detect files containing specific investigation-related indicators.

### 4.8 Sigma
Sigma is a generic format for describing detection rules for log events. Rules can be converted into queries for supported security platforms.

**Role in the project:**

- Define suspicious authentication behavior.
- Detect potentially suspicious process execution.
- Identify unusual PowerShell activity.
- Support portable detection logic across compatible SIEM platforms.

## 5. Proposed Laboratory Environment

The project uses the following logical architecture:

- **Endpoint system:** A Windows test machine used to generate endpoint events.
- **Monitoring platform:** Wazuh for endpoint monitoring and alert generation.
- **Network monitoring:** Security Onion for inspecting relevant network traffic.
- **Log investigation:** Kibana and Elasticsearch for searching and correlating collected events.
- **Incident management:** TheHive for recording the investigation.
- **Observable analysis:** Cortex for supported enrichment and analysis tasks.
- **Threat intelligence:** MISP for recording and sharing IoCs.
- **Detection engineering:** YARA and Sigma for defining detection logic.

The tools require appropriate installation, configuration, and integration before they can exchange events or produce actual investigation results. The architecture represents the intended laboratory workflow, not confirmation that every integration was deployed and tested.

## 6. Phase 1: Attack Simulation (Red Team)
### 6.1 Overview
The first phase models attacker behavior across multiple stages. The purpose is to understand which events a defensive team should monitor and what evidence may be available during an investigation.

The simulated scenario assumes that an attacker attempts to compromise a test endpoint and access additional resources before attempting to transfer data outside the laboratory.

### 6.2 Stage 1: Initial Access
**Objective:** Obtain an initial foothold on a target endpoint.

The scenario considers two possible initial access methods:

- A phishing message containing a suspicious link or attachment.
- Exploitation of an unpatched software vulnerability.

In a controlled exercise, initial access should be represented by safe test events or an approved simulation rather than sending malicious messages to real users or exploiting production systems.

**Expected evidence:**

- Email gateway events, if email is included in the lab.
- Browser or endpoint execution events.
- Security alerts associated with suspicious files.
- Unusual authentication activity.
- Network connections to unrecognized destinations.

**Detection approach:**

Wazuh can monitor available endpoint logs and generate alerts based on configured detection rules. Security Onion can help investigate suspicious network connections when the relevant traffic is visible to its sensors.

### 6.3 Stage 2: Persistence Establishment
**Objective:** Model an attacker's attempt to maintain access after the initial compromise.

Attackers may attempt to establish persistence through scheduled tasks, unauthorized services, startup mechanisms, or other system configuration changes.

For this exercise, scheduled-task creation and unusual PowerShell activity are treated as behaviors of interest.

**Expected evidence:**

- Windows Task Scheduler operational events.
- Process creation logs.
- PowerShell operational logs.
- Changes to relevant system configuration.
- Alerts related to unexpected persistence mechanisms.

**Detection approach:**

Endpoint monitoring should identify unusual task creation or changes to sensitive system locations. Analysts should examine the process that initiated the change, the account involved, the timestamp, and the associated command line.

### 6.4 Stage 3: Privilege Escalation
**Objective:** Identify behavior that could indicate an attempt to obtain higher privileges.

An attacker who gains access to a standard user account may attempt to exploit a vulnerability, abuse excessive permissions, or misuse a privileged account.

**Expected evidence:**

- Unexpected elevation of privileges.
- Unusual process execution.
- Changes in user-group membership.
- Access to restricted resources.
- Security events associated with privileged accounts.

**Detection approach:**

Wazuh can analyze available Windows security events and configured rules. Analysts should compare the observed activity against the user's normal permissions and approved administrative changes.

### 6.5 Stage 4: Lateral Movement
**Objective:** Model an attempt to move from one compromised system to another.

Common lateral movement techniques include the misuse of administrative tools such as PsExec and the abuse of authentication material, including Pass-the-Hash techniques.

These techniques can be investigated by examining authentication events, remote service activity, process execution, and network connections between internal systems.

**Expected evidence:**

- Unexpected remote logons.
- Connections between endpoints that do not normally communicate.
- Remote service creation.
- Unusual administrative account activity.
- Authentication failures followed by successful logins.

**Detection approach:**

Analysts should correlate authentication records with process and network telemetry. A single administrative connection may be legitimate; a combination of unusual logons, remote execution, and unexpected access patterns may indicate a larger incident.

### 6.6 Stage 5: Data Exfiltration
**Objective:** Model an attempt to transfer sensitive information outside the test environment.

The scenario considers two possible methods:

- Unusual DNS queries that may indicate DNS tunneling.
- Unexpected outbound HTTP or HTTPS communication.

**Expected evidence:**

- Repeated DNS requests to unusual domains.
- Abnormally long or high-entropy DNS subdomains.
- Unusual outbound traffic volume.
- Connections to destinations outside the normal communication profile.
- Large or repeated outbound transfers.

**Detection approach:**

Security Onion can support network traffic investigation when relevant DNS and network telemetry is available. Kibana can help correlate network events with endpoint activity and the earlier stages of the attack.

Encrypted traffic limits the visibility of payload contents. Therefore, analysts may need to rely on metadata, destination information, connection timing, traffic volume, and endpoint evidence.

### 6.7 Phase 1 Summary
The attack simulation establishes a sequence of behaviors that defenders can use to design and test detection logic.

The expected progression is initial access, persistence, privilege escalation, lateral movement, and possible data exfiltration. Each stage produces potential evidence that can be investigated through endpoint monitoring, network telemetry, and security logs.

No claim of successful compromise is made without corresponding laboratory evidence.

## 7. Phase 2: Attack Detection and Investigation (Blue Team)
### 7.1 Detection Overview
The Blue Team phase focuses on identifying suspicious activity and determining whether separate events are related to the same incident.

The investigation should consider the event timestamp, affected endpoint, account, process, source and destination addresses, and associated alerts.

### 7.2 Detecting Suspicious Activities with Wazuh
Wazuh can be configured to collect and analyze endpoint logs from supported operating systems.

The investigation focuses on:

- Repeated authentication failures.
- Successful logins following multiple failures.
- Unexpected process execution.
- Suspicious PowerShell activity.
- Unauthorized scheduled-task changes.
- Changes to monitored files.
- Unexpected administrative activity.
Analysts should review the original event details rather than relying only on an alert title or severity score.

**Expected investigation output:** A list of relevant endpoint events, their timestamps, affected systems, associated accounts, and explanations of why each event requires further review.

### 7.3 Network Investigation with Security Onion
Security Onion is used in the proposed workflow to investigate network-related indicators.

The analyst examines:

- DNS requests and responses.
- Unusual outbound connections.
- Communication with suspicious destinations.
- Connections between internal hosts.
- Repeated connections occurring around the same time as endpoint alerts.
- Network patterns consistent with possible data exfiltration.

The presence of an unusual connection does not independently establish malicious activity. Analysts should validate the destination, compare it with expected network behavior, and correlate it with other evidence.

### 7.4 Log Hunting with Kibana
Kibana can be used to search and investigate indexed logs.

Example searches for an Elasticsearch data view with compatible field mappings are shown below. These are illustrative queries; actual field names depend on the collected data.

**Search for failed authentication events**

event.category:authentication AND event.outcome:failure

**Search for PowerShell-related events**

process.name:powershell.exe

**Search for DNS events**

dns.question.name:*

**Search for network connections**

event.category:network

**Search for events associated with a particular host**

host.name:"TEST-WINDOWS-01"

The queries must be adapted to the actual data view and field mappings. Some data sources may use different field names, and a query will not return meaningful results if the relevant events have not been collected.

### 7.5 Correlating Events from Multiple Sources
Event correlation is important because an APT attack rarely depends on a single suspicious action.

The investigation should attempt to correlate:

1. Initial suspicious endpoint activity.
2. Unexpected persistence-related changes.
3. Privilege or account-related events.
4. Remote authentication and execution.
5. Unusual outbound network connections.

For example, an unexpected PowerShell process followed by the creation of a scheduled task and an unusual outbound connection would justify further investigation if the events occurred on the same endpoint within a related time window.

The analyst should verify whether the events share a common host, user account, process identifier, network indicator, or other reliable attribute.

### 7.6 Investigating Files, URLs, and IP Addresses
TheHive and Cortex can support the investigation when the relevant integrations and analyzers are configured.

The analyst records each observable, such as:

- Suspicious IP address.
- Domain name.
- URL.
- File hash.
- Hostname.
- Associated account.
- Relevant process or event identifier.

Cortex analyzers can be used to enrich supported observables with external or local intelligence. The availability of results depends on the installed analyzers, API configuration, and accessible intelligence sources.

The analyst should document the source, timestamp, and reliability of every enrichment result. An IP address or domain should not be labelled malicious solely because it is unfamiliar.

### 7.7 Incident Case Management with TheHive
A case can be created to organize the investigation.

**Example case details:**
- Case title: Suspected Multi-Stage APT Activity.
- Severity: To be determined from evidence.
- Status: Investigation.
- Affected asset: TEST-WINDOWS-01.
- Incident category: Suspected endpoint compromise.
- Evidence sources: Endpoint logs, authentication events, and network telemetry.
- Assigned tasks: Validate initial access, investigate persistence, review authentication, examine network connections, and identify potential data exposure.

The analyst records the evidence, investigation steps, observations, and recommended response actions in the case.

The case should be marked confirmed only when the available evidence supports that conclusion.

### 7.8 Reconstructing the Attack Timeline
A timeline helps the investigation team understand the sequence of events.

The following is an illustrative timeline for the simulated scenario, not a record of events collected from a running laboratory.

| Time | Simulated event | Relevant evidence |
| --- | --- | --- |
| 09:00 | Possible initial access | Endpoint or email events |
| 09:05 | Suspicious process execution | Process creation logs |
| 09:10 | Possible persistence activity | Task Scheduler events |
| 09:15 | Possible privilege escalation | Security and process logs |
| 09:25 | Possible lateral movement | Authentication and remote service logs |
| 09:35 | Unusual DNS or outbound connections | Network monitoring records |
| 09:45 | Alert review begins | Wazuh and Security Onion alerts |
| 10:00 | Evidence correlation | Kibana searches and log analysis |
| 10:15 | Incident case documentation | TheHive case records |
| 10:30 | Response recommendations prepared Investigation notes and IoCs |

Actual timestamps should be replaced with the timestamps observed during a real exercise. The illustrative sequence does not establish that the attack occurred.

### 7.9 Forensic Investigation Findings
The following findings describe what the analyst would seek to validate during the investigation.

**Finding 1: Initial access**

Review endpoint and, where applicable, email events for suspicious files, links, or execution activity. Determine whether the activity originated from a user action, an application, or another source.

**Finding 2: Persistence**

Examine scheduled-task events and related process information. Determine whether the change was approved and identify the account responsible.

**Finding 3: Privilege escalation**

Review privilege-related events, process activity, and account changes. Establish whether any access exceeded the user's authorized permissions.

**Finding 4: Lateral movement**

Compare authentication events and network connections across affected endpoints. Identify unusual remote logons or remote execution behavior.

**Finding 5: Possible data exfiltration**

Review DNS and outbound connection metadata. Determine whether the traffic volume, frequency, destination, and timing are consistent with authorized activity.

These findings remain investigation hypotheses until supported by actual logs or other evidence.

## 8. Phase 3: Threat Intelligence and Mitigation
### 8.1 Identifying Indicators of Compromise
Indicators of Compromise are artifacts or observations that may indicate malicious activity.

Examples include:

- Suspicious IP addresses.
- Malicious or suspicious domains.
- URLs associated with an incident.
- File hashes.
- Unexpected scheduled tasks.
- Unusual process command lines.
- Repeated authentication failures.
- Unusual outbound DNS or HTTP connections.

IoCs must be extracted from observed evidence or a clearly identified training dataset. Example indicators should not be presented as real attacker infrastructure without verification.

### 8.2 Recording IoCs in MISP
MISP can be used to create an event for the simulated incident.

**Example event details:**

- Event information: Simulated Multi-Stage APT Investigation.
- Threat level: Undetermined until the scenario and evidence are assessed.
- Analysis: Ongoing or completed, depending on the actual investigation.
- Distribution: Restricted to the authorized training environment.
- Attributes: Verified lab IP addresses, domains, file hashes, and other relevant observables.
- 
Each attribute should include its type, value, source, and contextual description.

Synthetic indicators should be labelled as test data. Real threat intelligence should be shared only when permitted by the applicable data-handling and disclosure policies.

### 8.3 Sigma Detection Rule
The following example Sigma rule identifies repeated failed logon events associated with a Windows host.

It demonstrates a detection concept; it does not independently establish a brute-force attack.
```
title: Repeated Windows Logon Failures
id: 8e1d30b4-6f22-4e7a-9b71-2f7e3b4c9101
status: experimental
description: >
Identifies Windows failed logon events for further investigation.
Correlation across repeated events is required to identify
potential brute-force behavior.
logsource:
product: windows
service: security
detection:
selection:
EventID: 4625
condition: selection
level: medium
tags:
 - attack.credential_access
 - attack.t1110
```
**Explanation:**

- Event ID 4625 represents a failed Windows logon.
- The rule identifies matching events.
- Repeated failures should be grouped by relevant fields, such as source address, account, and time window, using the capabilities of the target detection platform.
- The rule must be validated against the actual log source and supported Sigma backend.

### 8.4 YARA Detection Rule
The following example demonstrates how YARA can identify a specific marker in a controlled training file.
```
rule Simulated_APT_Lab_Marker
{
meta:
description = "Detects a synthetic marker used in an APT training
exercise"
author = "SOC Training Lab"
purpose = "Demonstration only"
strings:
$marker = "APT_LAB_SIMULATION_MARKER"
condition:
$marker
}
```
**Explanation:**

This rule matches a file containing the exact synthetic marker. It is not a general -purpose malware detection rule and does not detect all APT-related files.

The rule should be tested against a benign training file containing the marker and an unrelated file that does not contain it. Any resulting matches should be documented.

### 8.5 Mitigation Plan
The following mitigation measures address the attack stages included in the scenario.

**Initial access**

- Apply security updates and patches.
- Implement email filtering and attachment inspection.
- Use multifactor authentication where supported.
- Educate users about suspicious links and attachments.
- Apply least-privilege access controls.

**Persistence**

- Audit scheduled-task creation and modification.
- Monitor changes to startup mechanisms and services.
- Review endpoint security alerts.
- Remove unauthorized persistence mechanisms after validation.

**Privilege escalation**

- Apply operating system and application security updates.
- Limit administrative privileges.
- Monitor privileged account activity.
- Review group membership and permission changes.
- Investigate suspicious process execution.

**Lateral movement**

- Restrict unnecessary administrative access between systems.
- Use separate privileged accounts.
- Apply network segmentation.
- Monitor remote logons and service creation.
- Use appropriate protections for credential material.

**Data exfiltration**

- Monitor outbound network connections.
- Apply DNS filtering and logging.
- Investigate unusual DNS patterns.
- Restrict unnecessary outbound communication.
- Apply data loss prevention controls where appropriate.
- Review unusual transfer volumes and destinations.

**Detection and response**

- Centralize endpoint and network logs.
- Synchronize timestamps across systems.
- Validate detection rules regularly.
- Maintain an incident response procedure.
- Preserve relevant evidence before making changes when operationally feasible.
- Retest controls after remediation.

## 9. Incident Response Procedure
The following response procedure outlines how a SOC team should handle a suspected multi -stage APT incident.

### Step 1: Identification
Review alerts from Wazuh and Security Onion. Confirm whether the available events indicate suspicious activity requiring investigation.

### Step 2: Initial Triage
Identify affected hosts, users, event timestamps, suspicious processes, and relevant network connections. Determine the potential scope and severity.

### Step 3: Evidence Preservation
Preserve relevant endpoint logs, network records, file hashes, alert details, and timestamps. Maintain appropriate access controls and document evidence handling.

### Step 4: Investigation
Use Kibana to search and correlate events. Examine endpoint and network telemetry to determine whether the observed behavior is related.

### Step 5: Containment
When compromise is sufficiently suspected, follow the organization's incident response procedures to
isolate affected systems, disable compromised accounts where appropriate, and block validated malicious indicators.

Containment actions should be coordinated to minimize operational disruption and preserve evidence.

### Step 6: Eradication
Remove confirmed malicious artifacts, unauthorized persistence mechanisms, and unauthorized access. Patch exploited vulnerabilities and rotate credentials or other secrets when exposure is suspected.

### Step 7: Recovery
Restore affected systems using trusted procedures. Validate security controls, monitor the systems for recurring suspicious activity, and confirm that required services operate normally.

### Step 8: Lessons Learned
Document the root cause where established, detection gaps, response delays, remediation actions, and improvements required to reduce the likelihood of recurrence.

## 10. Security Gaps and Recommendations
Potential gaps to evaluate during the exercise include:

| Security area | Potential gap | Recommended improvement |
| --- | --- | --- |
| Endpoint monitoring | Missing process or authentication logs | Enable and validate relevant audit policies |
| Network visibility | Incomplete DNS or outbound traffic records | Improve sensor coverage and logging |
| Detection rules | Excessive false positives or missed events | Test and tune detection logic |
| Authentication | Excessive privileges or weak authentication | Enforce least privilege and MFA |
| Log correlation | Inconsistent timestamps or field mappings | Synchronize clocks and normalize logs |
| Threat intelligence | Indicators lack context | Record provenance and validate observables |
| Incident response | Unclear containment responsibilities | Maintain and test response procedures |
| Data protection | Limited visibility into outbound transfers | Apply suitable egress monitoring and data controls |

These are areas to assess, not confirmed weaknesses in a real environment.

## 11. Expected Results
The simulated exercise is designed to demonstrate the following outcomes:

- Identification of security events associated with common APT stages.
- Investigation of authentication, process, and network activity.
- Correlation of related events across available data sources.
- Creation of an incident case with documented observables.
- Reconstruction of an illustrative event timeline.
- Preparation of sample Sigma and YARA detection rules.
- Organization of validated indicators in MISP.
- Development of a structured incident response and mitigation plan.

The actual results depend on the laboratory configuration, event generation, log collection, rule deployment, and validation performed.

## 12. Challenges and Limitations
Several challenges may affect the effectiveness of the investigation:

- Missing or incomplete endpoint logs.
- Limited network visibility.
- Differences in field names and event formats.
- False-positive security alerts.
- Encrypted network traffic that limits payload inspection.
- Unavailable or incorrectly configured tool integrations.
- Incomplete threat intelligence.
- Difficulty distinguishing authorized administrative activity from malicious behavior.

The exercise also illustrates that installing security tools alone is not sufficient. Effective detection requires correct configuration, appropriate data sources, validated detection rules, and analyst investigation.

## 13. Conclusion
This project describes a structured approach to simulating and investigating a multi -stage Advanced Persistent Threat using open-source SOC technologies.

The Red Team phase models common attacker behaviors, including initial access, persistence, privilege escalation, lateral movement, and potential data exfiltration. The Blue Team phase explains how endpoint and network alerts can be investigated, correlated, and documented. The threat intelligence and mitigation phase demonstrates how IoCs, detection rules, and incident response procedures can support future prevention and detection.

Security Onion and Wazuh provide complementary network and endpoint monitoring capabilities, while Kibana supports log investigation. TheHive and Cortex support incident case management and observable analysis, and MISP provides a platform for organizing threat intelligence. YARA and Sigma demonstrate two approaches to creating reusable detection logic.

The overall workflow emphasizes evidence-based investigation, timely containment, remediation of security gaps, and continuous improvement of detection capabilities.

A successful practical validation would require collecting actual laboratory events, testing the detection rules, documenting observed alerts, and recording the results of each response action. The simulated findings in this report should be distinguished from results verified in a running environment.

## 14. References
1. Security Onion Documentation — https://docs.securityonion.net/
2. Wazuh Documentation — https://documentation.wazuh.com/
3. Elastic Kibana Documentation — https://www.elastic.co/guide/en/kibana/current/index.html
4. TheHive Documentation — https://docs.strangebee.com/thehive/
5. Cortex Documentation — https://docs.strangebee.com/cortex/
6. MISP Project — https://www.misp-project.org/
7. YARA Documentation — https://yara.readthedocs.io/
8. Sigma Documentation — https://sigmahq.io/
9. MITRE ATT&CK Framework — https://attack.mitre.org/
10. NIST Computer Security Incident Handling Guide — https://csrc.nist.gov/pubs/sp/800/61/r2/final

**Project status:** Detailed simulated exercise and investigation workflow prepared. Actual tool execution, alert generation, indicator validation, and forensic findings require practical verification in an authorized laboratory environment.
