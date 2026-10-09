# Development and Testing of a Custom SIEM Detection Rule for Suspicious PowerShell Activity Using the ELK Stack

## 1. Abstract
PowerShell is a command-line shell and scripting language commonly used for Windows administration and automation. Attackers may also abuse PowerShell to execute suspicious commands, download content, decode data, or perform other malicious activities.

This project focuses on developing a custom SIEM detection rule using the ELK Stack to identify potentially suspicious PowerShell execution. Elasticsearch is used to store and search event data, while Kibana provides the interface for analyzing logs and configuring detection rules.

The proposed detection logic searches PowerShell script-block events for selected command indicators, including encoded-command parameters, Base64 decoding functions, and download-related functions. Synthetic test events are used to validate the detection query without executing malicious payloads.

## 2. Introduction
PowerShell provides extensive capabilities for system administration, configuration management, and automation. Its flexibility can also make it attractive to attackers who want to execute commands through legitimate system tools.

Suspicious PowerShell activity may include encoded commands, hidden execution, attempts to bypass execution restrictions, downloading remote content, and unusual process relationships.

A SIEM can monitor relevant Windows events and identify command patterns that warrant investigation. Effective detection requires suitable event collection and an understanding of normal administrative behavior.

## 3. Objectives
- Understand PowerShell abuse and its security indicators.
- Identify the Windows telemetry needed for PowerShell monitoring.
- Collect and analyze PowerShell-related events.
- Develop a custom KQL detection query.
- Validate the query using synthetic event records.
- Investigate matching events and assess false-positive risks.
- Document the detection process and its limitations.

## 4. Tools and Technologies
- Kali Linux
- Oracle VirtualBox
- Elasticsearch
- Kibana
- Elastic Agent or another suitable Windows event collection mechanism
- Windows PowerShell event logs or synthetic PowerShell test events
- KQL for event searching

## 5. Attack Overview
PowerShell exploitation refers broadly to the abuse of PowerShell to perform unauthorized or malicious actions. The term does not mean that every use of PowerShell is an exploit.

Potential indicators include:

- Encoded-command parameters.
- Use of FromBase64String.
- Use of DownloadString.
- Use of Invoke-Expression.
- Hidden-window execution.
- Execution-policy-bypass parameters.
- Suspicious parent-child process relationships.
- Unusual network connections associated with PowerShell.

These indicators can also occur in legitimate administrative scripts. They should be interpreted alongside user, host, process, and network context.

## 6. System Architecture
Windows Endpoint → PowerShell Event Logging → Elastic Agent or Log Collection Pipeline → Elasticsearch → Kibana → Custom Detection Query → Alert Investigation

Windows endpoints generate PowerShell and process-related telemetry. The configured collection mechanism forwards relevant events to Elasticsearch, where Kibana can search and evaluate the detection logic.

When the main SIEM server runs Kali Linux, Windows telemetry must be collected from an authorized Windows endpoint or represented using synthetic test events.

## 7. Log Collection and Preparation
PowerShell detection requires appropriate Windows event collection.

Useful fields include:

- @timestamp: Event time.
- host.name: Host that generated the event.
- user.name: Associated user.
- winlog.event_id: Windows event identifier.
- message: Event content, where mapped and searchable.
- process.name: Process name.
- process.command_line: Command line, if collected.
- process.parent.name: Parent process, if available.

PowerShell Script Block Logging commonly generates Windows event ID 4104. Process creation telemetry can provide additional context about how PowerShell was launched.

For this project, the Windows or synthetic events are stored in an appropriate Elasticsearch index or data stream.

## 8. Detection Rule Development
**Rule name:** LAB-003 - Suspicious PowerShell Command Indicators

**Rule type:** Custom query

**Detection objective:** Identify PowerShell script-block events containing selected command indicators that may warrant investigation.

### Detection query
winlog.event_id: "4104"

and message: (

*EncodedCommand*

or *FromBase64String*

or *DownloadString*

or *Invoke-Expression*

)

This query assumes that the event identifier is available as winlog.event_id and searchable script content is available in message. Actual field names and mappings must match the collected event data.

If the query does not return expected events, inspect a representative event in Kibana Discover and verify the field names and search behavior.

### Rule configuration
| Parameter | Configuration |
| --- | --- |
| Rule ID | LAB-003 |
| Rule name | Suspicious PowerShell Command Indicators |
| Rule type | Custom query |
| Index | Configured Windows or PowerShell event index |
| Query language | KQL |
| Event source | PowerShell Script Block Logging |
| Severity | Medium to High, depending on context |
| Evaluation interval | 5 minutes |
| Lookback | Configured to cover the evaluation interval |

A custom query rule identifies matching events. It does not establish that a system has been exploited merely because a command indicator is present.

### Additional command-line indicators
Where process creation events are collected and mapped appropriately, a supplementary search can examine suspicious PowerShell launch options:

process.name: "powershell.exe"

and process.command_line: (

*-EncodedCommand*

or *-WindowStyle Hidden*

or *-ExecutionPolicy Bypass*

)

This supplementary query depends on the availability of process fields. It may need adjustment for capitalization, process naming, or the actual event schema.

## 9. Testing Methodology
Testing uses synthetic event records and benign control events. The test procedure does not require executing malicious commands or downloading a payload.

### Test Case 1: Encoded-Command Indicator
Create a synthetic script-block event containing the text EncodedCommand.

**Expected outcome:** The event matches the custom detection query if the expected fields are correctly indexed.

### Test Case 2: Base64-Decoding Indicator
Create a synthetic script-block event containing FromBase64String.

**Expected outcome:** The query returns the event.

### Test Case 3: Download-Related Indicator
Create a synthetic script-block event containing DownloadString.

**Expected outcome:** The query returns the event.

### Test Case 4: Normal PowerShell Activity
Create a control event containing a normal administrative command without any of the selected indicators.

**Expected outcome:** The control event does not match this particular query.

### Test Case 5: Process Command-Line Search
Import a synthetic process event containing a PowerShell executable name and one of the selected command-line options.

**Expected outcome:** The supplementary query returns the event if the process fields are correctly mapped and indexed.

### Test Case 6: Alert Validation
Enable the custom query rule and provide matching test data within its evaluation window.

**Expected outcome:** The event is identified by the query and an alert is generated if the rule is configured correctly and executes successfully.

## 10. Testing Results
The detection query is designed to identify script-block events containing one or more of the selected command indicators. The supplementary process query examines potentially suspicious PowerShell launch options.

A successful search match demonstrates that the query can identify the relevant event pattern. It does not, on its own, demonstrate successful exploitation or confirm that a real attack occurred.

The test results must distinguish between query matches and alerts generated by an enabled detection rule. Actual outcomes depend on the collected telemetry, event field mappings, rule configuration, and successful execution.

## 11. Limitations
- PowerShell telemetry may not be collected or indexed by default.
- The query depends on the correct event identifier and searchable content fields.
- Legitimate administrative scripts may match the selected indicators.
- Attackers can use techniques that do not contain the selected strings.
- Process command-line visibility depends on the configured collection mechanism.
- Script-block content and process events may require separate integrations.
- A matching indicator is not conclusive evidence of exploitation.
- 
## 12. Conclusion
This project develops a custom SIEM detection rule for identifying suspicious PowerShell activity using the ELK Stack. The rule searches PowerShell script-block events for selected command indicators, while a supplementary query can examine suspicious process command-line options.

The project demonstrates how endpoint telemetry and custom queries can support security monitoring and investigation. It also highlights the importance of validating field mappings, collecting the correct Windows events, and reviewing matching events in context.

The effectiveness of the detection rule depends on reliable telemetry, appropriate query configuration, controlled testing, and investigation of potential false positives.

## 13. References
1. Elastic Windows Integration: https://www.elastic.co/docs/reference/integrations/windows
2. Elastic Security Detection Rules: https://www.elastic.co/docs/solutions/security/detect-and-alert/
3. Microsoft PowerShell Documentation: https://learn.microsoft.com/en-us/powershell/
4. MITRE ATT&CK: https://attack.mitre.org/
