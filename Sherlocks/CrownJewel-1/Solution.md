# Solution Report

## 1. Attackers can abuse the vssadmin utility to create volume shadow snapshots and then extract sensitive files like NTDS.dit to bypass security mechanisms. Identify the time when the Volume Shadow Copy service entered a running state.

**Answer:** `2024-05-14 03:42:16`

**Explanation:**  

According to the question, I checked the System log in Event Viewer because it records information about system-level activities, including service state changes. I filtered by Event ID 7036, which logs when services enter a new state (e.g., running or stopped).
The reason for this is that attackers abusing the vssadmin utility rely on the Volume Shadow Copy Service (VSS) to create snapshots and extract sensitive files like NTDS.dit. 

![image](https://github.com/user-attachments/assets/23ea294b-bfaf-454c-86cc-4f92786d82e9)

---

## 2. When a volume shadow snapshot is created, the Volume shadow copy service validates the privileges using the Machine account and enumerates User groups. Find the two user groups the volume shadow copy process queries and the machine account that did it.

**Answer:** `Administrators, Backup Operators, DC01$`

**Explanation:**  

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/3a65ccd1-5293-417c-b40f-cdb007af8a8b" />

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/2d12b204-df89-4caa-bc92-b99523617fdf" />


---

