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
