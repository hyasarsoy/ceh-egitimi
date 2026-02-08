# Pratik Senaryo: Askeri Birim Ağı İçin Firewall Kuralları Tasarlama

## Senaryo Açıklaması

Bir askeri birliğin ağ altyapısı için firewall kuralları tasarlamanız istenmektedir. Aşağıdaki gereksinimlere göre uygun kuralları oluşturun.

## Ağ Topolojisi

```
İnternet (Dış Ağ)
       │
  ┌────┴────┐
  │ FIREWALL │
  └────┬────┘
       │
  ┌────┴──────────────────────────────────┐
  │              İÇ AĞ                    │
  │                                        │
  │  ┌──────────┐  ┌──────────┐           │
  │  │ Komuta    │  │ Personel │           │
  │  │ Sunucusu  │  │ Bilg.    │           │
  │  │ 10.0.1.10 │  │ 10.0.2.x │           │
  │  └──────────┘  └──────────┘           │
  │                                        │
  │  ┌──────────┐  ┌──────────┐           │
  │  │ Web       │  │ DNS      │           │
  │  │ Sunucusu  │  │ Sunucusu │           │
  │  │ 10.0.3.10 │  │ 10.0.3.20│           │
  │  └──────────┘  └──────────┘           │
  │                                        │
  │  ┌──────────┐                          │
  │  │ VPN       │                          │
  │  │ Sunucusu  │                          │
  │  │ 10.0.3.30 │                          │
  │  └──────────┘                          │
  └────────────────────────────────────────┘
```

### Ağ Segmentleri
- **10.0.1.0/24** — Komuta ağı (gizli seviye)
- **10.0.2.0/24** — Personel ağı (genel kullanım)
- **10.0.3.0/24** — Sunucu ağı (DMZ)

---

## Gereksinimler

### Zorunlu Kurallar
1. Dış dünyadan web sunucusuna HTTPS (443) erişimi olmalı
2. Dış dünyadan DNS sunucusuna DNS (53) sorguları cevaplanmalı
3. VPN kullanıcıları dışarıdan VPN sunucusuna bağlanabilmeli (UDP 1194)
4. Personel bilgisayarları internete HTTP/HTTPS çıkışı yapabilmeli
5. Komuta ağı internete **çıkış yapamamalı**
6. Dış dünyadan iç ağa doğrudan erişim **engellenmeli**
7. Tüm gelen SSH (22) bağlantıları **engellenmeli**
8. ICMP (ping) dışarıdan **engellenmeli**, içeriden serbest olmalı

---

## Alıştırma 1: Firewall Kural Tablosu Oluşturma

Aşağıdaki tabloyu gereksinimlere göre doldurun:

| # | Yön | Kaynak | Hedef | Port | Protokol | Aksiyon | Açıklama |
|---|-----|--------|-------|------|----------|---------|----------|
| 1 | IN | | | | | | |
| 2 | IN | | | | | | |
| 3 | IN | | | | | | |
| 4 | OUT | | | | | | |
| 5 | OUT | | | | | | |
| 6 | IN | | | | | | |
| 7 | IN | | | | | | |
| 8 | ANY | | | | | | |

### Beklenen Cevap Tablosu

<details>
<summary>Cevabı görmek için tıklayın</summary>

| # | Yön | Kaynak | Hedef | Port | Protokol | Aksiyon | Açıklama |
|---|-----|--------|-------|------|----------|---------|----------|
| 1 | IN | Any | 10.0.3.10 | 443 | TCP | ALLOW | HTTPS web erişimi |
| 2 | IN | Any | 10.0.3.20 | 53 | UDP/TCP | ALLOW | DNS sorguları |
| 3 | IN | Any | 10.0.3.30 | 1194 | UDP | ALLOW | VPN bağlantı |
| 4 | OUT | 10.0.2.0/24 | Any | 80,443 | TCP | ALLOW | Personel internet |
| 5 | OUT | 10.0.1.0/24 | Any | Any | Any | DENY | Komuta çıkış engel |
| 6 | IN | Any | 10.0.1.0/24 | Any | Any | DENY | Komuta giriş engel |
| 7 | IN | Any | Any | 22 | TCP | DENY | SSH engel |
| 8 | IN | Any | Any | Any | ICMP | DENY | Dış ping engel |
| 9 | OUT | Any | Any | Any | ICMP | ALLOW | İç ping serbest |
| 10 | ANY | Any | Any | Any | Any | DENY | Varsayılan engel |

</details>

---

## Alıştırma 2: Senaryo Soruları

### Soru 1: Kural Sırası
Aşağıdaki iki kural seti arasındaki fark nedir? Hangisi doğrudur?

**Set A:**
```
1. ALLOW TCP Any -> 10.0.3.10:443
2. DENY  Any Any -> Any:Any
```

**Set B:**
```
1. DENY  Any Any -> Any:Any
2. ALLOW TCP Any -> 10.0.3.10:443
```

<details>
<summary>Cevap</summary>

**Set A doğrudur.** Firewall kuralları yukarıdan aşağıya işlenir. Set B'de ilk kural tüm trafiği engellediği için ikinci kural hiçbir zaman uygulanmaz. Kural sırası kritik önem taşır.

</details>

### Soru 2: Güvenlik Açığı Tespiti
Aşağıdaki kural setinde güvenlik açığını bulun:

```
1. ALLOW TCP Any -> 10.0.3.10:443
2. ALLOW TCP Any -> 10.0.3.10:80
3. ALLOW TCP 10.0.2.0/24 -> Any:Any
4. DENY  Any Any -> Any:Any
```

<details>
<summary>Cevap</summary>

**Kural 2:** Port 80 (HTTP) açık bırakılmış. Şifresiz trafik güvenlik riski oluşturur. Port 80'den 443'e yönlendirme yapılmalı veya kapatılmalıdır.

**Kural 3:** Personel ağına tüm portlardan çıkış izni verilmiş. Bu, veri sızıntısına yol açabilir. Yalnızca 80 ve 443 portlarına izin verilmelidir.

</details>

### Soru 3: Gelişmiş Senaryo
Bir askeri birliğin ağında aşağıdaki durumlar tespit edilmiştir:
- Komuta sunucusuna dışarıdan SSH bağlantı denemeleri (brute force)
- Web sunucusuna yoğun trafik (olası DDoS)
- Personel bilgisayarından bilinmeyen bir IP'ye port 4444 üzerinden sürekli trafik

Her durum için:
1. Hangi firewall kuralı eklenmeli?
2. Hangi ek güvenlik önlemi alınmalı?

<details>
<summary>Cevap</summary>

**SSH Brute Force:**
- Kural: `DENY TCP Any -> 10.0.1.10:22` (zaten olmalı)
- Ek önlem: Fail2ban kurulumu, SSH key-only authentication

**DDoS:**
- Kural: Rate limiting kuralı ekleyin
- Ek önlem: WAF (Web Application Firewall), CDN, DDoS koruma servisi

**Port 4444 trafiği (olası reverse shell/Meterpreter):**
- Kural: `DENY TCP 10.0.2.0/24 -> Any:4444`
- Ek önlem: Bilgisayarı ağdan izole edin, forensic analiz yapın, IDS/IPS kuralı ekleyin

</details>

---

## Değerlendirme Kriterleri

| Kriter | Puan |
|--------|------|
| Doğru kural yapısı | 30 |
| Kural sıralaması | 20 |
| Güvenlik açıklarını tespit | 25 |
| Ek güvenlik önerilerinin kalitesi | 25 |
| **Toplam** | **100** |
