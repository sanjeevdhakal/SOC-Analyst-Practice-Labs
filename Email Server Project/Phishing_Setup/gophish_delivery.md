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

## 🎯 Current Lab Status
The Gophish administration portal loaded with 100% success inside Firefox. Port 3333 is actively listening on our localhost loopback, confirming Phase 3 Milestone 1 is completely operational!
