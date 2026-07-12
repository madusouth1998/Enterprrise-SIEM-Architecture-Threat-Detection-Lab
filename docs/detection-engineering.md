# Detection Engineering Methodology

## Overview

Detection engineering is the process of designing, implementing, validating, and continuously improving detection logic that identifies malicious or suspicious behavior within an organization's environment.

Within this lab, Microsoft Sentinel serves as the Security Information and Event Management (SIEM) platform, while Kusto Query Language (KQL) is used to transform raw telemetry into actionable security detections.

Rather than relying solely on predefined alerts, detection engineering focuses on understanding attacker behavior, identifying relevant telemetry, reducing false positives, and creating meaningful alerts that support effective incident response.

---

# Detection Engineering Lifecycle

Every detection follows a structured lifecycle.

```
Threat Intelligence

↓

Attack Technique

↓

Telemetry Source

↓

Data Collection

↓

Threat Hypothesis

↓

KQL Development

↓

Validation

↓

False Positive Tuning

↓

Analytics Rule

↓

Incident Creation

↓

Continuous Improvement
```

Each phase contributes to the effectiveness and reliability of the final detection.

---

# Detection Design Principles

Every detection developed within this project follows several core principles:

- Accuracy
- Simplicity
- Repeatability
- Low False Positive Rate
- High Visibility
- Actionable Output
- MITRE ATT&CK Alignment

A detection that generates excessive false positives can overwhelm analysts and reduce confidence in the SIEM.

---

# Telemetry Sources

Detection quality depends on the availability and integrity of telemetry.

The primary telemetry source for this lab is the Windows Security Event Log.

Key Event IDs include:

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4634 | Logoff |
| 4648 | Logon Using Explicit Credentials |
| 4672 | Special Privileges Assigned |

Additional telemetry may include:

- Heartbeat
- Azure Activity Logs
- Azure Resource Logs
- NSG Flow Logs
- Azure Monitor Metrics

---

# Detection Workflow

The engineering process begins by identifying suspicious behavior rather than immediately writing a query.

```
Identify Threat

↓

Understand Windows Event

↓

Locate Relevant Fields

↓

Write Initial Query

↓

Review Results

↓

Reduce Noise

↓

Optimize Query

↓

Deploy Detection

↓

Monitor Performance
```

This iterative approach ensures that detections remain effective over time.

---

# MITRE ATT&CK Mapping

The MITRE ATT&CK framework provides a common language for describing attacker behavior.

Within this project, detections are mapped to the following techniques.

| Technique | ID | Description |
|-----------|----|-------------|
| Brute Force | T1110 | Password guessing attacks |
| Valid Accounts | T1078 | Abuse of legitimate credentials |
| Remote Services | T1021 | Remote access via services such as RDP |
| Account Discovery | T1087 | Enumeration of user accounts |

MITRE mapping helps analysts understand the adversary tactics associated with each alert.

---

# Detection Categories

The lab focuses on several common detection scenarios.

## Brute Force Authentication

Purpose:

Identify repeated failed authentication attempts against one or more user accounts.

Typical indicators:

- High number of failed logons
- Single source IP
- Multiple targeted accounts
- Short time interval

Relevant Event ID:

4625

---

## Successful Logon After Multiple Failures

Purpose:

Detect successful authentication occurring shortly after numerous failed logon attempts.

Potential significance:

May indicate a successful brute-force attack.

Relevant Event IDs:

- 4625
- 4624

---

## Suspicious Remote Desktop Activity

Purpose:

Monitor Remote Desktop authentication attempts originating from unusual or unexpected locations.

Indicators include:

- High login frequency
- Multiple destination systems
- Unknown source IP addresses
- Unusual login times

---

## Privileged Logon Activity

Purpose:

Identify logons involving privileged accounts or special administrative privileges.

Relevant Event:

4672

---

# Query Development Process

Every KQL query follows a consistent methodology.

```
Select Data Source

↓

Filter Relevant Events

↓

Extract Required Fields

↓

Aggregate Data

↓

Apply Threshold

↓

Review Output

↓

Optimize Performance
```

The objective is to identify meaningful behavioral patterns rather than isolated events.

---

# Query Optimization

Efficient KQL queries improve both performance and readability.

Recommended practices include:

- Filter early using `where`
- Project only required fields
- Summarize only after filtering
- Avoid unnecessary joins
- Limit expensive operations

Example:

```kusto
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts=count() by IPAddress
```

---

# Threshold Selection

Detection thresholds should be selected based on expected organizational behavior.

Example considerations include:

- Number of authentication attempts
- Time window
- User population
- Geographic distribution
- Administrative activity

Thresholds should be periodically reviewed and adjusted as the environment changes.

---

# False Positive Reduction

Reducing false positives is essential for maintaining analyst confidence.

Common techniques include:

- Excluding known administrative accounts
- Ignoring trusted IP ranges
- Filtering service accounts
- Whitelisting vulnerability scanners
- Ignoring scheduled maintenance windows

Proper tuning significantly improves detection quality.

---

# Detection Validation

Before deploying a detection, it should be validated.

Validation includes:

- Confirming required telemetry exists
- Reviewing query output
- Testing against expected scenarios
- Measuring false positives
- Evaluating performance
- Reviewing MITRE mapping

---

# Detection Documentation

Every detection should include:

- Detection Name
- Purpose
- MITRE Technique
- Required Data Source
- Required Event IDs
- KQL Query
- Expected Output
- False Positives
- Tuning Recommendations
- Severity
- References

Consistent documentation improves long-term maintainability.

---

# Incident Creation

Once detection logic is validated, it can be converted into a Microsoft Sentinel Analytics Rule.

The Analytics Rule defines:

- Query frequency
- Lookup period
- Alert threshold
- Severity
- Entity mapping
- Incident grouping

When the rule conditions are met, Microsoft Sentinel automatically generates an incident for analyst investigation.

---

# Detection Maturity

Detection engineering is an ongoing process.

Each detection progresses through multiple stages.

```
Draft

↓

Test

↓

Validated

↓

Production Ready

↓

Continuously Tuned
```

Continuous tuning ensures detections remain effective as attacker behavior evolves.

---

# Best Practices

The following practices improve overall detection quality:

- Collect high-quality telemetry
- Keep queries simple
- Minimize false positives
- Validate every detection
- Map detections to MITRE ATT&CK
- Review detections regularly
- Document every change
- Monitor query performance

---

# Summary

Detection engineering transforms raw security telemetry into actionable intelligence.

By combining Windows Security Events, Azure Log Analytics, Microsoft Sentinel, and Kusto Query Language, analysts can detect suspicious authentication activity, investigate attacker behavior, and continuously improve their organization's monitoring capabilities.

Effective detection engineering is not a one-time activity but an ongoing process of hypothesis development, validation, tuning, and operational improvement.
