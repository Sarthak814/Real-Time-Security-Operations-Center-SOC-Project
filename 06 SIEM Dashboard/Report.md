# Custom SIEM Security Monitoring Dashboard Using the ELK Stack

## 1. Executive Summary
Security Information and Event Management (SIEM) systems centralize security records to help analysts identify suspicious activities, investigate security incidents, and prioritize potential threats.

This project presents a custom SIEM security monitoring dashboard design using the Elastic Stack, consisting of Elasticsearch and Kibana. Elasticsearch is used to store and search event records, while Kibana provides visualizations and dashboard functionality.

The proposed dashboard focuses on three security event categories: repeated authentication failures, unexpected privilege-related activity, and unusually large outbound data transfers. It also includes a security event timeline and a recent-events table to support investigation.

All sample records in this report are fictional and intended for demonstration. This project describes a dashboard design concept and does not claim that real attacks have been detected or that a live monitoring system has been fully implemented.

## 2. Objectives
The main objectives of this project are:

- To demonstrate a centralized view of security events.
- To visualize simulated brute-force login activity.
- To highlight simulated privilege-related events.
- To display potentially unusual outbound data transfers.
- To visualize security event trends over time.
- To provide a searchable table containing important event details.
- To demonstrate how a SIEM dashboard can help analysts review and prioritize suspicious activities.

## 3. Tools and Technologies
**Elasticsearch:** Stores, indexes, and makes security event records searchable.

**Kibana:** Provides data exploration, visualizations, and dashboard functionality.

**JSON:** Represents structured sample security events using fields such as timestamps, usernames, source IP addresses, event categories, and severity levels.

**Windows:** Serves as the intended project environment for the local Elastic Stack setup.

**Logstash or Elastic Agent (optional):** Can be used to collect, process, and forward event data depending on the selected ingestion method. Their configuration is not claimed as completed in this project.

## 4. Methodology
The proposed project follows these steps:

1. Prepare fictional security event records in JSON format.
2. Define a consistent structure for event fields, including timestamp, event category, source IP address, username, severity, and message.
3. Index the sample records into Elasticsearch.
4. Create a Kibana data view that recognizes the event timestamp field.
5. Build visualizations for authentication failures, privilege-related events, outbound data transfers, and event trends.
6. Arrange the visualizations into a single security monitoring dashboard.
7. Include a recent-events table to provide additional information about individual records.
8. Review the displayed events and identify records that may require further investigation.

These steps describe the intended workflow. The report does not claim that every implementation step has been completed in a live Kibana environment.

## 5. Conceptual Data Flow
The proposed dashboard follows this data flow:

**Fictional JSON Events → Elasticsearch Index → Kibana Data View → Dashboard Visualizations**

The JSON records provide structured sample data. Elasticsearch is intended to store and index those records, allowing Kibana to retrieve and display them through charts, timelines, and tables.

In a production environment, the same general architecture could be connected to appropriate log sources, such as authentication logs, endpoint security records, and network monitoring data.

## 6. Dashboard Components
### 6.1 Brute-Force Attempts
This visualization displays failed authentication events grouped by source IP address.

Repeated authentication failures from a single source may indicate a password-guessing attempt. An analyst can review the number of failures, affected accounts, event timestamps, and whether a successful login occurred afterward.

A high number of failed logins alone does not prove that an account has been compromised.

### 6.2 Privilege-Related Activity
This visualization displays simulated privilege-related events grouped by username.

Unexpected privilege changes may require investigation because unauthorized privilege escalation can allow an attacker to gain additional access to a system.

An analyst should review the event details, affected account, host, and whether the activity corresponds to an approved administrative operation.

The sample events in this report do not establish that a real privilege escalation occurred.

### 6.3 Possible Data Exfiltration
This visualization displays simulated outbound data-transfer events grouped by source IP address.

Unusually large outbound transfers may warrant investigation because they can sometimes indicate unauthorized data movement.

However, large transfers may also result from legitimate backups, software updates, or approved business operations. Analysts should examine the destination, transfer volume, application, normal activity patterns, and business context before drawing conclusions.

### 6.4 Security Events Over Time
This time-series visualization displays the number of security events across a selected time range.

It allows an analyst to compare event categories and identify periods with increased activity. Sudden increases in authentication failures or other suspicious events may indicate a need for closer investigation.

The visualization should be interpreted alongside the underlying records rather than treated as conclusive evidence of an attack.

### 6.5 Recent Security Events
This table displays individual event records with the following fields:

- Timestamp
- Severity
- Source IP address
- Username
- Event category
- Event message

The table provides additional context for investigating events highlighted by the dashboard visualizations.

## 7. Sample Security Event Records
The following records are fictional examples intended to demonstrate the proposed dashboard.

| Timestamp (UTC) | Event Category | Source IP | User | Severity | Message |
| --- | --- | --- | --- | --- | --- |
| 2026-10-12 09:00 | Brute-force | 192.0.2.10 | admin | 3 | Failed login attempt |
| 2026-10-12 09:03 | Brute-force | 192.0.2.10 | admin | 2 | Successful login after failures |
| 2026-10-12 09:05 | Privilege escalation | 192.0.2.20 | testuser | 4 | Unexpected privilege elevation |
| 2026-10-12 09:10 | Data exfiltration | 192.0.2.30 | analyst | 4 | Large outbound data transfer |

The IP addresses shown above belong to documentation ranges reserved for examples. They are not presented as real attacker addresses.

The severity numbers are illustrative labels. A real implementation would require a documented severity scale and consistent mapping of events to severity levels.

### 7.1 Interpretation of Sample Events
**Event 1: Failed Login Attempt**

The record represents a failed authentication attempt against the admin account. Repeated failures from the same source may warrant further review.

**Event 2: Successful Login After Failures**

The record represents a successful login following earlier failures. An analyst would correlate the events by account, source, and time window to determine whether the sequence is suspicious.

**Event 3: Unexpected Privilege Elevation**

The record represents a simulated privilege-related event affecting the testuser account. The event would require validation against the system's expected administrative activity.

**Event 4: Large Outbound Data Transfer**

The record represents a simulated outbound-transfer event. An analyst would examine the destination, amount of data transferred, application, and business justification before determining whether the activity is suspicious.

## 8. Proposed Investigation Workflow
When a dashboard highlights a suspicious event, an analyst should follow a structured investigation process.

### Step 1: Select the Relevant Time Range
Confirm that the dashboard's time range includes the event. Use consistent timestamp handling, preferably UTC when comparing records from multiple sources.

### Step 2: Examine the Underlying Event Records
Open the relevant records rather than relying only on aggregated chart values. Review the event message, username, source IP address, timestamp, and available host or destination information.

### Step 3: Validate the Event Context
Determine whether the activity is associated with a known user, expected system behavior, approved administration, or a legitimate data transfer.

### Step 4: Correlate Related Events
Look for related authentication attempts, successful logins, privilege changes, or network events occurring within the relevant time window.

### Step 5: Document the Finding
Record the evidence, affected account or system, relevant timestamps, investigation results, and recommended next steps.

### Step 6: Escalate When Appropriate
If the available evidence supports further investigation, escalate the event according to the organization's security incident response procedures.

## 9. Conceptual Detection Logic
The following detection ideas describe how the proposed dashboard could support security monitoring.

They are conceptual examples, not tested Elasticsearch queries or configured correlation rules.

### 9.1 Repeated Authentication Failures
Group failed authentication events by source IP address and username over a defined time window. Flag groups that exceed a configured threshold for analyst review.

### 9.2 Successful Login After Repeated Failures
Correlate repeated failed authentication attempts with a later successful login for the same account or source within a defined interval.

This pattern may warrant investigation, but it does not automatically prove that the login was malicious.

### 9.3 Privilege-Related Events
Identify events mapped to a privilege-change category and review whether the activity was expected and authorized.

### 9.4 Unusually Large Outbound Transfers
Compare outbound-transfer volumes against a defined threshold or normal activity baseline. Review the destination, application, timing, and business context before classifying the event as suspicious.

For operational use, detection logic requires consistent event fields, defined time windows, suitable thresholds, exception handling, and testing against actual log data.

## 10. Validation and Expected Results
A live implementation should be validated to confirm that the data pipeline and dashboard behave as intended.

Validation should include:

- Confirming that sample records are successfully ingested into Elasticsearch.
- Verifying that timestamp fields are parsed correctly.
- Checking that required fields are available and consistently named.
- Confirming that dashboard visualizations display the expected event counts.
- Testing filters and time-range selections across the relevant panels.
- Reviewing whether known test events appear in the appropriate categories.
- Checking for false positives and false negatives before relying on any detection rule.
- Confirming that any configured alerts behave according to their documented conditions.

For this report, the expected result is a clear dashboard design specification showing how fictional security events can be organized for analyst review. No live ingestion, alert execution, or detection accuracy is claimed.

## 11. Limitations
The project has the following limitations:

- All sample event records and displayed values are simulated.
- The dashboard illustration is a mockup, not a screenshot of a verified, running Kibana dashboard.
- The report does not demonstrate a completed live data-ingestion pipeline.
- No connection to real security log sources is claimed.
- The detection ideas are conceptual and have not been presented as tested correlation rules.
- Automated security alerts and production incident-response integrations are not claimed as configured.
- A real deployment would require data-source integration, field mapping, timestamp normalization, alert thresholds, access controls, and validation.

These limitations distinguish the dashboard design from an operational SIEM implementation.

## 12. Security and Data Considerations
A real SIEM deployment may contain sensitive information, including usernames, IP addresses, authentication events, and details about internal systems.

Access to security logs and dashboards should therefore be restricted to authorized personnel. Log retention, access permissions, and data sharing should follow the organization's security policies.

The fictional records used in this report are intended for safe demonstration and are not evidence of real security incidents.

## 13. Conclusion
This project presents a custom SIEM security monitoring dashboard design using Elasticsearch and Kibana. The proposed dashboard focuses on repeated authentication failures, privilege-related activity, potentially unusual outbound data transfers, security event trends, and recent event details.

The design demonstrates how security event information can be organized into a centralized view to help analysts review records and prioritize potential threats.

The sample events illustrate possible investigation scenarios, while the proposed detection logic explains how relevant patterns could be identified and reviewed.

To progress from a design concept to a working SIEM project, the dashboard must be implemented in Kibana, connected to a defined data source, and validated using documented test events and detection criteria.
