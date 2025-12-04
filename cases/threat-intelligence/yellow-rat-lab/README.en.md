# Yellow RAT: Behind the Scenes of a Threat Intelligence Case

Hello cybersecurity enthusiasts! In this article, we will solve step by step the "Yellow RAT" case that exists on the CyberDefenders platform and requires the mindset of a modern threat intelligence analyst. Our journey will begin with a single SHA256 hash and take us to illuminate an attack based not only on automatic scanning results, but also on external security reports and community-generated intelligence.

Ready to put on our intelligence hats and track this multi-layered threat!

## Crime Scene: Our First Evidence and Triage Phase

The case starts with a single **Indicator of Compromise (IOC)**:

![First Evidence - Malware Hash](assets/q.png)

- **SHA256 Hash:** `30E527E45F50D2BA82865C5679A6FA998EE0A1755361AB01673950810D071C85`

As analysts, our first step is to gather quick preliminary information (triage) about the file using this digital fingerprint. For this, our first and most important stop is **VirusTotal**.

![VirusTotal Overview](assets/q1.png)

VirusTotal's "Detection" tab provides us with instant critical information:
- **Popular Threat Label:** `trojan.msil/polazert`
- **Family Labels:** `polazert`, `fful`
- **File Type:** `DLL`

This information suggests that the file is a Trojan written in `.NET` and belonging to the `Polazert` family. However, as we will see, the story goes much deeper than this.

---

## Question 1: Malware Family Name

> **Question:** Understanding the adversary helps defend against attacks. What is the name of the malware family that causes abnormal network traffic? (Format: `****** ******** ***`)

### Analytical Approach

The automatic labels in VirusTotal (such as `Polazert`) don't match the answer format given in the question. This situation is a clue that the answer is hidden in a more specific threat intelligence report. In a real analysis, at this point we would search for external reports using the file's hash or known family names in search engines like Google. This case directs us to a report published by **Red Canary**.

### Evidence Review

The Red Canary report directly related to this case names the analyzed threat cluster **"Yellow Cockatoo"**. The report states that this activity is also known as `Jupyter Infostealer`. The CTF asks us for the name given by the company that published the report.

**Answer:** `Yellow Cockatoo RAT`

---

## Question 2-3-4: Filename, Compilation and First Detection Times

> **Question 2:** As part of our incident response, knowing common filenames the malware uses can help scan other workstations for potential infection. What is the common filename associated with the malware discovered on our workstations? (Format: `********-****-****-****-************.***`)

> **Question 3:** What is the compilation timestamp of the malware that infected our network?

> **Question 4:** When was the malware first submitted to VirusTotal?

### Analytical Approach

The answers to these three questions are all found in VirusTotal's **"Details"** tab. The filename is in the "Names" section, while compilation and detection times are in the "History" section. Answer formats provide important clues about the file's lifecycle and obfuscation techniques.

### Evidence Review

VirusTotal's **"Details"** tab lists the file's metadata and known names.

![VirusTotal File Details](assets/q2-q3-q4.png)

**Question 2 - Filename:** In the "Names" section, we see the filename `111bc461-1ca8-43c6-97ed-911e0e69fdf8.dll` which exactly matches the GUID format. This is the common filename that the malware uses when infecting the system.

**Question 3-4 - Time Information:** In the "History" section:
- **Creation Time (Compilation Time):** `2020-09-24 18:26:47 UTC`
- **First Submission:** `2020-10-15 02:47:37 UTC`

**Answer 2:** `111bc461-1ca8-43c6-97ed-911e0e69fdf8.dll`
**Answer 3:** `2020-09-24 18:26`
**Answer 4:** `2020-10-15 02:47`

---

## Question 5: .dat File Dropped in AppData Folder

> **Question:** To completely eradicate the threat from Industries' systems, we need to identify all components dropped by the malware. What is the name of the .dat file that the malware dropped in the AppData folder?

### Analytical Approach

To find the files that a malware drops (dropped files) in the system, we need behavioral analysis results. This information may not always be visible in VirusTotal's standard analysis. Therefore, we refer to Red Canary's threat intelligence report related to this case.

### Evidence Review

Red Canary's "Yellow Cockatoo" report explains the malware's behaviors in detail.

![Red Canary Report - Dropped File](assets/q5.png)

In the report, while describing the .NET RAT's capabilities, this statement appears: "It loads a randomly-generated string to `%USERPROFILE%\AppData\Roaming\solarmarker.dat`...". This definitively proves that the malware drops a file named `solarmarker.dat` in the `AppData` folder to track the victim.

**Answer:** `solarmarker.dat`

---

## Question 6: Command & Control (C2) Server

> **Question:** It is crucial to identify the C2 servers with which the malware communicates to block its communication and prevent further data exfiltration. What is the C2 server that the malware is communicating with?

### Analytical Approach

The same threat intelligence report will also contain the address of the malware's command and control server. This is one of the most critical pieces of information needed to block network traffic during an incident response.

### Evidence Review

When we examine the section of the Red Canary report related to C2 communication, we find the answer we're looking for.

![Red Canary Report - C2 Server](assets/q6.png)

In the report, the statement "It connects to the C2 server (address: `https://gogohid[.]com/gate`..." clearly reveals the domain name that the malware communicates with.

**Answer:** `https://gogohid.com`

---

## Conclusion: Complete Attack Chain

Starting from a single hash, we uncovered the complete anatomy of a multi-layered threat:

1. **Family Detection:** We determined that the malware belongs to the activity cluster named **Yellow Cockatoo RAT** by Red Canary.
2. **Persistence:** It copies itself to the system as a `.dll` file in **GUID** format.
3. **Lifecycle:** Created in late 2020 and detected by the security community shortly after.
4. **Tracking:** It drops a file named **`solarmarker.dat`** in the `AppData` folder to track the victim.
5. **Communication:** It communicates with the C2 server at **`https://gogohid.com`** to receive commands and exfiltrate data.

This case perfectly demonstrates that modern cybersecurity analysis is not dependent on just one tool; it requires combining different sources such as VirusTotal, external threat reports, and community intelligence to see the big picture.

---

## Technical Details and Lessons Learned

### Analysis Tools Used
- **VirusTotal:** Automatic malware detection and file analysis
- **Red Canary:** External threat intelligence reports
- **Google/Bing:** Hash-based search and community intelligence
- **Threat Intelligence Platforms:** IOC correlation and analysis

### Protocol and Technical Details
- **SHA256 Hash:** File integrity and authentication
- **GUID Format:** Unique identifiers in Windows systems
- **DLL Files:** Windows dynamic link libraries
- **AppData Folder:** Windows user data directory
- **HTTPS C2:** Encrypted command and control communication

### Threat Intelligence Methodology
1. **IOC Detection:** Indicators such as hash, filename, IP address
2. **Triage Analysis:** Quick assessment on platforms like VirusTotal
3. **External Reports:** Detailed analysis from security companies like Red Canary
4. **Community Intelligence:** Information shared by security researchers
5. **Correlation:** Combining information from different sources
6. **IOC Extraction:** Detection of new indicators

### Security Measures
1. **Hash-Based Detection:** Known malware signatures
2. **Behavioral Analysis:** Abnormal file activities
3. **Network Monitoring:** Traffic going to C2 servers
4. **Endpoint Protection:** Advanced threat detection
5. **Threat Intelligence:** Current IOC databases

### MITRE ATT&CK Mapping
- **T1027.002 (Obfuscated Files or Information: Software Packing):** DLL file hidden in GUID format
- **T1055.001 (Process Injection: Dynamic-link Library Injection):** Running as .NET DLL
- **T1071.001 (Application Layer Protocol: Web Protocols):** C2 communication over HTTPS
- **T1053.005 (Scheduled Task/Job: Scheduled Task):** Persistence file in AppData folder
- **T1036 (Masquerading):** Hidden as legitimate DLL file

---

## Reproducibility and Commands

The following commands are provided to work exactly with the directory structure in this repository.

```bash
# Hash analysis
echo "30E527E45F50D2BA82865C5679A6FA998EE0A1755361AB01673950810D071C85" | xxd -r -p | file -

# VirusTotal API usage (optional)
curl -s "https://www.virustotal.com/vtapi/v2/file/report" \
  --data-urlencode "apikey=YOUR_API_KEY" \
  --data-urlencode "resource=30E527E45F50D2BA82865C5679A6FA998EE0A1755361AB01673950810D071C85"

# Hash search (Google, Bing)
# "30E527E45F50D2BA82865C5679A6FA998EE0A1755361AB01673950810D071C85" + "Red Canary"
# "Yellow Cockatoo" + "malware analysis"

# File type analysis
file -b [filename]
strings [filename] | grep -i "yellow\|cockatoo\|jupyter"
```

---

*This article is prepared for cybersecurity education purposes. Each analysis contains unique details and challenges. In real cases, similar techniques are used to detect and prevent cyber attacks.*

---

Author: [iamhasancoskun](https://github.com/iamhasancoskun)
