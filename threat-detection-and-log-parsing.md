---
layout: default
title: "Threat Detection Engineering and Log Parsing"
---
# (IAM) Threat Detection Engineering & Log Parsing

## 🎯 The Goal
The objective of this project was to analyze raw security logs from a Microsoft environment to identify malicious authentication patterns. Instead of relying on automated cloud alert dashboards, I exported raw telemetry to demonstrate the ability to parse JSON data structures and find indicators of a brute-force or password-spray attack.

---

## 🛠️ The Walkthrough

* **Simulated an incident:** Generated raw security events by opening an InPrivate browser session and intentionally failing a login attempt five consecutive times on a test directory account.
* **Exported the telemetry:** Logged back in as the administrator, opened the Microsoft Entra ID monitoring portal, navigated to `Sign-in logs`, and downloaded the recorded activity as a raw JSON file.
* **Analyzed the raw log structure:** Loaded the JSON log file into VS Code to analyze the data schema. I isolated the core attributes that track authentication failures, specifically focusing on the tracking IDs and status properties.
* **Mapped the correlation logic:** Identified that Microsoft uses specific error codes to denote different types of failures. I isolated error code `50126` (which represents an invalid username or password) and mapped it against the originating IP address fields to form a basic detection baseline.

---

## 🔍 The Proof

### 1. Log Schema Analysis
![VS Code JSON Log Screenshot](images/json-log-analysis.png)  
*Caption: Raw Microsoft Entra ID sign-in log schema open in VS Code, highlighting the error code and location data blocks used to detect anomalies.*

---

## 📋 Analyst Investigative Brief

When evaluating this log data, an entry-level analyst looks for specific fields to confirm an attack:

*   `properties.status.errorCode`: A value of `50126` confirms an authentication failure due to bad credentials.
*   `properties.ipAddress`: Tracks the public IP location of the request.
*   `properties.userPrincipalName`: Identifies the targeted account.

> 💡 **Conclusion & Remediation Strategy**
> If an enterprise monitoring system observes code `50126` triggering multiple times in rapid succession from a single external IP address targeting one or more directory accounts, it indicates an active brute-force or password-spray attempt. The remediation step is to flag the originating IP address and trigger an immediate verification or conditional access block policy.
