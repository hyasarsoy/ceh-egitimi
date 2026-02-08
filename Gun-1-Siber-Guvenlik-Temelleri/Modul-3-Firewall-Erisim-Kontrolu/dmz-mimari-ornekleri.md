# DMZ (Demilitarized Zone) Mimari Örnekleri

## DMZ Nedir?

DMZ (Demilitarized Zone - Askerden Arındırılmış Bölge), iç ağ (güvenilir) ile dış ağ (güvenilmeyen/internet) arasında yer alan bir ara ağ segmentidir. Dışarıya açık servislerin (web sunucu, e-posta sunucu vb.) iç ağdan izole edilmiş bir ortamda çalışmasını sağlar.

## Temel DMZ Mimarileri

### 1. Tek Firewall DMZ (Three-Legged Firewall)

```
                    İnternet
                       │
                       │
                ┌──────┴──────┐
                │   FIREWALL   │
                │  (3 Bacaklı) │
                └──┬───┬───┬──┘
                   │   │   │
          ┌────────┘   │   └────────┐
          │            │            │
     ┌────┴────┐  ┌────┴────┐  ┌───┴─────┐
     │ İÇ AĞ   │  │  DMZ    │  │ İnternet│
     │(Güvenli) │  │         │  │ Bölgesi │
     │          │  │ Web Srv │  │         │
     │ İstemci  │  │ Mail Srv│  │         │
     │ DB Srv   │  │ DNS Srv │  │         │
     └─────────┘  └─────────┘  └─────────┘
```

**Avantajlar:**
- Basit yapı, düşük maliyet
- Tek cihaz yönetimi

**Dezavantajlar:**
- Tek hata noktası (Single Point of Failure)
- Firewall ele geçirilirse tüm ağ tehlikeye girer

---

### 2. Çift Firewall DMZ (Önerilen)

```
     İnternet
        │
  ┌─────┴─────┐
  │ FIREWALL 1 │  (Dış Firewall - Perimeter)
  │  (Harici)  │
  └─────┬─────┘
        │
  ┌─────┴─────┐
  │    DMZ     │
  │            │
  │  Web Srv   │
  │  Mail Srv  │
  │  VPN Srv   │
  │  DNS Srv   │
  └─────┬─────┘
        │
  ┌─────┴─────┐
  │ FIREWALL 2 │  (İç Firewall - Internal)
  │  (Dahili)  │
  └─────┬─────┘
        │
  ┌─────┴─────┐
  │  İÇ AĞ    │
  │            │
  │ İstemciler │
  │ DB Sunucu  │
  │ Dosya Srv  │
  │ AD Srv     │
  └────────────┘
```

**Avantajlar:**
- Derinlemesine savunma (Defense in Depth)
- Farklı üreticilerin firewall'ları kullanılabilir
- İç ağ ekstra koruma altında

**Dezavantajlar:**
- Daha yüksek maliyet
- Daha karmaşık yönetim

---

## Askeri Ağ DMZ Mimarisi

### Askeri Birim Ağ Topolojisi

```
                        İnternet
                           │
                    ┌──────┴──────┐
                    │    IPS/IDS   │  (Saldırı Tespit)
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │  DIŞ FW     │  (Harici Firewall)
                    │  (Katman 1) │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────┴────┐ ┌────┴─────┐ ┌────┴─────┐
        │ GENEL    │ │ VPN      │ │ E-POSTA  │
        │ DMZ      │ │ BÖLGESI  │ │ BÖLGESI  │
        │          │ │          │ │          │
        │ Web Srv  │ │ VPN Srv  │ │ Mail GW  │
        │ DNS Srv  │ │          │ │ Spam Flt │
        └─────┬────┘ └────┬─────┘ └────┬─────┘
              │            │            │
              └────────────┼────────────┘
                           │
                    ┌──────┴──────┐
                    │  İÇ FW      │  (Dahili Firewall)
                    │  (Katman 2) │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────┴────┐ ┌────┴─────┐ ┌────┴─────┐
        │ KOMUTA   │ │ PERSONEL │ │ LOJİSTİK │
        │ AĞI      │ │ AĞI      │ │ AĞI      │
        │ (GİZLİ)  │ │ (HİZMETE │ │ (GENEL)  │
        │          │ │  ÖZEL)   │ │          │
        │ C2 Srv   │ │ İstemci  │ │ ERP Srv  │
        │ Harita   │ │ AD Srv   │ │ Depo Srv │
        └──────────┘ └──────────┘ └──────────┘
```

---

## DMZ Güvenlik Kuralları

### Dış Firewall Kuralları (Internet → DMZ)

| # | Kaynak | Hedef | Port | Protokol | Aksiyon | Açıklama |
|---|--------|-------|------|----------|---------|----------|
| 1 | Any | Web Srv | 443 | TCP | İZİN | HTTPS erişimi |
| 2 | Any | Mail Srv | 25 | TCP | İZİN | Gelen e-posta |
| 3 | Any | DNS Srv | 53 | UDP/TCP | İZİN | DNS sorguları |
| 4 | VPN Users | VPN Srv | 1194 | UDP | İZİN | VPN bağlantı |
| 5 | Any | Any | Any | Any | ENGELLE | Varsayılan engel |

### İç Firewall Kuralları (DMZ → İç Ağ)

| # | Kaynak | Hedef | Port | Protokol | Aksiyon | Açıklama |
|---|--------|-------|------|----------|---------|----------|
| 1 | Web Srv | DB Srv | 3306 | TCP | İZİN | Veritabanı erişimi |
| 2 | Mail Srv | Mail Backend | 993 | TCP | İZİN | İç mail sunucu |
| 3 | VPN Srv | İç Ağ | Belirli | TCP | İZİN | VPN erişimi (kısıtlı) |
| 4 | Any | Any | Any | Any | ENGELLE | Varsayılan engel |

### Kritik Kural: DMZ → İç Ağ Erişimi
- DMZ'den iç ağa **doğrudan erişim OLMAMALIDIR**
- Yalnızca belirli sunucu-sunucu iletişimine izin verilmeli
- İç ağdan DMZ'ye erişim yönetim amaçlı ve kısıtlı olmalı

---

## En İyi Uygulamalar

1. **Ağ Segmentasyonu:** Her bölge ayrı VLAN'da olmalı
2. **En Az Yetki:** Yalnızca gerekli portlar açık olmalı
3. **Loglama:** Tüm firewall trafiği loglanmalı
4. **İzleme:** IDS/IPS ile sürekli trafik izlemesi
5. **Yama Yönetimi:** DMZ sunucuları düzenli güncellenmelidir
6. **Yedekleme:** DMZ sunucuları için ayrı yedekleme stratejisi
7. **Sertleştirme:** DMZ sunucularında gereksiz servisler kapatılmalı
