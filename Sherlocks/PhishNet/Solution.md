# Solution Report

## 1. What is the originating IP address of the sender?

**Answer:** `45.67.89.10`

**Explanation:**  

The sender’s IP address could be found in the header field X-Sender-IP.

```
❯ cat email.eml | grep X-Sender-IP 
X-Sender-IP: 45.67.89.10
```
---

## 2. Which mail server relayed this email before reaching the victim?

**Answer:** `203.0.113.25`

**Explanation:**  

The “Received” headers reveal the route the email took from sender to recipient.

```
❯ cat email.eml | grep Received 
Received-SPF: Pass (protection.outlook.com: domain of business-finance.com designates 45.67.89.10 as permitted sender) 
Received: from mail.business-finance.com ([203.0.113.25]) 	by mail.target.com (Postfix) with ESMTP id ABC123; 	Mon, 26 Feb 2025 10:15:00 +0000 (UTC) 
Received: from relay.business-finance.com ([198.51.100.45]) 	by mail.business-finance.com with ESMTP id DEF456; 	Mon, 26 Feb 2025 10:10:00 +0000 (UTC) 
Received: from finance@business-finance.com ([198.51.100.75]) 	by relay.business-finance.com with ESMTP id GHI789; 	Mon, 26 Feb 2025 10:05:00 +0000 (UTC)
```
---

## 3. What is the sender's email address??

**Answer:** `finance@business-finance.com`

**Explanation:**  

```
❯ cat email.eml | grep From 
X-Envelope-From: finance@business-finance.com 
From: "Finance Dept" <finance@business-finance.com>
```
---

## 4. What is the 'Reply-To' email address specified in the email?

**Answer:** `support@business-finance.com`

**Explanation:**  

This cpuld be an indication of email spoofing — attackers often set a separate “Reply-To” to receive responses on a different mailbox.

```
❯ cat email.eml | grep Reply-To 
Reply-To: <support@business-finance.com>
```
---

## 5. What is the SPF (Sender Policy Framework) result for this email?

**Answer:** `pass`

**Explanation:**  

Overall, it means that the sending IP, which is 45.67.89.10, authorized to send mail on behalf of business-finance.com.
However, a passing SPF check does not confirm legitimacy, as the domain itself could have been compromised

```
❯ cat email.eml | grep SPF 
Received-SPF: Pass (protection.outlook.com: domain of business-finance.com designates 45.67.89.10 as permitted sender)
```
---

## 6. What is the domain used in the phishing URL inside the email?

**Answer:** `secure.business-finance.com`

**Explanation:**  

The HTML content of the email reveals the phishing link, attackers often use subdomains that appear trustworthy to trick recipients.

```
<a href="https://secure.business-finance.com/invoice/details/view/INV2025-0987/payment">Download Invoice</a>
```
---

## 7. What is the fake company name used in the email?

**Answer:** `Business Finance Ltd.`

**Explanation:**  

In the email footer we could found the fake comapny name.

<img width="592" height="227" alt="image" src="https://github.com/user-attachments/assets/306f1900-64e8-4444-8a5f-a01d90d8feff" />

---

## 8. What is the name of the attachment included in the email?

**Answer:** `Invoice_2025_Payment.zip`

**Explanation:**  

From the MIME section of the email:

```
Content-Type: application/zip; name="Invoice_2025_Payment.zip" Content-Disposition: attachment; filename="Invoice_2025_Payment.zip"
```
---

## 9.What is the SHA-256 hash of the attachment?

**Answer:** `8379c41239e9af845b2ab6c27a7509ae8804d7d73e455c800a551b22ba25bb4a`

**Explanation:**  

We should extract the attachment using munpack.

```
❯ munpack email.eml Invoice_2025_Payment.zip (application/zip) ❯ sha256sum Invoice_2025_Payment.zip 8379c41239e9af845b2ab6c27a7509ae8804d7d73e455c800a551b22ba25bb4a  Invoice_2025_Payment.zip
```
---

## 10. What is the filename of the malicious file contained within the ZIP attachment?

**Answer:** `invoice_document.pdf.bat`

**Explanation:**  

unzip and zipinfo failed due to an invalid archive header, but 7z managed to read partial data:

```
❯ 7z l Invoice_2025_Payment.zip

7-Zip 23.01 (x64) : Copyright (c) 1999-2023 Igor Pavlov : 2023-06-20
 64-bit locale=en_US.UTF-8 Threads:4 OPEN_MAX:1024

Scanning the drive for archives:
1 file, 75 bytes (1 KiB)

Listing archive: Invoice_2025_Payment.zip

--
Path = Invoice_2025_Payment.zip
Type = zip
ERRORS:
Unexpected end of archive
Physical Size = 75
Characteristics = Local

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2025-02-26 15:56:48 .....      1690811      1249907  invoice_document.pdf.bat
------------------- ----- ------------ ------------  ------------------------
2025-02-26 15:56:48            1690811      1249907  1 files

Errors: 1
```
The “double extension” technique (.pdf.bat) is a common obfuscation used to trick users into thinking the file is a safe PDF when it’s actually an executable batch script.

---

## 11. Which MITRE ATT&CK techniques are associated with this attack?

**Answer:** `T1566.001`

**Explanation:**  

This campaign uses a malicious attachment delivered via phishing email, which related to Phishing - Spearphishing Attachment

---
