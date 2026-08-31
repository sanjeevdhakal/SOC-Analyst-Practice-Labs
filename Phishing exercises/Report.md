# Incident Triage Report: Unauthorized Credential Harvester Campaign

## 📊 Incident Overview
*   **Incident ID:** INC-2026-0831
*   **Severity Rating:** High (Targeted Phishing)
*   **Triage Status:** Contained / Resolved
*   **Investigator:** Tier 1 SOC Analyst

---

## 1. Executive Summary
An employee flagged a suspicious, highly targeted email themed as an urgent Microsoft Outlook Security Alert. The email attempted to induce panic by claiming the user's mailbox would be terminated within 24 hours unless they completed identity verification via an embedded link button. 

Cross-layer forensic analysis confirmed that the email originated from outside the corporate ecosystem, and the destination link was a verified password-harvesting portal. SIEM log analysis confirmed zero infrastructure impact; no internal assets successfully established outbound sessions to the malicious site.

---

## 2. Artifact & Indicator Log
The following forensic indicators of compromise (IOCs) were extracted directly from the attack sample:

| Artifact Type | Artifact Value | Description / Context |
| :--- | :--- | :--- |
| **Email Sender** | `Microsoft Outlook Security Team <outlook-alerts-noreply@gmail.com>` | Display name spoofing a corporate entity using a free consumer account. |
| **Email Recipient** | `victim-employee@company.com` | Internal target employee corporate mailbox footprint. |
| **Email Subject** | `CRITICAL: Your Mailbox Will Be Closed - Verify Identity Now` | Social engineering hook utilizing urgent/coercive phrasing. |
| **Network Domain** | `emailsecalerts[.]net` | Lookalike/typo-squatted domain used for malicious hosting infrastructure. |
| **Malicious URL** | `http://emailsecalerts[.]net/outlook-login/verify.html` | The explicit web directory path hosting the credential-grabbing form. |
| **File Extension** | `phishing_alert.eml` | The raw text file container containing the core headers and email body. |

---

## 3. Forensic Artifact Analysis

| Forensic Layer | Analysis Methodology & Tools | Key Findings & Indicators |
| :--- | :--- | :--- |
| **Mail Headers** | Linux Command Line Terminal (`grep -E "From:\|Received:"`) | Confirmed an absolute mismatch. The `Received:` path routes authentic Google mail transfer agents, proving the message originated from a public Gmail box, completely invalidating the official Microsoft corporate branding. |
| **Reputation Audit** | Open-Source Threat Intelligence (`VirusTotal`) | **11 out of 92 enterprise security engines** flagged the domain as explicitly malicious. Leading vendors (BitDefender, Sophos, Fortinet) have categorized this domain as an active password-harvesting site. |
| **Internal Impact** | Centralized SIEM Monitoring (`Wazuh Dashboard Query`) | Executed a broad global query for network metadata matching `emailsecalerts[.]net`. The engine returned **0 results**, mathematically proving that no employee clicked the link or initialized an outbound connection block. |

---

## 4. Recommended Defensive Remediation Controls

| Strategic Phase | Recommended Defensive Action | Operational Purpose |
| :--- | :--- | :--- |
| **Immediate Containment** | Gateway-Wide Mail Purge | Run a query on the enterprise email gateway to search for and permanently delete all messages containing the sender address or subject line from all user folders. |
| **Network Hardening** | Perimeter Firewall Domain Block | Add `emailsecalerts[.]net` to the corporate firewall, proxy server, and DNS blacklists to instantly drop traffic if any endpoint attempts a connection later. |
| **Endpoint Protection** | EDR / SIEM Alert Configuration | Create a custom tracking rule inside the SIEM to trigger an immediate, high-severity alert if the string `emailsecalerts[.]net` appears anywhere inside local machine logs. |
| **User Awareness** | Targeted Simulation & Training | Route the targeted team through an immediate 5-minute refresher training module covering display-name spoofing and corporate notification verification rules. |
