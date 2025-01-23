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

![image](https://github.com/user-attachments/assets/b807bba4-5c7f-4e99-8378-4304b5e5e94d)

---

The follwing filter `llmnr” or udp.port == 5355` will display all LLMNR-based data in the capture.

![image](https://github.com/user-attachments/assets/30555f90-07d1-49a5-9dca-ad446b30f9ad)

As can be seen 172.17.79.136 (Forela-Wkstn002) requested a resource and was responded to by 172.17.79.135, which may be the threat actor.

![image](https://github.com/user-attachments/assets/9dd77819-291a-4ed4-8c57-2ae86324114c)

Using `dhcp` filter we can see that the address in question has a hostname that does not align with the domain schema.

---

## 3. What is the username of the account whose hash was stolen by attacker?

**Answer:** `arthur.kyle`

**Explanation:**  

NTLM credentials are based on data obtained during the interactive logon process and consist of a domain name, a username, and a one-way hash of the user’s password. These credentials can be leveraged during attacks, such as an NTLM relay attack, to gain unauthorized access to resources. For more details on NTLM, refer to the Wireshark documentation on NTLMSSP.

In the provided pcap file, an NTLM relay attack is evident, targeting access to a file share via SMB Relay. Here's what happens:

- An NTLM authentication request originates from a known host to an address that does not belong to any other known hosts in the domain. Instead, the request is sent to a rogue device.
- The rogue (malicious) host relays these requests to Forela-Wkstn001's file share to achieve access and enumerate the IPC$ share. This is suspicious and indicative of malicious behavior.

![image](https://github.com/user-attachments/assets/ba1f9698-c361-47da-a2e2-9397cf27ab06)

The scenario indicates that a user’s NTLM hash was stolen and relayed to a file share for authentication. Filtering for “LLMNR” in the provided packet capture reveals the LLMNR poisoning attack, which enabled a man-in-the-middle (MITM) situation.

---

## 4. What is the IP Address of Unknown Device used by the attacker to intercept credentials?

**Answer:** `172.17.79.135`

**Explanation:**  

- 172.17.79.136 IP Address for Forela-Wkstn002
- 172.17.79.135 IP Address of Unknown Device

---

## 5. What was the fileshare navigated by the victim user account?

**Answer:** `\\DC01\Trip`

**Explanation:**  

- Specifies the Server Message Block (SMB) Protocol Versions 2 and 3, which support the sharing of file and print resources.
- Tree connect which responses with Error, means file share could not be found or was not recognized by the server.

![image](https://github.com/user-attachments/assets/5ecc36fe-5420-4707-a841-af717bc68f0b)

---

We’ve identified that the compromised account is arthur.kyle, who primarily uses Forela-Wkstn002. To investigate further, we’ll examine the Security logs to detect the source port associated with the suspicious activity. 

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
