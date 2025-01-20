# Solution Report

## 1. What is the SHA-256 hash of this malware binary?

**Answer:** `6acd8a362def62034cbd011e6632ba5120196e2011c83dc6045fcb28b590457c`

**Explanation:**  

![image](https://github.com/user-attachments/assets/b00d458f-9901-47d6-9c49-b6870f6f45af)

---

## 2. What programming language (and version) is this malware written in?

**Answer:** `Golang 1.22.3`

**Explanation:**  

I tried searching for any language mentions using the strings command.

![image](https://github.com/user-attachments/assets/26ea6bb6-dfff-4de6-a92d-167f74916520)

![image](https://github.com/user-attachments/assets/4aaf1539-e380-43ec-9d15-53c2cfb85f1f)

---

## 3. There are multiple GitHub repos referenced in the static strings. Which GitHub repo would be most likely suggest the ability of this malware to exfiltrate data?

**Answer:** `github.com/jlaffaye/ftp`

**Explanation:**  

![image](https://github.com/user-attachments/assets/cd51e0a4-a9b3-46d3-9105-0c43bf2f69c3)

![image](https://github.com/user-attachments/assets/8fad730a-0a26-4510-af08-4ac2f6416a52)

---

## 4. What dependency, expressed as a GitHub repo, supports Janice’s assertion that she thought she downloaded something that can just take screenshots?

**Answer:** `github.com/kbinani/screenshot`

**Explanation:**  

![image](https://github.com/user-attachments/assets/e6fe5d53-1716-4403-88dd-862b27842965)

---

## 5. Which function call suggests that the malware produces a file after execution?

**Answer:** ``

**Explanation:**  

---

## 6. You observe that the malware is exfiltrating data over FTP. What is the domain it is exfiltrating data to?

**Answer:** ``

**Explanation:**  

---

## 7. What are the threat actor’s credentials?

**Answer:** ``

**Explanation:**  

---

## 8. What file keeps getting written to disk?

**Answer:** `keylog.txt`

**Explanation:**  

![image](https://github.com/user-attachments/assets/a02f7606-47c0-47c5-a628-ef91410a7f75)

---

## 9. When Janice changed her password, this was captured in a file. What is Janice's username and password?

**Answer:** `janice:Password123`

**Explanation:**  

![image](https://github.com/user-attachments/assets/ceba593f-f9e9-48ad-a24d-cd3366e67e62)

---

## 10. What app did Janice have open the last time she ran the "screenshot app"?

**Answer:** `Solitaire`

**Explanation:**  

![image](https://github.com/user-attachments/assets/29629a25-6061-488e-9694-d7a781b7cc2e)


---

## 2. What programming language (and version) is this malware written in?

**Answer:** `Golang 1.22.3`

**Explanation:**  

---

## 2. What programming language (and version) is this malware written in?

**Answer:** `Golang 1.22.3`

**Explanation:**  

I found several mentions of "Go runtime" keywords. Therefore, I used another command with regex to dig out the version of Go.

![image](https://github.com/user-attachments/assets/1b4268bb-c356-4738-a86e-a637cc8e670a)



---
