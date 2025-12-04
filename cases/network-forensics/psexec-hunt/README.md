English ([README.en.md](README.en.md)) | Türkçe (bu sayfa)

# Vaka Analizi: PsExec Hunt

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Wireshark](https://img.shields.io/badge/Wireshark-Network%20Forensics-blue)
![Platform](https://img.shields.io/badge/Platform-Windows%20Network-lightgrey)
[![Author: DFIR Analyst](https://img.shields.io/badge/Author-DFIR%20Analyst-black)](https://github.com/dfir-analyst)

| Kategori | Açıklama |
| :--- | :--- |
| **Odak Alanı** | Network Forensics, Lateral Movement Analysis, PsExec Investigation |
| **Delil** | `psexec-hunt.pcapng` (Network Capture, 40,294 packets) |
| **Araçlar** | Wireshark, SMB/SMB2 Protocol Analysis, NTLM Authentication Analysis |
| **Seviye** | Easy |
| **Süre** | 30 minutes |
| **Kategori** | Network Forensics |
| **Taktikler** | Execution, Defense Evasion, Discovery, Lateral Movement |

---

## 1. Özet (Executive Summary)

Bu vaka analizi, bir kurumsal ağda gerçekleşen **PsExec lateral movement** saldırısının network forensics incelemesini kapsamaktadır. Saldırgan, **10.0.0.130** IP adresinden başlayarak **SALES-PC (10.0.0.133)** ve **MARKETING-PC (10.0.0.131)** makinelerine sırasıyla erişim sağlamıştır. **ssales** kullanıcı hesabını kullanarak SMB protokolü üzerinden **PSEXESVC.exe** service executable'ını kurmuş ve uzaktan komut çalıştırma yetkisi elde etmiştir. Saldırı, **ADMIN$** ve **IPC$** network share'larını kullanarak gerçekleştirilmiş ve toplam **40,294 paket** içeren PCAP dosyasında tespit edilmiştir. Bu lateral movement aktivitesi, kurumsal ağda **multi-target** saldırının kanıtıdır ve **privilege escalation** riskini ortaya koymaktadır.

---

## 2. Teknik Analiz (Network Forensics Odaklı)
Her fazda hipotez → test → bulgu; görseller aşağıda yer almaktadır.

### Faz 1 — Saldırganın İlk Erişim Noktası Tespiti
- **Hipotez:** Saldırganın ilk erişim IP adresini SMB Negotiate Protocol Request paketlerinden tespit edebiliriz
- **Filtre:** `smb || smb2`
- **Bulgular:** 10.0.0.130 IP adresi, SMB Negotiate Protocol Request gönderen makine olarak tespit edildi

![Q1 - Saldırganın İlk Erişim IP'si](assets/q1.png)

**Analiz:** Packet 126'da görüldüğü üzere, 10.0.0.130 IP adresinden 10.0.0.133 IP adresine "SMB Negotiate Protocol Request" gönderilmiştir. PsExec lateral movement'te, saldırgan ilk bağlantıyı kuran taraftır.

### Faz 2 — İlk Hedef Makinenin Hostname Tespiti
- **Hipotez:** NTLMSSP_CHALLENGE response paketlerinde hedef makinenin hostname'i bulunur
- **Filtre:** `smb || smb2` (mevcut filtreyi kullanarak)
- **Bulgular:** SALES-PC hostname'i, NTLMSSP_CHALLENGE response'unda Target Name field'ında tespit edildi

![Q2 - İlk Hedef Makine Hostname'i](assets/q2.png)

**Analiz:** Packet 131'de görüldüğü üzere, NTLMSSP_CHALLENGE response'unda "Target Name: SALES-PC" bilgisi yer almaktadır. Bu, 10.0.0.133 IP adresine sahip makinenin hostname'idir.

### Faz 3 — Saldırganın Kimlik Doğrulama Hesabı Tespiti
- **Hipotez:** NTLMSSP_AUTHENTICATE paketlerinde saldırganın kullandığı kullanıcı adı bulunur
- **Filtre:** `ip.addr == 10.0.0.130 and ip.addr == 10.0.0.133 and (smb or smb2)`
- **Bulgular:** ssales kullanıcı adı, NTLMSSP_AUTHENTICATE paketinde User name field'ında tespit edildi

![Q3 - Saldırganın Kullandığı Username](assets/q3.png)

**Analiz:** Packet 132'de görüldüğü üzere, NTLMSSP_AUTHENTICATE request'inde "User name: ssales" bilgisi yer almaktadır. Bu, saldırganın kimlik doğrulama için kullandığı hesaptır.

### Faz 4 — Service Executable Adı Tespiti
- **Hipotez:** SMB2 Create Request paketlerinde PsExec'in kurduğu service executable adı bulunur
- **Filtre:** `ip.addr == 10.0.0.130 and ip.addr == 10.0.0.133 and (smb or smb2)`
- **Bulgular:** PSEXESVC.exe dosyası, SMB2 Create Request paketlerinde tespit edildi

![Q4 - Service Executable Adı](assets/q4.png)

**Analiz:** Packet 144'te görüldüğü üzere, "Create Request File: PSEXESVC.exe" isteği gönderilmiştir. Bu, PsExec'in hedef makinede kurduğu service executable'ın adıdır.

### Faz 5 — Service Installation için Network Share Tespiti
- **Hipotez:** SMB2 Tree Connect Request paketlerinde service installation için kullanılan share bulunur
- **Filtre:** `ip.addr == 10.0.0.130 and ip.addr == 10.0.0.133 and (smb or smb2)`
- **Bulgular:** ADMIN$ share'ı, service installation için kullanıldı

![Q5 - Service Installation Share'ı](assets/q5.png)

**Analiz:** Packet 138'de görüldüğü üzere, "Tree Connect Request Tree: \\10.0.0.133\ADMIN$" isteği gönderilmiştir. ADMIN$ share'ı, PSEXESVC.exe dosyasını C:\Windows dizinine kopyalamak için kullanılmıştır.

### Faz 6 — Communication için Network Share Tespiti
- **Hipotez:** SMB2 Tree Connect Request paketlerinde communication için kullanılan share bulunur
- **Filtre:** `ip.addr == 10.0.0.130 and ip.addr == 10.0.0.133 and (smb or smb2)`
- **Bulgular:** IPC$ share'ı, communication için kullanıldı

![Q6 - Communication Share'ı](assets/q6.png)

**Analiz:** Packet 134'te görüldüğü üzere, "Tree Connect Request Tree: \\10.0.0.133\IPC$" isteği gönderilmiştir. IPC$ share'ı, service management ve command execution için kullanılmıştır.

### Faz 7 — İkinci Lateral Movement Hedefi Tespiti

#### 7.1 — Yeni Hedef IP Tespiti
- **Hipotez:** İlk pivot trafiğini hariç tutarak yeni hedef IP'leri Host Announcement paketlerinden tespit edebiliriz
- **Filtre:** `(smb or smb2) and not (ip.addr == 10.0.0.130 or ip.addr == 10.0.0.133)`
- **Bulgular:** 10.0.0.131 IP adresi, Host Announcement paketlerinden yeni lateral movement hedefi olarak tespit edildi

![Q7.1 - Yeni Hedef IP Tespiti](assets/q7.1.png)

#### 7.2 — Hedef Makine Hostname Tespiti
- **Hipotez:** Q2'deki gibi NTLMSSP_CHALLENGE response paketlerinde hedef makinenin hostname'ini tespit edebiliriz
- **Filtre:** `ip.addr == 10.0.0.130 and ip.addr == 10.0.0.131 and (smb or smb2)`
- **Bulgular:** MARKETING-PC hostname'i, NTLMSSP_CHALLENGE response'unda Target Name field'ında tespit edildi

![Q7.2 - Hedef Makine Hostname Tespiti](assets/q7.2.png)

**Analiz:** Q7'de iki aşamalı analiz uyguladık. İlk olarak, ilk pivot trafiğini hariç tutarak 10.0.0.131 IP'sini bulduk. İkinci aşamada, Q2 ile aynı yöntemi kullanarak saldırgan (10.0.0.130) ile ikinci hedef makine (10.0.0.131) arasındaki SMB trafiğini analiz ettik ve NTLMSSP_CHALLENGE response paketlerinde "Target Name: MARKETING-PC" bilgisini bulduk. Bu, Q2'deki SALES-PC tespiti ile aynı analiz yöntemidir ve saldırganın ikinci lateral movement hedefini doğrular.

---

## 3. Göstergeler (IOCs)
| Tür | Değer |
|---|---|
| **Saldırgan IP** | `10.0.0.130` |
| **İlk Hedef** | `10.0.0.133` (SALES-PC) |
| **İkinci Hedef** | `10.0.0.131` (MARKETING-PC) |
| **Compromised Account** | `ssales` |
| **Service Executable** | `PSEXESVC.exe` |
| **Network Shares** | `ADMIN$`, `IPC$` |
| **Protokoller** | `SMB2`, `NTLMSSP`, `BROWS` |
| **Port** | `445` (SMB) |
| **Attack Vector** | `PsExec Lateral Movement` |

> **Notlar:**
> - PsExec, Microsoft'un resmi administrative tool'udur (kötü amaçlı değil)
> - PSEXESVC.exe, PsExec'in service component'idir
> - ADMIN$ share'ı C:\Windows dizinine eşlenir
> - IPC$ share'ı inter-process communication için kullanılır

---

## Ek A — MITRE ATT&CK Eşleştirmesi
- **T1021.002 (Remote Services: SMB/Windows Admin Shares)** — PsExec'in ADMIN$ ve IPC$ share'larını kullanması
- **T1055 (Process Injection)** — PSEXESVC.exe service executable'ının hedef makinede çalıştırılması
- **T1021.001 (Remote Services: Remote Desktop Protocol)** — SMB protokolü üzerinden uzaktan erişim
- **T1078.002 (Valid Accounts: Domain Accounts)** — ssales kullanıcı hesabının kullanılması
- **T1021.003 (Remote Services: Distributed Component Object Model)** — DCERPC protokolü kullanımı
- **T1047 (Windows Management Instrumentation)** — Service management ve command execution

