# 🔍 DFIR Portfolio - Digital Forensics & Incident Response Case Studies

<p align="center">
  <img src="https://img.shields.io/badge/Cases-10-blue?style=for-the-badge" alt="Total Cases"/>
  <img src="https://img.shields.io/badge/Memory%20Forensics-0-green?style=for-the-badge" alt="Memory"/>
  <img src="https://img.shields.io/badge/Network%20Forensics-8-orange?style=for-the-badge" alt="Network"/>
  <img src="https://img.shields.io/badge/Languages-EN%20%7C%20TR-red?style=for-the-badge" alt="Languages"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License"/>
</p>

<p align="center">
  <a href="README.md"><strong>English</strong></a> | <strong>Türkçe</strong>
</p>

---

## 👤 Hakkımda

SOC Analyst ve DFIR tutkunu olarak, blue team operasyonları ve olay müdahalesi konularına odaklanıyorum. CyberDefenders platformundaki gerçek dünya saldırı senaryolarını analiz ederek öğrenme yolculuğumu belgeliyorum.

**Odak Alanlarım:** Memory Forensics | Network Analysis | Threat Intelligence | Malware Analysis  
**Araçlar:** Volatility 3 | Wireshark | MITRE ATT&CK Framework

🔗 [LinkedIn](https://www.linkedin.com/in/hsnco) | 🌐 [GitHub](https://github.com/iamhasancoskun)

> *Not: Bu portföy 100+ saatlik pratik analiz, dokümantasyon ve öğrenme sürecini temsil ediyor. Her vaka, metodolojimi, bulgularımı ve öğrendiklerimi içeriyor.*

---

## 📊 Vaka Kategorileri

| Kategori | Vaka Sayısı | Araçlar | Durum |
|----------|-------------|---------|-------|
| 🧠 **Memory Forensics** | 0 | Volatility 3, Rekall | 📝 Yakında |
| 🌐 **Network Forensics** | 8 | Wireshark, tcpdump, tshark | ✅ Aktif |
| 🔎 **Threat Intelligence** | 1 | VirusTotal, OSINT | ✅ Aktif |
| 🦠 **Malware Analysis** | 1 | IDA, x64dbg, Any.run | ✅ Aktif |
| 🕵️ **OSINT** | 1 | Google Lens, WhatsMyName | ✅ Aktif |
| 📱 **Mobile Forensics** | 1 | ALEAPP, DBeaver | ✅ Aktif |
| 🌐 **Web Application Security** | 1 | Wireshark, Burp Suite | ✅ Aktif |

---

## 🎯 Öne Çıkan Vakalar

### 1. 🏆 [Tomcat Takeover - Web Server Breach Analysis](cases/network-forensics/tomcat-takeover/)
**Tehdit:** Web Shell Deployment | **Teknikler:** Directory Bruteforce, WAR Upload, Reverse Shell  
**Araçlar:** Wireshark, gobuster  
**Highlights:** Saldırının baştan sona PCAP analizi ile ortaya çıkarılması

### 2. 🕵️ [Lespion - Insider Threat OSINT Investigation](cases/osint/lespion/)
**Tehdit:** Insider Threat | **Teknikler:** GitHub Secrets Analysis, Geolocation  
**Araçlar:** Google Lens, GitHub, OSINT Tools  
**Highlights:** Tek bir GitHub profilinden çok katmanlı istihbarat çıkarımı

### 3. 🎯 [PsExec Hunt - Lateral Movement Detection](cases/network-forensics/psexec-hunt/)
**Tehdit:** Lateral Movement | **Teknikler:** PsExec, SMB Analysis  
**Araçlar:** Wireshark, SMB Protocol Analysis  
**Highlights:** Multi-target saldırının ağ trafiğinden tespiti

### 4. 🦠 [Yellow RAT - Threat Intelligence Study](cases/threat-intelligence/yellow-rat-lab/)
**Tehdit:** Yellow Cockatoo RAT | **Teknikler:** Threat Intelligence, IOC Analysis  
**Araçlar:** VirusTotal, Red Canary Reports  
**Highlights:** Tek bir hash'ten tüm saldırı zincirinin çıkarılması

### 5. 📱 [The Crime Lab - Android Forensics](cases/mobile-forensics/the-crime-lab/)
**Tehdit:** Financial Crime Investigation | **Teknikler:** Android Data Extraction, Timeline Analysis  
**Araçlar:** ALEAPP, SQLite, DBeaver  
**Highlights:** Cinayet soruşturması kapsamında mobil forensics

[→ Tüm Vakaları Görüntüle](#tüm-vakalar)

---

## 📚 Tüm Vakalar

### 🌐 Network Forensics
<details>
<summary>8 vaka - Genişletmek için tıklayın</summary>

- **[Tomcat Takeover](cases/network-forensics/tomcat-takeover/)** - Web Server Breach via File Upload
- **[PsExec Hunt](cases/network-forensics/psexec-hunt/)** - Lateral Movement Detection via SMB
- **[DanaBot](cases/network-forensics/danabot/)** - Banking Malware Network Analysis
- **[Poisoned Credentials](cases/network-forensics/poisoned-credentials/)** - NBNS/LLMNR Poisoning Attack
- **[XLM RAT](cases/network-forensics/xlm-rat/)** - PowerShell Obfuscation & AsyncRAT

</details>

### 🕵️ OSINT & Threat Intelligence
<details>
<summary>2 vaka - Genişletmek için tıklayın</summary>

- **[Lespion](cases/osint/lespion/)** - Insider Threat Investigation via GitHub & Social Media
- **[Yellow RAT Lab](cases/threat-intelligence/yellow-rat-lab/)** - Threat Intel Analysis from Hash to IOC

</details>

### 🦠 Malware Analysis
<details>
<summary>1 vaka - Genişletmek için tıklayın</summary>

- **[Oski Lab](cases/malware-analysis/oski-lab/)** - Infostealer Analysis via VirusTotal & Any.run

</details>

### 📱 Mobile Forensics
<details>
<summary>1 vaka - Genişletmek için tıklayın</summary>

- **[The Crime Lab](cases/mobile-forensics/the-crime-lab/)** - Android Forensics in Murder Investigation

</details>

### 🌐 Web Application Security
<details>
<summary>1 vaka - Genişletmek için tıklayın</summary>

- **[WebStrike](cases/web-application-security/webstrike/)** - Web Shell Upload & Reverse Shell Attack

</details>

### 🧠 Memory Forensics
<details>
<summary>Yakında</summary>

- **Amadey Trojan** - Advanced Memory Forensics with Volatility 3
  - *Yakında* - Process Hollowing & PEB Unlinking Analysis

</details>

---

## 🛠️ Kullanılan Araçlar ve Teknolojiler

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

## 📖 Metodoloji

Tüm vaka analizlerimde **"Hipotez Odaklı Yaklaşım"** kullanıyorum:

1. **Hipotez** - Bulgu hakkında bir varsayım oluştur
2. **Test** - Uygun araçlarla hipotezi test et
3. **Kanıt** - Bulguları görsellerle belgele
4. **IOC** - Göstergeleri çıkar ve paylaş
5. **MITRE ATT&CK** - TTPs ile eşleştir

Detaylı metodoloji: [English](docs/METHODOLOGY.md) | [Türkçe](docs/METHODOLOGY.tr.md)

---

## 🎓 Eğitim ve Kaynak

### 📚 **Challenge Kaynağı**

Bu depodaki tüm vakalar, [**CyberDefenders**](https://cyberdefenders.org) platformundaki, özellikle [**SOC Analyst Tier 1 Track**](https://cyberdefenders.org/tracks/soc-analyst-tier-1/) içerisindeki **çözülmüş challenge'lerdir**.

**🔗 CyberDefenders Platformu:**  
- Website: [cyberdefenders.org](https://cyberdefenders.org)
- Track: [SOC Analyst Tier 1](https://cyberdefenders.org/tracks/soc-analyst-tier-1/)
- Lab Türü: Blue Team Hands-On Labs

### ⚖️ **Eğitim Amaçlı Kullanım ve Atıf**

- ✅ **Challenge Dosyaları:** Orijinal forensic artifact'ler (PCAP, memory dump, vb.) CyberDefenders platformunda mevcuttur
- ✅ **Bu Depo İçeriği:** Benim analizim, metodolojim, bulgularım, ekran görüntülerim ve IOC çıkarımlarım
- ✅ **Amaç:** Eğitim dokümantasyonu ve portfolio sergileme
- ⚠️ **Not:** Platform şartlarına saygı göstermek adına, bu depo orijinal challenge dosyalarını YENIDEN DAĞITMAZ

### 📖 **Analiz Özellikleri**

- ✅ **Tekrarlanabilir** - Tüm komutlar ve adımlar paylaşılmıştır
- ✅ **Eğitici** - Öğrenme odaklı, adım adım açıklamalar
- ✅ **İki Dilli** - Türkçe ve İngilizce dokümantasyon
- ✅ **Görsel** - Her adım ekran görüntüleri ile desteklenmiştir

---

## 🤝 Katkıda Bulunma

Bu proje, DFIR topluluğuna katkı sağlamak amacıyla oluşturulmuştur. Katkılarınızı bekliyorum!

- 🐛 Hata bildirimi
- 💡 Yeni analiz teknikleri önerisi
- 📝 Dokümantasyon iyileştirmeleri

Issue açmaktan veya pull request göndermekten çekinmeyin!

---

## 📈 İstatistikler

- **Toplam Analiz:** 10 vaka
- **Toplam IOC:** 70+ gösterge
- **Desteklenen Diller:** English, Türkçe
- **Platform:** CyberDefenders Blue Team Labs
- **Son Güncelleme:** Aralık 2025

---

## 📜 Lisans

Bu proje [MIT Lisansı](LICENSE) altında lisanslanmıştır.

---

## 📞 İletişim

- **LinkedIn:** [linkedin.com/in/hsnco](https://www.linkedin.com/in/hsnco)
- **GitHub:** [@iamhasancoskun](https://github.com/iamhasancoskun)

---

<p align="center">
  <strong>Yazardan Bir Not</strong><br>
  Bu portföydeki her vaka, gerçek bir soruşturma sürecini temsil ediyor - hipotez oluşturmaktan kanıt toplamaya, IOC çıkarımından MITRE haritalamaya kadar. İki dilli dokümantasyon, hem Türk hem de uluslararası DFIR topluluklarına erişilebilir olması için önemli bir emek gerektirdi.
  <br><br>
  Made with ❤️ by <a href="https://github.com/iamhasancoskun">Hasan Coşkun</a>
  <br><br>
  ⭐ Faydalı bulduysanız, yıldızlayın ve ağınızla paylaşın!
</p>

