# Solution Report

## 1. What is the IP Address for Forela-Wkstn001?

**Answer:** `172.17.79.129`

**Explanation:**  

<img width="944" alt="image" src="https://github.com/user-attachments/assets/087ad8fa-8d82-4918-963a-8cfb373a1e0f" />

---

## 2. What is the IP Address for Forela-Wkstn002?

**Answer:** `172.17.79.136`

**Explanation:**  

<img width="959" alt="image" src="https://github.com/user-attachments/assets/8cd9f320-3b05-4fee-b08e-f4b1ae483230" />

---

## 3. What is the username of the account whose hash was stolen by attacker?

**Answer:** `arthur.kyle`

**Explanation:**  

- NTLM credentials are based on data obtained during the interactive logon process and consist of a domain name, a user name, and a one-way hash of the user's password.
- https://www.wireshark.org/docs/dfref/n/ntlmssp.html

<img width="958" alt="image" src="https://github.com/user-attachments/assets/e2d7bbfe-7ad1-4159-9578-0e2f2edb3318" />

---

## 4. What is the IP Address of Unknown Device used by the attacker to intercept credentials?

**Answer:** `172.17.79.135`

**Explanation:**  

- 172.17.79.136 IP Address for Forela-Wkstn002, therefore the IP Address of Unknown Device is 172.17.79.135

<img width="958" alt="image" src="https://github.com/user-attachments/assets/57ca5047-1af4-4143-bbb0-8388b8744ab6" />

---

## 5. What was the fileshare navigated by the victim user account?

**Answer:** `\\DC01\Trip`

**Explanation:**  

- Specifies the Server Message Block (SMB) Protocol Versions 2 and 3, which support the sharing of file and print resources.
- Tree connect which responses with Error, means file share could not be found or was not recognized by the server.

<img width="956" alt="image" src="https://github.com/user-attachments/assets/0ca5dfdf-dfbc-40fb-83aa-f8ba6aa72320" />

---

## 6. What is the source port used to logon to target workstation using the compromised account?

**Answer:** `40252`

**Explanation:**  

<img width="1277" alt="image" src="https://github.com/user-attachments/assets/f7989302-4593-40fd-804b-344dd4ab2a7d" />

---

## 7. What is the Logon ID for the malicious session?

**Answer:** `0x64A799`

**Explanation:**

<img width="1277" alt="image" src="https://github.com/user-attachments/assets/1907ee63-d0af-42bf-8322-113248f49ba3" />

---

## 8. The detection was based on the mismatch of hostname and the assigned IP Address.What is the workstation name and the source IP Address from which the malicious logon occur?

**Answer:** `FORELA-WKSTN002, 172.17.79.135`

**Explanation:**

<img width="1277" alt="image" src="https://github.com/user-attachments/assets/fd0c9c54-0094-4e4c-925a-85cacf4366c0" />

---

## 9. At what UTC time did the the malicious logon happen?

**Answer:** `2024-07-31 04:55:16`

**Explanation:**

<img width="1277" alt="image" src="https://github.com/user-attachments/assets/6b73394c-edcf-44ee-b9f8-b3ffa14046bb" />

<img width="1279" alt="image" src="https://github.com/user-attachments/assets/e9bf2579-c278-4cc9-984e-9a4235295489" />

---

## 10. What is the share Name accessed as part of the authentication process by the malicious tool used by the attacker?

**Answer:** `\\*\IPC$`

**Explanation:**

<img width="1277" alt="image" src="https://github.com/user-attachments/assets/0b6ae055-689c-4b01-bd4a-971b6207f1f5" />

---
