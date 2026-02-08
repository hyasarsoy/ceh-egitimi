# Lab Ortamı Gereksinimleri

## Donanım Gereksinimleri

### Minimum Gereksinimler
| Bileşen | Minimum | Önerilen |
|---------|---------|----------|
| İşlemci | Intel i5 / AMD Ryzen 5 (4 çekirdek) | Intel i7 / AMD Ryzen 7 (8 çekirdek) |
| RAM | 8 GB | 16 GB |
| Disk | 100 GB boş alan (SSD önerilir) | 200 GB boş alan (SSD) |
| Ağ | Ethernet portu | Ethernet + Wi-Fi |
| USB | USB 3.0 port | USB 3.0+ |

> **Not:** Sanallaştırma yazılımı ve birden fazla VM çalıştırılacağı için yeterli RAM kritik öneme sahiptir.

### BIOS/UEFI Gereksinimleri
- **Intel VT-x** veya **AMD-V** sanallaştırma desteği etkinleştirilmiş olmalı
- BIOS'a giriş: Bilgisayar açılırken F2, F10, DEL veya ESC tuşuna basın
- Virtualization Technology → Enabled

---

## Yazılım Gereksinimleri

### Ana Makine İşletim Sistemi
- Windows 10/11 (64-bit)
- macOS 12+ (Intel veya Apple Silicon)
- Linux (Ubuntu 22.04+ önerilir)

### Sanallaştırma Yazılımı (Birini Seçin)
| Yazılım | Platform | Lisans |
|---------|----------|--------|
| VirtualBox 7.x | Windows, macOS, Linux | Ücretsiz |
| VMware Workstation Player | Windows, Linux | Kişisel kullanım ücretsiz |
| VMware Fusion | macOS | Ücretli |

### Sanal Makineler

| VM | Amaç | Disk | RAM |
|----|------|------|-----|
| Kali Linux 2024.x | Saldırı/Analiz makinesi | 80 GB | 4 GB |
| Metasploitable 2 | Hedef makine | 10 GB | 1 GB |
| Windows 10 Eval (opsiyonel) | Windows hedef | 50 GB | 4 GB |

### Ek Yazılımlar (Kali Linux üzerinde mevcut)
- Wireshark (ağ analizi)
- Nmap (ağ tarama)
- Metasploit Framework (sızma testi)
- Burp Suite Community (web güvenlik testi)
- John the Ripper (parola analizi)
- Aircrack-ng (kablosuz ağ analizi)

---

## Ağ Gereksinimleri

### Lab Ağı
- İzole lab ağı (Host-Only veya Internal Network)
- DHCP servisi aktif
- VM'ler arası iletişim sağlanmış

### İnternet Erişimi
- Güncelleme ve indirmeler için gerekli
- Sadece Kali Linux'un ikinci adaptöründen (NAT)
- Hedef makineler internete bağlanmamalı

### Ağ Adresleme Planı
```
Lab Ağı: 10.0.2.0/24
├── Kali Linux:      10.0.2.10
├── Metasploitable:  10.0.2.20
├── Windows (ops.):  10.0.2.30
└── Gateway:         10.0.2.1
```

---

## Eğitim Öncesi Hazırlık Kontrol Listesi

### Katılımcılar İçin
- [ ] Bilgisayar donanım gereksinimlerini karşılıyor
- [ ] BIOS'ta sanallaştırma desteği açık
- [ ] VirtualBox veya VMware kurulu
- [ ] Kali Linux VM indirilmiş ve kurulmuş
- [ ] Metasploitable 2 VM indirilmiş ve kurulmuş
- [ ] VM'ler arası ağ bağlantısı test edilmiş
- [ ] Wireshark çalışıyor ve trafik yakalayabiliyor

### Eğitmen İçin
- [ ] Tüm VM'lerin yedekleri alınmış (snapshot)
- [ ] Örnek pcap dosyaları hazır
- [ ] Lab kılavuzları basılmış veya dijital olarak paylaşılmış
- [ ] Yedek USB belleklerde VM imajları var
- [ ] Ağ altyapısı test edilmiş
- [ ] Kahoot quiz'i oluşturulmuş ve test edilmiş

---

## Güvenlik Uyarıları

1. **Lab ortamı izole olmalıdır** - Hedef makineler kesinlikle üretim ağına veya internete bağlanmamalıdır
2. **Eğitim amaçlı kullanım** - Öğrenilen teknikler yalnızca yetkili test ortamlarında kullanılmalıdır
3. **Yasal sorumluluk** - Yetkisiz sistemlere sızma girişimi yasal suçtur (TCK Madde 243-245)
4. **VM Snapshot** - Lab çalışmalarına başlamadan önce her VM'in snapshot'ını alın
5. **Parola değişimi** - Varsayılan parolaları ilk girişte mutlaka değiştirin
