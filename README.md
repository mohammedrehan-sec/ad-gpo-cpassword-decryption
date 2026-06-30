 Directory GPO Password Recovery

Objective
To analyze a packet capture (PCAP) file generated during the boot process of a workstation connected to an Active Directory domain and recover the administrator password by extracting and decrypting the cpassword stored within Group Policy Preferences.

Scenario
During a security audit, network traffic during the boot sequence of a workstation enrolled in Active Directory was recorded as a PCAP file. The objective of the investigation was to analyze the captured traffic, identify Group Policy Preference files transmitted over the network, extract encrypted credentials (cpassword), and decrypt them to obtain the administrator password.

Tools Used
Wireshark – Packet capture analysis
SMB Export Objects – To extract Group Policy files
Mousepad (Linux Text Editor) – To examine XML content
gpp-decrypt (Kali Linux) – To decrypt cpassword values


Methodology
Opened the provided PCAP file in Wireshark
Analyzed SMB traffic by filtering packets related to file-sharing services
Used File → Export Objects → SMB to extract files transferred during the network session
Identified a Groups.xml file within the Group Policy Preferences directory
Opened Groups.xml using Mousepad in Linux to inspect its contents
Identified multiple user entries within the XML structure, including the administrator account (Administrateur)
Extracted the cpassword value associated with the administrator account
Decrypted the encrypted password using gpp-decrypt in Kali Linux

Analysis & Findings
During analysis of the extracted Groups.xml file, multiple user accounts were identified. One entry corresponded to the administrator account with the username Administrateur. The associated cpassword value was extracted and decrypted using gpp-decrypt.

The decryption process successfully revealed the administrator password as: TuM@STrouv3
This confirms that sensitive credentials were stored insecurely within Group Policy Preferences and transmitted over the network in an encrypted-but-crackable format, making them recoverable during forensic/packet analysis, a known and well-documented Active Directory misconfiguration (MS14-025).
