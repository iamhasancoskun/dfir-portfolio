English ([README.en.md](README.en.md)) | Türkçe (bu sayfa)

# Vaka Analizi: XLM-RAT - Network Forensics ve Malware Analizi

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Network Forensics](https://img.shields.io/badge/Network-Forensics-blue)
![Platform](https://img.shields.io/badge/Platform-Windows-lightgrey)
[![Author: hasancoskun](https://img.shields.io/badge/Author-hasancoskun-black)](https://github.com/hasancoskun)

| Kategori | Açıklama |
| :--- | :--- |
| **Odak Alanı** | Network Forensics, Malware Analizi, PowerShell Obfuscation |
| **Delil** | `236-XLMRat.pcap` (Network Capture) |
| **Araçlar** | Wireshark, CyberChef, VirusTotal, Hosting Checker |

---

## 1. Özet (Executive Summary)

Bu vaka, şüpheli ağ trafiği nedeniyle işaretlenen bir makinenin analizini içermektedir. Saldırgan, PowerShell betikleri kullanarak XLM-RAT (AsyncRAT) ailesinden kötü amaçlı yazılımı hedef sisteme indirmiş ve çalıştırmıştır. Saldırı, `45.126.209.4:222` adresinden `mdm.jpg` dosyası olarak gizlenmiş PowerShell betiğinin indirilmesiyle başlamış, ardından obfuscate edilmiş kod ile ikincil payload'ı çalıştırmıştır. Analiz sonucunda, saldırganın LOLBin teknikleri kullanarak `RegSvcs.exe` ile gizli süreç yürütme gerçekleştirdiği ve sistemde kalıcılık sağlamak için birden fazla dosya bıraktığı tespit edilmiştir.

---

## 2. Teknik Analiz (Hipotez Odaklı)

Her fazda hipotez → test → bulgu; görseller `assets/` altında.

### Faz 1 — İlk Aşama Malware İndirme Analizi (Q1)
- **Hipotez**: Saldırgan, HTTP trafiği üzerinden ilk aşama kötü amaçlı yazılımı indirmiş olmalı
- **Analiz**: Wireshark'ta HTTP trafiği filtrelenerek `GET` istekleri incelendi
- **Bulgular**: 
  - **Kaynak IP**: `10.1.9.101` (Kurban makinesi)
  - **Hedef IP**: `45.126.209.4:222`
  - **İndirilen Dosya**: `mdm.jpg` (JPEG olarak gizlenmiş PowerShell betiği)
  - **Tam URL**: `http://45.126.209.4:222/mdm.jpg`

![HTTP GET İsteği](assets/q1.1.png)
![HTTP Yanıt ve PowerShell İçeriği](assets/q1.2.png)

### Faz 2 — Hosting Sağlayıcısı Tespiti (Q2)
- **Hipotez**: Saldırganın kullandığı IP adresi, belirli bir hosting sağlayıcısına ait olmalı
- **Analiz**: IP adresi `45.126.209.4` için WHOIS sorgulaması yapıldı
- **Bulgular**: 
  - **Hosting Sağlayıcısı**: `ReliableSite.Net LLC`
  - **Lokasyon**: Miami, ABD
  - **AS Number**: AS23470

![Hosting Sağlayıcısı Analizi](assets/q2.png)

### Faz 3 — Malware Executable Hash Analizi (Q3)
- **Hipotez**: PowerShell betiği içinde hex formatında gömülü ikincil payload bulunmalı
- **Analiz**: `$hexString_bbb` değişkenindeki hex verisi çıkarıldı ve CyberChef ile işlendi
- **Çözüm Yolu**:
  1. **Hex Verisi Çıkarma**: PowerShell betiğinden `$hexString_bbb` değişkeninin değeri kopyalandı
  2. **CyberChef Recipe Oluşturma**:
     - **Find/Replace**: `-` karakterlerini kaldırmak için (Global match, REGEX)
     - **From Hex**: Hex string'i binary formata dönüştürmek için (Delimiter: Auto)
     - **SHA2**: SHA256 hash hesaplamak için (Size: 256, Rounds: 64)
  3. **Anti-Forensics Tespiti**: `$hexString_bbb` değişkeni seçilerek PE dosyası yerine "bb" dosyası olarak gizlenmiş
- **Bulgular**: 
  - **SHA-256**: `1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798`
  - **Dosya Tipi**: PE32 Executable (.NET Assembly)
  - **Boyut**: 65.00 KB
  - **Anti-Forensics**: Dosya adı "bb" olarak gizlenmiş (PE yerine)

![PE Dosyası Tespiti](assets/q3.1-_PE-fake-for-anti-forensics.png)
![CyberChef ile Hash Hesaplama](assets/q3.2-after-deleting-all-content-except-from-$hexString_bbb.png)

### Faz 4 — Malware Ailesi Tanımlama (Q4)
- **Hipotez**: Obfuscate edilmiş PowerShell kodları, belirli bir malware ailesinin imzalarını taşımalı
- **Analiz**: VirusTotal analizi ve PowerShell kod deobfuscation işlemi
- **Çözüm Yolu**:
  1. **VirusTotal Analizi**: SHA256 hash'i VirusTotal'a yüklenerek detaylı analiz yapıldı
  2. **PowerShell Deobfuscation**: CyberChef ile obfuscate edilmiş string'ler temizlendi
     - **Find/Replace**: `#` karakterlerini kaldırmak için (Global match, Multiline matching)
     - **Sonuç**: `N#ew#PE#2.P#E` → `NewPE2.PE`, `L#o#a#d` → `Load`
  3. **Malware Ailesi Tespiti**: Alibaba'nın `Backdoor:MSIL/AsyncRat.a2786761` etiketi ile AsyncRAT ailesi doğrulandı
- **Bulgular**: 
  - **Malware Ailesi**: `AsyncRAT` (Alibaba etiketi: `Backdoor:MSIL/AsyncRat.a2786761`)
  - **Obfuscated Strings**: `N#ew#PE#2.P#E` → `NewPE2.PE`, `L#o#a#d` → `Load`
  - **Tespit Oranı**: 56/70 güvenlik satıcısı
  - **Obfuscation Tekniği**: `#` karakteri ile string masking

![VirusTotal Analizi](assets/q4.png)
![PowerShell Deobfuscation](assets/q4.1-load.png)

### Faz 5 — Malware Oluşturma Zaman Damgası (Q5)
- **Hipotez**: HTTP yanıt başlıkları, dosyanın oluşturulma zamanını içermeli
- **Analiz**: HTTP yanıt paketlerindeki `Date` başlığı incelendi
- **Bulgular**: 
  - **Oluşturma Zamanı**: `2023-10-30 15:08:44 UTC` (VirusTotal'dan)
  - **İndirme Zamanı**: `Tue, 09 Jan 2024 17:27:29 GMT` (HTTP yanıtından)

![Zaman Damgası Analizi](assets/q5.png)

### Faz 6 — LOLBin Kullanımı Analizi (Q6)
- **Hipotez**: PowerShell betiği, gizli süreç yürütme için yasal Windows araçlarını kötüye kullanmalı
- **Analiz**: Obfuscate edilmiş string'lerin deobfuscation işlemi
- **Çözüm Yolu**:
  1. **PowerShell Kod İncelemesi**: Betikte obfuscate edilmiş dosya yolu string'leri tespit edildi
  2. **CyberChef ile Deobfuscation**:
     - **Find/Replace**: `#` karakterlerini kaldırmak için (Global match, Multiline matching)
     - **Sonuç**: `C:\W#######indow############s\Mi####cr` → `C:\Windows\Micr`
     - **İkinci Kısım**: `osof#####t.NET\Fra###mework\v4.0.303###19\R##egSvc#####s.exe` → `osoft.NET\Framework\v4.0.30319\RegSvcs.exe`
  3. **String Concatenation**: İki kısım birleştirilerek tam yol oluşturuldu
- **Bulgular**: 
  - **LOLBin**: `RegSvcs.exe`
  - **Tam Yol**: `C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe`
  - **Kullanım Amacı**: .NET derlemelerini kaydetmek için kullanılan yasal araç
  - **Anti-Forensics**: Dosya yolu obfuscate edilmiş ve parçalara bölünmüş

![LOLBin Deobfuscation](assets/q6.png)

### Faz 7 — Dropped Files Analizi (Q7)
- **Hipotez**: PowerShell betiği, kalıcılık sağlamak için birden fazla dosya oluşturmalı
- **Analiz**: `[IO.File]::WriteAllText` komutlarının analizi
- **Çözüm Yolu**:
  1. **HTTP Stream Analizi**: Wireshark'ta `Follow HTTP Stream` ile PowerShell betiğinin tam içeriği incelendi
  2. **WriteAllText Komutları Tespiti**: Betikte üç farklı `[IO.File]::WriteAllText` komutu bulundu
  3. **Dosya Adları Çıkarma**: Her komuttan dosya adları ve içerikleri çıkarıldı
  4. **Dosya Türleri Analizi**: Her dosyanın farklı bir script türü olduğu tespit edildi
- **Bulgular**: 
  - **Dropped Files**: `Conted.ps1`, `Conted.bat`, `Conted.vbs`
  - **Hedef Dizin**: `C:\Users\Public\`
  - **Amaç**: Kalıcılık ve zincirleme çalıştırma
  - **Anti-Forensics**: Dosya adları "Conted" olarak obfuscate edilmiş (Content yerine)

![Dropped Files Analizi](assets/q7.png)

---

## 3. Soruşturma Sonuçları

### Q1: İlk Aşama Malware URL'i
**Cevap**: `http://45.126.209.4:222/mdm.jpg`

**Analiz**: Wireshark'ta HTTP trafiği incelendiğinde, kaynak IP `10.1.9.101`'den hedef IP `45.126.209.4:222`'ye bir `GET /mdm.jpg` isteği gönderildiği görüldü. Yanıt paketi incelendiğinde, bu isteğin `200 OK` yanıtı aldığı ve içeriğinin PowerShell betiği olduğu belirlendi.

### Q2: Hosting Sağlayıcısı
**Cevap**: `ReliableSite.Net`

**Analiz**: IP adresi `45.126.209.4` için yapılan WHOIS sorgulaması, IP adresinin `ReliableSite.Net LLC`'ye ait olduğunu ortaya çıkardı.

### Q3: Malware Executable SHA256
**Cevap**: `1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798`

**Analiz**: PCAP'tan çıkarılan PowerShell betiğindeki `$hexString_bbb` değişkenindeki hex verisi, CyberChef ile işlenerek SHA256 hash değeri elde edildi.

### Q4: Malware Ailesi (Alibaba)
**Cevap**: `AsyncRAT`

**Analiz**: VirusTotal analizi ve PowerShell kod deobfuscation işlemi sonucunda, malware'in AsyncRAT ailesinden olduğu tespit edildi.

### Q5: Malware Oluşturma Zaman Damgası
**Cevap**: `2023-10-30 15:08`

**Analiz**: VirusTotal'daki dosya detaylarından alınan `Creation Time` değeri kullanıldı.

### Q6: Kullanılan LOLBin
**Cevap**: `C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe`

**Analiz**: PowerShell betiğindeki obfuscate edilmiş string'lerin deobfuscation işlemi sonucunda, `RegSvcs.exe` dosyasının kullanıldığı tespit edildi.

### Q7: Dropped Files
**Cevap**: `Conted.ps1,Conted.bat,Conted.vbs`

**Analiz**: PowerShell betiğindeki `[IO.File]::WriteAllText` komutları incelendiğinde, üç farklı dosyanın `C:\Users\Public\` dizinine yazıldığı görüldü.

---

## 4. Kritik Bulgular ve IOCs

| Kategori | Değer |
|---|---|
| **Saldırgan IP** | 45.126.209.4:222 |
| **Kurban IP** | 10.1.9.101 |
| **Hosting Sağlayıcısı** | ReliableSite.Net LLC |
| **Malware Ailesi** | AsyncRAT (XLM-RAT) |
| **SHA-256** | 1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798 |
| **LOLBin** | C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe |
| **Dropped Files** | Conted.ps1, Conted.bat, Conted.vbs |
| **Obfuscation Tekniği** | String replacement (# karakteri) |

---

## 5. Sonuç ve Değerlendirme

Bu network forensics analizi, XLM-RAT (AsyncRAT) ailesinden kötü amaçlı yazılımın sofistike bir şekilde dağıtıldığını ortaya çıkarmıştır. Saldırgan, PowerShell obfuscation teknikleri kullanarak kodlarını gizlemiş ve yasal Windows araçlarını (LOLBin) kötüye kullanarak tespit edilmesini zorlaştırmıştır. 

**Önemli Bulgular:**
- JPEG dosyası olarak gizlenmiş PowerShell betiği
- Hex formatında gömülü ikincil payload
- RegSvcs.exe kullanılarak gizli süreç yürütme
- Çoklu dosya bırakma stratejisi ile kalıcılık

**Güvenlik Önerileri:**
- PowerShell execution policy'lerinin sıkılaştırılması
- LOLBin kullanımının izlenmesi
- Suspicious file extensions'ların (JPEG içinde script) tespiti
- Network trafiğinde hex payload'ların analizi

Bu vaka, modern malware'lerin ne kadar sofistike anti-forensics teknikleri kullandığını ve network forensics analizinin kritik önemini göstermektedir.