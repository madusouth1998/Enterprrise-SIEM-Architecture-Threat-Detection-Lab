# Lessons Learned

## Overview

Building this Microsoft Sentinel lab provided practical experience in deploying and understanding the core components of a cloud-native Security Information and Event Management (SIEM) solution.

Beyond learning how to provision Azure resources, the project demonstrated how security telemetry is collected, centralized, and transformed into actionable information for analysts.

The lab also reinforced the importance of proper architecture, visibility, and monitoring when protecting internet-facing systems.

---

# Key Technical Lessons

## Cloud Infrastructure Matters

One of the most significant takeaways from this project was understanding that security monitoring begins long before alerts appear inside a SIEM.

Proper planning of Azure resources—including Resource Groups, Virtual Networks, Network Security Groups, and Virtual Machines—is essential for building a secure monitoring environment.

Poor infrastructure design directly affects visibility, scalability, and security.

---

## Centralized Logging is Critical

Before this project, Windows Event Viewer was primarily viewed as a local troubleshooting tool.

By forwarding Windows Security Events into Azure Log Analytics, it became clear how centralized logging enables analysts to:

- Search historical events
- Correlate activity
- Investigate incidents
- Build detections
- Perform threat hunting

Without centralized logging, large-scale investigations become significantly more difficult.

---

## Visibility is More Valuable Than Volume

Collecting large amounts of telemetry does not automatically improve security.

Effective monitoring depends on collecting relevant data that supports investigations while minimizing unnecessary noise and storage costs.

This highlighted the importance of selecting appropriate data sources and configuring Data Collection Rules carefully.

---

## Microsoft Sentinel Extends Log Analytics

A key realization during the project was that Microsoft Sentinel does not replace Azure Log Analytics.

Instead, Sentinel builds on top of Log Analytics by adding capabilities such as:

- Threat Hunting
- Incident Management
- Analytics Rules
- Investigation Tools
- Dashboards
- MITRE ATT&CK Mapping

Understanding this relationship clarified the overall architecture of Microsoft's security monitoring platform.

---

## Network Security Groups are the First Line of Defense

The project reinforced the role of Network Security Groups in controlling traffic entering Azure workloads.

Although the lab intentionally allowed Remote Desktop Protocol (RDP) access for demonstration purposes, production environments should adopt a defense-in-depth strategy by combining NSGs with additional controls such as Azure Bastion and Just-in-Time (JIT) VM Access.

---

## Security Monitoring is Continuous

Deploying a SIEM is only the beginning.

Maintaining an effective monitoring solution requires:

- Continuous log review
- Detection tuning
- Infrastructure maintenance
- Incident analysis
- Regular validation of telemetry sources

Security monitoring is an ongoing operational process rather than a one-time deployment.

---

# Challenges Encountered

Several challenges were identified throughout the project.

## Azure Service Dependencies

Many Azure services rely on one another.

For example:

- Microsoft Sentinel depends on a Log Analytics Workspace.
- Azure Monitor Agent depends on Data Collection Rules.
- Log ingestion depends on correct agent configuration.

Understanding these dependencies was essential for successful deployment.

---

## Log Collection Verification

Ensuring that Windows Security Events were successfully reaching Log Analytics required careful verification.

Checking ingestion before beginning investigations helped avoid troubleshooting inaccurate or incomplete datasets later in the process.

---

## Security vs Accessibility

Exposing a Windows Virtual Machine to the public Internet increased visibility into authentication attempts but also introduced unnecessary risk.

The project reinforced why production systems should minimize public exposure wherever possible.

---

# Production Improvements

If this environment were expanded into a production deployment, the following improvements would be recommended:

- Azure Bastion
- Just-in-Time VM Access
- Microsoft Defender for Endpoint
- Microsoft Defender for Cloud
- Conditional Access Policies
- Multi-Factor Authentication
- Automated Playbooks
- Threat Intelligence Integration
- User Entity Behavior Analytics (UEBA)

These enhancements would strengthen the overall security posture while reducing operational risk.

---

# Professional Growth

This project improved practical understanding of:

- Microsoft Azure
- Microsoft Sentinel
- Azure Monitor
- Azure Log Analytics
- Windows Security Events
- SIEM Architecture
- Security Monitoring
- Threat Investigation
- Cloud Infrastructure
- Security Operations

It also reinforced the importance of documenting technical implementations clearly so that environments can be reproduced, maintained, and improved over time.

---

# Final Thoughts

This lab demonstrated how Microsoft Azure services can be integrated to create a centralized security monitoring environment capable of collecting, storing, and investigating Windows security telemetry.

Although intentionally simplified for educational purposes, the architecture reflects many of the same concepts used in enterprise Security Operations Centers (SOCs), including centralized logging, event analysis, and cloud-native security monitoring.

The project provides a solid foundation for future enhancements such as custom analytics rules, automated response workflows, additional telemetry sources, and advanced threat detection capabilities.
