# 🛡️ Phase 2: Protecting Our Email Domain (Anti-Spoofing)

## 📋 The Goal
Right now, anyone on our network can fake an email address and pretend to be our CEO. In this phase, we set up three global security rules (**SPF**, **DKIM**, and **DMARC**) on our Ubuntu Server. These rules act like identity checks to stop hackers from faking our `@corporate-firm.local` domain name.

---

## 🧱 The Three Defensive Walls Explained

We are setting up three security features that work together like a corporate security team:

### 1. SPF (The Approved Delivery List)
*   **What it does:** A public text list naming the exact server IP addresses allowed to send emails for our company.
*   **Real-World Example:** Like a bank publishing a list of their official courier trucks. If a random sedan shows up claiming to deliver bank funds, it gets turned away.

### 2. DKIM (The Digital Wax Seal)
*   **What it does:** Adds a hidden, unique digital signature code into the header of every email our server sends out.
*   **Real-World Example:** Like a king stamping a wax seal onto a letter. If a spy intercepts the letter and changes even one word, the seal shatters, showing the message was tampered with.

### 3. DMARC (The Security Guard)
*   **What it does:** The master rule book. It tells the network exactly what to do if an incoming email fails the SPF or DKIM checks.
*   **Real-World Example:** A security guard at the door who is told: *"If an unrecognized courier arrives, shove their letters in the trash folder (Quarantine) or reject them at the door entirely (Reject)."*

---

## 🛠️ Step-by-Step Implementation Guide

### 📍 Step 1: Installing the OpenDKIM Signature Engine
To generate our digital wax seal keys, we installed the OpenDKIM open-source security tool suite on our Ubuntu Server:

```bash
sudo apt update && sudo apt install -y opendkim opendkim-tools
```

### 📍 Step 2: Creating the Secure Key Vault
We created a dedicated, restricted directory on our server's file system to store our cryptographic identity files:

```bash
sudo mkdir -p /etc/opendkim/keys/corporate-firm.local
cd /etc/opendkim/keys/corporate-firm.local
```

### 📍 Step 3: Generating the Asymmetric Cryptographic Keys
We ran the key generation tool to build our public and private key tokens using the standard `default` selector tag:

```bash
sudo opendkim-genkey -s default -d corporate-firm.local
```

**Verification Check:** Running `ls -l` confirms two files were generated:
1.  `default.private` (The secret stamp used by the server to sign outbound mail).
2.  `default.txt` (The public verification file containing our public cryptographic key key).

---

### 📍 Step 4: Configuring the OpenDKIM Database Mapping Tables
We injected precise routing instructions to tell OpenDKIM which domain names match our private keys and trusted hosts:

```bash
# Link our corporate domain suffix to our selector tag
echo "*@corporate-firm.local default._domainkey.corporate-firm.local" | sudo tee -a /etc/opendkim/signing.table

# Link the selector tag straight to the physical private key on the drive
echo "default._domainkey.corporate-firm.local corporate-firm.local:default:/etc/opendkim/keys/corporate-firm.local/default.private" | sudo tee -a /etc/opendkim/key.table

# Declare our local server IP blocks as trusted entities
echo -e "127.0.0.1\nlocalhost\n192.168.42.130\ncorporate-firm.local" | sudo tee -a /etc/opendkim/trusted.hosts
```

---

### 📍 Step 5: Updating the Master OpenDKIM Control Blueprint
We opened the main configuration file at `/etc/opendkim.conf` and appended these master integration parameters to force the engine to read our mapping tables over internal network port `8891`:

```text
# Master Lab Configuration Overrides
Mode                    sv
SubDomains              no

KeyTable                /etc/opendkim/key.table
SigningTable            /etc/opendkim/signing.table
ExternalIgnoreList      /etc/opendkim/trusted.hosts
InternalHosts           /etc/opendkim/trusted.hosts

# Open communication portal for local applications
Socket                  inet:8891@localhost
```

---

### 📍 Step 6: Connecting the Mail Server Conveyor Belt to OpenDKIM
Finally, we opened the primary Postfix mail configuration file at `/etc/postfix/main.cf` and appended our signature integration tracking rules. This forces Postfix to route every single email through OpenDKIM before it leaves the server:

```text
# OpenDKIM Integration Rules
milter_protocol = 6
milter_default_action = accept
smtpd_milters = inet:localhost:8891
non_smtpd_milters = inet:localhost:8891
```

---

## 🚦 System Operational Status
We ran a full system service refresh to wake up our new defenses cleanly:

```bash
sudo systemctl restart opendkim postfix
```

**Result:** Both services loaded with **zero errors**, confirming our encrypted mail validation pipeline is fully stable, interconnected, and ready for deployment!
