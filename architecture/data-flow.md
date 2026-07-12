# SIEM Data Flow Architecture

## Overview

This document explains how security telemetry flows through the Azure environment, beginning with an external authentication attempt and ending with an incident inside Microsoft Sentinel.

Understanding the data flow is essential when designing a Security Information and Event Management (SIEM) solution because every detection rule depends on accurate log collection, normalization, storage, and analysis.

---

# High-Level Architecture

```
Internet
    │
    ▼
Public IP
    │
    ▼
Network Security Group (NSG)
    │
    ▼
Windows 10 Virtual Machine (Honeypot)
    │
Windows Security Events
    │
    ▼
Azure Monitor Agent (AMA)
    │
    ▼
Azure Log Analytics Workspace
    │
    ▼
Microsoft Sentinel
    │
    ├──────────────┐
    ▼              ▼
Threat Hunting   Analytics Rules
    │              │
    └──────┬───────┘
           ▼
Incident Investigation
```

---

# Environment Components

The solution is composed of several Azure services working together to provide centralized security monitoring.

| Component | Function |
|-----------|----------|
| Azure Subscription | Hosts all cloud resources |
| Resource Group | Logical container for the SOC lab |
| Virtual Network | Network segmentation |
| Network Security Group | Controls inbound and outbound traffic |
| Windows Virtual Machine | Generates Windows Security Events |
| Azure Monitor Agent | Collects telemetry |
| Log Analytics Workspace | Stores collected logs |
| Microsoft Sentinel | SIEM and Security Analytics Platform |

---

# Data Flow

The architecture follows a sequential flow of security telemetry.

## Step 1 — Internet Traffic

The Windows virtual machine is assigned a Public IP address.

Because the system is reachable from the Internet, it continuously receives unsolicited authentication attempts from automated scanners and brute-force tools operating across the Internet.

These connection attempts generate Windows Security Events regardless of whether authentication succeeds.

---

## Step 2 — Network Security Group

Before reaching the virtual machine, every packet passes through the Azure Network Security Group (NSG).

The NSG evaluates inbound traffic against configured rules.

Responsibilities include:

- Allowing RDP traffic
- Blocking unwanted traffic
- Logging network activity
- Applying access control rules

The NSG represents the first layer of network-level access control.

---

## Step 3 — Windows Virtual Machine

The Windows 10 Pro virtual machine functions as the primary telemetry source.

When authentication occurs, Windows automatically records security events inside Event Viewer.

Important security events include:

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4634 | Logoff |
| 4648 | Explicit Credential Logon |
| 4672 | Special Privileges Assigned |

These events become the foundation for threat detection.

---

# Windows Event Generation

Authentication attempts generate security events similar to the following process.

```
External Authentication Attempt

↓

Windows Authentication Service

↓

Windows Security Log

↓

Event ID Generated

↓

Event Viewer
```

These events remain local until collected by Azure Monitor Agent.

---

# Azure Monitor Agent (AMA)

Azure Monitor Agent continuously collects Windows Security Events from the virtual machine.

The agent is responsible for:

- Reading Windows Event Logs
- Packaging telemetry
- Securely forwarding events
- Maintaining reliable log delivery

Without AMA, Microsoft Sentinel has no visibility into Windows authentication activity.

---

# Log Analytics Workspace

The Log Analytics Workspace serves as the centralized log repository.

Responsibilities include:

- Receiving logs
- Storing telemetry
- Indexing security events
- Supporting KQL queries
- Providing historical search capability

All Windows Security Events collected by AMA are stored inside this workspace.

---

# Microsoft Sentinel

Microsoft Sentinel is deployed on top of the Log Analytics Workspace.

Rather than storing logs itself, Sentinel analyzes the data already present inside Log Analytics.

Its responsibilities include:

- Threat Hunting
- Analytics Rules
- Incident Creation
- Investigation
- Workbooks
- Dashboards
- MITRE ATT&CK Mapping

---

# Threat Hunting Workflow

Security analysts begin by querying Windows Security Events.

Typical workflow:

```
Collect Logs

↓

Search Events

↓

Filter Relevant Event IDs

↓

Correlate Related Activity

↓

Identify Suspicious Behaviour

↓

Investigate

↓

Respond
```

---

# Detection Pipeline

Detection engineering follows a layered process.

```
Windows Events

↓

Azure Monitor Agent

↓

Log Analytics Workspace

↓

KQL Query

↓

Analytics Rule

↓

Alert

↓

Incident

↓

SOC Investigation
```

---

# Event Processing Lifecycle

Every authentication attempt follows this sequence.

```
Internet

↓

Authentication Attempt

↓

Windows Authentication

↓

Windows Security Event

↓

Azure Monitor Agent

↓

Log Analytics

↓

Microsoft Sentinel

↓

KQL Query

↓

Analytics Rule

↓

Incident Queue
```

---

# Data Sources

The primary telemetry source is Windows Security Events.

Example data collected includes:

- Username
- Source IP Address
- Computer Name
- Event ID
- Time Generated
- Authentication Status
- Activity Description

These fields allow analysts to reconstruct authentication activity.

---

# Detection Logic

Detection rules analyze authentication patterns rather than individual events.

Examples include:

- Multiple failed logons
- Successful logon after repeated failures
- Excessive authentication volume
- Geographic anomalies
- Suspicious RDP activity

The purpose is to identify abnormal behaviour instead of isolated events.

---

# Benefits of Centralized Logging

Centralized log collection provides several advantages.

- Single source of truth
- Faster investigations
- Historical event analysis
- Threat hunting
- Detection engineering
- Compliance reporting
- Alert generation
- Correlation across multiple hosts

---

# Security Considerations

For demonstration purposes, the lab intentionally exposes the Windows virtual machine to the public Internet to generate authentication telemetry.

This configuration enables realistic security monitoring but is **not** recommended for production environments.

Production deployments should implement:

- Azure Bastion
- Just-In-Time VM Access
- Multi-Factor Authentication
- Conditional Access Policies
- Restricted NSG Rules
- Microsoft Defender for Cloud

---

# Summary

The architecture demonstrates how Microsoft Sentinel transforms raw Windows Security Events into actionable security intelligence.

By combining Azure Monitor Agent, Azure Log Analytics Workspace, Microsoft Sentinel, and Kusto Query Language, defenders can centralize log collection, develop detection logic, investigate suspicious activity, and improve an organization's security posture.

The design also illustrates the end-to-end lifecycle of an authentication event—from the moment an external connection reaches the virtual machine to the point where an analyst investigates the generated incident within Microsoft Sentinel.
