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

## 🧠 MITRE ATT&CK Implementation & Core Architectural Discovery: The Wazuh Manager Secret

During this phase, we ran into a massive, fascinating technical lesson about how enterprise security software actually talks to a network.

### ❌ The Conflict: Why We Couldn't Install the Standalone Agent
When we tried to install a standalone tracking guard (`wazuh-agent`) onto our Ubuntu Server, the installation crashed with a strict system conflict error. 

**Why it happened:** Our Ubuntu Server already houses the master brain of our security system—the **Wazuh Manager**. The manager application and the agent application use the exact same directory house folders on the hard drive (`/var/ossec/`). Trying to install both standalone programs forces them to fight over the same files, so the system blocked the installation to prevent database corruption.

### 💡 The Solution: Native Self-Monitoring
We discovered a "hidden gem" of security design: **The Wazuh Manager can monitor its own host computer natively!** It does not need a separate agent client service turned on because it already has a built-in log-reading engine sitting right inside its core processes. 

To turn on the security cameras and watch our mail activity, we just had to point the Manager to the right text files using a single configuration file layout.

---

## 🛠️ Step-by-Step Implementation Guide

### 📍 Step 1: Opening the Master Wazuh Configuration Brain
We logged into our **Ubuntu Server terminal** and opened the central configuration blueprint file for the entire Wazuh server using the `nano` text editor:
```bash
sudo nano /var/ossec/etc/ossec.conf
```

### 📍 Step 2: Injecting the Mail Tracking Instructions
Wazuh is incredibly strict about its layout rules. Everything must be tucked inside its proper "department." We scrolled to the top half of the document, located the native log collection section, and dropped a permanent command telling the manager to actively watch our mail and authentication text files:

```xml
<!-- Instructing Wazuh to watch our Postfix Mail Server transactions -->
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/mail.log</location>
</localfile>

<!-- Instructing Wazuh to watch system login and security password attempts -->
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/auth.log</location>
</localfile>
```
*Plain-English Logic:* This simple rule tells the server: *"The exact millisecond a new line of text appears inside `mail.log` (like an email arriving or an authentication check triggering), copy that text line and pull it straight onto our monitoring console dashboard!"*

![rules](rules.png)

### 📍 Step 3: Refreshing the SIEM Master Engine
To force the master manager to read our new instructions and activate the log channels, we ran a full system service restart:
```bash
sudo systemctl restart wazuh-manager
```

---

## 🚦 Final Verification (The Proof in the Dashboard)
We logged into our web browser and opened the visual **Wazuh Dashboard**. When we typed **`postfix`** into the search bar, the console immediately populated live security tracking records matching our mail server! 

This proves our entire infrastructure telemetry loop is 100% stable, fully interconnected, and actively waiting to map our upcoming Gophish attacks straight onto our **MITRE ATT&CK Matrix console**!
![testlog](test_log.png)
