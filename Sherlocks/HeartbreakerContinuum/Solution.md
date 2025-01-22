# Solution Report

## 1. To accurately reference and identify the suspicious binary, please provide its SHA256 hash.

**Answer:** `12DAA34111BB54B3DCBAD42305663E44E7E6C3842F015CCCBBE6564D9DFD3EA3`

**Explanation:**  

![image](https://github.com/user-attachments/assets/dd4d600c-93db-4124-9bbb-5bc241cd9e77)

---

## 2. When was the binary file originally created, according to its metadata (UTC)?

**Answer:** `2024-03-13 10:38:06`

**Explanation:**  

![image](https://github.com/user-attachments/assets/d2cde1e0-b288-4244-af6e-35b3c580a680)


---

## 3. Examining the code size in a binary file can give indications about its functionality. Could you specify the byte size of the code in this binary?

**Answer:** `38400`

**Explanation:**  

![image](https://github.com/user-attachments/assets/0f3b5cf6-0eb3-4edc-a152-e3efe8804acf)

---

## 4. It appears that the binary may have undergone a file conversion process. Could you determine its original filename?

**Answer:** `newILY.ps1`

**Explanation:**  

![image](https://github.com/user-attachments/assets/0d1a613f-e75b-40a7-94a2-f515d9677984)

---

## 5. Specify the hexadecimal offset where the obfuscated code of the identified original file begins in the binary.

**Answer:** `2C74`

**Explanation:**  

![image](https://github.com/user-attachments/assets/4f692cea-ca3d-4607-bcaf-80f8cc76efd8)

---

## 6. The threat actor concealed the plaintext script within the binary. Can you provide the encoding method used for this obfuscation?

**Answer:** `Base64`

**Explanation:**  

We can get a hint using the "strings" command.

![image](https://github.com/user-attachments/assets/bb709a2d-4d13-4117-a9b3-5c86ec5a486b)

![image](https://github.com/user-attachments/assets/038518af-c67d-41af-9de1-b41b997bf8a0)

---

## 7. What is the specific cmdlet utilized that was used to initiate file downloads?

**Answer:** `Invoke-WebRequest`

**Explanation:**

The value of `$sCrt` was decoded using [CyberChef](https://cyberchef.org/).

![image](https://github.com/user-attachments/assets/e686e781-7811-470c-8e03-d68151da468e)


---

## 8. Could you identify any possible network-related Indicators of Compromise (IoCs) after examining the code? Separate IPs by commas and in ascending order.

**Answer:** 35.169.66.138, 44.206.187.144

**Explanation:**

By scrolling down we will get the asked IP addresses.

![image](https://github.com/user-attachments/assets/c2f37731-bf20-4303-8a3a-28570e7148ec)

![image](https://github.com/user-attachments/assets/fef59ffa-9e56-48ae-9c72-684e2acf9928)

Or we can use the exreact IP method instead.

![image](https://github.com/user-attachments/assets/75c6f069-7910-417e-a9a8-2e56c2c7e5ec)

---

## 9. The binary created a staging directory. Can you specify the location of this directory where the harvested files are stored?

**Answer:** `C:\Users\Public\Public Files`

**Explanation:**

The staging directory can be seen in the deobfuscated code being assigned to a variable named $targetDir.

![image](https://github.com/user-attachments/assets/65b57577-50cc-4dc6-8506-878364f0bfa4)



---

## 10. What is the MITRE ATT&CK Technique ID associated with this activity?

**Answer:** `T1119`

---

## 11. Password Used for Exfiltration
What is the password utilized to exfiltrate the collected files through the file transfer program within the binary?

**Answer:** `M8&C!i6KkmGL1-#`

**Explanation:**  

![image](https://github.com/user-attachments/assets/465ca635-451a-4d3d-b368-179b48c3ee35)

