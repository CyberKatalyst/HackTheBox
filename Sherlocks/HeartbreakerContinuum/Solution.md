# Solution Report

## 1. To accurately reference and identify the suspicious binary, please provide its SHA256 hash.

**Answer:** `12DAA34111BB54B3DCBAD42305663E44E7E6C3842F015CCCBBE6564D9DFD3EA3`

**Explanation:**  

![SHA256 Hash Explanation](https://github.com/user-attachments/assets/1e517492-8815-4a0d-880f-4d649676efcf)

---

## 2. When was the binary file originally created, according to its metadata (UTC)?

**Answer:** `2024-03-13 10:38:06`

**Explanation:**  

![image](https://github.com/user-attachments/assets/066a87cb-56b2-4e20-9f70-64640141045f)

---

## 3. Examining the code size in a binary file can give indications about its functionality. Could you specify the byte size of the code in this binary?

**Answer:** `38400`

**Explanation:**  

![image](https://github.com/user-attachments/assets/5b56e912-bd90-4d96-82f4-c215d519424b)

---

## 4. It appears that the binary may have undergone a file conversion process. Could you determine its original filename?

**Answer:** `newILY.ps1`

**Explanation:**  

![image](https://github.com/user-attachments/assets/f886df8d-421f-455e-a5b5-78af45fcd7e9)

---

## 5. Specify the hexadecimal offset where the obfuscated code of the identified original file begins in the binary.

**Answer:** `2C74`

**Explanation:**  

![image](https://github.com/user-attachments/assets/6a73d0d6-f8f1-4bdf-a22c-83d72a559cb2)

The obfuscated code begins at offset `2C74`, which is 4 bytes after `2C70` (where the string `$sCrt` begins).  

To determine this:  
- Open the content of the code.txt file in [Hex Editor](https://hexed.it/).  
- Search for the Hex cell of `$sCrt`, which is `24 73 43 72`.  
- The obfuscated code in this case begins at offset 2c74, which is 4 bytes after 2c70 (where the string $sCr begins), therefore the offset is `2C74`.

![image](https://github.com/user-attachments/assets/a053545c-343f-4378-9d85-414ba214ca0a)

![image](https://github.com/user-attachments/assets/b9ed6797-9897-4858-a5c8-167312209e5f)


---

## 6. The threat actor concealed the plaintext script within the binary. Can you provide the encoding method used for this obfuscation?

**Answer:** `Base64`

**Explanation:**  

![image](https://github.com/user-attachments/assets/bebf3ae4-a820-4c7a-a3cd-bd63a6978ae2)

---

## 7. What is the specific cmdlet utilized that was used to initiate file downloads?

**Answer:** `Invoke-WebRequest`

**Explanation:**

The value of `$sCrt` was decoded using [CyberChef](https://cyberchef.org/).

![image](https://github.com/user-attachments/assets/0a4ad2fb-a72a-4886-9ebf-2e3c363320fe)

---

## 8. Could you identify any possible network-related Indicators of Compromise (IoCs) after examining the code? Separate IPs by commas and in ascending order.

**Answer:** 35.169.66.138, 44.206.187.144

**Explanation:**

![image](https://github.com/user-attachments/assets/2ca03aa0-519b-44e5-8169-2ca775a615b7)

![image](https://github.com/user-attachments/assets/f2d97d5d-f067-4669-a388-c33bb4bc412a)

---

## 9. The binary created a staging directory. Can you specify the location of this directory where the harvested files are stored?

**Answer:** `C:\Users\Public\Public Files`

**Explanation:**

![image](https://github.com/user-attachments/assets/5d9fe242-e80f-47b2-bb34-d42dbf118141)

---

## 10. What is the MITRE ATT&CK Technique ID associated with this activity?

**Answer:** `T1119`

---

## 11. Password Used for Exfiltration
What is the password utilized to exfiltrate the collected files through the file transfer program within the binary?

**Answer:** `M8&C!i6KkmGL1-#`

**Explanation:**  

![image](https://github.com/user-attachments/assets/587d817b-6524-45d5-bdea-7b9d36045464)
