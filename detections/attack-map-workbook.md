# Microsoft Sentinel Attack Map Workbook

## Overview

The Microsoft Sentinel Attack Map Workbook provides a geographic visualization of authentication attempts observed within the lab environment.

The workbook uses Windows Security Events enriched with geolocation data to display the origin of inbound authentication attempts on an interactive world map.

---

## Purpose

The workbook helps analysts:

- Visualize attack origins
- Identify geographic attack patterns
- Detect repeated activity from the same locations
- Improve situational awareness during investigations

---

## Data Source

- Microsoft Sentinel
- Azure Log Analytics Workspace
- Windows Security Events
- Geolocation enrichment

---

## Components

The workbook includes:

- Interactive world map
- Source IP addresses
- Country of origin
- Authentication count
- Event timestamps

---

## Investigation Workflow

```
Windows Security Events

↓

Azure Monitor Agent

↓

Log Analytics Workspace

↓

Microsoft Sentinel

↓

Attack Map Workbook

↓

Analyst Investigation
```

---

## Security Benefits

The Attack Map enables analysts to:

- Detect high-risk regions
- Identify repeated authentication attempts
- Monitor attack trends
- Support incident investigations

---

## Production Recommendations

For production deployments, the workbook can be extended with:

- Threat Intelligence feeds
- Watchlists
- Automated incident creation
- Microsoft Defender integration
- Analytics Rules
