![Header](https://capsule-render.vercel.app/api?type=waving&color=0:2541D6,100:0056D6&height=200&section=header&text=Njabulo%20Shabalala&fontSize=42&fontColor=ffffff&animation=fadeIn&desc=Aspiring%20Data%20Analyst%20%7C%20Microsoft%20Stack&descAlignY=62&descSize=18)

**📍 Johannesburg, South Africa**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/njabulo-shabalala)
[![Hashnode](https://img.shields.io/badge/Hashnode-%232962FF.svg?style=flat-square&logo=hashnode&logoColor=white)](https://hashnode.com/@njabuloshabalala)
[![Email Me](https://img.shields.io/badge/Email%20Me-njabshabs%40gmail.com-%23D14836.svg?style=flat-square&logo=gmail&logoColor=white)](mailto:njabshabs@gmail.com)
[![Download CV](https://img.shields.io/badge/Download%20CV-PDF-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)](njabulo-shabalala-data-cv.pdf)

<!-- ⚠️ PLACEHOLDER: CV file above still points to the old SOC CV. Replace with a data-analyst-targeted CV before publishing. -->

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

## 🧭 About Me

<!-- ⚠️ PLACEHOLDER: rewrite this paragraph once the SQL project and Nompumelelo write-up exist — right now it's making a claim ("data analyst") the projects section below can't back up yet. Draft below is a starting point, not final copy. -->

I'm building toward a career in data analysis, with a Microsoft-stack focus (SQL, Power BI, Excel). My background is an unconventional BA in Organisational Psychology and International Relations, which shapes how I approach analysis: structured reasoning, clear documentation, and translating findings into something a non-technical stakeholder can act on.

My most direct evidence of analytical work so far is Project Nompumelelo, an AI-powered WhatsApp document ingestion pipeline I built end-to-end — OCR extraction, structured data parsing, and a revenue-range calculation engine — for South African informal traders. I also have hands-on query experience with Kusto Query Language (KQL) in a security context, which I'm now extending to SQL.

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

## 🛠️ Tech Stack & Skills

<table>
<tr>
<td align="center"><img src="https://skillicons.dev/icons?i=postgres" width="48" height="48" alt="SQL"/><br/><sub><b>SQL</b></sub></td>
<td align="center"><img src="powerbi.png" width="48" height="48" alt="Power BI"/><br/><sub><b>Power BI</b></sub></td>
<td align="center"><img src="https://skillicons.dev/icons?i=azure" width="48" height="48" alt="Azure"/><br/><sub><b>Azure</b></sub></td>
<td align="center"><img src="excel.png" width="48" height="48" alt="Excel"/><br/><sub><b>Excel</b></sub></td>
<td align="center"><img src="https://img.shields.io/badge/-005A9E?style=flat-square&logo=microsoft&logoColor=white" width="48" height="20" alt="KQL"/><br/><sub><b>KQL</b></sub></td>
</tr>
</table>

*   **Querying & Data Prep:** SQL <!-- ⚠️ PLACEHOLDER: not yet proven by a project — see Projects section -->, KQL, Advanced Excel
*   **Visualization & Reporting:** Power BI <!-- ⚠️ PLACEHOLDER: no project yet -->
*   **Data Pipelines:** Azure Functions, OCR/document ingestion (proven via Nompumelelo)
*   **Core Competencies:** Data cleaning, stakeholder-facing reporting, investigative documentation

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

## 🏅 Certifications & Progress

*   🎯 **Google Data Analytics Professional Certificate**
*   🎯 **Microsoft PL-300 (Power BI Data Analyst)**
*   🛡️ **(ISC)2 Certified in Cybersecurity (CC)**
*   ☁️ **Microsoft SC-900**

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

## 📊 Data Analytics Projects

<!-- ⚠️ This entire section is placeholders except Nompumelelo. Do not publish this README until at least the SQL project is filled in — an "About Me" that says "data analyst" over an empty projects section will read worse than the old version. -->

### 🧾 Project Nompumelelo — WhatsApp Document Ingestion & Revenue Engine
* Built an end-to-end data pipeline (Azure Function App) that ingests handwritten and printed receipts via WhatsApp, runs OCR extraction, and reconciles line items against expected totals with mismatch flagging.
* Designed a revenue-range engine that converts unstructured receipt data into a low/high revenue estimate for informal traders, including a clarification flow to resolve ambiguous entries before they hit the calculation layer.
* Tested against founder-generated South African spaza-style receipts; built and stored via Azure Table Storage.
* [Full write-up →](#) <!-- ⚠️ PLACEHOLDER: link to a proper project page once written -->

### 🗄️ SQL Project — *Placeholder*
<!-- ⚠️ PLACEHOLDER: your own stated first-priority skill. Nothing here yet. -->
*   Status: Not started.
*   Suggested scope: a real dataset (e.g. Nompumelelo's own transaction data, or a public SA-relevant dataset), multi-table joins, CTEs/window functions, a written explanation of business questions answered.

### 📈 Power BI Project #1 — *Placeholder*

### 📈 Power BI Project #2 — *Placeholder*

### 🧮 Excel Project #1 — *Placeholder*

### 🧮 Excel Project #2 — *Placeholder*

### 🧩 Data Modeling Project — *Placeholder*

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

## 🔬 Prior Security Work (Applied Query & Investigation Experience)

<!-- Kept as supporting evidence, not the headline — demoted from the top of the original README. Demonstrates KQL fluency and structured investigation, both transferable to analysis work. -->

### 🫆 TitanShield: Threat Investigation with Microsoft Defender XDR
* Investigated two intrusions targeting a fictional defence company, tracing attacker activity from initial phishing delivery through reconnaissance, staging, and exfiltration.
* Used KQL across six log tables (ProcessEvents, FileCreationEvents, OutboundNetworkEvents, Email, etc.), applying `has_all()`, `has_any()`, `parse_url()`, and joins to pivot between datasets and extract structured findings.
* Attributed both attacks to named threat actors and mapped observed behaviour to MITRE ATT&CK.

### 🕵️ Dynamic Threat Investigations (KC7 Cyber Labs)
* Worked through an election security breach scenario, using KQL to trace an attacker's activity from a spoofed phishing domain through credential theft, lateral movement, and attempted access to sensitive infrastructure data.
* Documented compromised accounts, attacker infrastructure, and remediation steps.

### 💻 Setting Up Secure Access (RBAC) in Microsoft Entra ID
* Designed and implemented a least-privilege access structure using Security Groups tied to built-in Azure roles.
* Verified enforcement by testing that lower-privileged accounts were correctly blocked from restricted actions.

### 💻 Threat Detection Engineering & Log Parsing
* Generated real sign-in failure logs in a test Azure environment and examined raw JSON log data directly.
* Built a detection rule combining a specific Azure failure code with outbound network activity to flag potential brute-force attempts.

### 💻 Secure Asset Governance & Access Auditing
* Audited permissions (including inherited) across a simulated Azure environment against least-privilege principles.
* Wrote a report identifying overbroad access and remediation steps.

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

## 📬 Let's Connect

I'm actively looking to connect with data teams, hiring managers, and analysts in the South African data ecosystem.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/njabulo-shabalala)
[![Hashnode](https://img.shields.io/badge/Hashnode-%232962FF.svg?style=flat-square&logo=hashnode&logoColor=white)](https://hashnode.com/@njabuloshabalala)
[![Email Me](https://img.shields.io/badge/Email%20Me-njabshabs%40gmail.com-%23D14836.svg?style=flat-square&logo=gmail&logoColor=white)](mailto:njabshabs@gmail.com)
[![Download CV](https://img.shields.io/badge/Download%20CV-PDF-%230078D4.svg?style=flat-square&logo=microsoft&logoColor=white)](njabulo-shabalala-data-cv.pdf)

![divider](https://capsule-render.vercel.app/api?type=rect&color=0056D6&height=3&section=header)

<sub>Building from a household, in Johannesburg</sub>
