# Network Topology

## Overview

This document describes the network architecture implemented for the Enterprise SIEM Architecture & Threat Detection Lab.

The environment was designed to simulate a cloud-hosted Windows endpoint connected to Microsoft Sentinel for centralized security monitoring. Each Azure component plays a specific role in collecting, protecting, and analyzing security telemetry.

Although this is a lab environment, the architecture follows many of the same design principles used in enterprise Microsoft Azure deployments.

---

# High-Level Network Architecture

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
                  Windows 10 Virtual Machine
                              │
                 Windows Security Event Logs
                              │
                              ▼
                  Azure Monitor Agent (AMA)
                              │
                              ▼
                 Log Analytics Workspace
                              │
                              ▼
                  Microsoft Sentinel (SIEM)
                              │
             ┌────────────────┴────────────────┐
             │                                 │
             ▼                                 ▼
      Threat Hunting                    Incident Investigation
             │                                 │
             └────────────────┬────────────────┘
                              ▼
                        Security Analyst
```

---

# Network Components

## Azure Subscription

The Azure Subscription provides the foundation for the deployment by hosting all cloud resources.

Responsibilities include:

- Resource management
- Billing
- Identity integration
- Security configuration
- Access control

---

## Resource Group

A Resource Group provides logical organization for Azure resources.

Benefits include:

- Simplified administration
- Resource lifecycle management
- Cost tracking
- RBAC assignment
- Infrastructure organization

---

## Virtual Network (VNet)

The Virtual Network provides private networking between Azure resources.

Configuration Example

| Property | Value |
|----------|-------|
| Address Space | 10.0.0.0/16 |
| Subnet | 10.0.1.0/24 |

Purpose

- Network isolation
- Secure communication
- Traffic routing
- Future scalability

---

## Public IP Address

A Public IP Address enables remote connectivity to the Windows virtual machine.

In this lab, the VM is intentionally exposed to the Internet to generate authentication activity for monitoring and investigation.

In production environments, public administrative access should be minimized.

---

## Network Security Group (NSG)

The Network Security Group controls inbound and outbound network traffic.

Responsibilities include:

- Filtering inbound traffic
- Restricting unauthorized access
- Controlling administrative ports
- Supporting network segmentation

Example Rule

| Priority | Protocol | Port | Action |
|----------|----------|------|--------|
| 100 | TCP | 3389 | Allow |
| 200 | Any | Any | Deny |

---

## Windows 10 Virtual Machine

The Windows Virtual Machine is the primary telemetry source within the environment.

It generates Windows Security Events related to:

- User authentication
- Account management
- Logon activity
- Privilege assignment
- System events

These events are forwarded to Azure Monitor for centralized analysis.

---

## Azure Monitor Agent (AMA)

Azure Monitor Agent collects telemetry from the virtual machine and securely forwards it to Azure Log Analytics.

Responsibilities include:

- Event collection
- Telemetry forwarding
- Performance monitoring
- Agent health reporting

AMA replaces the legacy Log Analytics Agent and supports modern Data Collection Rules (DCRs).

---

## Log Analytics Workspace

The Log Analytics Workspace serves as the centralized repository for collected telemetry.

Capabilities include:

- Long-term log storage
- Log indexing
- KQL querying
- Data retention
- Integration with Microsoft Sentinel

The workspace acts as the primary data layer for security investigations.

---

## Microsoft Sentinel

Microsoft Sentinel provides cloud-native SIEM capabilities on top of Azure Log Analytics.

Primary capabilities include:

- Security monitoring
- Threat hunting
- Incident management
- Analytics rules
- Workbooks
- Automation
- MITRE ATT&CK mapping

Rather than collecting logs directly, Sentinel consumes telemetry stored within Log Analytics.

---

# Security Data Flow

The following diagram illustrates the movement of telemetry through the environment.

```
Authentication Attempt

↓

Windows Authentication

↓

Windows Security Event

↓

Azure Monitor Agent

↓

Log Analytics Workspace

↓

Microsoft Sentinel

↓

Threat Hunting

↓

Incident Investigation

↓

Response
```

---

# Trust Boundaries

The architecture includes several trust boundaries.

## External Boundary

The Public IP Address represents the transition between the public Internet and the Azure environment.

Potential threats include:

- Brute-force attacks
- Password guessing
- Port scanning
- Unauthorized authentication attempts

---

## Network Boundary

The Network Security Group acts as the first network security boundary.

Responsibilities:

- Traffic filtering
- Access enforcement
- Network segmentation

---

## Endpoint Boundary

The Windows VM represents the endpoint security boundary.

Security events generated here provide the visibility required for investigations.

---

## Monitoring Boundary

Azure Monitor Agent creates the boundary between endpoint telemetry and centralized monitoring.

Its role is to ensure reliable and secure transmission of security events.

---

## SIEM Boundary

Microsoft Sentinel represents the analytical boundary where telemetry becomes actionable security intelligence.

---

# Network Security Considerations

The lab intentionally exposes RDP to the Internet to simulate real-world attack activity.

While this approach is useful for educational purposes, production environments should adopt stronger controls, including:

- Azure Bastion
- Just-In-Time VM Access
- VPN or ExpressRoute
- Conditional Access
- Private Endpoints
- Microsoft Defender for Cloud

---

# Design Decisions

Several architectural decisions were made during the design of this environment.

## Why Microsoft Sentinel?

Microsoft Sentinel was selected because it provides:

- Cloud-native SIEM functionality
- Native Azure integration
- Kusto Query Language (KQL) support
- Built-in incident management
- MITRE ATT&CK mapping
- Automation through Logic Apps

---

## Why Azure Monitor Agent?

Azure Monitor Agent was selected because it is Microsoft's recommended agent for Azure workloads.

Benefits include:

- Improved performance
- Centralized configuration
- Support for Data Collection Rules
- Long-term platform support

---

## Why Azure Log Analytics?

Azure Log Analytics provides a scalable platform for storing and querying large volumes of telemetry.

Its integration with Microsoft Sentinel enables efficient threat hunting and investigation.

---

## Why Windows Security Events?

Windows Security Events provide detailed visibility into authentication activity and system behavior.

These logs are essential for:

- Detecting brute-force attacks
- Investigating compromised accounts
- Monitoring privileged access
- Building authentication timelines

---

# Production Enhancements

To evolve this lab into an enterprise-ready deployment, consider implementing:

- Azure Bastion
- Microsoft Defender for Endpoint
- Microsoft Defender for Cloud
- Azure Policy
- Azure Backup
- Azure Key Vault
- Sentinel Playbooks
- Threat Intelligence Integration
- User and Entity Behavior Analytics (UEBA)

These enhancements improve security, scalability, and operational maturity.

---

# Conclusion

The network topology demonstrates how Azure infrastructure, Windows endpoints, Azure Monitor Agent, Azure Log Analytics, and Microsoft Sentinel work together to provide centralized visibility into security events.

The design emphasizes layered security, centralized logging, and cloud-native monitoring while serving as a foundation for future enhancements such as automated response, advanced analytics, and enterprise-scale security operations.
