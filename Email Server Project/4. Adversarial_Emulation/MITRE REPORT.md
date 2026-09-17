---

## 📊 MITRE ATT&CK Adversarial Emulation Mapping Report

To align our active threat simulations with industry standards, the actions executed by our Kali Linux attack platform have been precisely cross-referenced against the **MITRE ATT&CK Matrix**. This layout allows us to visualize how adversarial behaviors map against our Ubuntu Mail Server and Lubuntu Client defenses.

| MITRE Tactic | Technique Name | Technique ID | Lab Simulation Action | Defensive Telemetry Caught / Impact |
| :--- | :--- | :--- | :--- | :--- |
| **Reconnaissance** | Gather Victim Identity Info | **T1589** | Hidden `1x1` Tracking Pixel template macro (`{{.Tracker}}`) embedded inside email HTML body text. | **Bypassed / Triggered:** Logged a real-time `Email Opened` callback connection to Kali Linux the moment the user allowed remote images. |
| **Initial Access** | Phishing: Spearphishing Link | **T1566.002** | Crafting an urgent IT synchronization email template targeted explicitly at `hr-manager@corporate-firm.local`. | **Bypassed:** Evaded automated gateway rejection via internal trusted network parameters (`permit_mynetworks`). |
| **Defense Evasion** | Masquerading: Match Legitimate Name | **T1036.005** | Spoofing the email envelope and display fields to mimic a high-privilege corporate account (`exec-ceo@corporate-firm.local`). | **Logged:** The Postfix processing engine recorded the explicit identity mismatch string natively on disk inside `/var/log/mail.log`. |
| **Credential Access** | Input Capture: Keylogging | **T1056.001** | Running a cloned form interface armed with data intercept variables (`Capture Submitted Data` + `Capture Passwords`). | **Exploited:** Intercepted keyboard text stream buffers natively over the wire, yielding plaintext user secrets inside the Gophish DB. |

---

## 🛡️ Blue Team Detection & Mitigation Playbook

An emulation campaign is only successful if it improves our defensive posture. Based on the data streams captured during this simulation, our Security Operations Center (SOC) logs the following detection and mitigation rules:

### 1. Technical Detection Rules (SIEM Monitoring)
*   **Postfix Log Parsing:** Our **Wazuh SIEM Manager** is now actively decoding `/var/log/mail.log` text lines. Future alerts will flag sudden bursts of inbound mail originating from unrecognized internal endpoints mapping to external host names.
*   **Credential Harvesting Indicators:** Monitor network proxy traffic for unexpected outbound sessions navigating directly to raw, unencrypted local network IP pathways (`http://42.xxx`) mimicking corporate domains.

### 2. Preventive Controls (Hardening the Shield)
*   **Network Segmentation:** Restrict mail routing rules (`permit_mynetworks`) so that sandbox attacker environments (like Kali Linux) are forced to pass the strict internal **SPF** and **DKIM** validation gates before a message is allowed into a user's inbox folder directory.
*   **Endpoint Hardening:** Enforce strict application parameters inside **Thunderbird** across all client machines to ensure the `Block Remote Content` policy remains permanently active, keeping hidden tracking pixel scripts asleep by default.
