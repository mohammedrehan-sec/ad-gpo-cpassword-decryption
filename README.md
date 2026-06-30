 # Active Directory GPO Password Recovery

## Objective
To analyze a packet capture (PCAP) file generated during the boot process of a workstation connected to an Active Directory domain and recover the administrator password by extracting and decrypting the cpassword stored within Group Policy Preferences.

## Scenario
During a security audit, network traffic during the boot sequence of a workstation enrolled in Active Directory was recorded as a PCAP file. The objective of the investigation was to analyze the captured traffic, identify Group Policy Preference files transmitted over the network, extract encrypted credentials (cpassword), and decrypt them to obtain the administrator password.

## Tools Used
- **Wireshark** – Packet capture analysis
- **SMB Export Objects** – To extract Group Policy files
- **Mousepad (Linux Text Editor)** – To examine XML content
- **gpp-decrypt (Kali Linux)** – To decrypt cpassword values

## Methodology
1. Opened the provided PCAP file in Wireshark
2. Analyzed SMB traffic by filtering packets related to file sharing services

<img width="1331" height="526" alt="Screenshot 2026-04-10 183346" src="https://github.com/user-attachments/assets/607d36de-05e3-4af9-b9cb-4b5d64f78e96" />
 
3. Used **File → Export Objects → SMB** to extract files transferred during the network session
4. Identified a `Groups.xml` file within the Group Policy Preferences directory

<img width="861" height="523" alt="Screenshot 2026-04-10 183039" src="https://github.com/user-attachments/assets/e2e5ae14-f103-4a02-ace8-32510fe86356" />

5. Opened `Groups.xml` using Mousepad in Linux to inspect its contents
6. Identified multiple user entries within the XML structure, including the administrator account (`Administrateur`)

<img width="752" height="219" alt="Screenshot 2026-04-10 182330" src="https://github.com/user-attachments/assets/3364d37c-0be0-4955-a992-0a8613f3d037" />

7. Extracted the `cpassword` value associated with the administrator account
8. Decrypted the encrypted password using `gpp-decrypt` in Kali Linux

<img width="660" height="435" alt="Screenshot 2026-04-10 183806" src="https://github.com/user-attachments/assets/c5c3f649-fd99-4440-b1bd-b8ca55da28ce" />

## Analysis & Findings
During analysis of the extracted `Groups.xml` file, multiple user accounts were identified. One entry corresponded to the administrator account with the username **Administrateur**. The associated `cpassword` value was extracted and decrypted using `gpp-decrypt`.

The decryption process successfully revealed the administrator password as:

**`TuM@STrouv3`**

<img width="582" height="120" alt="Screenshot 2026-04-10 184514" src="https://github.com/user-attachments/assets/640b3a1d-3943-48b2-944c-f3c4c3bec02d" />
 />

This confirms that sensitive credentials were stored insecurely within Group Policy Preferences and transmitted over the network in an encrypted-but-crackable format, making them recoverable during forensic/packet analysis — a known and well-documented Active Directory misconfiguration (MS14-025).

## Conclusion
The forensic analysis of the captured network traffic successfully identified and decrypted the administrator's cpassword stored in Group Policy Preferences. This highlights a critical security weakness in legacy Active Directory deployments, where credentials pushed via GPO can be trivially recovered by an attacker with network capture access.

---
*Part of the Cyber50 Defense Blue Team Internship Programme – Digital Forensics module.*
