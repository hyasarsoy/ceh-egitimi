# Wireshark Lab Kılavuzu

## Lab Amacı
Bu lab çalışmasında Wireshark kullanarak ağ trafiğini yakalama, filtreleme ve analiz etme becerilerini kazanacaksınız.

## Ön Gereksinimler
- Wireshark kurulu bir bilgisayar (versiyon 4.x önerilir)
- Ağ erişimi olan bir lab ortamı
- Yönetici/root yetkisi

## Bölüm 1: Wireshark'a Giriş

### 1.1 Wireshark Arayüzü
- **Menü çubuğu:** Dosya, düzenleme, yakalama işlemleri
- **Araç çubuğu:** Hızlı erişim düğmeleri
- **Filtre çubuğu:** Görüntüleme filtreleri
- **Paket listesi:** Yakalanan paketlerin listesi
- **Paket detayları:** Seçili paketin protokol katmanları
- **Paket baytları:** Ham veri görünümü

### 1.2 Yakalama Başlatma
1. Wireshark'ı açın
2. Uygun ağ arayüzünü seçin (genellikle `eth0` veya `wlan0`)
3. Yakalama başlatmak için mavi köpekbalığı yüzgeci simgesine tıklayın
4. Durdurmak için kırmızı kare düğmesine tıklayın

---

## Bölüm 2: Temel Filtreleme

### 2.1 Görüntüleme Filtreleri (Display Filters)

| Filtre | Açıklama |
|--------|----------|
| `tcp` | Sadece TCP trafiğini göster |
| `udp` | Sadece UDP trafiğini göster |
| `http` | Sadece HTTP trafiğini göster |
| `dns` | Sadece DNS trafiğini göster |
| `icmp` | Sadece ICMP (ping) trafiğini göster |
| `ip.addr == 192.168.1.1` | Belirli IP adresine ait trafik |
| `ip.src == 10.0.0.1` | Kaynak IP filtresi |
| `ip.dst == 10.0.0.1` | Hedef IP filtresi |
| `tcp.port == 80` | TCP port 80 trafiği |
| `tcp.port == 443` | HTTPS trafiği |
| `tcp.flags.syn == 1` | SYN paketleri |
| `http.request.method == "GET"` | HTTP GET istekleri |
| `http.request.method == "POST"` | HTTP POST istekleri |

### 2.2 Filtre Operatörleri

| Operatör | Anlam | Örnek |
|----------|-------|-------|
| `==` | Eşittir | `ip.addr == 10.0.0.1` |
| `!=` | Eşit değildir | `ip.addr != 10.0.0.1` |
| `>` | Büyüktür | `frame.len > 1000` |
| `<` | Küçüktür | `tcp.port < 1024` |
| `&&` veya `and` | VE | `tcp && ip.addr == 10.0.0.1` |
| `\|\|` veya `or` | VEYA | `http \|\| dns` |
| `!` veya `not` | DEĞİL | `!arp` |

### 2.3 Filtre Örnekleri (Birleşik)
```
# Belirli IP'den gelen HTTP trafiği
ip.src == 192.168.1.100 && http

# 443 portu dışındaki tüm TCP trafiği
tcp && tcp.port != 443

# DNS veya DHCP trafiği
dns || udp.port == 67

# SYN flood tespiti - çok sayıda SYN paketi
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

---

## Bölüm 3: Lab Alıştırmaları

### Alıştırma 1: TCP Üçlü El Sıkışma Analizi

**Amaç:** Bir TCP bağlantısının kurulma sürecini gözlemlemek

**Adımlar:**
1. Wireshark'ta yakalamayı başlatın
2. Tarayıcıda bir web sitesine gidin
3. Yakalamayı durdurun
4. Filtre: `tcp.flags.syn == 1 || tcp.flags.ack == 1`
5. SYN → SYN-ACK → ACK sırasını bulun

**Sorular:**
- Üçlü el sıkışmada kaç paket var?
- Her paketin TCP bayrakları nelerdir?
- Sequence ve Acknowledgment numaralarını takip edin

### Alıştırma 2: DNS Trafiği İnceleme

**Amaç:** DNS sorgularını ve yanıtlarını analiz etmek

**Adımlar:**
1. Wireshark'ta yakalamayı başlatın
2. Komut satırında: `nslookup www.example.com`
3. Yakalamayı durdurun
4. Filtre: `dns`

**Sorular:**
- DNS sorgusu hangi porta gönderildi?
- Sorgu tipi nedir (A, AAAA, CNAME)?
- Yanıtta dönen IP adresi nedir?
- DNS protokolü TCP mi UDP mi kullanıyor?

### Alıştırma 3: HTTP Trafik Analizi

**Amaç:** Şifrelenmemiş HTTP trafiğinin risklerini görmek

**Adımlar:**
1. Wireshark'ta yakalamayı başlatın
2. Bir HTTP (HTTPS değil) web sitesine girin
3. Yakalamayı durdurun
4. Filtre: `http`
5. Bir HTTP paketine sağ tıklayın → "Follow TCP Stream"

**Sorular:**
- HTTP başlıklarında hangi bilgiler görünüyor?
- User-Agent bilgisi ne ifade ediyor?
- HTTP trafiğindeki veriler okunabilir mi?
- Bu neden bir güvenlik riski oluşturur?

### Alıştırma 4: Anormal Trafik Tespiti

**Amaç:** Normal ve anormal ağ trafiğini ayırt etmeyi öğrenmek

**Anormal Trafik İşaretleri:**
1. **Yüksek hacimli ICMP trafiği** → Olası ping flood
   - Filtre: `icmp`
   - Normal: Düşük frekanslı ping/pong
   - Anormal: Saniyede yüzlerce ICMP paketi

2. **Çok sayıda SYN paketi (ACK olmadan)** → Olası SYN flood
   - Filtre: `tcp.flags.syn == 1 && tcp.flags.ack == 0`
   - Normal: Arada bir SYN paketi
   - Anormal: Aynı hedefe sürekli SYN paketleri

3. **Bilinmeyen portlara trafik** → Olası backdoor/C2
   - Filtre: `tcp.port > 49152`
   - Normal: Bilinen servis portları
   - Anormal: Yüksek numaralı portlara düzenli trafik

4. **ARP storm** → Olası ARP spoofing
   - Filtre: `arp`
   - Normal: Arada ARP sorguları
   - Anormal: Aynı IP için farklı MAC yanıtları

---

## Bölüm 4: Wireshark İstatistikleri

### Kullanışlı İstatistik Menüleri
- **Statistics → Conversations:** Ağdaki konuşmaları görme
- **Statistics → Protocol Hierarchy:** Protokol dağılımı
- **Statistics → Endpoints:** Ağdaki uç noktalar
- **Statistics → I/O Graphs:** Trafik grafikleri

---

## Önemli Notlar

- Wireshark yalnızca **eğitim ve yetkili test ortamlarında** kullanılmalıdır
- Yetkisiz ağ trafiği yakalama **yasal suç** teşkil edebilir
- Lab ortamında çalışırken yalnızca kendi trafiğinizi yakalayın
- Promiscuous mod (karışık mod) kullanımı için ağ yöneticisinden izin alın
