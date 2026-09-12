# 🎣 Phase 3: Setting Up Gophish & Tracking Attacks via Wazuh

## 📋 The Goal
We are moving from defense to attack. We successfully initialized the **Gophish open-source phishing framework** on our **Kali Linux VM** to act as our external threat actor command center. We will use this platform to simulate credential harvesting campaigns against our corporate users.

---

## 🛠️ Step-by-Step Implementation Guide

### 📍 Step 1: Troubleshooting Repository Package Mismatches
When trying to initialize the platform archive extraction tools (`unzip`), the Kali package manager threw a network 404 path block. We resolved this by refreshing the system's global network repository index maps:
```bash
sudo apt update
```

### 📍 Step 2: Activating the Built-in Corporate Hacking Engine
Because Kali Linux utilizes a pre-configured package boundary ecosystem, running custom folder downloads conflicted with local security policies. We bypassed the connection deadlocks by triggering the official, built-in operating system service script command:
```bash
sudo gophish-start
```

**System Activation Parameters Verification:**
*   **Web Console UI Portal:** `https://127.0.0.1:3333`
*   **Default Master Username:** `admin`
*   **Default Master Password Account Hook:** `kali-gophish`

---
![dashboard](gophish_dashboard.png)
## 🎯 Current Lab Status
The Gophish administration portal loaded with 100% success inside Firefox. Port 3333 is actively listening on our localhost loopback, confirming Phase 3 Milestone 1 is completely operational!

---

### 🧠 Core Architectural Discovery: Manager-Side Native Telemetry
During Phase 3 implementation, we discovered a key structural design rule of the Wazuh SIEM ecosystem:

1.  **Remote Monitoring (Lubuntu User VM):** Requires a standalone `wazuh-agent` client package to push data across network paths.
2.  **Local Monitoring (Ubuntu Mail Server):** Because the machine already houses the master `wazuh-manager` engine, installing a standalone agent causes a software conflict. The Manager can parse its own host operating system logs natively.

By embedding the `<localfile>` log path pointing to `/var/log/mail.log` directly inside the Manager's central `/var/ossec/etc/ossec.conf` file, we successfully established a direct telemetry link. Searching `postfix` on our SIEM dashboard automatically populated live mail transaction events, proving our blue-team visibility loop is fully operational.

