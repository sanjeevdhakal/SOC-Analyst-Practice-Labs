
```text
              [ PUBLIC INTERNET SIMULATION ]
             ┌──────────────────┴──────────────────┐
             ▼                                     ▼
   ┌───────────────────┐                 ┌───────────────────┐
   │    KALI LINUX     │                 │   UBUNTU SERVER   │
   │ (Malicious Actor) │                 │  (Open-Source MX) │
   │ - Spoofed Domains │                 │ - Postfix / Dovecot│
   │ - Gophish Engine  │                 │ - ClamAV Scanner  │
   └─────────┬─────────┘                 └─────────┬─────────┘
             │                                     │
             │ (Phishing Transmission)             │ (Internal Delivery)
             ▼                                     ▼
   ┌─────────────────────────────────────────────────────────┐
   │                  LUBUNTU VICTIM CLIENT                  │
   │ - Thunderbird Mail / MailHog Sandbox Emulator            │
   │ - Wazuh Endpoint Security Monitoring Agent Enabled      │
   └─────────────────────────────────────────────────────────┘
```



Advanced Email Security & Phishing Simulation Lab

## 📋 Project Overview
Email is the #1 vector used by cybercriminals to breach corporate networks. To truly understand how phishing attacks work and learn how to defend against them, I am expanding my home lab into a complete, private enterprise email ecosystem. 

Instead of just reading textbook theories, I am building the mail servers from scratch, launching simulated attacks using Kali Linux, monitoring the traffic with my Wazuh SIEM, and executing full Security Operations Center (SOC) investigation playbooks.

---

## 🏗️ How the Lab is Set Up (The Simple Map)

I am using three virtual machines connected to an isolated private network to simulate the entire lifecycle of an email attack:

1. **The Attacker (Kali Linux):** 
   Acts as the external hacker group. It uses automated tools to send fake emails, craft tracking links, fake domain names, and build malicious file attachments.
2. **The Mail Post Office (Ubuntu Server):** 
   Acts as our corporate Mail Exchange server using open-source tools called **Postfix** and **Dovecot**. It handles the delivery of messages and enforces company security rules.
3. **The Target Employee (Lubuntu Client):** 
   Acts as a regular office worker checking their inbox via a mail app (Thunderbird). This machine is monitored 24/7 by our **Wazuh Security Agent** to track if the user clicks a bad link or runs a dangerous file.

---

## 🛣️ My Learning Roadmap (Step-by-Step)

I am breaking this advanced project down into small, practical steps to ensure complete hands-on mastery:

*   **Step 1: Mail Infrastructure:** Learning how email travels across networks by setting up SMTP (delivery), IMAP (syncing), and POP3 protocols from scratch.
*   **Step 2: Anti-Spoofing Defense:** Implementing corporate email authentication rules (**SPF, DKIM, and DMARC**) to physically block hackers from faking our company domain name.
*   **Step 3: Hacker Reconnaissance:** Simulating how attackers sneak hidden "tracking pixels" into emails to secretly find out when a victim opens a message.
*   **Step 4: Credential Harvesters:** Building copycat login pages to simulate how hackers trick employees into typing their corporate passwords.
*   **Step 5: Malicious Attachments:** Packaging safe test payloads inside everyday office files (`.zip`, `.docx`, `.pdf`, `.xlsm` macros) to see how malware creates a hidden backdoor into a network.
*   **Step 6: Automated Forensic Analysis:** Using real-world incident response tools like **PhishTool**, **Urlscan.io**, and **VirusTotal** to extract and evaluate threat evidence.
*   **Step 7: SOC Incident Reporting:** Writing comprehensive, industry-standard triage summaries tracking affected users, defensive actions taken, and long-term security lessons learned.


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 🎣 Enterprise Security Lab: End-to-End Phishing Simulation & Custom SIEM Detection

## 📋 Project Overview
This portfolio project documents the architecture, implementation, and execution of a full-scale cyber attack and defense simulation. Operating inside an isolated virtual sandbox environment, we built a functional corporate mail server, integrated a centralized **Wazuh SIEM Dashboard** monitoring network, launched advanced adversarial campaigns using **Gophish on Kali Linux**, and engineered custom detection rules to intercept threats.

The goal of this lab is to validate defensive security controls against real-world threat vectors tracked by the global **MITRE ATT&CK Framework**.

---

## 🗺️ Master Lab Architecture Blueprint

```text
       [ ATTACK ZONE: KALI LINUX ]
         • Runs: Gophish Attacker Dashboard (Port 3333)
         • Payload Factory: Fake Login Portals & Weaponized Archives
                     │
                     │ (SMTP Network Mail Stream over Port 25)
                     ▼
       [ DEFENSE ZONE: UBUNTU MAIL SERVER ]
  ┌──────────────────────────────────────────────┐
  │  1. NETWORK GATES                            │
  │     • Port 25  ──► Open for SMTP Transit     │
  │     • Port 143 ──► Open for IMAP Reading     │
  │                                              │
  │  2. MAIL PROCESSING DAEMONS                  │
  │     • Postfix ──► The Delivery Mail Truck    │
  │     • Dovecot ──► The Mailbox Storage Vault  │
  │                                              │
  │  3. ANTI-SPOOFING & SIEM WATCHTOWER          │
  │     • SPF, DKIM, DMARC ──► Identity Checks   │
  │     • Wazuh Manager ──► Monitoring mail.log  │
  │     • Custom Rule 100003 ──► Forces live     │
  │       Postfix logs onto visual SIEM screen   │
  └──────────────────┬───────────────────────────┘
                     │
                     │ (IMAP Folder Sync over Port 143)
                     ▼
       [ USER ZONE: LUBUNTU GRAPHICAL VM ]
  ┌──────────────────────────────────────────────┐
  │  1. ENDPOINT APPLICATION UI                  │
  │     • Account: hr-manager                    │
  │     • Thunderbird Client ──► Renders Inbox    │
  │                                              │
  │  2. THE MALICIOUS COMPROMISE POINT           │
  │     • corporate_trap.sh (Infinite Firefox    │
  │       Browser loop that freezes desktop)     │
  │                                              │
  │  3. LOCAL DEVICE SURVEILLANCE                │
  │     • Auditd ──► Captures hidden process     │
  │       executions & alerts Wazuh Agent        │
  │                                              │
  │  4. PERMANENT REMEDIATION SHIELD             │
  │     • /etc/fstab 'noexec' flag ──► Kernel    │
  │       automatically blocks script execution  │
  └──────────────────────────────────────────────┘
```

---

## 🛠️ Step-by-Step Implementation Guide

### 📂 Phase 1: Building the Corporate Mail Infrastructure
Our first milestone was building a legitimate, working enterprise post office core from scratch on an **Ubuntu Server VM** to handle our target domain `corporate-firm.local`.

1. **Installing the Mail Server Engine:** We updated our system packages and installed **Postfix** to act as our outbound mail delivery truck operating over network **Port 25 (SMTP)**.
2. **Installing the Storage Vault:** We installed **Dovecot** to act as our local mailbox storage vault, opening **Port 143 (IMAP)** so users could securely dial in and view their letters.
3. **Creating User Directories:** We configured the system to use the clean Linux `Maildir` structure. This ensures that every email processed by the server is cleanly sorted and saved as an individual, readable text file straight onto the hard drive partition paths:
   * `~/Maildir/new/` ──► Houses unread incoming messages.
   * `~/Maildir/cur/` ──► Houses opened messages.
4. **Provisioning Employee Accounts:** We built dedicated system accounts for our lab roles: `hr-manager` (the victim) and `exec-ceo` (the identity we spoofed).
5. **Setting Up the Endpoint Workspace:** Moving over to our **Lubuntu VM**, we launched the graphical **Thunderbird Mail Client** and linked it to the server over Port 143, establishing a visual user inbox interface.

---

### 🛡️ Phase 2: Deploying the Defensive Shields (Anti-Spoofing & SIEM Integration)
With the mail pipeline active, we turned on advanced corporate authentication controls to protect against spoofing, and interconnected our centralized logging watches.

1. **Configuring the Checklist (SPF):** We generated a **Sender Policy Framework** text record stating that *only* our official Ubuntu Server IP address is authorized to send mail for our domain.
2. **Applying the Identity Seal (DKIM):** We installed **OpenDKIM** to act as a digital cryptographic wax seal. It automatically signs outgoing headers with a secret private key token so receiving clients can confirm a message wasn't manipulated mid-transit.
3. **Enforcing Policy (DMARC):** We created a **DMARC** rule instructing mail engines exactly how to handle unauthenticated fakes that fail SPF/DKIM verification.
4. **Hooking the SIEM Surveillance System:** We opened the master control brain file of our central **Wazuh Manager** (`sudo nano /var/ossec/etc/ossec.conf`) and dropped a permanent monitoring directive telling it to scan our mail text files natively on disk:
   ```xml
   <localfile>
     <log_format>syslog</log_format>
     <location>/var/log/mail.log</location>
   </localfile>
   ```
5. **Resolving Account Permissions:** To ensure the security manager process had the physical rights to open system log paths, we added the `wazuh` user account straight into the Linux logging privilege group:
   ```bash
   sudo usermod -aG adm wazuh
   sudo systemctl restart wazuh-manager
   ```

---

### 🎣 Phase 3: Activating the Attacker Command Center
Next, we flipped our focus to the threat actor zone to initialize our phishing delivery platform on **Kali Linux**.

1. **Waking Up the Platform Engine:** We launched Kali Linux's pre-installed phishing infrastructure daemon command via the terminal prompt:
   ```bash
   sudo gophish-start
   ```
2. **Accessing the Console:** We opened Firefox inside Kali, bypassed the local testing certificate block warning, navigated to `https://127.0.0.1:3333`, and logged into the gray command console management portal.
3. **Building the Outbound Routing Profile:** Inside Gophish, we created a new **Sending Profile** named `Corporate SMTP Relay`. We pointed its interface settings directly to our Ubuntu Server's network address over open Port 25 (`192.168.42.130:25`) and set the display sender field to masquerade as `Executive CEO <exec-ceo@corporate-firm.local>`.
4. **Validating the Connection:** We clicked `Send Test Email` inside Gophish. The transaction cleared the network switches with 100% success, delivering a baseline test verification email straight into the user's Thunderbird client inbox.

---

### ⚔️ Phase 4: Executing Active Attack & Detection Engineering
With all systems fully interconnected, we simulated three advanced adversarial techniques to evaluate our security monitoring console's responsiveness.

#### 🕵️‍♂️ Technique 1: Hacker Reconnaissance (Hidden Tracking Pixels)
* *The Attack:* We embedded Gophish's native tracking tag macro (`{{.Tracker}}`) at the absolute bottom of an urgent IT security email template. This automatically hides an invisible, `1x1` transparent pixel token inside the email formatting code.
* *The Result:* The exact millisecond the victim opened the mail inside Thunderbird on Lubuntu and clicked **Allow Remote Content**, the client app quietly downloaded the hidden image from Kali. The Gophish dashboard immediately spiked from **0 to 1 under `Email Opened`**, logging the timeline exposure data without user awareness.

#### 🔑 Technique 2: Credential Harvesting (Copycat Login Portals)
* *The Attack:* Inside Gophish's `Landing Pages` tab, we hit raw code entry view and manually compiled a custom HTML verification portal mockup complete with input forms, an encrypted lock logo icon, and a blue submit button. We checked both `Capture Submitted Data` and `Capture Passwords` options and configured a stealth redirect link leading to `google.com`.
* *The Result:* We linked this fake page directly to the bold blue text inside our email template. The user clicked the link, filled out their credentials (`username: sanjeev@gmail.com` / `password: Hello123!`), and hit submit. The browser seamlessly tossed them to the real Google website to avoid suspicion, while **Gophish intercepted their keyboard text buffers over the wire, capturing the plaintext secrets live inside our attacker database database table fields!**

#### 📦 Technique 3: Malicious Attachments (Simulating Backdoor Deliveries)
* *The Attack:* On Kali, we manufactured a mock attack script (`corporate_trap.sh`) designed to simulate a runaway adware loop by forcing Lubuntu to infinitely open browser tabs until system memory froze. We zipped this script into an archive container named `payroll_updates.zip`, attached it to a financial bonus email template in Gophish, and launched the campaign.
* *The Forensics:* The file bypassed simple filtering layers because it originated from our trusted local sandbox network loop range (`permit_mynetworks`). By dropping into the server terminal and filtering by timestamp (`sudo grep "15:44" /var/log/mail.log`), we extracted the raw metadata transaction logs proving the file size metrics were written right down to the disk sector tracks.

---

### 🧠 Phase 5: Custom SIEM Detection Optimization





