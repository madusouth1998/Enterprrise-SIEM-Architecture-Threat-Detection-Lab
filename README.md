# Detect and Investigate Unauthorized Login Attempts Using Microsoft Sentinel in an Azure SOC Lab

# Overview

This project demonstrates the design and implementation of a cloud-native Security Information and Event Management (SIEM) solution using **Microsoft Sentinel**, **Azure Log Analytics**, and **Azure Monitor Agent** within Microsoft Azure.

The objective of this lab is to simulate how a Security Operations Center (SOC) monitors, detects, investigates, and responds to unauthorized authentication attempts targeting an internet-facing Windows Virtual Machine.

The project walks through the complete lifecycle of a modern SIEM deployment, from provisioning Azure infrastructure to centralizing Windows Security Event Logs, developing Kusto Query Language (KQL) detections, investigating brute-force authentication attempts, and recommending security improvements aligned with enterprise security best practices.

---

# Project Objectives

The primary objectives of this lab were to:

- Build a cloud-native SIEM using Microsoft Sentinel
- Deploy an internet-facing Windows Virtual Machine
- Collect Windows Security Event Logs
- Forward security logs into Azure Log Analytics
- Configure Microsoft Sentinel
- Develop KQL detection queries
- Investigate unauthorized authentication attempts
- Map attacker behavior to the MITRE ATT&CK Framework
- Produce remediation recommendations based on observed attack activity

---

# Technologies Used

| Technology | Purpose |
|------------|----------|
| Microsoft Azure | Cloud Infrastructure |
| Microsoft Sentinel | Security Information & Event Management |
| Azure Log Analytics Workspace | Log Repository |
| Azure Monitor Agent | Log Collection |
| Windows 10 Pro VM | Honeypot Endpoint |
| Azure Virtual Network | Network Segmentation |
| Network Security Group | Traffic Filtering |
| Kusto Query Language (KQL) | Threat Hunting |
| Event Viewer | Windows Security Logs |
| MITRE ATT&CK Framework | Threat Classification |

---

# Architecture

The architecture follows a centralized cloud security monitoring model.

```
                Internet
                    │
                    ▼
          Public IP Address
                    │
                    ▼
      Network Security Group
                    │
                    ▼
      Windows 10 Honeypot VM
                    │
      Windows Security Events
                    │
                    ▼
     Azure Monitor Agent (AMA)
                    │
                    ▼
      Log Analytics Workspace
                    │
                    ▼
       Microsoft Sentinel
          │             │
          ▼             ▼
 Threat Hunting     Analytics Rules
          │
          ▼
   Incident Investigation
```

---

# Lab Environment

The Azure environment consists of:

- Azure Subscription
- Resource Group
- Virtual Network
- Network Security Group
- Public IP Address
- Windows 10 Pro Virtual Machine
- Azure Monitor Agent
- Log Analytics Workspace
- Microsoft Sentinel

---

# Deployment Workflow

The deployment followed these major phases.

## Phase 1

Azure Environment Deployment

- Azure Subscription
- Resource Group
- Virtual Network
- Public IP
- Network Security Group

---

## Phase 2

Virtual Machine Deployment

- Windows 10 Pro
- Standard D2s_v3
- East US 2
- Public Internet Access

---

## Phase 3

Security Event Collection

- Event Viewer
- Windows Security Logs
- Event ID 4625
- Event ID 4624

---

## Phase 4

Centralized Log Collection

- Azure Monitor Agent
- Log Analytics Workspace
- Microsoft Sentinel Connector

---

## Phase 5

Threat Hunting

Threat hunting activities included:

- Failed Logon Analysis
- Successful Logon Analysis
- Source IP Investigation
- Authentication Timeline Analysis
- Geographic Attack Visualization

---

# Detection Engineering

Example detection scenarios include:

- Brute Force Authentication
- Password Spraying
- Suspicious RDP Activity
- Successful Logon After Multiple Failures
- High Volume Authentication Attempts

Detection logic was implemented using Kusto Query Language (KQL) and mapped to MITRE ATT&CK techniques where appropriate.

---

# MITRE ATT&CK Mapping

| Technique | ID |
|------------|-----|
| Brute Force | T1110 |
| Valid Accounts | T1078 |
| Remote Services | T1021 |

---

# Investigation Workflow

The investigation process followed a standard SOC methodology.

```
Alert

↓

Validate Alert

↓

Collect Evidence

↓

Analyze Logs

↓

Identify Source IP

↓

Determine Impact

↓

Recommend Remediation
```

---

# Sample KQL Query

```kusto
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts=count() by Account, IPAddress
| order by FailedAttempts desc
```

This query identifies accounts experiencing repeated failed authentication attempts.

---

# Security Findings

The lab demonstrates how internet-facing Windows systems rapidly receive automated authentication attempts from external sources.

The collected telemetry allows defenders to:

- Identify brute-force activity
- Investigate suspicious authentication behavior
- Develop detection rules
- Produce actionable security recommendations

---

# Recommendations

Recommended improvements include:

- Restrict RDP exposure
- Implement Just-in-Time VM Access
- Enable Multi-Factor Authentication
- Use Azure Bastion
- Deploy Microsoft Defender for Cloud
- Enable Microsoft Defender for Endpoint
- Configure Sentinel Analytics Rules
- Implement Sentinel Playbooks
- Restrict Network Security Group Rules
- Enable Continuous Monitoring

---

# Repository Structure

```
Enterprise-SIEM-Architecture-Threat-Detection-Lab/

│
├── README.md
│
├── architecture/
│   ├── architecture-diagram.png
│   └── data-flow.md
│
├── detections/
│   ├── brute-force-detection.kql
│   ├── suspicious-rdp-activity.kql
│   ├── lateral-movement.kql
│   └── privilege-escalation.kql
│
├── docs/
│   ├── deployment.md
│   ├── detection-engineering.md
│   └── lessons-learned.md
│
└── screenshots/
```

---

# Skills Demonstrated

- Microsoft Sentinel
- Azure Monitor
- Azure Log Analytics
- Azure Networking
- Windows Security Event Analysis
- KQL Query Development
- Threat Hunting
- Detection Engineering
- SIEM Architecture
- Incident Investigation
- Security Monitoring
- MITRE ATT&CK Mapping

---

# Future Improvements

Future enhancements may include:

- Microsoft Defender XDR Integration
- Microsoft Defender for Cloud
- Threat Intelligence Integration
- Automated Sentinel Playbooks
- Azure Logic Apps
- Watchlists
- User Entity Behavior Analytics (UEBA)
- Custom Analytics Rules
- Incident Automation

---

# Author

Madu South Okechukwu

Cybersecurity Engineer

---

> This repository is intended for educational purposes to demonstrate Microsoft Sentinel architecture, security monitoring concepts, detection engineering techniques, and SOC investigation workflows within a controlled Azure lab environment.
