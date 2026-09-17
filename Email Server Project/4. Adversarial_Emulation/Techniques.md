# 🎣 Phase 4: Active Attack Simulations & Adversarial Emulation

## 📋 Operational Objective
This playbook documents the step-by-step execution of advanced offensive simulation techniques launched from our **Kali Linux attack station**. By mimicking real-world hacking behaviors, we successfully tested our mail server security controls and mapped the entire breach lifecycle straight to the global **MITRE ATT&CK Framework**.

---

## 🕵️‍♂️ Technique 1: Hacker Reconnaissance (Hidden Tracking Pixels)
*   **MITRE ATT&CK ID:** Tactic: Reconnaissance | Gather Victim Identity Info (**T1589**)
*   **Status:** ✅ 100% COMPLETE

### 💡 What it is in Plain English
A tracking pixel is an invisible, transparent `1x1` image block hidden deep inside the formatting code of a phishing email. The victim cannot see it. However, the exact millisecond the victim clicks open the email, their email application automatically reaches out to the hacker's computer over the network to download that tiny image. This silent network connection alerts the hacker instantly that the email was opened, tracking the exact date and time of exposure.

### 🛠️ Detailed Step-by-Step Lab Implementation

#### Step 1: Crafting the Urgent Bait Message
We logged into our **Gophish Admin Panel** over secure browser port `https://127.0.0.1:3333`, navigated to `Email Templates`, and clicked `+ New Template`. We designed a deceptive email engineered to force immediate user compliance:
*   **Template Name:** `Urgent Security Policy Update`
*   **Subject Line:** `ACTION REQUIRED: Mandatory Corporate Password Synchronization`

#### Step 2: Hiding the Tracking Code
We clicked directly on Gophish's **`HTML` view tab** to modify the raw underlying script lines of the email body. We added our urgent corporate IT text warning and placed Gophish's native tracking shortcut right at the absolute bottom line of the text pool:
```text
{{.Tracker}}
```
*Plain-English Logic:* When the campaign launches, this simple shorthand code tells Gophish to automatically manufacture an invisible, transparent image tag wrapper and tuck it hidden into the outbound mail package.

#### Step 3: Triggering the Trap on the Target Machine
We logged onto our **Lubuntu Victim VM**, opened the **Thunderbird** mail client, and clicked `Get Messages`. The deceptive email arrived safely in our inbox. 
*   **The Security Behavior:** By default, Thunderbird's privacy engine blocked external remote content, keeping our tracking pixel asleep. 
*   **The Breakthrough:** The exact second we clicked **`Allow Remote Content`** inside Thunderbird's alert bar, the mail client downloaded the invisible pixel, and our **Kali Linux Gophish dashboard immediately spiked from 0 to 1 under `Email Opened`**, confirming total tracking success.

---

## 🔑 Technique 2: Credential Harvesting (Copycat Login Portals)
*   **MITRE ATT&CK ID:** Tactic: Initial Access | Phishing: Spearphishing Link (**T1566.002**)
*   **Status:** ✅ 100% COMPLETE

### 💡 What it is in Plain English
Credential harvesting is the process of building an exact duplicate copycat version of a legitimate company login screen. Hackers embed a link to this fake portal inside a phishing email. When an employee runs through their morning mail and clicks the link, they get hit with a login box that looks normal, but is wired up to an attacker database. The moment they type their keys and hit submit, the hacker instantly steals their plaintext username and password.

### 🛠️ Detailed Step-by-Step Lab Implementation

#### Step 1: Manufacturing the Copycat Website Form
Inside our Gophish browser panel, we clicked on `Landing Pages` and selected `+ New Page`. We named it `Fake Corporate Login`. Because our testing network is isolated in a private sandbox without a public internet link, we couldn't use the automatic cloner. Instead, we clicked the **`Source` button** on the text editor toolbar to drop straight into raw code entry and pasted our own clean, custom HTML corporate verification blueprint block:
```html
<div class="login-box">
    <h2>🔒 Corporate Identity Verification</h2>
    <p>Please re-authenticate your account credentials...</p>
    <form action="" method="POST">
        <input type="text" name="username" placeholder="Corporate Email Address" required />
        <input type="password" name="password" placeholder="Account Password" required />
        <input type="submit" value="Synchronize Account" />
    </form>
</div>
```

#### Step 2: Arming the Keylogger Hooks
To turn our static visual mockup webpage into an active trap capable of stealing data, we scrolled directly beneath the HTML code text window and activated two critical configuration checkboxes:
*   [x] **Capture Submitted Data** ──► *Tells Gophish to record user keystroke events.*
*   [x] **Capture Passwords** ──► *Forces Gophish to extract raw password text strings.*
*   **Redirect To:** We pasted `https://google.com` into the redirect text field. This guarantees that the split second the user types their keys and clicks submit, they are instantly thrown to the real Google website. They assume the page simply glitched or refreshed, completely hiding the fact that they were hacked.

#### Step 3: Launching the Campaign Interconnection Relay
We clicked over to the `Campaigns` menu bar, selected `+ New Campaign`, and stitched all our independent lab pieces together:
*   **Target Group:** Tied to `hr-manager@corporate-firm.local` inside our user registry list.
*   **The Critical URL Binding:** We pointed the campaign web anchor field directly to our Kali Attacker machine's network card identity path block: `http://<YOUR_KALI_IP_HERE>`. Gophish automatically mapped this network IP right onto the bold blue link text inside our email template: **`Click Here to Synchronize Corporate Account Credentials`**.

#### Step 4: Reaping the Stolen Secrets (The Proof)
We returned to our **Lubuntu Victim VM**, opened the fresh email message, and clicked the bold blue link text. The browser instantly snapped open, routed across our virtual network switch, and cleanly rendered our custom lock-box verification page. 

We typed our testing data into the active prompt fields and clicked submit:
*   **Target Username Entered:** `sanjeev@gmail.com`
*   **Target Password Entered:** `Hello123!`

The user browser session immediately redirected to the real Google landing page seamlessly. When we returned to our **Kali Linux Gophish dashboard timeline**, the **`Submitted Data` dial exploded to 1**. We opened the target expansion menu, and our attacker database displayed the exact, unencrypted plaintext username and password strings cleanly harvested off the victim's keyboard buffers!
