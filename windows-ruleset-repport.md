# Wazuh Rule Set Report

This report provides a summary of the Wazuh ruleset configurations related to Windows events, focusing on successful logins, privileged user logins, failed login attempts, account management events, and operational management events.

---

## Core Util Ruleset

| Rule ID | Level | Description                              | Fields / Match        | MITRE IDs | Group |
| ------- | ----- | ---------------------------------------- | --------------------- | --------- | ----- |
| 100100  | 0     | Parent rule for privileged events rules. | Elevated Token: `yes` |           |       |

---

## Windows User Logon Events

### Successful Logon

| Rule ID | Level | Description                                          | Fields / Match | MITRE IDs    | Group                            |
| ------- | ----- | ---------------------------------------------------- | -------------- | ------------ | -------------------------------- |
| 100101  | 12    | Successful Physical Interactive Logon                | Logon Type: 2  | T1078        | windows, authentication\_success |
| 100102  | 7     | Successful Batch Logon On Behalf Of A User           | Logon Type: 4  | T1569, T1053 | windows, authentication\_success |
| 100103  | 3     | A service was started by the Service Control Manager | Logon Type: 5  | T1569, T1053 | windows, authentication\_success |
| 100104  | 3     | This workstation was unlocked                        | Logon Type: 7  | T1078        | windows, authentication\_success |
| 100105  | 13    | The user's password was passed in cleartext form     | Logon Type: 8  | T1569, T1078 | windows, authentication\_success |
| 100106  | 12    | Windows Logon using New Credentials (RunAs /netonly) | Logon Type: 9  | T1569, T1078 | windows, authentication\_success |
| 100107  | 10    | Successful RDP Interactive Logon                     | Logon Type: 10 | T1078        | windows, authentication\_success |
| 100108  | 10    | First time this user logged in this system           |                | T1078        | windows, authentication\_success |

### Privileged User Successful Logon

| Rule ID | Level | Description                                                                 | Fields / Match                     | MITRE IDs           | Group                                                     |
| ------- | ----- | --------------------------------------------------------------------------- | ---------------------------------- | ------------------- | --------------------------------------------------------- |
| 100110  | 15    | Successful Physical Interactive Logon As Privileged User                    | Logon Type: 2, Parent rule 100100  | T1078, T1668        | windows, authentication\_success, privileged              |
| 100111  | 10    | Successful Batch Logon On Behalf Of A Privileged User                       | Logon Type: 4, Parent rule 100100  | T1068, T1569, T1053 | windows, authentication\_success, privileged              |
| 100112  | 4     | A service was started by the Service Control Manager With Privileged Access | Logon Type: 5, Parent rule 100100  | T1068, T1569, T1053 | windows, authentication\_success, privileged              |
| 100113  | 4     | This Admin workstation was unlocked                                         | Logon Type: 7, Parent rule 100100  | T1078               | windows, authentication\_success, privileged              |
| 100114  | 15    | The Admin user's password was passed in cleartext form                      | Logon Type: 8, Parent rule 100100  | T1078, T1569        | windows, authentication\_success, privileged              |
| 100115  | 9     | Windows Logon using Privileged New Credentials (RunAs /netonly)             | Logon Type: 9, Parent rule 100100  | T1078, T1134        | windows, authentication\_success, privileged              |
| 100116  | 12    | Successful RDP Interactive Logon With Privileged Account                    | Logon Type: 10, Parent rule 100100 | T1078               | windows, authentication\_success, persistence, privileged |
| 100117  | 10    | First time this privileged user logged in this system                       |                                    | T1078               | windows, authentication\_success, privileged              |

---

## Failed Logon

| Rule ID | Level | Description                  | Fields / Match | MITRE IDs | Group                           |
| ------- | ----- | ---------------------------- | -------------- | --------- | ------------------------------- |
| 100118  | 4     | Windows Failed Logon Attempt | Event ID 4625  | T1110     | windows, authentication\_failed |

---

## Failed Logon Attempts

| Rule ID | Level | Frequency | Timeframe | Description                                                       | Fields / Match     | MITRE IDs | Group                                |
| ------- | ----- | --------- | --------- | ----------------------------------------------------------------- | ------------------ | --------- | ------------------------------------ |
| 100119  | 3     | 3         | 60s       | More than 3 failed login attempts by the same user in 60 seconds  | Parent rule 100115 | T1110     | authentication\_failed, brute\_force |
| 100120  | 7     | 5         | 60s       | More than 5 failed login attempts by the same user in 60 seconds  | Parent rule 100115 | T1110     | authentication\_failed, brute\_force |
| 100121  | 12    | 10        | 60s       | More than 10 failed login attempts by the same user in 60 seconds | Parent rule 100115 | T1110     | authentication\_failed, brute\_force |
| 100122  | 12    | 25        | 3600s     | More than 25 failed login attempts by the same user in 60 seconds | Parent rule 100115 | T1110     | authentication\_failed, brute\_force |

---

## Windows Account Management Events

| Rule ID | Level | Description                                 | Fields / Match                    | MITRE IDs | Group                                       |
| ------- | ----- | ------------------------------------------- | --------------------------------- | --------- | ------------------------------------------- |
| 100123  | 7     | New User Account Created                    | Event ID ^624\$ or ^4720\$        | T1136     | windows, account\_changed, adduser          |
| 100124  | 12    | User Account Enabled                        | Event ID ^626\$ or ^4722\$        | T1098     | windows, account\_changed                   |
| 100125  | 9     | User attempt to reset an account's password | Event ID ^628\$                   | T1098     | windows, account\_changed                   |
| 100126  | 8     | User Account Changed                        | Event ID ^685\$, ^4738\$, ^4781\$ | T1098     | windows, account\_changed                   |
| 100127  | 12    | User Account deleted                        | Event ID ^630\$ or ^4726\$        | T1098     | windows, account\_changed                   |
| 100128  | 7     | User Account disabled                       | Event ID ^629\$ or ^4725\$        | T1098     | windows, account\_changed                   |
| 100129  | 15    | User Added to Domain Admins Group           | Event ID 18107                    | T1098     | windows, privilege\_escalation, persistence |
| 100130  | 5     | Windows: Computer account changed           | Event ID ^646\$ or ^4742\$        | T1098     | windows, account\_changed                   |
| 100131  | 11    | Windows: Computer account added             | Event ID ^645\$ or ^4741\$        | T1098     | windows, account\_changed                   |
| 100132  | 12    | Windows: Computer account deleted           | Event ID ^647\$ or ^4743\$        | T1098     | windows, account\_changed                   |
| 100133  | 15    | Audit Log Cleared                           | Event ID 18107                    | T1070.001 | windows, log\_integrity                     |
| 100134  | 11    | Service Installed on System                 | Event ID 18107                    | T1071     | windows, service\_creation                  |

---

### Notes:

* This report focuses on key Windows logon events, privilege escalation attempts, and account changes.
* The **MITRE IDs** help link specific attack tactics and techniques to the respective rules.
* **Groupings** indicate the category of the event, such as `authentication_success`, `authentication_failed`, `privileged`, and more.
