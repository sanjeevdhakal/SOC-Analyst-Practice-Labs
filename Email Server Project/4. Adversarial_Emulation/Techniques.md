#  Active Attack Simulations & Adversarial Emulation

## 📋 Operational Objective
This playbook documents the implementation of three major hacking techniques executed from our **Kali Linux attack platform**. We will use these simulations to test our mail server defenses and map all activities straight to the real-world **MITRE ATT&CK Framework**.

---

---

## 🕵️‍♂️ Technique 1: Hacker Reconnaissance (Hidden Tracking Pixels)
*   **MITRE ATT&CK ID:** Tactic: Reconnaissance | Gather Victim Identity Info (**T1589**)
*   **Status:** ✅ 100% COMPLETE

### 🛠️ Implementation Summary
Successfully designed an urgent security synchronization email template inside the Gophish workspace framework. Embedded automated system tracking parameters (`{{.Tracker}}`) directly into the underlying text body. When the victim opens the mail package payload, this hook triggers an invisible, real-time background link connection back to the attacker's console infrastructure, logging the network exposure timeline automatically.

![email_template](email_template.png)

---

---

## 🔑 Technique 2: Credential Harvesting (Copycat Login Portals)
*   **MITRE ATT&CK ID:** Tactic: Initial Access | Phishing: Spearphishing Link (**T1566.002**)
*   **Status:** ✅ 100% COMPLETE

### 🛠️ Implementation Summary
Successfully compiled a custom HTML credential verification form inside the local Gophish database layer. The portal code structure was injected manually to bypass upstream internet routing blocks. The page is configured with automated keystroke tracking variables to intercept inbound credential data strings before redirecting the target user endpoint to an external resource link.
![Landing_page](landing_page.png)

### 🛠️ Post-Incident Forensic Summary
The simulation was executed with total technical success. When the target user account entered data parameters into the cloned identity layout and clicked submit, our credential harvesting engine intercepted the transaction hooks before any data could reach external endpoints. The system recorded the raw, plaintext authentication credentials instantly inside the attacker-controlled database:
*   **Harvested Account:** `sanjeev@gmail.com`
*   **Intercepted Password Payload:** `Hello123!`

The user's local web browser session was automatically and seamlessly redirected to an external resource pool (`google.com`) to mask the exploitation indicators, ensuring complete defensive evasion during the compromise window.

---

## 📦 Technique 3: Malicious Attachments (Simulating Backdoor Deliveries)
*   **MITRE ATT&CK ID:** Tactic: Initial Access | Phishing: Spearphishing Attachment (**T1566.001**)
*   **Status:** ⏳ NOT STARTED

