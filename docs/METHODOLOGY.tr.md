# DFIR Analiz Metodolojisi

**Dil:** [English](METHODOLOGY.md) | [Türkçe](METHODOLOGY.tr.md)

Bu dokümanda, tüm vaka analizlerimde kullandığım **Hipotez Odaklı Yaklaşım** (Hypothesis-Driven Approach) metodolojisi detaylandırılmıştır.

---

## CyberDefenders Challenge'ler Hakkında

Bu depodaki tüm vakalar, [CyberDefenders](https://cyberdefenders.org) platformundaki, özellikle [SOC Analyst Tier 1 Track](https://cyberdefenders.org/tracks/soc-analyst-tier-1/) içerisindeki **çözülmüş challenge'lerdir**.

### Challenge Yapısı

Her CyberDefenders challenge şunları içerir:

- **Forensic Artifact'ler:** Memory dump'lar, PCAP dosyaları, disk imajları, mobil veriler vb.
- **Sorular:** Soruşturmayı yönlendiren senaryo bazlı sorular
- **Öğrenme Hedefleri:** Pratik yapılacak spesifik beceriler ve teknikler

### Yaklaşımım

Sadece soruları cevaplamak yerine, şunları belgeliyorum:

- **Tam Soruşturma Süreci:** Her cevaba nasıl ulaştığım
- **Komutlar ve Araçlar:** Tekrarlanabilir komutlarla tüm teknik adımlar
- **Görsel Kanıtlar:** Her bulguyu destekleyen ekran görüntüleri
- **IOC Çıkarımı:** Analiz sırasında keşfedilen göstergeler
- **MITRE ATT&CK Haritalama:** Saldırı zincirinde tespit edilen teknikler

**Not:** Orijinal challenge dosyaları (memory dump'lar, PCAP'ler vb.) CyberDefenders platformunda mevcuttur. Bu depo, benim analizimi, metodolojimi ve bulgularımı içerir.

---

## Hipotez Odaklı Yaklaşım

Her vaka analizinde aşağıdaki beş adımlı süreci takip ediyorum:

### 1. **Hipotez**

Her analiz fazında, bulgu hakkında test edilebilir bir varsayım oluşturuyorum.

**Örnek:**
> "Saldırgan, keşif aşamasından sonra bulduğu bir zafiyeti kullanarak sunucuya kötü amaçlı bir dosya yüklemiş olmalıdır."

### 2. **Test**

Hipotezi test etmek için uygun araç ve teknikleri seçiyorum.

**Örnek:**
> Trafik, `http.request.method == "POST"` filtresi ile incelendi ve ilgili TCP akışı analiz edildi.

### 3. **Kanıt**

Bulguları görsellerle ve teknik detaylarla belgeliyorum.

**Örnek:**
> ![TCP Stream - Dosya Yükleme](assets/upload.png)

### 4. **IOC Çıkarımı**

Tespit edilen göstergeleri yapılandırılmış formatta sunuyorum.

**Örnek:**

| Tür | Değer |
|-----|-------|
| Saldırgan IP | `117.11.88.124` |
| Yüklenen Dosya | `image.jpg.php` |
| C2 Portu | `8080` |

### 5. **MITRE ATT&CK Eşleştirmesi**

Tespit edilen teknikleri MITRE ATT&CK çerçevesi ile eşleştiriyorum.

**Örnek:**
- **T1190** (Exploit Public-Facing Application) - Dosya yükleme zafiyeti
- **T1059.004** (Unix Shell) - Ters kabuk üzerinden komut çalıştırma

---

## Analiz Türlerine Göre Yaklaşımlar

### **Memory Forensics**
1. **Profiling** - `windows.info` ile sistem bilgilerini topla
2. **Process Analysis** - `windows.pslist`, `windows.pstree` ile süreç hiyerarşisi
3. **Anti-Forensics Detection** - `windows.cmdline`, `windows.dlllist` ile manipülasyon tespiti
4. **Malware Extraction** - `windows.malware.malfind` ile enjekte kod çıkarımı
5. **Network & Persistence** - `windows.netscan`, `windows.registry.printkey` ile IOC tespiti

### **Network Forensics**
1. **Traffic Overview** - Protokol hiyerarşisi ve konuşma analizi
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

## Dokümantasyon Standartları

Her vaka analizi aşağıdaki yapıyı takip eder:

```markdown
# Vaka Analizi: [Vaka Adı]

## CyberDefenders Challenge Bağlamı
- **Challenge Adı:** [CyberDefenders'taki adı]
- **Challenge Linki:** [Challenge URL'si]
- **Kategori:** [Memory/Network/Mobile/vb.]
- **Zorluk:** [Kolay/Orta/Zor]

## 1. Özet
- 5-7 cümlelik özet
- Tehdit ailesi, kritik bulgular, etki

## 2. Teknik Analiz (Hipotez Odaklı)
### Faz 1 - [Başlık]
- **Soru:** [CyberDefenders sorusu]
- **Hipotez:** ...
- **Komut/Filtre:** ...
- **Bulgular:** ...
- ![Görsel](assets/...)

## 3. Göstergeler (IOCs)
| Tür | Değer |
|-----|-------|

## Ek A - MITRE ATT&CK Eşleştirmesi
- T1XXX.XXX (Teknik Adı) - Kanıt

## Ek B - Tekrarlanabilirlik
```bash
# Komutlar
```
```

---

## Kalite Standartları

Her analiz aşağıdaki kriterleri karşılamalıdır:

- **Tekrarlanabilir** - Tüm komutlar ve adımlar paylaşılmış
- **Görsel** - Her kritik adım ekran görüntüsü ile desteklenmiş
- **Teknik** - Teknik detaylar ve açıklamalar net
- **Yapılandırılmış** - Tutarlı format ve organizasyon
- **İki Dilli** - Türkçe ve İngilizce dokümantasyon

---

## Kaynaklar

- **CyberDefenders Platformu:** https://cyberdefenders.org/
- **SOC Analyst Tier 1 Track:** https://cyberdefenders.org/tracks/soc-analyst-tier-1/
- **MITRE ATT&CK:** https://attack.mitre.org/
- **Volatility 3 Dokümantasyonu:** https://volatility3.readthedocs.io/
- **Wireshark Kullanıcı Rehberi:** https://www.wireshark.org/docs/

---

*Son Güncelleme: Aralık 2025*

