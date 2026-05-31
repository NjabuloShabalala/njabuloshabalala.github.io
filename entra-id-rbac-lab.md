---
layout: default
title: "IAM: Setting Up Secure Access (RBAC) in Microsoft Entra ID" 
---
# (IAM) Setting Up Secure Access (RBAC) in Microsoft Entra ID
## 🛠️ The Walkthrough

* **Planned the Access Matrix:** Before even attempting to touch the cloud portal (Entra ID), I had to define the two roles for this project: a Tier 1 Analyst who needs read-only access to logs, and an Engineer who requires full security administration rights.
* **Created Security Groups:** Logged into the Microsoft Azure Portal, navigated to Microsoft Entra ID, and created two groups: `Group-SecOps-Analysts` and `Group-SecOps-Engineers`.
* **Provisioned Test Identities:** Created two dummy user accounts (`analyst.test` and `engineer.test`) and added them to their respective security groups to simulate real team members.
* **Assigned Directory Roles:** Mapped the Microsoft built-in `Security Reader` role to the Analyst group, and the `Security Administrator` role to the Engineer group.
* **Enforced the Security Boundary:** Logged in as the test analyst user in an InPrivate window and attempted to perform an unauthorised administrative action (deleting a directory user) to verify that Entra ID blocked the request.

---

## 🔍 The Proof

### 1. Group Membership Configuration
![Group Members Screenshot] 
(portfolio-project-1-iam/analyst-test-groups-1.png) 
(portfolio-project-1-iam/engineer-test-group-2.png) 
*Caption: Microsoft Entra ID group membership configuration showing the test analyst identity successfully mapped to the Security Analyst group.*

---

### 2. Role Assignment Configuration
![Role Assignment Screenshot]
(portfolio-project-1-iam/security-administrator-assignments.png) 
(portfolio-project-1-iam/security-reader-assignments.png)
*Caption: Directory role assignments showing the Security Reader role explicitly bound to the Analyst group.*

---

### 3. Boundary Enforcement Verification
![Access Denied Screenshot](images/access-denied.png)  
*Caption: The "Insufficient privileges" error banner triggered when the test analyst account attempted to delete directory data, validating the RBAC boundary.*
