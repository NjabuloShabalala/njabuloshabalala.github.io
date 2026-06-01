---
layout: default
title: "KC7 Cyber Labs"
---
About this lab and game.

The scenario of the following lab is - "The Valdoria Board of Elections is gearing up for the most critical election in recent memory. To ensure a smooth voting experience, the board has hired additional poll workers in the past month, preparing them to support operations and assist voters. Officials have made it clear that the voting machines are highly secure, working tirelessly to reassure the public about the integrity of the election process.

However, malicious actors are actively working to sow doubt, hoping to make citizens question the validity of their vote. As Election Day approaches, Valdoria's citizens anxiously watch, wondering if democracy will withstand these challenges."

The following screenshots are taken from various moments within the investigation. (not all 70+ questions and answers were screenshotted because that would have been unreasonable)
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
