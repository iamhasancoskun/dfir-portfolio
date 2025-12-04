# 🔍 DFIR Portfolio - Digital Forensics & Incident Response Case Studies

<p align="center">
  <img src="https://img.shields.io/badge/Cases-11-blue?style=for-the-badge" alt="Total Cases"/>
  <img src="https://img.shields.io/badge/Memory%20Forensics-1-green?style=for-the-badge" alt="Memory"/>
  <img src="https://img.shields.io/badge/Network%20Forensics-5-orange?style=for-the-badge" alt="Network"/>
  <img src="https://img.shields.io/badge/Languages-EN%20%7C%20TR-red?style=for-the-badge" alt="Languages"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License"/>
</p>

<p align="center">
  <strong>English</strong> | <a href="README.tr.md">Türkçe</a>
</p>

---

## 👤 About Me

SOC Analyst & DFIR Enthusiast passionate about blue team operations and incident response. I document my learning journey through hands-on analysis of real-world attack scenarios from CyberDefenders platform.

**Focus Areas:** Memory Forensics | Network Analysis | Threat Intelligence | Malware Analysis  
**Tools:** Volatility 3 | Wireshark | MITRE ATT&CK Framework

🔗 [LinkedIn](https://www.linkedin.com/in/hsnco) | 🌐 [GitHub](https://github.com/iamhasancoskun)

> *Note: This portfolio represents 100+ hours of hands-on analysis, documentation, and learning. Each case includes my methodology, findings, and lessons learned.*

---

## 📊 Case Categories

| Category | Case Count | Tools | Status |
|----------|------------|-------|--------|
| 🧠 **Memory Forensics** | 1 | Volatility 3, Rekall | ✅ Active |
| 🌐 **Network Forensics** | 5 | Wireshark, tcpdump, tshark | ✅ Active |
| 🔎 **Threat Intelligence** | 1 | VirusTotal, OSINT | ✅ Active |
| 🦠 **Malware Analysis** | 1 | IDA, x64dbg, Any.run | ✅ Active |
| 🕵️ **OSINT** | 1 | Google Lens, WhatsMyName | ✅ Active |
| 📱 **Mobile Forensics** | 1 | ALEAPP, DBeaver | ✅ Active |
| 🌐 **Web Application Security** | 1 | Wireshark, Burp Suite | ✅ Active |

---

## 🎯 Featured Cases

### 1. 🏆 [Tomcat Takeover - Web Server Breach Analysis](cases/network-forensics/tomcat-takeover/)
**Threat:** Web Shell Deployment | **Techniques:** Directory Bruteforce, WAR Upload, Reverse Shell  
**Tools:** Wireshark, gobuster  
**Highlights:** Complete PCAP analysis revealing end-to-end attack chain

### 2. 🕵️ [Lespion - Insider Threat OSINT Investigation](cases/osint/lespion/)
**Threat:** Insider Threat | **Techniques:** GitHub Secrets Analysis, Geolocation  
**Tools:** Google Lens, GitHub, OSINT Tools  
**Highlights:** Multi-layered intelligence extraction from a single GitHub profile

### 3. 🎯 [PsExec Hunt - Lateral Movement Detection](cases/network-forensics/psexec-hunt/)
**Threat:** Lateral Movement | **Techniques:** PsExec, SMB Analysis  
**Tools:** Wireshark, SMB Protocol Analysis  
**Highlights:** Multi-target attack detection from network traffic

### 4. 🦠 [Yellow RAT - Threat Intelligence Study](cases/threat-intelligence/yellow-rat-lab/)
**Threat:** Yellow Cockatoo RAT | **Techniques:** Threat Intelligence, IOC Analysis  
**Tools:** VirusTotal, Red Canary Reports  
**Highlights:** Complete attack chain extraction from a single hash

### 5. 📱 [The Crime Lab - Android Forensics](cases/mobile-forensics/the-crime-lab/)
**Threat:** Financial Crime Investigation | **Techniques:** Android Data Extraction, Timeline Analysis  
**Tools:** ALEAPP, SQLite, DBeaver  
**Highlights:** Mobile forensics in murder investigation context

[→ View All Cases](#all-cases)

---

## 📚 All Cases

### 🌐 Network Forensics
<details>
<summary>5 cases - Click to expand</summary>

- **[Tomcat Takeover](cases/network-forensics/tomcat-takeover/)** - Web Server Breach via File Upload
- **[PsExec Hunt](cases/network-forensics/psexec-hunt/)** - Lateral Movement Detection via SMB
- **[DanaBot](cases/network-forensics/danabot/)** - Banking Malware Network Analysis
- **[Poisoned Credentials](cases/network-forensics/poisoned-credentials/)** - NBNS/LLMNR Poisoning Attack
- **[XLM RAT](cases/network-forensics/xlm-rat/)** - PowerShell Obfuscation & AsyncRAT

</details>

### 🕵️ OSINT & Threat Intelligence
<details>
<summary>2 cases - Click to expand</summary>

- **[Lespion](cases/osint/lespion/)** - Insider Threat Investigation via GitHub & Social Media
- **[Yellow RAT Lab](cases/threat-intelligence/yellow-rat-lab/)** - Threat Intel Analysis from Hash to IOC

</details>

### 🦠 Malware Analysis
<details>
<summary>1 case - Click to expand</summary>

- **[Oski Lab](cases/malware-analysis/oski-lab/)** - Infostealer Analysis via VirusTotal & Any.run

</details>

### 📱 Mobile Forensics
<details>
<summary>1 case - Click to expand</summary>

- **[The Crime Lab](cases/mobile-forensics/the-crime-lab/)** - Android Forensics in Murder Investigation

</details>

### 🌐 Web Application Security
<details>
<summary>1 case - Click to expand</summary>

- **[WebStrike](cases/web-application-security/webstrike/)** - Web Shell Upload & Reverse Shell Attack

</details>

### 🧠 Memory Forensics
<details>
<summary>1 case - Click to expand</summary>

- **[Amadey Trojan](cases/memory-forensics/amadey-trojan/)** - Advanced Memory Forensics with Volatility 3
  - *Coming Soon* - Process Hollowing & PEB Unlinking Analysis

</details>

---

## 🛠️ Tools & Technologies

### Forensics & Analysis
- **Volatility 3** - Memory forensics framework
- **Wireshark / tshark** - Network protocol analyzer
- **ALEAPP** - Android Logs Events And Protobuf Parser

### Malware Analysis
- **Any.run** - Interactive malware sandbox
- **VirusTotal** - File and URL analysis
- **CyberChef** - Data manipulation and decoding

### Threat Intelligence
- **OSINT Tools** - Google Lens, WhatsMyName, IP Geolocation
- **MITRE ATT&CK** - Threat modeling framework

---

## 📖 Methodology

All case analyses follow a **"Hypothesis-Driven Approach"**:

1. **Hypothesis** - Form an assumption about the finding
2. **Test** - Test the hypothesis with appropriate tools
3. **Evidence** - Document findings with visuals
4. **IOC** - Extract and share indicators
5. **MITRE ATT&CK** - Map to TTPs

Detailed methodology: [English](docs/METHODOLOGY.md) | [Türkçe](docs/METHODOLOGY.tr.md)

---

## 🎓 Education & Resources

### 📚 **Challenge Source**

All cases in this repository are **solved challenges** from the [**CyberDefenders**](https://cyberdefenders.org) platform, specifically from the [**SOC Analyst Tier 1 Track**](https://cyberdefenders.org/tracks/soc-analyst-tier-1/).

**🔗 CyberDefenders Platform:**  
- Website: [cyberdefenders.org](https://cyberdefenders.org)
- Track: [SOC Analyst Tier 1](https://cyberdefenders.org/tracks/soc-analyst-tier-1/)
- Lab Type: Blue Team Hands-On Labs

### ⚖️ **Educational Use & Attribution**

- ✅ **Challenge Files:** Original forensic artifacts (PCAP, memory dumps, etc.) are available on CyberDefenders platform
- ✅ **This Repository Contains:** My analysis, methodology, findings, screenshots, and IOC extraction
- ✅ **Purpose:** Educational documentation and portfolio showcase
- ⚠️ **Note:** This repository does NOT redistribute original challenge files to respect platform terms

### 📖 **Analysis Features**

- ✅ **Reproducible** - All commands and steps are shared
- ✅ **Educational** - Learning-focused, step-by-step explanations
- ✅ **Bilingual** - Documentation in Turkish and English
- ✅ **Visual** - Every step supported with screenshots

---

## 🤝 Contributing

This project was created to contribute to the DFIR community. Contributions are welcome!

- 🐛 Bug reports
- 💡 New analysis technique suggestions
- 📝 Documentation improvements

Feel free to open an issue or submit a pull request!

---

## 📈 Statistics

- **Total Analyses:** 11 cases
- **Total IOCs:** 50+ indicators
- **Supported Languages:** English, Turkish
- **Platform:** CyberDefenders Blue Team Labs
- **Last Updated:** December 2025

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).

---

## 📞 Contact

- **LinkedIn:** [linkedin.com/in/hsnco](https://www.linkedin.com/in/hsnco)
- **GitHub:** [@iamhasancoskun](https://github.com/iamhasancoskun)

---

<p align="center">
  <strong>A Note from the Author</strong><br>
  Every case in this portfolio represents real investigation work - from hypothesis formation to evidence collection, IOC extraction, and MITRE mapping. The bilingual documentation took significant effort to ensure accessibility for both Turkish and international DFIR communities.
  <br><br>
  Made with ❤️ by <a href="https://github.com/iamhasancoskun">Hasan Coşkun</a>
  <br><br>
  ⭐ If you found this helpful, star it and share with your network!
</p>
