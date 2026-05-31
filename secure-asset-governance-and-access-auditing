# (IAM) Secure Asset Governance & Access Auditing

## 🎯 The Goal
The objective of this project was to perform a manual identity compliance audit on a cloud data storage resource. This project simulates the governance tasks an analyst handles to verify that sensitive corporate data assets are properly locked down and that access control lists (ACLs) match internal security policies.

---

## 🛠️ The Walkthrough

* **Provisioned a Target Asset:** Created a resource group named `RG-Corporate-Data` and deployed a basic Azure Storage Account to simulate a repository holding sensitive corporate files.
* **Mapped Identity Assignments:** Navigated to the Access Control (IAM) blade of the storage resource and extracted the Role assignments list to review every user and group with permissions to the data.
* **Identified Governance Flaws:** Compared the active permissions list against least-privilege standards to identify over-privileged accounts or groups with broad modification capabilities they did not operationally require.
* **Produced Compliance Documentation:** Documented the findings in an audit format, outlined the exact adjustments needed to reduce risk exposure, and demonstrated how to clean up the asset's access control boundaries.

---

## 🔍 The Proof

### 1. Identity & Access Management (IAM) Identity Audit
![Access Control IAM Role Assignments Screenshot](images/azure-iam-audit.png)  
*Caption: The Azure IAM Role Assignments dashboard displaying the explicit user permissions and roles assigned to the storage resource.*

---

## 📄 Access Certification & Audit Report

| Audit Parameter | Details |
| :--- | :--- |
| **Target Resource** | Azure Storage Account (Blob Container) inside `RG-Corporate-Data` |
| **Current Operational State** | The group `Group-SecOps-Engineers` has broad `Owner` / `Contributor` permissions assigned to the asset root. |

### ⚠️ Security Finding
Granting structural modification or administrative rights over storage objects to engineering teams during standard operations violates least-privilege principles. Excessive rights increase the likelihood of accidental data exposure or unauthorized deletion.

> 🛠️ **Remediation Action Plan**
> To mitigate this risk, the assignment must be modified immediately:
> 1. **Downgrade Privileges:** The engineering group `Group-SecOps-Engineers` should be downgraded to a standard `Reader` role or a specific data-access role (such as `Storage Blob Data Reader`) for day-to-day operations.
> 2. **Enforce Just-In-Time Access:** Any structural infrastructure modifications or configuration changes should require explicit, justified elevation requests through a formal change management or PIM process rather than static assignments.
