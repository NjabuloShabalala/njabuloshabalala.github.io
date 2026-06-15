# Hello there, I'm Njabulo Shabalala. Welcome to my portfolio. 
### Junior Cloud Security Analyst | Associate Security Operations (SOC) Analyst  
**📍 Johannesburg, South Africa**  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/njabulo-shabalala)
[![Hashnode](https://img.shields.io/badge/Hashnode-%232962FF.svg?style=flat-square&logo=hashnode&logoColor=white)](https://hashnode.com/@njabuloshabalala)
[![KC7](https://img.shields.io/badge/KC7%20Cyber-Investigator-%23E8121C.svg?style=flat-square&logoColor=white)](https://kc7cyber.com/profile/Njabulo%20Shabalala)
[![Email Me](https://img.shields.io/badge/Email%20Me-njabshabs%40gmail.com-%23D14836.svg?style=flat-square&logo=gmail&logoColor=white)](mailto:njabshabs@gmail.com) 
[![Download CV](https://img.shields.io/badge/Download%20CV-PDF-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)](njabulo-shabalala-soc-cv.pdf)

---

## 🧭 About Me

I am a Junior SOC Analyst candidate specializing in the Microsoft Security stack, with hands-on experience building threat-detection pipelines in Microsoft Sentinel and writing custom Kusto Query Language (KQL) rules.

My background features an unconventional BA in Organisational Psychology and International Relations, which directly drives my approach to security operations: structured analytical reasoning, rigorous investigative documentation, and the ability to translate complex technical alerts into actionable incident reports for stakeholders. 

I hold the (ISC)2 Certified in Cybersecurity (CC) and Microsoft SC-900 credentials, and am currently preparing to sit for the SC-200 (Security Operations Analyst). My practical capabilities include a self-directed Sentinel threat-hunting lab and hands-on KQL investigation work via KC7.

My target is to join a Microsoft-aligned South African MSSP or internal SOC team where I can immediately contribute to log monitoring and incident response, while developing into cloud security over time.

---

## 🛠️ Tech Stack & Skills

![Microsoft Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)
![Microsoft Defender](https://img.shields.io/badge/Defender%20XDR-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)
![Microsoft Entra](https://img.shields.io/badge/Microsoft%20Entra-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)
![KQL](https://img.shields.io/badge/KQL%20(Kusto)-%23005A9E.svg?style=flat-square&logo=microsoft&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-%23217346.svg?style=flat-square&logo=microsoft-excel&logoColor=white) 
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)

*   **SIEM / XDR:** Microsoft Sentinel, Microsoft Defender XDR, Microsoft Entra ID, Microsoft Purview
*   **Data Analysis:** Kusto Query Language (KQL), Advanced Excel
*   **Core Competencies:** Incident Response, Log Analysis, Threat Hunting, Information Protection & DLP, Investigative Documentation

---

## 🏅 Certifications & Verified Applied Skills

### Certifications
*   🛡️ **ISC2 Certified in Cybersecurity (CC)** 
*   ☁️ **Microsoft Certified: Security, Compliance, and Identity Fundamentals (SC-900)** 
*   🎯 *Currently preparing for the Microsoft Security Operations Analyst (SC-200) exam.*

### Verified Microsoft Applied Skills
*   **Defend Against Cyberthreats with Microsoft Defender XDR** (`ID: 2FA5BC2845849178`)  
*   **Implement Information Protection and Data Loss Prevention using Microsoft Purview** (`ID: C3002D00338101B`)  
*   **Get Started with Identities and Access using Microsoft Entra** (`ID: 410A7344BDF5EF04`) 

---

## 🔬 Practical Projects & Security Labs

### 🫆 [TitanShield: Threat Investigation with Microsoft Defender XDR](titanshield-threat-investigation.md) 
* Investigated two intrusions targeting a fictional defence company where one traced to Moonstone Sleet (North Korea) delivering a trojanised game via phishing to compromise a senior engineer's machine, and one traced to Crimson Sandstorm (Iran) using a romance scheme to socially engineer defence personnel into opening a malicious file.
* I used KQL across six log source tables including ProcessEvents, FileCreationEvents, OutboundNetworkEvents, and Email to trace each attack from initial delivery through reconnaissance and data staging to exfiltration, using operators like has_all(), has_any(), parse_url(), and inner joins to pivot between datasets and extract structured findings from raw log data.
* Attributed both attacks to named threat actors using the Microsoft Defender XDR Threat Intelligence Portal, identified cloud storage (Google Drive) as the exfiltration channel for the second attack chain, and mapped all observed attacker behaviour to MITRE ATT&CK techniques covering initial access, execution, discovery, staging, and exfiltration.

### 🕵️ [Dynamic Threat Investigations (KC7 Cyber Labs)](kc7-cyber-labs.md)
* Worked through a realistic election security breach scenario involving a threat actor campaign targeting the Valdoria Board of Elections ahead of a critical election cycle.
* Used KQL to trace the attacker's full activity from the phishing domain they stood up to mimic a government site, through credential theft of a staff account, lateral movement, and their attempts to extract information about voting machine infrastructure.
* Documented findings covering the compromised accounts, the attacker-controlled IP addresses and domains, the internal data they accessed, and the remediation steps needed to contain the threat before Election Day.

### 💻 [Setting Up Secure Access (RBAC) in Microsoft Entra ID](entra-id-rbac-lab)
* Designed an access structure that separates what junior analysts can do from what senior engineers can do, based on the idea that people should only have the minimum access required for their role.
* Set up that structure in Microsoft Entra ID using Security Groups tied to built-in Azure roles Security Reader for analysts, Security Administrator for engineers so permissions are managed centrally rather than assigned individually.
* Tested whether the boundaries actually held by attempting actions that lower-privileged accounts shouldn't be able to perform, and confirmed that those actions were blocked as intended.

### 💻 [Threat Detection Engineering & Log Parsing](threat-detection-and-log-parsing)
* Deliberately triggered repeated failed login attempts against a test Azure environment to generate real sign-in failure logs that could be used for detection work.
* Pulled the raw log data in JSON format and examined it directly outside of Azure's dashboard to understand exactly what fields and values the logs contain at a technical level.
* Built a detection rule that flags suspicious behaviour by combining a specific Azure login failure code (ResultType 50126) with outbound network activity, creating a reliable signal for potential brute-force or credential-stuffing attacks.

### 💻 [Secure Asset Governance & Access Auditing](secure-asset-governance-and-access-auditing)
* Set up a test environment in Microsoft Azure to simulate a real company's cloud infrastructure for security review purposes.
* Pulled a full list of who has access to what within that environment, then mapped out every permission assigned which including inherited ones to check whether they followed the principle of least privilege.
* Wrote an audit report identifying where access was broader than it needed to be, and outlined the specific steps required to bring those permissions in line with security best practice.

---

## 📬 Let's Connect

I am actively looking to connect with security teams, hiring managers, and professionals within the South African cybersecurity ecosystem.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/njabulo-shabalala)
[![Hashnode](https://img.shields.io/badge/Hashnode-%232962FF.svg?style=flat-square&logo=hashnode&logoColor=white)](https://hashnode.com/@njabuloshabalala)
[![KC7](https://img.shields.io/badge/KC7%20Cyber-Investigator-%23E8121C.svg?style=flat-square&logoColor=white)](https://kc7cyber.com/profile/Njabulo%20Shabalala)
[![Email Me](https://img.shields.io/badge/Email%20Me-njabshabs%40gmail.com-%23D14836.svg?style=flat-square&logo=gmail&logoColor=white)](mailto:njabshabs@gmail.com) 
[![Download CV](https://img.shields.io/badge/Download%20CV-PDF-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)](njabulo-shabalala-soc-cv.pdf)

---

<sub>Building from a household, in Johannesburg</sub>
