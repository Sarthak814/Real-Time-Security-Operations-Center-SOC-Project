Development and Testing of a Custom SIEM
Correlation Rule for Credential Stuffing Detection
Using the ELK Stack
1. Abstract
Credential stuffing is a cyberattack in which attackers use previously compromised usernames and
passwords to attempt unauthorized access to other systems. Detecting this activity is important because
repeated authentication attempts can lead to account compromise.
This project focuses on developing a custom Security Information and Event Management (SIEM) detection
rule using the ELK Stack. Elasticsearch is used to store and search authentication events, while Kibana is
used to analyze logs and configure detection logic. Log collection is performed through an appropriate log
collection mechanism.
The detection rule identifies repeated authentication failures originating from the same source IP address
within a specified time window. Threshold-based detection is used to identify suspicious login patterns.
Controlled synthetic authentication events are used to validate the detection logic.
2. Introduction
Authentication systems generate logs containing information about login attempts, usernames,
timestamps, source IP addresses, and authentication outcomes. Analyzing these logs can help identify
suspicious activity.
Credential stuffing differs from traditional brute-force attacks because it relies on previously exposed
credentials rather than necessarily trying many different passwords against a single account. Attackers may
target multiple accounts using the same source IP address or distribute attempts across several addresses.
A SIEM system can help identify suspicious authentication patterns by collecting and correlating security
events.
3. Objectives
• Understand credential stuffing and its indicators.
• Collect and analyze authentication logs.
• Develop a custom detection query using Kibana Query Language (KQL).
• Configure a threshold-based detection rule.
• Test the detection logic using synthetic authentication events.
• Identify potential false positives and improve detection reliability.
4. Tools and Technologies
• Kali Linux
• Oracle VirtualBox
• Elasticsearch
• Kibana
• Elastic Agent or Logstash, depending on the log collection configuration
• Synthetic authentication logs in JSON format
5. Attack Overview
Credential stuffing involves attempting to authenticate with username-and-password combinations
obtained from previous data breaches or other sources.
Common indicators include repeated failed login attempts, multiple accounts targeted by the same source,
and suspicious successful logins following a series of failures.
A single failed login is not sufficient evidence of an attack. However, a large number of failures within a
short period may indicate automated authentication attempts.
6. System Architecture
Authentication Logs → Log Collection → Elasticsearch → Kibana → Custom Detection Rule → Alert
Investigation
Authentication events are collected and indexed in Elasticsearch. Kibana provides the interface for
searching the events and configuring the detection rule. When the rule's conditions are satisfied, the
configured detection mechanism can generate an alert.
7. Log Collection and Preparation
Authentication events should contain the following fields where available:
• @timestamp: Date and time of the event.
• source.ip: Source IP address.
• user.name: Username involved in the attempt.
• event.category: Event category.
• event.outcome: Authentication result.
• host.name: Destination host.
The field names depend on the log source and ingestion configuration.
For this project, authentication events are stored in an appropriate Elasticsearch index or data stream,
such as logs-auth-lab.
8. Detection Rule Development
Rule name: LAB-001 - Possible Credential Stuffing
Rule type: Threshold
Detection objective: Identify repeated failed authentication attempts originating from the same source IP
address.
Detection query
event.category: "authentication"
and event.outcome: "failure"
and source.ip: *
The query filters authentication failures that contain a source IP address.
Rule configuration
Parameter Configuration
Rule ID LAB-001
Rule name Possible Credential Stuffing
Index
logs-auth-lab or the configured authentication
index
Query language KQL
Group by source.ip
Threshold 10 events
Time window 5 minutes
Severity Medium
The threshold rule counts matching events for each source IP address. An alert condition is met when the
configured threshold is reached within the evaluation window.
The five-minute evaluation window must be implemented using the rule's schedule and lookback settings.
Detection enhancement
The rule can be enhanced by examining the number of unique usernames targeted by a source IP address.
For example, five or more distinct usernames within the same period may indicate an attempt to access
multiple accounts.
A successful login following repeated failures can also be investigated as a potentially compromised
account.
9. Testing Methodology
Testing is performed using synthetic authentication events in a controlled environment.
Test Case 1: Repeated Authentication Failures
Synthetic events are generated with ten or more failed authentication attempts from the same test source
IP address within five minutes.
Expected outcome: The events match the KQL query and satisfy the configured threshold.
Test Case 2: Normal Authentication Activity
A separate dataset contains fewer than ten failed authentication attempts from the same source IP
address within five minutes.
Expected outcome: The threshold condition is not met.
Test Case 3: Multiple User Accounts
Synthetic failed-login events contain multiple usernames associated with the same source IP address.
Expected outcome: The events can be grouped and examined to determine whether several accounts are
being targeted.
Test Case 4: Successful Login Following Failures
The test dataset contains repeated failed authentication events followed by a successful authentication
event.
Expected outcome: The analyst can identify the sequence and investigate whether the successful login is
suspicious.
10. Testing Results
The detection logic is designed to identify concentrated authentication failures and distinguish them from
activity that remains below the configured threshold.
The expected positive-test result is that the threshold condition is satisfied when ten or more matching
failures occur from the same source within five minutes. The expected negative-test result is that fewer
than ten matching events do not satisfy the threshold.
The successful execution of the query and generation of an alert must be verified against the actual
Elasticsearch data and Kibana rule configuration.
11. Limitations
• Distributed attacks may use multiple source IP addresses and avoid a per-IP threshold.
• Legitimate users may generate repeated authentication failures.
• Missing or incorrectly mapped authentication fields may prevent detection.
• Thresholds require adjustment according to normal authentication activity.
• Authentication failures alone do not prove that credential stuffing is occurring.
12. Conclusion
This project develops a custom SIEM detection rule for identifying possible credential stuffing through
authentication log analysis. The rule uses KQL to filter failed authentication events and threshold-based
correlation to identify repeated failures from the same source IP address.
The approach demonstrates how authentication logs can be analyzed to identify suspicious patterns and
support security investigations. Additional context, such as the number of accounts targeted and
subsequent successful logins, can improve the quality of the detection.
The effectiveness of the rule depends on correct log collection, field mapping, threshold configuration, and
validation using controlled test events.
13. References
1. Elastic Documentation: https://www.elastic.co/docs/solutions/security/detect-and-alert/
2. Elastic Threshold Rules: https://www.elastic.co/docs/solutions/security/detect-and-alert/threshold
3. MITRE ATT&CK: https://attack.mitre.org/
