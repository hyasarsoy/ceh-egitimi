# TCP/IP Protokol Referans Kılavuzu

## OSI Modeli ve TCP/IP Karşılaştırması

```
OSI Modeli (7 Katman)          TCP/IP Modeli (4 Katman)
┌─────────────────────┐
│  7. Uygulama        │        ┌─────────────────────┐
│  6. Sunum           │───────>│  4. Uygulama        │
│  5. Oturum          │        │  (HTTP,DNS,SMTP,SSH) │
├─────────────────────┤        ├─────────────────────┤
│  4. Taşıma          │───────>│  3. Taşıma          │
│                     │        │  (TCP, UDP)          │
├─────────────────────┤        ├─────────────────────┤
│  3. Ağ              │───────>│  2. İnternet         │
│                     │        │  (IP, ICMP, ARP)     │
├─────────────────────┤        ├─────────────────────┤
│  2. Veri Bağlantısı │───────>│  1. Ağ Erişimi      │
│  1. Fiziksel        │        │  (Ethernet, Wi-Fi)   │
└─────────────────────┘        └─────────────────────┘
```

## Önemli Protokoller

### Taşıma Katmanı

#### TCP (Transmission Control Protocol)
- **Bağlantı tipi:** Bağlantı yönelimli (Connection-oriented)
- **Güvenilirlik:** Güvenilir (ACK mekanizması)
- **Kullanım:** HTTP, HTTPS, SSH, FTP, SMTP

**TCP Üçlü El Sıkışma (Three-Way Handshake):**
```
İstemci                    Sunucu
   │                          │
   │──── SYN ────────────────>│  1. Bağlantı isteği
   │                          │
   │<─── SYN-ACK ────────────│  2. İstek onayı
   │                          │
   │──── ACK ────────────────>│  3. Bağlantı kuruldu
   │                          │
```

#### UDP (User Datagram Protocol)
- **Bağlantı tipi:** Bağlantısız (Connectionless)
- **Güvenilirlik:** Güvenilir değil (best-effort)
- **Kullanım:** DNS, DHCP, VoIP, video streaming

### Ağ Katmanı

#### IP (Internet Protocol)
- **IPv4:** 32-bit adres (örn: 192.168.1.1)
- **IPv6:** 128-bit adres (örn: 2001:db8::1)
- **Sınıflar:**

| Sınıf | Aralık | Özel Ağ Aralığı |
|-------|--------|-----------------|
| A | 1.0.0.0 - 126.255.255.255 | 10.0.0.0/8 |
| B | 128.0.0.0 - 191.255.255.255 | 172.16.0.0/12 |
| C | 192.0.0.0 - 223.255.255.255 | 192.168.0.0/16 |

#### ICMP (Internet Control Message Protocol)
- Hata raporlama ve tanılama
- `ping` ve `traceroute` komutları bu protokolü kullanır

## Yaygın Port Numaraları

| Port | Protokol | Servis | Güvenlik Notu |
|------|----------|--------|---------------|
| 20/21 | TCP | FTP | Şifresiz - SFTP tercih edin |
| 22 | TCP | SSH | Güvenli uzak erişim |
| 23 | TCP | Telnet | Şifresiz - SSH tercih edin |
| 25 | TCP | SMTP | E-posta gönderimi |
| 53 | TCP/UDP | DNS | DNS spoofing riski |
| 80 | TCP | HTTP | Şifresiz - HTTPS tercih edin |
| 110 | TCP | POP3 | E-posta alımı |
| 143 | TCP | IMAP | E-posta alımı |
| 443 | TCP | HTTPS | Şifreli web trafiği |
| 445 | TCP | SMB | Dosya paylaşımı - WannaCry hedefi |
| 3389 | TCP | RDP | Uzak masaüstü - brute force riski |
| 8080 | TCP | HTTP Proxy | Alternatif HTTP |

## Ağ Cihazları ve Güvenlik

### Router (Yönlendirici)
- Farklı ağlar arası trafik yönlendirme
- ACL (Access Control List) ile filtreleme
- **Güvenlik:** Varsayılan parolaları değiştirin, gereksiz servisleri kapatın

### Switch (Anahtarlayıcı)
- Aynı ağ içinde çerçeve iletimi
- MAC adresi tablosu ile çalışır
- **Güvenlik:** Port security, VLAN segmentasyonu, STP koruması

### Firewall (Güvenlik Duvarı)
- Ağ trafiğini kurallara göre filtreleme
- Stateful/Stateless inspection
- **Güvenlik:** En az yetki prensibi, varsayılan olarak engelle (deny all)

## Ağ Saldırı Türleri

| Saldırı | Katman | Açıklama |
|---------|--------|----------|
| ARP Spoofing | 2 | MAC adresi tablosunu zehirleme |
| IP Spoofing | 3 | Kaynak IP adresini sahteleme |
| SYN Flood | 4 | TCP bağlantı taleplerini doldurma |
| DNS Poisoning | 7 | DNS yanıtlarını sahteleme |
| Man-in-the-Middle | 2-7 | İletişimi araya girme |

## Hızlı Başvuru - Komut Satırı Araçları

```bash
# Ağ yapılandırmasını görüntüleme
ip addr show          # Linux
ipconfig /all         # Windows

# Bağlantı testi
ping 192.168.1.1
traceroute 8.8.8.8    # Linux
tracert 8.8.8.8       # Windows

# Açık portları görüntüleme
netstat -tuln         # Linux
netstat -an           # Windows

# DNS sorgusu
nslookup example.com
dig example.com       # Linux

# ARP tablosu
arp -a
```
