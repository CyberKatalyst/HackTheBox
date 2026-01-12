# Solution Report

## Quick Background on NBNS and LLMNR Protocols

NBNS (NetBIOS Name Service) and LLMNR (Link Local Multicast Name Resolution) are Windows protocols used for hostname resolution when a hosts file entry or DNS is unavailable. These protocols are significant in penetration testing as they can be exploited if enabled. Errors in resolving resource names can trigger lookups that attackers can poison, leading to attacks like NTLM relay or LLMNR poisoning.
LLMNR poisoning captures NTLM password hashes, which can either be relayed to a domain host for authentication or taken offline for decryption.

---

## 1. What is the IP Address for Forela-Wkstn001?

**Answer:** `172.17.79.129`

**Explanation:**  

We can filter for NBNS (NetBIOS Name Service) Refresh packets, which allow a device to refresh its NetBIOS name registration on the network.

![image](https://github.com/user-attachments/assets/63ecad48-b8ea-49b9-b97d-7af5c08fc0da)

---

## 2. What is the IP Address for Forela-Wkstn002?

**Answer:** `172.17.79.136`

**Explanation:**  

We could do the same as previous task on this task to get an IP address of Forela-Wkstn002.

![image](https://github.com/user-attachments/assets/b807bba4-5c7f-4e99-8378-4304b5e5e94d)

---

## 3. What is the username of the account whose hash was stolen by attacker?

**Answer:** `arthur.kyle`

**Explanation:**  

<img width="1100" height="196" alt="image" src="https://github.com/user-attachments/assets/245b36e3-ead0-44c3-acdf-d2d4f0cd6c40" />

We can  see other IPs jumped in on NBNS protocol to query status of a NetBIOS name or machine, including a list of all NetBIOS names from 172.17.79.1 and as the blog says that the IP address that sent this request could be an unknown device / threat actor device so we can use this IP address to filter for SMB traffic and see if the threat actor has accessed to any file share.

<img width="1100" height="217" alt="image" src="https://github.com/user-attachments/assets/ec992e7f-8e0b-4029-933c-27c313251221" />

After filtered with smb2 && ip.addr == 172.17.79.135, we can see that this unknown device successfully authenticated as arthur.kyle and tried to access other file shares.

<img width="1100" height="468" alt="image" src="https://github.com/user-attachments/assets/a09e424b-1341-42fe-8826-ee2868ee2bad" />


<img width="1100" height="725" alt="image" src="https://github.com/user-attachments/assets/a2c52400-905e-4d09-a466-4af9c11aecdd" />

Its time to open audit log to see that there is one login event that look very suspicious since there is no Logon ID (NULL SID) and it was authenticated via NTLM and the source IP address is the suspicious device we are after.

---

## 4. What is the IP Address of Unknown Device used by the attacker to intercept credentials?

**Answer:** `172.17.79.135`

---

## 5. What was the fileshare navigated by the victim user account?

**Answer:** `\\DC01\Trip`

**Explanation:**  

We should find for Tree Connect that response with Error like this which mean file share could not be found or was not recognized by the server. This could happen if the share doesn’t exist, is offline, or the client doesn’t have access rights.

![image](https://github.com/user-attachments/assets/5ecc36fe-5420-4707-a841-af717bc68f0b)

---

## 6. What is the source port used to logon to target workstation using the compromised account?

**Answer:** `40252`

**Explanation:**  

We will filter for Event ID 4624:
- Event ID 4624 corresponds to Logon events, which will show successful authentication attempts. This will help us locate logins associated with arthur.kyle
- Once the 4624 events are filtered, use CTRL+F to search for the username “arthur”.
This will narrow down the logs to only those involving arthur.kyle.

![image](https://github.com/user-attachments/assets/891879b7-431d-41b8-a1b8-b1c6c8b339e2)

---

## 7. What is the Logon ID for the malicious session?

**Answer:** `0x64A799`

**Explanation:**

![image](https://github.com/user-attachments/assets/de8a4079-d7a5-4c0f-a22d-71db7423673f)

---

## 8. The detection was based on the mismatch of hostname and the assigned IP Address.What is the workstation name and the source IP Address from which the malicious logon occur?

**Answer:** `FORELA-WKSTN002, 172.17.79.135`

---

## 9. At what UTC time did the the malicious logon happen?

**Answer:** `2024-07-31 04:55:16`

**Explanation:**

![image](https://github.com/user-attachments/assets/20797a38-ca4f-4dd2-8289-cd2c6afca10f)

---

## 10. What is the share Name accessed as part of the authentication process by the malicious tool used by the attacker?

**Answer:** `\\*\IPC$`

**Explanation:**

By filtering the logs for `Event ID 5140`, which tracks access to shared resources, we identified a key event tied to the suspicious activity. 
The logs revealed that the compromised account, `arthur.kyle`, accessed a file share from a `172.17.79.135` malicious IP address. 
This IP does not correspond to any known hosts within the domain, confirming its association with the rogue device involved in earlier stages of the attack, such as LLMNR poisoning or an SMB relay. 
Further analysis shows that the Process ID associated with this event matches the one recorded during the prior malicious logon event. 
This correlation solidifies the connection between the unauthorized login and the file share access attempt. 
The targeted share in this case was `*\IPC$`, an administrative share often used for Inter-Process Communication (IPC). This type of access is frequently leveraged by attackers to enumerate network resources or escalate privileges. The evidence from this log entry links the compromised credentials, the rogue IP, and the unauthorized activity, providing clear proof of malicious intent and highlighting the attacker’s efforts to exploit arthur.kyle’s credentials for further network reconnaissance or privilege escalation.

![image](https://github.com/user-attachments/assets/ce2bd86e-3540-4e6e-8e72-8653e2b52f6f)

---
