# DFIR Analysis Methodology

**Language:** [English](METHODOLOGY.md) | [Türkçe](METHODOLOGY.tr.md)

This document details the **Hypothesis-Driven Approach** methodology used in all case analyses.

---

## About CyberDefenders Challenges

All cases in this repository are **solved challenges** from [CyberDefenders](https://cyberdefenders.org) platform, specifically the [SOC Analyst Tier 1 Track](https://cyberdefenders.org/tracks/soc-analyst-tier-1/).

### Challenge Structure

Each CyberDefenders challenge includes:

- **Forensic Artifacts:** Memory dumps, PCAP files, disk images, mobile data, etc.
- **Questions:** Scenario-based questions guiding the investigation
- **Learning Objectives:** Specific skills and techniques to practice

### My Approach

Rather than just answering questions, I document:

- **Complete Investigation Process:** How I arrived at each answer
- **Commands & Tools:** All technical steps with reproducible commands
- **Visual Evidence:** Screenshots supporting every finding
- **IOC Extraction:** Indicators discovered during analysis
- **MITRE ATT&CK Mapping:** Techniques identified in the attack chain

**Note:** Original challenge files (memory dumps, PCAPs, etc.) are available on CyberDefenders platform. This repository contains my analysis, methodology, and findings.

---

## Hypothesis-Driven Approach

I follow a five-step process in every case analysis:

### 1. **Hypothesis**

Form a testable assumption about the finding in each analysis phase.

**Example:**
> "The attacker likely uploaded a malicious file to the server by exploiting a vulnerability discovered during the reconnaissance phase."

### 2. **Testing**

Select appropriate tools and techniques to test the hypothesis.

**Example:**
> Traffic was examined with `http.request.method == "POST"` filter and the relevant TCP stream was analyzed.

### 3. **Evidence**

Document findings with visuals and technical details.

**Example:**
> ![TCP Stream - File Upload](assets/upload.png)

### 4. **IOC Extraction**

Present identified indicators in structured format.

**Example:**

| Type | Value |
|------|-------|
| Attacker IP | `117.11.88.124` |
| Uploaded File | `image.jpg.php` |
| C2 Port | `8080` |

### 5. **MITRE ATT&CK Mapping**

Map identified techniques to MITRE ATT&CK framework.

**Example:**
- **T1190** (Exploit Public-Facing Application) - File upload vulnerability
- **T1059.004** (Unix Shell) - Command execution via reverse shell

---

## Analysis Type-Specific Approaches

### **Memory Forensics**
1. **Profiling** - Collect system info with `windows.info`
2. **Process Analysis** - Process hierarchy with `windows.pslist`, `windows.pstree`
3. **Anti-Forensics Detection** - Detect manipulation with `windows.cmdline`, `windows.dlllist`
4. **Malware Extraction** - Extract injected code with `windows.malware.malfind`
5. **Network & Persistence** - IOC detection with `windows.netscan`, `windows.registry.printkey`

### **Network Forensics**
1. **Traffic Overview** - Protocol hierarchy and conversation analysis
2. **Filtering** - Isolate relevant traffic with display filters
3. **Stream Analysis** - Payload examination with TCP/HTTP streams
4. **Timeline** - Chronological attack chain reconstruction
5. **IOC Extraction** - Extract IPs, ports, domains, file hashes

### **OSINT Investigation**
1. **Username Enumeration** - Search username across platforms
2. **Image Analysis** - Reverse image search with Google Lens
3. **Geolocation** - Extract location info from images
4. **Cross-Platform Correlation** - Correlate info from different platforms
5. **Timeline Construction** - Build digital footprint timeline

### **Malware Analysis**
1. **Static Analysis** - Hash, file type, strings analysis
2. **Dynamic Analysis** - Behavior analysis on sandbox (Any.run)
3. **Deobfuscation** - Clean obfuscated code with CyberChef
4. **C2 Detection** - Detect C2 server from network artifacts
5. **Family Attribution** - Identify malware family with VirusTotal & threat intel

---

## Documentation Standards

Each case analysis follows this structure:

```markdown
# Case Analysis: [Case Name]

## CyberDefenders Challenge Context
- **Challenge Name:** [Name on CyberDefenders]
- **Challenge Link:** [URL to challenge]
- **Category:** [Memory/Network/Mobile/etc.]
- **Difficulty:** [Easy/Medium/Hard]

## 1. Executive Summary
- 5-7 sentence summary
- Threat family, critical findings, impact

## 2. Technical Analysis (Hypothesis-Driven)
### Phase 1 - [Title]
- **Question:** [CyberDefenders question]
- **Hypothesis:** ...
- **Command/Filter:** ...
- **Findings:** ...
- ![Visual](assets/...)

## 3. Indicators of Compromise (IOCs)
| Type | Value |
|------|-------|

## Appendix A - MITRE ATT&CK Mapping
- T1XXX.XXX (Technique Name) - Evidence

## Appendix B - Reproducibility
```bash
# Commands
```
```

---

## Quality Standards

Each analysis must meet the following criteria:

- **Reproducible** - All commands and steps are shared
- **Visual** - Every critical step supported with screenshots
- **Technical** - Technical details and explanations are clear
- **Structured** - Consistent format and organization
- **Bilingual** - Turkish and English documentation

---

## Resources

- **CyberDefenders Platform:** https://cyberdefenders.org/
- **SOC Analyst Tier 1 Track:** https://cyberdefenders.org/tracks/soc-analyst-tier-1/
- **MITRE ATT&CK:** https://attack.mitre.org/
- **Volatility 3 Documentation:** https://volatility3.readthedocs.io/
- **Wireshark User Guide:** https://www.wireshark.org/docs/

---

*Last Updated: December 2025*
