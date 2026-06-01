---
layout: default
title: "KC7 Cyber Labs"
---
## 🕵️ About This Lab

This investigation is part of [KC7](https://kc7cyber.com), which is a Microsoft cybersecurity training platform that simulates real-world breach scenarios using **KQL** and **Azure Data Explorer**.

---

## 🗳️ The Scenario

The Valdoria Board of Elections is preparing for a high-stakes election. To manage increased demand, the board recently onboarded additional poll workers. While officials have publicly assured citizens that voting infrastructure is secure, threat actors are actively running an influence operation, seeding doubt about the integrity of the vote before Election Day.

As the analyst, the objective was to investigate the threat activity, trace the attack chain, and determine the full scope of the compromise.

---

## 📸 A Note on Screenshots

The screenshots below capture key moments and findings from the investigation. The full lab spans **70+ questions** — only significant findings and pivotal query results are documented here.

---

![KC7 Lab screenshots](kc7-screenshots/basic-employee-lookup-1.png) 
In this screenshot we see the basic employee table lookup using a role filter. This was the entry point into the Valdoria dataset, this helped me with establishing familiarity with the schema before pivoting to threat-relevant tables and questions.

![KC7 Lab screenshots](kc7-screenshots/passive-dns-query-2.png) 
PassiveDns query filtering by a suspicious IP (55.49.227.170). The KQL query returned two domain associations where one was a legitimate government domain and the one was an attacker-controlled domain, confirming IP reuse as a pivot point

![KC7 Lab screenshots](kc7-screenshots/domain-based-passive-dns-3.png) 
Domain-based PassiveDns lookup on the fraudulent government lookalike domain and the result shows us that the domain resolved to three distinct IPs across different dates this can be seen as evidence of infrastructure rotation by the threat actor.

![KC7 Lab screenshots](kc7-screenshots/multi-stage-investigation-4.png) 
In this screenshot we make use of a multi-stage investigation: a let statement stores IPs associated with the fraudulent domain, which is then used to filter InboundNetworkEvents via subquery. Separately, OutboundNetworkEvents is queried to surface internal user traffic to the phishing domain. 

![KC7 Lab screenshots](kc7-screenshots/chained-query-block-5.png) 
Chained query block spanning AuthenticationEvents (with a timestamp range filter bracketing the suspected compromise window), Email, and Employees tables. Ive made the use of comments to firstly take note but to also help me identify the compromised account and who it belongs to (Anderson Snooper) and the first confirmed threat actor login timestamp.

![KC7 Lab screenshots](kc7-screenshots/iterative-narrowing-across-table-6.png) 
Iterative narrowing across the AIPrompts table, first a broad take 10 to inspect structure and see the tables that are available for query, then filtering by prompt content, then by response content. Used to surface vendor information disclosed by an internal AI chatbot to the threat actors.

![KC7 Lab screenshots](kc7-screenshots/email-query-table-7.png) 
Email table query filtering on a specific recipient to surface inbound messages. The results of the query gave us the attachment column which reviewed to identify a PDF (ValdoriaVotingMachinesNetworkGuide.pdf) sent to the Election Commissioner(the threat actor) and this is a document that the threat actors could exploit for physical access planning.

![KC7 Lab screenshots](kc7-screenshots/authentication-events-8.png) 
Here i used the AuthenticationEvents analysis on the arbobama account,in which  the results set shows a pattern of failed logins from internal IPs followed by a successful login from an external IP (214.85.104.248) on a Mac user-agent. This behaviorally distinct from all prior sessions, confirming unauthorized external access.

![KC7 Lab screenshots](kc7-screenshots/final-employee-lookup.png) 
Employee lookup by role to identify the Election Commissioner. There was a single result which was used to anchor subsequent email and authentication queries targeting that account specifically.

--- 

## 📋 Investigative Summary

The attack started with a **phishing email** that tricked a senior manager into giving up their login credentials. Because the manager held elevated privileges, the threat actor didn't need to hack their way deeper into the system — they already had the keys.

Once inside, they used the compromised account to identify **who would be counting the votes and how**. The end goal was to feed that information into an AI tool to generate **fake voting results** and undermine public trust in the election.

The attack was caught before it reached that stage. The compromised account was **isolated and contained**, cutting off the threat actor's access before any votes were tampered with.

---

💡 **The lesson here is straightforward:** One phishing email targeting the right person was enough to bypass all technical defences. The strongest controls against this attack would have been **MFA on privileged accounts** and **behavioural alerts** flagging unusual activity from senior user accounts.
