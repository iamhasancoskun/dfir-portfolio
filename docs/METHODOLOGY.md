# 📖 DFIR Analiz Metodolojisi

Bu dokümanda, tüm vaka analizlerimde kullandığım **Hipotez Odaklı Yaklaşım** (Hypothesis-Driven Approach) metodolojisi detaylandırılmıştır.

---

## 🎯 Hipotez Odaklı Yaklaşım

Her vaka analizinde aşağıdaki beş adımlı süreci takip ediyorum:

### 1. **Hipotez (Hypothesis)**
```
Her analiz fazında, bulgu hakkında test edilebilir bir varsayım oluşturuyorum.
```

**Örnek:**
> "Saldırgan, keşif aşamasından sonra bulduğu bir zafiyeti kullanarak sunucuya kötü amaçlı bir dosya yüklemiş olmalıdır."

### 2. **Test (Testing)**
```
Hipotezi test etmek için uygun araç ve teknikleri seçiyorum.
```

**Örnek:**
> Trafik, `http.request.method == "POST"` filtresi ile incelendi ve ilgili TCP akışı analiz edildi.

### 3. **Kanıt (Evidence)**
```
Bulguları görsellerle ve teknik detaylarla belgeliyorum.
```

**Örnek:**
> ![TCP Stream - Dosya Yükleme](assets/upload.png)

### 4. **IOC Çıkarımı (Indicators of Compromise)**
```
Tespit edilen göstergeleri yapılandırılmış formatta sunuyorum.
```

**Örnek:**
| Tür | Değer |
|-----|-------|
| Saldırgan IP | `117.11.88.124` |
| Yüklenen Dosya | `image.jpg.php` |
| C2 Portu | `8080` |

### 5. **MITRE ATT&CK Eşleştirmesi**
```
Tespit edilen teknikleri MITRE ATT&CK çerçevesi ile eşleştiriyorum.
```

**Örnek:**
- **T1190** (Exploit Public-Facing Application) - Dosya yükleme zafiyeti
- **T1059.004** (Unix Shell) - Ters kabuk üzerinden komut çalıştırma

---

## 🔍 Analiz Türlerine Göre Yaklaşımlar

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

## 📊 Dokümantasyon Standartları

Her vaka analizi aşağıdaki yapıyı takip eder:

```markdown
# Vaka Analizi: [Vaka Adı]

## 1. Özet (Executive Summary)
- 5-7 cümlelik özet
- Tehdit ailesi, kritik bulgular, etki

## 2. Teknik Analiz (Hipotez Odaklı)
### Faz 1 - [Başlık]
- Hipotez: ...
- Komut/Filtre: ...
- Bulgular: ...
- ![Görsel](assets/...)

## 3. Göstergeler (IOCs)
| Tür | Değer |
|-----|-------|

## Ek A - MITRE ATT&CK Eşleştirmesi
- T1XXX.XXX (Teknik Adı) - Kanıt

## Ek B - Tekrarlanabilirlik ve Komutlar
```bash
# Komutlar
```

---

## 🎯 Kalite Standartları

Her analiz aşağıdaki kriterleri karşılamalıdır:

- ✅ **Reproducible** - Tüm komutlar ve adımlar paylaşılmış
- ✅ **Visual** - Her kritik adım ekran görüntüsü ile desteklenmiş
- ✅ **Technical** - Teknik detaylar ve açıklamalar net
- ✅ **Structured** - Tutarlı format ve organizasyon
- ✅ **Bilingual** - Türkçe ve İngilizce dokümantasyon

---

## 🔗 Kaynaklar

- **MITRE ATT&CK:** https://attack.mitre.org/
- **CyberDefenders:** https://cyberdefenders.org/
- **Volatility 3 Docs:** https://volatility3.readthedocs.io/
- **Wireshark User Guide:** https://www.wireshark.org/docs/

---

*Son Güncelleme: Aralık 2025*

