# 📧 Phishing Email Header Analysis Project

This repository demonstrates a complete phishing email investigation using a real-world sample.  
It follows eight systematic steps — from collecting a sample email to identifying spoofing,  
analyzing headers, decoding the body, and summarizing phishing indicators.

 
### Analyze a Phishing Email Sample

---

## 🎯 Objective
Identify and document phishing characteristics in a suspicious email sample.

---

## 🧰 Tools Used
- Email client / saved email file (`.txt`)
- Online header analyzer — [https://mha.azurewebsites.net](https://mha.azurewebsites.net)
- Text editor
- Screenshot tool

---

## 📁 Files in This Repository
- `sample_email.txt` — raw phishing email (headers + base64 body)  
- `Screenshot_2025-10-22_215103.png` — header analysis screenshot  

![Header Analysis Screenshot](Screenshot_2025-10-22_215103.png)

---

## 🧩 Key Concepts
**Phishing**, **Email Spoofing**, **Header Analysis**, **Social Engineering**, **Threat Detection**

---

# 📊 Step-by-Step Phishing Email Analysis

---

## Step 1 — Obtain a Sample Phishing Email

I used a sample phishing email pretending to be from **Banco do Bradesco Livelo**, a Brazilian bank.  
The email claimed the user’s reward points were expiring soon.

**Saved File:** `sample_email.txt`
https://github.com/rf-peixoto/phishing_pot
**Purpose:** To analyze headers, sender info, URLs, and message intent.

---

## Step 2 — Examine Sender’s Email Address for Spoofing

**Header Extract:**
From: BANCO DO BRADESCO LIVELO banco.bradesco@atendimento.com.br

Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06


**Observation:**
- The visible “From” address looks legitimate.
- But the **Return-Path** shows a Linux server (`root@ubuntu...`) — this is a clear **spoofing indicator**.
- The sender domain (`atendimento.com.br`) and server (`ubuntu-s-1vcpu...`) mismatch.

**Interpretation:**  
The sender faked the display name and domain to trick users into thinking it’s from the bank.

---

## Step 3 — Check Email Headers for Discrepancies

**Analyzed using:** [Microsoft Header Analyzer](https://mha.azurewebsites.net)

**Important Lines from Header:**
Authentication-Results: spf=temperror (sender IP is 137.184.34.4) ... dkim=none ... dmarc=temperror ... compauth=fail reason=001
Received-SPF: TempError (protection.outlook.com: error in processing during lookup of ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06: DNS Timeout)
Received: by ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06 (Postfix, from userid 0)
Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06
X-Sender-IP: 137.184.34.4
X-MS-Exchange-Organization-SCL: 5


**Interpretation:**
- **SPF/DKIM/DMARC all failed** — these are the primary authentication checks.
- Sent via **Postfix** on a personal Ubuntu VPS (not a corporate mail server).
- **SCL: 5** — Microsoft Exchange flagged it as *spam or phishing*.
- IP `137.184.34.4` belongs to a VPS hosting service, not a bank.

✅ **Conclusion:** Strong evidence of a forged sender identity and spoofed origin.

---

## Step 4 — Identify Suspicious Links or Attachments

The email body was encoded in Base64.  
Decoded it using the command:

```bash
base64 --decode email_body_base64.txt > email_body.html

Finding:
Links inside the email led to:

http://blog1seguiment.com.br/... 


(not a Bradesco or Livelo domain)

Interpretation:
The attackers used a fake website mimicking Bradesco’s login page to steal credentials.

No attachments were detected, but the external link itself is malicious.

##*Step 5 — Look for Urgent or Threatening Language*

Subject Line:

CLIENTE PRIME - BRADESCO LIVELO: Seu cartão tem 92.990 pontos LIVELO expirando hoje!


Body Content Highlights:

Mentions expiring points and act today.

Uses urgency to make the user click quickly.

Interpretation:
This is social engineering — exploiting the fear of losing benefits to drive user action.

##*Step 6 — Note Mismatched URLs*

Examining the decoded HTML revealed links such as:

<a href="http://blog1seguiment.com.br/...">Acesse sua conta Bradesco</a>


Visible text: “Acesse sua conta Bradesco”
Actual link: blog1seguiment.com.br

✅ Mismatch: The visible text shows a trusted brand, but the link redirects to a fake domain — clear phishing technique.


##*Step 7 — Check for Spelling or Grammar Errors*

After decoding the Base64 HTML and converting it to plain text, several issues were visible:

Awkward Portuguese grammar.

Random capitalizations.

Encoding artifacts (unnecessary symbols, wrong accents).

Example:

“Clique AQUI para garantir seus pontos de fidelidade hoje mesmo!” — poor phrasing typical of automated phishing generators.

Interpretation:
Poor grammar and non-professional formatting indicate that the message was not crafted by the legitimate organization.


