English ([README.en.md](README.en.md)) | Türkçe (bu sayfa)

# Vaka Analizi: DanaBot - Network Forensics Analizi

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Wireshark](https://img.shields.io/badge/Wireshark-4.0-blue)
![Platform](https://img.shields.io/badge/Platform-Network-lightgrey)
[![Author: hasancoskun](https://img.shields.io/badge/Author-hasancoskun-black)](https://github.com/hasancoskun)

| Kategori | Açıklama |
| :--- | :--- |
| **Odak Alanı** | Network Forensics, Malware Analizi, PCAP Analizi |
| **Delil** | `205-DanaBot.pcap` (Network Capture) |
| **Araçlar** | Wireshark, Online Hash Calculator, Google Search |

---

## 1. Özet (Executive Summary)

Bu vaka, SOC ekibinin tespit ettiği şüpheli ağ trafiği analizini içermektedir. Kurban makinesi (10.2.14.101) saldırgan sunucusuna (62.173.142.148) bağlanarak kötü amaçlı dosyalar indirmiştir. İlk olarak `allegato_708.js` JavaScript dosyası indirilmiş ve `wscript.exe` ile çalıştırılmıştır. Ardından `resources.dll` dosyası indirilerek sistem ele geçirilmiştir. Saldırı, HTTP protokolü üzerinden gerçekleştirilmiş ve TLS şifreli iletişim ile C2 sunucusuna bağlantı kurulmuştur. Wireshark ile PCAP analizi yapılarak tüm saldırı adımları tespit edilmiş ve kötü amaçlı dosyaların hash değerleri hesaplanmıştır.

---

## 2. Teknik Analiz (Hipotez Odaklı)
Her fazda hipotez → test → bulgu; görseller `Assets/` altında.

### Faz 1 — Initial Access Analizi (Q1)
- **Hipotez**: İlk erişim için IP adresini bulursak saldırganın IP adresine ulaşmış oluruz
- **Analiz**: PCAP dosyasında DNS sorguları ve HTTP trafiği incelendi
- **Bulgular**: 
  - **DNS Sorgusu**: `portfolio.serveirc.com` → `62.173.142.148`
  - **Saldırgan IP**: `62.173.142.148`
  - **Kurban IP**: `10.2.14.101`

![DNS Sorgusu ve IP Tespiti](Assets/q1.png)

### Faz 2 — Kötü Amaçlı Dosya Tespiti (Q2)
- **Hipotez**: İlk erişim sırasında kötü amaçlı dosya indirilmiş olmalı
- **Analiz**: HTTP response'ları ve Content-Disposition header'ları incelendi
- **Bulgular**:
  - **Dosya Adı**: `allegato_708.js`
  - **Content-Type**: `application/octet-stream`
  - **Content-Disposition**: `attachment;filename=allegato_708.js`

![Kötü Amaçlı Dosya Tespiti](Assets/q2.png)

### Faz 3 — Dosya Hash Analizi (Q3)
- **Hipotez**: Kötü amaçlı dosyanın SHA-256 hash değeri hesaplanabilir
- **Analiz**: Wireshark'tan dosya export edildi ve online hash hesaplayıcı kullanıldı
- **Bulgular**:
  - **SHA-256**: `847b4ad90b1daba2d9117a8e05776f3f902dda593fb1252289538acf476c4268`
  - **Dosya Boyutu**: 5558 bytes

![SHA-256 Hash Hesaplama](Assets/q3.1.png)
![Online Hash Calculator](Assets/q3.2.png)

### Faz 4 — Process Analizi (Q4)
- **Hipotez**: JavaScript dosyası belirli bir process ile çalıştırılmış olmalı
- **Analiz**: Network trafiği ve port analizi yapıldı
- **Bulgular**:
  - **Process**: `wscript.exe` (Windows Script Host)
  - **Sebep**: Windows'ta .js dosyaları default olarak wscript.exe ile çalıştırılır
  - **Port Analizi**: Port 49788'de sürekli veri transferi

![Process Tespiti](Assets/q4.png)

### Faz 5 — İkinci Kötü Amaçlı Dosya Tespiti (Q5)
- **Hipotez**: Saldırganın ikinci kötü amaçlı dosyasını HTTP isteklerinde bulabiliriz
- **Analiz**: HTTP trafiği filtre edilerek tüm dosya transferleri incelendi
- **Bulgular**:
  - **Dosya Adı**: `resources.dll`
  - **Dosya Uzantısı**: `.dll`
  - **Hedef IP**: `188.114.97.3`

![İkinci Kötü Amaçlı Dosya](Assets/q5.png)

### Faz 6 — İkinci Dosya Hash Analizi (Q6)
- **Hipotez**: File sekmesinden Export edilen İkinci kötü amaçlı dosyanın MD5 hash değeri hesaplanabilir
- **Analiz**: Online hash hesaplayıcı kullanılarak MD5 hash hesaplandı
- **Bulgular**:
  - **MD5**: `e758e07113016aca55d9eda2b0ffeebe`
  - **Dosya Boyutu**: 11922432 bytes
  - **Dosya Türü**: Dynamic Link Library (DLL)

![MD5 Hash Hesaplama](Assets/q6.png)

---

## 3. Soruşturma Sonuçları

### Q1: Saldırgan IP Adresi
**Cevap**: `62.173.142.148`
- **Domain**: portfolio.serveirc.com
- **Tespit Yöntemi**: DNS sorgusu analizi

### Q2: İlk Kötü Amaçlı Dosya
**Cevap**: `allegato_708.js`
- **Content-Type**: application/octet-stream
- **Content-Disposition**: attachment;filename=allegato_708.js
- **Dosya Boyutu**: 5558 bytes

### Q3: İlk Dosyanın SHA-256 Hash'i
**Cevap**: `847b4ad90b1daba2d9117a8e05776f3f902dda593fb1252289538acf476c4268`
- **Hesaplama Yöntemi**: Online hash calculator
- **Dosya**: allegato_708.js

### Q4: Kötü Amaçlı Dosyayı Çalıştıran Process
**Cevap**: `wscript.exe`
- **Sebep**: Windows'ta .js dosyaları default olarak wscript.exe ile çalıştırılır
- **Tespit Yöntemi**: Port analizi ve dosya uzantısı analizi

### Q5: İkinci Kötü Amaçlı Dosyanın Uzantısı
**Cevap**: `.dll`
- **Dosya Adı**: resources.dll
- **Dosya Türü**: Dynamic Link Library
- **Hedef IP**: 188.114.97.3

### Q6: İkinci Dosyanın MD5 Hash'i
**Cevap**: `e758e07113016aca55d9eda2b0ffeebe`
- **Hesaplama Yöntemi**: Online hash calculator
- **Dosya**: resources.dll
- **Dosya Boyutu**: 11922432 bytes

---

## 4. Kritik Bulgular ve IOCs

| Kategori | Değer |
|---|---|
| **Saldırgan IP** | 62.173.142.148 |
| **Kurban IP** | 10.2.14.101 |
| **Domain** | portfolio.serveirc.com |
| **İlk Kötü Amaçlı Dosya** | allegato_708.js |
| **İlk Dosya SHA-256** | 847b4ad90b1daba2d9117a8e05776f3f902dda593fb1252289538acf476c4268 |
| **Process** | wscript.exe |
| **İkinci Kötü Amaçlı Dosya** | resources.dll |
| **İkinci Dosya MD5** | e758e07113016aca55d9eda2b0ffeebe |
| **Protokol** | HTTP/HTTPS |
| **Port** | 80, 443 |

---

## 5. Sonuç ve Değerlendirme

Bu network forensics analizi, DanaBot malware ailesinin saldırı metodolojisini başarıyla ortaya çıkarmıştır. Saldırgan, kurban makinesini sahte bir web sitesi üzerinden kandırarak kötü amaçlı dosyalar indirmesini sağlamıştır. İlk olarak JavaScript dosyası (`allegato_708.js`) indirilmiş ve Windows Script Host (`wscript.exe`) ile çalıştırılmıştır. Ardından daha büyük bir DLL dosyası (`resources.dll`) indirilerek sistem ele geçirilmiştir. 

Saldırı, HTTP protokolü üzerinden gerçekleştirilmiş ve TLS şifreli iletişim ile C2 sunucusuna bağlantı kurulmuştur. Wireshark ile PCAP analizi yapılarak tüm saldırı adımları tespit edilmiş ve kötü amaçlı dosyaların hash değerleri hesaplanmıştır. Bu analiz, benzer saldırıların tespiti ve önlenmesi için kritik bilgiler sağlamaktadır.

