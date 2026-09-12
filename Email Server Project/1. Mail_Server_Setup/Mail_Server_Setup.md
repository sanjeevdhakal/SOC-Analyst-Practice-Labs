# Technical Walkthrough: Building the Corporate Mail Server

This document explains the step-by-step process of installing and configuring our private mail infrastructure on the Ubuntu Server. 

---

## 🛠️ Step 1: Updating the Server and Installing Postfix

Before installing new software, we must tell the Ubuntu operating system to refresh its list of available packages. Then, we download the core mail delivery program (**Postfix**) and a tool helper package (**mailutils**).

command in  Ubuntu Server terminal:
```bash
sudo apt update && sudo apt install -y postfix mailutils
```

---

## ⚙️ Step 2: Navigating the Configuration Screen

During the installation, a blue-and-pink interactive setup wizard will appear inside your terminal screen. 

1. **First Screen (General Type):** Use your arrow keys to highlight **`Internet Site`** and press **Enter**. This tells the server to use standard web communication rules to route emails.
2. **Second Screen (System Mail Name):** Type our custom corporate domain name exactly: **`corporate-firm.local`** and press **Enter**. This ensures any internal users we create later will have email addresses ending in `@corporate-firm.local`.

---

## 📂 Step 3: Configuring Mail Storage Rules

By default, Linux saves emails in an old, cluttered format. We need to edit the Postfix configuration file to force the server to save every email as a neat, separate file inside a folder called `Maildir`. This makes it much easier for our Wazuh security agent to read and monitor the logs later.

1. Open the main configuration file using the Nano text editor:
   ```bash
   sudo nano /etc/postfix/main.cf
   ```
2. Use  arrow keys to scroll all the way down to the very bottom of the file.
3. Type out these three lines exactly as shown (use the spacebar for spacing, do not press Tab):
   ```text
   home_mailbox = Maildir/
   mailbox_command = 
   inet_interfaces = all
   ```
   * *`inet_interfaces = all`* opens the server doors so it can receive emails sent from our Kali Linux attacker machine or Lubuntu client.
4. Press **`Ctrl + O`** and then hit **`Enter`** to save  file modifications.
5. Press **`Ctrl + X`** to exit the text editor and return to the main command prompt.

---

## 🚀 Step 4: Starting and Testing the Mail Service

Now that our configuration files are updated, we need to restart the background mail engine so it can read our changes and turn itself on automatically every time the server boots up.

Run this command:
```bash
sudo systemctl restart postfix && sudo systemctl enable postfix
```

Finally, we run a network check command to verify that our mail server is actively awake and listening for traffic on standard SMTP **Port 25**:
```bash
ss -tuln | grep :25
```


  ![SMTP](SMTP.png)

### Expected Output:
If the setup is working correctly, we will see a text line on your screen containing the word **`LISTEN`** right next to 0.0.0.0:25 and [::]:25. This means our private network post office is officially live and waiting to process messages!


---

## 📬 Step 5: Installing and Configuring Dovecot (IMAP Storage)

While Postfix handles delivering mail (SMTP), we need a separate engine to handle storing and syncing mail folders so users can read them. We use an open-source system called **Dovecot** to provide IMAP capabilities.

Run this installation command in your Ubuntu Server terminal:
```bash
sudo apt install -y dovecot-imapd dovecot-pop3d
```

### 1. Activating IMAP Protocols
We need to tell Dovecot to explicitly listen for email reading requests. Open the main configuration file:
```bash
sudo nano /etc/dovecot/dovecot.conf
```
Find the `protocols` line and adjust it to look exactly like this:
```text
protocols = imap pop3
```
Save and close the file (`Ctrl + O` -> `Enter` -> `Ctrl + X`).

### 2. Mapping the Storage Directory Location
Dovecot needs to look inside the exact same folder structure where Postfix drops the incoming mail. Open the directory rules file:
```bash
sudo nano /etc/dovecot/local.conf
```
Locate the `mail_path` tracking variable line and update it exactly to:
```text
# 1. Enforce the exact folder paths on the disk layout
mail_driver = maildir
mail_path = ~/Maildir
mail_inbox_path = ~/Maildir
maildir_stat_dirs = yes

# 2. Allow plaintext password exchange over our private local network paths
auth_allow_cleartext = yes
auth_mechanisms = plain login

# 3. Grant Dovecot permissions to look inside the Linux password database
service auth {
  unix_listener auth-userdb {
    mode = 0660
    group = shadow
  }
}

```
Save and close the file (`Ctrl + O` -> `Enter` -> `Ctrl + X`).

### 3. Activating and Verifying the Sync Engine
Restart the Dovecot tracking engine to apply all changes and set it to load automatically on server startup:
```bash
sudo systemctl restart dovecot && sudo systemctl enable dovecot
```

Finally, check that both SMTP (Port 25) and IMAP (Port 143) are officially running on your network infrastructure loop:
```bash
ss -tuln | grep -E ":25|:143"
```

![IMAP](IMAP.png)

## Step 6: Creating Local Corporate User Profiles

To test enterprise authentication and message routing flows, two dedicated non-privileged user accounts were created directly on the core server infrastructure:
1.  **HR Manager Endpoint:** `hr-manager@corporate-firm.local`  sudo adduser hr-manager
2.  **Executive CEO Endpoint:** `exec-ceo@corporate-firm.local` sudo adduser exec-ceo


These accounts automatically generated secure personal `Maildir/` folders inside their respective Linux user profiles to store telemetry tracking data.

---

## 🧪 Step 7: Native Delivery Pipeline Validation

To confirm absolute system integration between the core Linux account management framework and the Postfix mail transfer daemon, an internal diagnostic email string was initiated natively via the command line interface:

```bash
echo "this is a local security infrastructure connection test." | mail -s "Lab Verification" hr-manager@corporate-firm.local
```

### Forensic Storage Verification:
Checked the underlying user storage partition path using directory verification tools:
```bash
sudo ls -l /home/hr-manager/Maildir/new/
```

### Result Findings:
The server successfully intercepted the network routing string and dynamically built a unique cryptographic text block artifact file within the endpoint target's inbox directory layout. This completes the core server infrastructure verification check phase.

![test_email](test_email.png)

### Raw Header Metadata Verification
Switched to root privileges (`sudo -i`) and navigated directly into the user endpoint's secure mailbox partition directory to audit the raw message headers:

```text
Return-Path: <dhakalsanjeev@azuh-server>
X-Original-To: hr-manager@corporate-firm.local
Delivered-To: hr-manager@corporate-firm.local
Received: by azuh-server (Postfix, from userid 1000)
    id 67C23200602; Wed, 02 Sep 2026 10:37:53 +0000 (UTC)
Subject: Lab Verification
To: <hr-manager@corporate-firm.local>
User-Agent: mail (GNU Mailutils 3.20)
Date: Wed, 2 Sep 2026 10:37:53 +0000
Message-Id: <20260902103753.67C23200602@azuh-server>
From: Sanjeev <dhakalsanjeev@azuh-server>

this is a local security infrastructure connection test.
```

![email_test_header](email_test_header.png)
### Analysis Finding:
The raw metadata structure confirms that internal routing functionality over SMTP (Port 25) is fully operating. The local mail delivery system perfectly mapped the data payload straight to the target home profile partition directory.

---
##  Step 8: Graphical Client Integration via Thunderbird

To complete the full enterprise mail network environment layout, the endpoint client machine (`Lubuntu-Victim`) was configured to handle visual mail streams.

1.  **Local Address Mapping:** Updated `/etc/hosts` to translate network IP address routing blocks (`192.168.42.130`) straight to the virtual domain identity path (`corporate-firm.local`).

Because we are inside a private lab network, our Lubuntu machine doesn't automatically know that corporate-firm.local is the name of  Ubuntu Server. We need to map it in the local address book file.

Lubuntu Victim VM
sudo nano /etc/hosts

Scroll all the way to the very bottom of the file using  arrow keys and add a clean new line that links Ubuntu Server's static IP address to our corporate domain name:
192.168.42.130 corporate-firm.local

2.  **App Configuration:** Initialized Thunderbird and mapped incoming sync actions explicitly over IMAP (Port 143) and outgoing transmission rules over SMTP (Port 25).

![incoming_server](incoming_server.png)

![outgoing_server](outgoing_server.png)
   

   
3. **Validation Success:** Confirmed the mail interface successfully synchronized with the backend storage partition, downloading the original `System Integration Success` sample email cleanly into the user interface.

![email_test](email_test.png)

## SUMMARY: 


The Corporate Email Architecture Blueprint

To simulate email security and phishing in a real-world company, we had to build an enterprise email ecosystem from scratch inside our private lab. Every real-world email network requires four main ingredients:

The Post Office (The Mail Server): A machine to route, deliver, and store messages.

The Delivery Languages (The Network Protocols): The specific rules used to move text across the network.

The People (The System Users): The employee profiles who own the inboxes.

The Mailbox App (The Graphical Email Client): The visual interface the employee uses to read messages.Here is exactly how these pieces look, how they connect, and what we typed to build them.


###############################################################################################################

## Step 1: Building the Post Office (The Mail Server)

The Tool We Used: Postfix and Dovecot running on an Ubuntu Server VM.

In the real world, companies like Google (Gmail) or Microsoft (Outlook) run massive mail servers. In our private lab, we turned our single Ubuntu Server into our own private corporate post office named corporate-firm.local.

Inside this server, we split the post office into two separate departments:

The Delivery Department (Postfix): Handles outgoing and incoming traffic moving between servers.

The Storage Vault (Dovecot): Handles sorting mail and holding it securely inside the users' personal folders.

###############################################################################################################

## Step 2: The Three Core Email Languages (The Protocols)

To move letters from an attacker to a server, or from a server to an employee, the computers must speak specific languages over designated network doors (Ports):

1. SMTP (Simple Mail Transfer Protocol) ──► Port 25
> What it does: This is the language used to Send and Deliver mail.
> How it works: When an email is transmitted across the network, it knocks on Port 25 and drops the text payload into the server's processing queue.
> The Tool in our Lab: Postfix listens on Port 25.

2. IMAP (Internet Message Access Protocol) ──► Port 143
> What it does: This is the language used to Read and Sync mail folders.
> How it works: When you look at an email on your phone or computer, IMAP leaves the message safely on the server and just lets you view a synchronized live copy of it. This is the global standard for modern businesses.
> The Tool in our Lab: Dovecot listens on Port 143.

3. POP3 (Post Office Protocol v3) ──► Port 110
> What it does: An older, legacy language used to download mail.
> How it works: It physically downloads the email to your computer and completely deletes it off the mail server. It is rarely used in modern corporate environments because you cannot sync multiple devices.

###############################################################################################################

## Step 3: Creating the People & Allocating Email Addresses

Where are users created? 
>Directly on the Ubuntu Server operating system database.In Linux architecture, a user account and an email address are the exact same thing. Linux treats email mailboxes like physical mail slots on an employee's office door.

when we ran these commands on our server: 

sudo adduser hr-manager
sudo adduser exec-ceo

Linux instantly performed three backend actions:
> It created a system user account profile with a password.
> It built a physical home folder room on the hard drive located at /home/hr-manager/
> Because Postfix is configured to own the domain name corporate-firm.local, the server automatically allocated the official email address hr-manager@corporate-firm.local.

###############################################################################################################

## Step 4: Where are Emails Actually Saved? (The Maildir Format)

The Directory Path: /home/username/Maildir/new/
Emails are not magic database entries; they are simply raw text documents saved directly onto the server's hard drive!

By configuring our server to use the Maildir/ format, we instructed the post office to create three subfolders inside each user's private home room:

/Maildir/new/ ──► Where fresh, unread incoming emails land as separate text files.
/Maildir/cur/ ──► Where opened, read emails are shifted.
/Maildir/tmp/ ──► Where temporary sync index data is held.

Example: 
When we ran our terminal delivery test command:

echo "Test message body" | mail -s "Subject Line" hr-manager@corporate-firm.local

Postfix stripped away the @corporate-firm.local domain suffix, saw the username was hr-manager, opened their secure home path, and wrote a unique text file directly inside /home/hr-manager/Maildir/new/.

###############################################################################################################

## Step 5: Connecting the Mailbox App (The Graphical Email Client)

The Tool We Used: Thunderbird running on a Lubuntu Desktop VM.

Employees don't read raw text documents inside dark server terminals; they use a clean, graphic window application like Outlook or Apple Mail. We used an open-source app called Thunderbird.

To link the visual app on the employee's computer back to our Ubuntu mail server, we had to build a bridge across the network network cards using our manual configurations:

Steps: 
1. The Address Map (/etc/hosts): We edited Lubuntu's local host file to tell the computer: "Whenever you hear the name corporate-firm.local, drive straight down the private network path to the Ubuntu Server's IP address (192.168.42.130)."
2. The Client Rules: We opened Thunderbird, typed in hr-manager@corporate-firm.local, and mapped the protocol doors manually:
> Incoming: Speak IMAP over Port 143 to pull folder indexes down.
> Outgoing: Speak SMTP over Port 25 to push new emails out

