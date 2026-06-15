---
layout: default
title: "TitanShield Investigation"
---

# 🫆 TitanShield: Threat Investigation with Microsoft Defender XDR

---

## Overview

TitanShield is a fictional defence company facing simultaneous intrusions from two separate nation-state threat actors. This investigation required tracing attacker activity from initial infection through to data exfiltration across multiple log sources, using KQL for analysis and Microsoft Defender XDR Threat Intelligence for attribution.

**Two attack chains were uncovered:**

- **Moonstone Sleet (North Korea):** A weaponised game distributed via phishing used to infect endpoints and stage data from Project Omega which happens to be TitanShield's most sensitive project.
- **Crimson Sandstorm (Iran):** A romance-based social engineering scheme targeting defence personnel, leading to malicious file delivery and cloud-based data exfiltration.

---

## Skills Demonstrated in this project

- Writing and iterating KQL queries across multiple log source tables
- Pivoting between datasets to follow attacker activity across machines and timelines
- Identifying reconnaissance and data staging patterns in process execution logs
- Tracing phishing campaigns through email log analysis
- Using `parse_url()`, `join`, `summarize`, `has_all()`, and `has_any()` operators to extract structured insights from raw log data
- Threat actor attribution using the Microsoft Defender XDR Threat Intelligence Portal
- Mapping observed behaviour to MITRE ATT&CK techniques
- Connecting social engineering tactics to technical indicators across two separate attack chains

---

## Investigation Walkthrough

---

### Identifying the Initial Infection Vector

![Screenshot 1](project-4-titansdefend-xdr/titans-shield-screenshot-1.png) 

**What's happening:**

The investigation starts with a suspicious file on `UB9I-DESKTOP`. Querying the `Employees` table identified the machine's owner as **James Douglas** a Lead Defense Engineer with no MFA configured on his account. A follow-up query against `FileCreationEvents` filtered for the filename `DeTankWar` and returned a single result: `DeTankWar.exe`, with a SHA256 hash scoring **100/100 for maliciousness** in threat intelligence.

**Action taken:**

Cross-referenced the file hash with threat intelligence, which attributed DeTankWar directly to **Moonstone Sleet** which is a North Korean state-sponsored group known for distributing trojanised games as an initial access method.

**Why this matters:**

This establishes the initial infection vector for the first attack chain. The absence of MFA on James Douglas's account made credential abuse significantly easier for the attacker after the malware executed. The fact that he's a Lead Defense Engineer means the access obtained was high-value from the start.

---

### Identifying Which Employee Roles Were Targeted

![Screenshot 2](project-4-titansdefend-xdr/titans-shield-screenshot-2.png)

**What's happening:**

Suspicious commands were detected across multiple machines — specifically `echo` commands redirecting output to `logs.txt`. A KQL query using `has_all()` against `ProcessEvents` identified all hostnames and usernames running these commands. The results were then joined against the `Employees` table and aggregated by job role using `summarize count() by role`.

**Action taken:**

The query returned **Network Engineers as the most affected role (14 accounts)**. This is not random because network engineers hold access to infrastructure configuration, routing tables, and internal network topology. The attacker was deliberately targeting personnel with the most operationally sensitive access.

**Why this matters:**

Understanding which roles are targeted tells you what the attacker was after. This shaped the rest of the investigation toward network-level evidence and away from general endpoint noise.

---

### Establishing the Attack Timeline

![Screenshot 3](project-4-titansdefend-xdr/titans-shield-screenshot-3.png)

**What's happening:**

Pivoting to `IL5M-DESKTOP` (Taylor's machine), a `ProcessEvents` query projecting `timestamp` and `process_commandline` returned **376 rows** of process execution history. The question was: when did the suspicious activity start?

**Action taken:**

The earliest suspicious command on Taylor's machine was timestamped **2024-07-01T08:39:50.000Z**. This became the anchor point for the investigation timeline everything before this is baseline; everything after is attacker activity.

**Why this matters:**

Without a precise start time, an incident investigation has no scope boundary. This timestamp defines the evidence window and determines which logs are relevant. It also tells you how long the attacker was present before detection, which is a key metric in any post-incident report.

---

### Reconnaissance and Data Staging

![Screenshot 4](project-4-titansdefend-xdr/titans-shield-screenshot-4.png)

**What's happening:**

Filtering `ProcessEvents` on `IL5M-DESKTOP` using `has_any()` against a set of suspicious keywords — `wmic`, `curl`, `logs.txt`, `TopSecret`, `StagingArea`, `echo` — and ordering by timestamp returned **39 rows** of attacker-controlled commands.

The results showed a systematic pattern: `cmd.exe` commands collecting OS details, running processes, installed software, antivirus configuration, drive inventory, and task lists — all redirected into `logs.txt`.

**Action taken:**

Identified this as a structured **data staging operation**. Before exfiltrating anything, the attacker built a local file containing a full inventory of the compromised machine's environment. The `logs.txt` filename and consistent redirect pattern across commands confirmed intentional staging, not background system activity.

**Why this matters:**

Staging before exfiltration is a deliberate technique it reduces the number of outbound connections required and makes the eventual exfiltration harder to detect. Recognising this pattern in process logs is a core skill in any detection or incident response role.

---

### Phishing Email and File Exfiltration (Second Attack Chain)

![Screenshot 5](project-4-titansdefend-xdr/titans-shield-screenshot-5.png)

**What's happening:**

`FileCreationEvents` on `IL5M-DESKTOP` confirmed the creation of a file named `New_Diet_Plan_For_My_Love.xlsx`. `OutboundNetworkEvents` filtered on the source IP and filename confirmed the file was subsequently exfiltrated via a GET request to `healthylifestyle.com`. Querying the `Email` table traced the delivery of this file to a sender — `marcella_flores@gmail.com` — who sent a link containing the malicious URL.

**Action taken:**

Used `parse_url()` to extract distinct domains from all links sent by this address, returning three domains used across the campaign: `outlook-services.com`, `yogalifestyle.com`, and `healthylifestyle.com`.

**Why this matters:**

The filename is deliberate tradecraft a document that looks like a personal file shared between two people in a relationship. This is the delivery mechanism for the second attack chain, a romance-based scheme. The `parse_url()` pivot is the key technique here: it converts raw link strings into structured domain data, enabling attribution across multiple phishing infrastructure domains from a single query.

---

### Threat Actor Attribution via Defender XDR Threat Intelligence

![Screenshot 6](project-4-titansdefend-xdr/titans-shield-screenshot-6.png)

**What's happening:**

The three extracted domains were submitted to the **Microsoft Defender XDR Threat Intelligence Portal** for lookup. The portal linked these domains to a known threat actor.

**Action taken:**

The domains were attributed to **Crimson Sandstorm** an Iranian state-sponsored threat actor known specifically for romance-based social engineering campaigns targeting defence, aerospace, and technology organisations.

**Why this matters:**

This closes the attribution loop on the second attack chain. Crimson Sandstorm's known operational pattern — building fictitious personal relationships with targets to deliver malware matches exactly what was observed in the email logs and file evidence. Defender XDR Threat Intelligence turns technical indicators (domains, hashes) into adversary context, which is what transforms a detection into an actionable threat report.

---

### Confirming Cloud-Based Exfiltration

![Screenshot 7](project-4-titansdefend-xdr/titans-shield-screenshot-7.png)

**What's happening:**

Final investigation queries targeted `InboundNetworkEvents` from two suspicious external IPs, `Email` with "data" and "exfil" in the subject line, and `OutboundNetworkEvents` from the internal subnet. The `OutboundNetworkEvents` query filtered for POST requests containing "google" in the URL, returning **94 rows** of POST requests to Google Drive and Google Docs.

**Action taken:**

Connected the full kill chain: phishing delivery → malware execution → data staging via `cmd.exe` → exfiltration via cloud storage. Both attack chains fully traced and attributed.

**Why this matters:**

Exfiltrating via Google Drive is deliberate defence evasion. POST requests to Google infrastructure blend in with legitimate organisational traffic and don't trigger most perimeter security rules. Detecting this requires filtering on internal source IPs posting to cloud storage URLs a query pattern worth keeping in any detection library. This is a known Crimson Sandstorm operational preference.

---

## Summary of Findings

| Finding | Detail |
|---|---|
| Attack Chain 1 | Moonstone Sleet (trojanised DeTankWar game via phishing) |
| Attack Chain 2 | Crimson Sandstorm (romance scheme targeting defence personnel) |
| Primary Victims | Network Engineers (14 accounts affected) |
| Initial Access | Phishing links delivering malicious executables and files |
| Staging Method | `cmd.exe` enumeration output redirected to `logs.txt` |
| Exfiltration Method | Cloud storage (Google Drive/Docs) via POST requests |
| Key Security Gap | No MFA on James Douglas's account |
| Attribution Tool | Microsoft Defender XDR Threat Intelligence Portal |

---

## KQL Techniques Used

| Operator / Function | Purpose |
|---|---|
| `has_all()` | Filter for records containing all specified strings |
| `has_any()` | Filter for records containing any of a set of strings |
| `join kind=inner` | Link datasets on a shared key (e.g. username) |
| `summarize count() by` | Aggregate results by a field |
| `order by ... desc/asc` | Sort results by timestamp or count |
| `parse_url().Host` | Extract domain from a full URL string |
| `distinct` | Deduplicate results |
| `project` | Select specific columns for output |
| `extend` | Create a new calculated column |
| `contains` | Partial string match |
