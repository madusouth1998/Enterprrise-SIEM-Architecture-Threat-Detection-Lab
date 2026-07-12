# Incident Response Playbook

## Overview

This document outlines the investigation and response process for security incidents identified within the Microsoft Sentinel environment.

The objective is to provide a structured methodology for validating alerts, determining the scope of an incident, containing threats, and recommending remediation actions.

The workflow follows industry best practices based on the NIST Incident Response Lifecycle while leveraging Microsoft Sentinel for investigation and analysis.

---

# Incident Response Lifecycle

Preparation

↓

Detection & Analysis

↓

Containment

↓

Eradication

↓

Recovery

↓

Lessons Learned
```

Each phase plays a critical role in reducing organizational risk and restoring normal operations.

---

# Preparation

Preparation ensures that analysts have the tools, visibility, and procedures required to respond effectively.

### Requirements

- Microsoft Sentinel
- Azure Log Analytics Workspace
- Azure Monitor Agent
- Windows Security Event Logs
- Incident Response Procedures
- Administrative Access
- Threat Intelligence Sources

---

# Detection & Analysis

## Alert Generation

Security incidents begin when Microsoft Sentinel detects suspicious activity through log analysis or analytics rules.

Common indicators include:

- Repeated failed logon attempts
- Successful logon after multiple failures
- Privileged account activity
- Unusual authentication patterns
- Authentication from unfamiliar IP addresses

---

## Initial Triage

When an alert is generated, the analyst should answer the following questions:

- What triggered the alert?
- Which user account was involved?
- Which host generated the event?
- When did the activity occur?
- Is the activity expected?
- Has similar activity occurred previously?

---

## Verify the Alert

Before escalating an incident, validate the authenticity of the alert.

Review:

- Event ID
- Username
- Computer Name
- Source IP Address
- Time Generated
- Authentication Type

Example KQL:

```kusto
SecurityEvent
| where EventID == 4625
| order by TimeGenerated desc
```

---

# Scope Determination

Determine the extent of the activity.

Questions to answer include:

- Is one account affected?
- Are multiple accounts targeted?
- Is one endpoint involved?
- Is lateral movement suspected?
- Has the attacker successfully authenticated?

Understanding scope helps prioritize containment activities.

---

# User Investigation

Review authentication history for the affected account.

Example query:

```kusto
SecurityEvent
| where Account == "\\USERNAME"
| order by TimeGenerated desc
```

Review:

- Successful logons
- Failed logons
- Administrative activity
- Logon frequency
- Systems accessed

---

# Source IP Investigation

Review activity originating from the source IP address.

Identify:

- Number of authentication attempts
- Targeted accounts
- Time window
- Geographic location
- Previous activity

This information helps determine whether the activity is malicious or expected.

---

# Timeline Analysis

Construct a timeline of events.

```
Initial Failed Logon

↓

Repeated Authentication Attempts

↓

Successful Authentication (if any)

↓

Privilege Assignment

↓

Additional Activity

↓

Incident Closed
```

Timeline analysis provides context for understanding attacker behavior.

---

# Containment

If malicious activity is confirmed, immediate containment actions should be considered.

Recommended actions include:

- Disable compromised accounts
- Block malicious IP addresses
- Restrict RDP access
- Isolate affected systems
- Reset passwords
- Enable Multi-Factor Authentication

The goal is to prevent additional attacker activity while preserving evidence.

---

# Eradication

Following containment, remove the root cause of the incident.

Possible actions include:

- Remove malicious accounts
- Delete unauthorized scheduled tasks
- Remove persistence mechanisms
- Update vulnerable systems
- Apply missing security patches

---

# Recovery

Once the threat has been removed:

- Restore affected systems
- Verify system integrity
- Monitor authentication activity
- Confirm normal business operations
- Continue enhanced monitoring

Recovery should only begin after eradication activities are complete.

---

# Post-Incident Review

Every incident provides an opportunity to improve security operations.

Review:

- Root cause
- Detection effectiveness
- Analyst response
- Investigation timeline
- Areas for improvement

Lessons learned should be documented and incorporated into future detection logic.

---

# Recommended Improvements

Based on the investigation, consider implementing:

- Azure Bastion
- Just-in-Time VM Access
- Conditional Access Policies
- Multi-Factor Authentication
- Microsoft Defender for Endpoint
- Microsoft Defender for Cloud
- Sentinel Analytics Rules
- Sentinel Automation Playbooks

These controls reduce attack surface and improve response capabilities.

---

# SOC Analyst Checklist

Before closing an incident, confirm the following:

- Alert validated
- Timeline established
- User investigated
- Source IP investigated
- Scope determined
- Containment completed
- Recovery completed
- Documentation updated
- Lessons learned recorded

---

# Summary

An effective incident response process combines technology, investigation, and operational discipline.

Microsoft Sentinel provides centralized visibility into security events, while a structured response process enables analysts to investigate threats, contain malicious activity, and continuously improve organizational security posture.

Consistent incident response procedures reduce response time, improve investigation quality, and strengthen overall security operations.
