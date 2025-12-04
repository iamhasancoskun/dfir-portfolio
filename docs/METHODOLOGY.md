# 📖 DFIR Analysis Methodology

This document details the **Hypothesis-Driven Approach** methodology used in all case analyses.

*Bu dokümanda, tüm vaka analizlerimde kullandığım **Hipotez Odaklı Yaklaşım** (Hypothesis-Driven Approach) metodolojisi detaylandırılmıştır.*

---

## 🎓 About CyberDefenders Challenges

All cases in this repository are **solved challenges** from [CyberDefenders](https://cyberdefenders.org) platform, specifically the [SOC Analyst Tier 1 Track](https://cyberdefenders.org/tracks/soc-analyst-tier-1/).

### Challenge Structure

Each CyberDefenders challenge includes:
- **📦 Forensic Artifacts:** Memory dumps, PCAP files, disk images, mobile data, etc.
- **❓ Questions:** Scenario-based questions guiding the investigation
- **🎯 Learning Objectives:** Specific skills and techniques to practice

### My Approach

Rather than just answering questions, I document:
- ✅ **Complete Investigation Process:** How I arrived at each answer
- ✅ **Commands & Tools:** All technical steps with reproducible commands
- ✅ **Visual Evidence:** Screenshots supporting every finding
- ✅ **IOC Extraction:** Indicators discovered during analysis
- ✅ **MITRE ATT&CK Mapping:** Techniques identified in the attack chain

**📌 Note:** Original challenge files (memory dumps, PCAPs, etc.) are available on CyberDefenders platform. This repository contains my analysis, methodology, and findings.

---

## 🎯 Hypothesis-Driven Approach / Hipotez Odaklı Yaklaşım

I follow a five-step process in every case analysis:

*Her vaka analizinde aşağıdaki beş adımlı süreci takip ediyorum:*

### 1. **Hypothesis / Hipotez**

Form a testable assumption about the finding in each analysis phase.

*Her analiz fazında, bulgu hakkında test edilebilir bir varsayım oluşturuyorum.*

**Example / Örnek:**
> "The attacker likely uploaded a malicious file to the server by exploiting a vulnerability discovered during the reconnaissance phase."

> *"Saldırgan, keşif aşamasından sonra bulduğu bir zafiyeti kullanarak sunucuya kötü amaçlı bir dosya yüklemiş olmalıdır."*

### 2. **Testing / Test**

Select appropriate tools and techniques to test the hypothesis.

*Hipotezi test etmek için uygun araç ve teknikleri seçiyorum.*

**Example / Örnek:**
> Traffic was examined with `http.request.method == "POST"` filter and the relevant TCP stream was analyzed.

> *Trafik, `http.request.method == "POST"` filtresi ile incelendi ve ilgili TCP akışı analiz edildi.*

### 3. **Evidence / Kanıt**

Document findings with visuals and technical details.

*Bulguları görsellerle ve teknik detaylarla belgeliyorum.*

**Example / Örnek:**
> ![TCP Stream - File Upload](assets/upload.png)

### 4. **IOC Extraction / IOC Çıkarımı**

Present identified indicators in structured format.

*Tespit edilen göstergeleri yapılandırılmış formatta sunuyorum.*

**Example / Örnek:**

| Type / Tür | Value / Değer |
|------------|---------------|
| Attacker IP / Saldırgan IP | `117.11.88.124` |
| Uploaded File / Yüklenen Dosya | `image.jpg.php` |
| C2 Port / C2 Portu | `8080` |

### 5. **MITRE ATT&CK Mapping / MITRE ATT&CK Eşleştirmesi**

Map identified techniques to MITRE ATT&CK framework.

*Tespit edilen teknikleri MITRE ATT&CK çerçevesi ile eşleştiriyorum.*

**Example / Örnek:**
- **T1190** (Exploit Public-Facing Application) - File upload vulnerability / Dosya yükleme zafiyeti
- **T1059.004** (Unix Shell) - Command execution via reverse shell / Ters kabuk üzerinden komut çalıştırma

---

## 🔍 Analysis Type-Specific Approaches / Analiz Türlerine Göre Yaklaşımlar

### **Memory Forensics**
1. **Profiling** - `windows.info` ile sistem bilgilerini topla
2. **Process Analysis** - `windows.pslist`, `windows.pstree` ile süreç hiyerarşisi
3. **Anti-Forensics Detection** - `windows.cmdline`, `windows.dlllist` ile manipülasyon tespiti
4. **Malware Extraction** - `windows.malware.malfind` ile enjekte kod çıkarımı
5. **Network & Persistence** - `windows.netscan`, `windows.registry.printkey` ile IOC tespiti

### **Network Forensics**
1. **Traffic Overview** - Protocol hierarchy ve conversation analysis
2. **Filtering** - Display filters ile ilgili trafiği izole et
3. **Stream Analysis** - TCP/HTTP stream ile payload incelemesi
4. **Timeline** - Saldırı zincirinin kronolojik yeniden yapılandırılması
5. **IOC Extraction** - IP, port, domain, file hash çıkarımı

### **OSINT Investigation**
1. **Username Enumeration** - Aynı kullanıcı adının farklı platformlarda aranması
2. **Image Analysis** - Google Lens ile tersine görsel arama
3. **Geolocation** - Görsellerden konum bilgisi çıkarma
4. **Cross-Platform Correlation** - Farklı platformlardaki bilgilerin birleştirilmesi
5. **Timeline Construction** - Dijital ayak izlerinin zaman çizelgesi

### **Malware Analysis**
1. **Static Analysis** - Hash, dosya türü, strings analizi
2. **Dynamic Analysis** - Sandbox (Any.run) üzerinde davranış analizi
3. **Deobfuscation** - CyberChef ile obfuscate edilmiş kod temizleme
4. **C2 Detection** - Network artifact'lerinden C2 sunucu tespiti
5. **Family Attribution** - VirusTotal ve threat intel raporları ile aile tespiti

---

## 📊 Documentation Standards / Dokümantasyon Standartları

Each case analysis follows this structure:

*Her vaka analizi aşağıdaki yapıyı takip eder:*

```markdown
# Case Analysis: [Case Name] / Vaka Analizi: [Vaka Adı]

## 🎯 CyberDefenders Challenge Context
- **Challenge Name:** [Name on CyberDefenders]
- **Challenge Link:** [URL to challenge]
- **Category:** [Memory/Network/Mobile/etc.]
- **Difficulty:** [Easy/Medium/Hard]

## 1. Executive Summary / Özet
- 5-7 sentence summary
- Threat family, critical findings, impact

*5-7 cümlelik özet, tehdit ailesi, kritik bulgular, etki*

## 2. Technical Analysis (Hypothesis-Driven) / Teknik Analiz (Hipotez Odaklı)
### Phase 1 - [Title]
- **Question:** [CyberDefenders question]
- **Hypothesis:** ...
- **Command/Filter:** ...
- **Findings:** ...
- ![Visual](assets/...)

## 3. Indicators of Compromise (IOCs) / Göstergeler
| Type / Tür | Value / Değer |
|------------|---------------|

## Appendix A - MITRE ATT&CK Mapping / Ek A - MITRE ATT&CK Eşleştirmesi
- T1XXX.XXX (Technique Name) - Evidence

## Appendix B - Reproducibility / Ek B - Tekrarlanabilirlik
```bash
# Commands
```

---

## 🎯 Quality Standards / Kalite Standartları

Each analysis must meet the following criteria:

*Her analiz aşağıdaki kriterleri karşılamalıdır:*

- ✅ **Reproducible** - Tüm komutlar ve adımlar paylaşılmış
- ✅ **Visual** - Her kritik adım ekran görüntüsü ile desteklenmiş
- ✅ **Technical** - Teknik detaylar ve açıklamalar net
- ✅ **Structured** - Tutarlı format ve organizasyon
- ✅ **Bilingual** - Türkçe ve İngilizce dokümantasyon

---

## 🔗 Resources / Kaynaklar

- **CyberDefenders Platform:** https://cyberdefenders.org/
- **SOC Analyst Tier 1 Track:** https://cyberdefenders.org/tracks/soc-analyst-tier-1/
- **MITRE ATT&CK:** https://attack.mitre.org/
- **Volatility 3 Documentation:** https://volatility3.readthedocs.io/
- **Wireshark User Guide:** https://www.wireshark.org/docs/

---

*Last Updated: December 2025 / Son Güncelleme: Aralık 2025*

