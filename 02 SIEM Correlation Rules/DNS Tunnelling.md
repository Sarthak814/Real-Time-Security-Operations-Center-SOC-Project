Development and Testing of a Custom SIEM
Detection Rule for DNS Tunnelling Using the ELK
Stack
1. Abstract
DNS tunnelling is a technique in which DNS queries or responses are used to carry data through the
Domain Name System. Attackers may use this technique for covert communication, command-and-control
activity, or unauthorized data transfer.
This project focuses on developing a custom SIEM detection rule using the ELK Stack to identify DNS
activity that may indicate tunnelling. Elasticsearch stores DNS event data, while Kibana provides the
interface for searching, analyzing, and detecting suspicious query patterns.
The proposed detection approach examines DNS query volume, source IP addresses, query types, and
suspicious query-name characteristics. Controlled synthetic DNS events are used to evaluate the detection
logic in a lab environment.
2. Introduction
The Domain Name System translates domain names into IP addresses and is essential for normal network
communication. Because DNS traffic is commonly permitted across networks, attackers may attempt to
misuse it to transfer data or establish covert communication channels.
DNS tunnelling can produce unusual patterns, including long subdomains, repeated requests to a particular
domain, unusually high query volumes, and frequent TXT-record queries.
A SIEM system can help identify these patterns by collecting DNS telemetry and applying custom detection
rules.
3. Objectives
• Understand DNS tunnelling and its common indicators.
• Collect and analyze DNS query logs.
• Develop custom detection queries using KQL.
• Configure a threshold-based rule for unusually high DNS query volume.
• Examine suspicious DNS query types and query-name characteristics.
• Validate the detection logic using synthetic DNS events.
• Document limitations and possible improvements.
4. Tools and Technologies
• Kali Linux
• Oracle VirtualBox
• Elasticsearch
• Kibana
• Elastic Agent, Logstash, or another suitable DNS log collection mechanism
• Synthetic DNS event records
5. Attack Overview
DNS tunnelling abuses the DNS protocol to transport data within DNS queries or responses. Data may be
encoded into subdomains, while responses can carry additional information.
Potential indicators include:
• Unusually long DNS query names.
• Large numbers of unique subdomains under one parent domain.
• Repeated DNS requests from the same host.
• Unusual concentrations of TXT-record queries.
• Query names containing apparently encoded or high-entropy strings.
These characteristics can also occur during legitimate operations. Therefore, multiple indicators should be
considered before classifying activity as malicious.
6. System Architecture
DNS Logs → Log Collection → Elasticsearch → Kibana → Custom Detection Rules → Alert Investigation
DNS events are collected from an appropriate resolver, endpoint, or network monitoring source.
Elasticsearch stores the events, and Kibana is used to examine DNS activity and apply detection logic.
7. Log Collection and Preparation
DNS event records should contain the following fields where available:
• @timestamp: Date and time of the query.
• source.ip: Source IP address.
• dns.question.name: Requested domain name.
• dns.question.type: DNS record type.
• dns.response_code: DNS response status.
• host.name: Host generating the query.
The field names depend on the integration and event format.
For this project, DNS events are stored in an appropriate Elasticsearch index or data stream, such as logsdns-
lab.
8. Detection Rule Development
Rule name: LAB-002 - Suspicious DNS Query Activity
Rule type: Threshold
Detection objective: Identify unusually high DNS query volume from a single source IP address.
Detection query
dns.question.name: *
and source.ip: *
This KQL query selects DNS events containing a query name and source IP address. It does not
independently establish that tunnelling is occurring.
Rule configuration
Parameter Configuration
Rule ID LAB-002
Rule name Suspicious DNS Query Activity
Index
logs-dns-lab or the configured DNS
index
Query language KQL
Group by source.ip
Threshold 100 events
Time window 5 minutes
Severity Medium
The initial threshold is a lab testing value. It should be adjusted according to normal DNS traffic and the
characteristics of the monitored environment.
Supplementary TXT-query search
dns.question.type: "TXT"
and dns.question.name: *
This query identifies DNS TXT queries when the event fields are mapped as expected.
TXT records are used by many legitimate services. A matching event should therefore be investigated
alongside query volume, domain characteristics, and host behavior.
Additional detection indicators
The detection process can be improved by examining:
1. Query-name length.
2. Number of unique subdomains under the same parent domain.
3. Frequency of requests to the same domain.
4. Unusual character distributions or apparently encoded subdomains.
5. TXT-query frequency.
6. Available DNS response sizes and associated endpoint activity.
Query-name length and character-entropy analysis require appropriate field processing or analytics; the
basic KQL queries above do not calculate these values automatically.
9. Testing Methodology
Testing is conducted using synthetic DNS records in a controlled lab environment.
Test Case 1: High DNS Query Volume
Synthetic DNS records are created with at least 100 queries from one test source IP address within five
minutes.
Expected outcome: The query matches the events and the threshold condition is met.
Test Case 2: Normal DNS Activity
A control dataset contains fewer than 100 DNS queries from the same source within the evaluation
window.
Expected outcome: The threshold condition is not met.
Test Case 3: TXT-Record Queries
Synthetic DNS records include TXT queries with valid query names.
Expected outcome: The supplementary KQL query identifies the TXT-query events.
Test Case 4: Unusually Long Query Names
The test dataset includes long subdomains that resemble encoded strings.
Expected outcome: The relevant events can be examined in Kibana. Additional query-name analysis is
required to determine whether they meet a defined length or entropy condition.
Test Case 5: Combined Indicators
A synthetic dataset contains high DNS query volume, repeated requests to one parent domain, and
multiple long subdomains.
Expected outcome: The combined evidence supports further investigation of possible DNS tunnelling.
10. Testing Results
The proposed volume-based rule is designed to identify sources that generate at least 100 matching DNS
events within five minutes. The supplementary query identifies DNS TXT-record requests, while the queryname
analysis provides additional context.
A high query volume or TXT query alone is not sufficient evidence of DNS tunnelling. The final assessment
should consider multiple indicators and the normal behavior of the monitored host.
Actual query matches and alerts must be verified using the configured DNS data source and Kibana rule
execution results.
11. Limitations
• DNS telemetry must be available for the monitored host or network.
• Legitimate applications may generate high DNS query volumes.
• DNS tunnelling may operate at low volumes and avoid a simple threshold.
• Encrypted DNS traffic may limit visibility when suitable endpoint or resolver logs are unavailable.
• TXT records and long query names are not inherently malicious.
• Query-name length and entropy analysis require additional processing or analytics.
12. Conclusion
This project develops a custom SIEM detection approach for identifying suspicious DNS activity that may
indicate DNS tunnelling. The detection logic combines DNS query filtering, source-based thresholds, and
supplementary examination of TXT queries and query-name characteristics.
The approach demonstrates how DNS telemetry can support security monitoring and investigation.
Although threshold rules can identify unusual query volumes, stronger detection requires contextual
analysis and multiple independent indicators.
The effectiveness of the rule depends on reliable DNS telemetry, suitable field mappings, threshold tuning,
and validation using controlled test data.
13. References
1. Elastic Documentation: https://www.elastic.co/docs/solutions/security/detect-and-alert/
2. Elastic Integrations: https://www.elastic.co/docs/reference/integrations
3. MITRE ATT&CK: https://attack.mitre.org/
