# Enterprise SIEM Deployment Guide

## Overview

This document outlines the deployment process for the Enterprise SIEM Architecture & Threat Detection Lab built on Microsoft Azure. The environment is designed to simulate a Security Operations Center (SOC) workflow by collecting, centralizing, and analyzing Windows security telemetry using Microsoft Sentinel.

The deployment emphasizes infrastructure design, centralized logging, detection engineering, and security monitoring.

---

# Solution Architecture

The deployment consists of the following Azure services:

| Azure Resource | Purpose |
|---------------|---------|
| Resource Group | Logical container for all lab resources |
| Virtual Network | Provides network segmentation |
| Network Security Group | Controls inbound and outbound traffic |
| Public IP Address | Exposes the virtual machine to the Internet |
| Windows 10 Virtual Machine | Generates security telemetry |
| Azure Monitor Agent | Collects Windows Event Logs |
| Log Analytics Workspace | Centralized log storage |
| Microsoft Sentinel | SIEM and security analytics platform |

---

# Deployment Objectives

The lab was designed to demonstrate the following capabilities:

- Cloud infrastructure deployment
- Windows security log collection
- Centralized logging
- SIEM integration
- Threat hunting using KQL
- Detection engineering
- Security event investigation
- MITRE ATT&CK mapping

---

# Phase 1 — Azure Resource Group

## Purpose

The Resource Group provides logical organization for every Azure resource deployed during the project.

Grouping resources together simplifies:

- Deployment
- Administration
- Cost tracking
- Resource cleanup
- Access management

Example configuration:

| Setting | Value |
|----------|------|
| Name | RG-SOC-LAB |
| Region | East US 2 |

---

# Phase 2 — Virtual Network

## Purpose

The Virtual Network (VNet) provides secure communication between Azure resources.

Example configuration:

| Setting | Value |
|----------|------|
| Address Space | 10.0.0.0/16 |
| Subnet | 10.0.1.0/24 |

Benefits include:

- Network isolation
- Traffic routing
- Resource segmentation
- Future scalability

---

# Phase 3 — Network Security Group

## Purpose

The Network Security Group (NSG) acts as the first layer of network access control by evaluating inbound and outbound traffic before it reaches the virtual machine.

Typical inbound rules:

| Priority | Port | Protocol | Action |
|----------|------|----------|--------|
| 100 | 3389 | TCP | Allow |
| 200 | * | * | Deny |

> **Note:** Opening RDP to the Internet is done only for controlled lab purposes. Production environments should use Azure Bastion or Just-in-Time (JIT) VM access.

---

# Phase 4 — Public IP Address

A Public IP Address allows remote connectivity to the Windows virtual machine.

Purpose:

- Remote administration
- Authentication telemetry generation
- External attack simulation
- SOC monitoring

---

# Phase 5 — Windows 10 Virtual Machine

The Windows VM serves as the primary telemetry source.

Recommended configuration:

| Setting | Value |
|----------|------|
| Operating System | Windows 10 Pro |
| Size | Standard D2s_v3 |
| Authentication | Username & Password |
| Public IP | Enabled |
| Boot Diagnostics | Enabled |

The VM generates Windows Security Events that will later be ingested into Microsoft Sentinel.

---

# Phase 6 — Configure Windows Event Logging

Windows automatically records authentication events in the Security Event Log.

Important Event IDs:

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4634 | Logoff |
| 4648 | Logon with Explicit Credentials |
| 4672 | Special Privileges Assigned |

These events become the primary data source for threat hunting.

---

# Phase 7 — Install Azure Monitor Agent

## Purpose

Azure Monitor Agent (AMA) collects telemetry from the virtual machine and forwards it to Azure Log Analytics.

Responsibilities:

- Read Windows Security Logs
- Collect performance metrics
- Securely transmit telemetry
- Maintain reliable log forwarding

Without AMA, Microsoft Sentinel cannot analyze Windows security events.

---

# Phase 8 — Deploy Log Analytics Workspace

## Purpose

Azure Log Analytics Workspace acts as the centralized repository for collected telemetry.

Example configuration:

| Setting | Value |
|----------|------|
| Name | LAW-SOC-LAB |
| Region | East US 2 |
| Pricing Tier | Pay-As-You-Go |

Capabilities:

- Log storage
- Historical search
- KQL queries
- Data retention
- Integration with Sentinel

---

# Phase 9 — Enable Microsoft Sentinel

Microsoft Sentinel extends Azure Log Analytics by adding SIEM and SOAR capabilities.

Features enabled:

- Threat Hunting
- Analytics Rules
- Incident Management
- Investigation Graph
- Workbooks
- MITRE ATT&CK Mapping
- Automation

Sentinel uses the Log Analytics Workspace as its data source.

---

# Phase 10 — Configure Data Collection Rules

Data Collection Rules (DCRs) specify which telemetry is collected.

Typical configuration:

- Windows Security Events
- Event ID Filtering
- Windows Performance Metrics
- Heartbeat
- Agent Health

Proper DCR configuration helps reduce unnecessary ingestion costs.

---

# Phase 11 — Validate Data Collection

Validation steps include:

1. Confirm Azure Monitor Agent is connected.
2. Verify the VM appears in Azure Monitor.
3. Run a simple KQL query.
4. Confirm Windows Security Events are being ingested.
5. Verify Microsoft Sentinel can access the workspace.

Example query:

```kusto
SecurityEvent
| take 10
```

If results are returned, log ingestion is functioning correctly.

---

# Phase 12 — Threat Hunting

Once telemetry is available, analysts can begin searching for suspicious behavior.

Example hunting activities:

- Failed authentication attempts
- Successful authentication
- Repeated logon failures
- Source IP analysis
- Privileged account activity
- RDP monitoring

KQL is used to investigate and correlate these events.

---

# Operational Workflow

```
Deploy Infrastructure
        │
        ▼
Deploy Virtual Machine
        │
        ▼
Install Azure Monitor Agent
        │
        ▼
Create Log Analytics Workspace
        │
        ▼
Enable Microsoft Sentinel
        │
        ▼
Configure Data Collection Rules
        │
        ▼
Verify Log Ingestion
        │
        ▼
Develop KQL Queries
        │
        ▼
Create Analytics Rules
        │
        ▼
Investigate Alerts
```

---

# Cost Considerations

A production SIEM environment should be designed with cost optimization in mind.

Recommended practices:

- Limit unnecessary log collection.
- Configure appropriate retention periods.
- Filter noisy Event IDs.
- Monitor daily ingestion volume.
- Archive infrequently accessed logs.

These practices help control Azure Monitor and Sentinel costs.

---

# Security Best Practices

Although this lab intentionally exposes a Windows VM to generate telemetry, production environments should implement additional controls, including:

- Azure Bastion
- Just-in-Time VM Access
- Multi-Factor Authentication
- Conditional Access Policies
- Microsoft Defender for Endpoint
- Microsoft Defender for Cloud
- Least Privilege Access
- Privileged Identity Management (PIM)

---

# Deployment Summary

At the conclusion of deployment, the environment provides:

- A centralized logging platform
- Windows Security Event collection
- Microsoft Sentinel integration
- KQL-based threat hunting
- Detection engineering capabilities
- Incident investigation workflows
- A foundation for developing custom analytics rules and SOC automation

The completed deployment forms the basis of an enterprise-ready SIEM architecture that can be extended with additional data sources, automation, and security tooling.
