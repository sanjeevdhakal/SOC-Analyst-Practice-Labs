# 🛡️ Phase 2: Protecting Our Email Domain (Anti-Spoofing)

## 📋 The Goal
Right now, anyone on our network can fake an email address and pretend to be our CEO. In this phase, we will set up three global security rules (**SPF**, **DKIM**, and **DMARC**) on our Ubuntu Server. These rules act like identity checks to stop hackers from faking our `@corporate-firm.local` domain name.

---

## 🧱 The Three Defensive Walls Explained

We are setting up three security features that work together like a corporate security team:

### 1. SPF (The Approved Delivery List)
*   **What it does:** It is a public text list that names the exact server IP addresses allowed to send emails for our company.
*   **How it protects us:** If a hacker tries to send an email using our domain name from an unapproved machine (like a Kali Linux VM), the receiving system checks this list, notices the mismatch, and flags it as fake.

### 2. DKIM (The Digital Wax Seal)
*   **What it does:** It adds a hidden, unique digital signature code into the header of every email our server sends out.
*   **How it protects us:** The receiving email client uses a public key to verify this signature. If a hacker intercepts the email and alters the text, the digital seal breaks instantly, showing the email was tampered with.

### 3. DMARC (The Security Guard)
*   **What it does:** This is the master rule book. It tells the network exactly what to do if an incoming email fails the SPF or DKIM checks.
*   **How it protects us:** We can configure this guard to take one of three actions when a fake email shows up:
    *   **None:** Just watch and log the traffic.
    *   **Quarantine:** Automatically shove the fake email straight into the user's Spam folder.
    *   **Reject:** Block the email completely at the front door so the employee never even sees it.

---

## 🛠️ Step-by-Step Implementation Tracker

To build this defense, we will execute these three major laboratory milestones inside our environment:

- [ ] **Step 1:** Install and configure **OpenDKIM** on our Ubuntu Server to generate our cryptographic digital keys.
- [ ] **Step 2:** Create our **SPF** and **DMARC** rule files to establish our domain protection policy.
- [ ] **Step 3:** Launch a simulated domain spoofing attack from **Kali Linux** to verify our new walls successfully block the threat.

---


---

### 🔑 Milestone 1 Logs: DKIM Key Generation
Successfully initialized the OpenDKIM engine package repositories and generated the core 2048-bit asymmetric cryptographic key pairs inside the server database:
*   **Storage Path:** `/etc/opendkim/keys/corporate-firm.local/`
*   **Selector Label:** `default`

### Steps 

Let's configure OpenDKIM to handle our domain in three easy steps.

- [ ] **Step 1:**  **Create the Secret Key Vault**

First, we need a secure folder directory on the server to hold our cryptographic files. Run this command block to create the paths and move directly into it:

sudo mkdir -p /etc/opendkim/keys/corporate-firm.local && cd /etc/opendkim/keys/corporate-firm.local

 - [ ] **Step 2:** **Generate the Cryptographic Keys**

Now, we will run the key generation tool to build our asymmetric token pairs [🔎]. Type this command exactly and hit Enter:

sudo opendkim-genkey -s default -d corporate-firm.local

What the flags mean:

-s default: This sets the Selector name to default. It acts as a tag label so mail clients know which key to look up.
-d corporate-firm.local: Links the signature directly to your custom domain identity.


- [ ] **Step 3:** **Verify the Files on Your Disk**

Let's make sure the engine created your files correctly. Run a simple list directory command:

ls -l

We will see exactly two fresh files built side-by-side inside your terminal view [🔎]:
default.private ──► The secret key. This is the confidential stamp Postfix will use to sign our emails.
default.txt ──► The public key file. This contains the exact text configuration we need to publish so clients can verify our identity.

