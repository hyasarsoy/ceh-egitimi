# Sanal Makine (VM) Kurulum Kılavuzu

## Genel Bakış

Eğitim boyunca lab çalışmaları için sanal makine ortamı kullanılacaktır. Bu kılavuz, gerekli VM'lerin kurulumunu adım adım anlatmaktadır.

---

## 1. Sanallaştırma Yazılımı Kurulumu

### VirtualBox (Önerilen - Ücretsiz)

**İndirme:** https://www.virtualbox.org/wiki/Downloads

**Kurulum Adımları:**
1. İşletim sisteminize uygun sürümü indirin
2. Kurulum dosyasını çalıştırın
3. Varsayılan ayarlarla "Next" diyerek ilerleyin
4. Ağ adaptörü uyarısını "Yes" ile onaylayın
5. Kurulumu tamamlayın

**Extension Pack (Opsiyonel):**
- USB 3.0 desteği için Extension Pack'i de kurun
- Aynı sayfadan indirilebilir

### VMware Workstation Player (Alternatif)

**İndirme:** https://www.vmware.com/products/workstation-player.html

- Kişisel kullanım için ücretsiz
- Performans açısından VirtualBox'a göre daha iyi olabilir

---

## 2. Kali Linux Kurulumu

Kali Linux, sızma testi ve güvenlik analizi için kullanılan bir Linux dağıtımıdır.

### Hazır VM İmajı ile Kurulum (Önerilen)

1. **İndirme:** https://www.kali.org/get-kali/#kali-virtual-machines
2. VirtualBox veya VMware için uygun imajı seçin
3. İndirilen dosyayı çıkartın (.7z veya .ova)
4. VirtualBox'ta: File → Import Appliance → .ova dosyasını seçin
5. Varsayılan ayarları kontrol edin ve "Import" tıklayın

### VM Ayarları
| Ayar | Değer |
|------|-------|
| RAM | Minimum 2 GB, önerilen 4 GB |
| CPU | 2 çekirdek |
| Disk | 80 GB (dinamik) |
| Ağ | NAT veya Bridged Adapter |

### Varsayılan Giriş Bilgileri
- **Kullanıcı adı:** kali
- **Parola:** kali

> **Önemli:** İlk girişte parolayı mutlaka değiştirin!

```bash
passwd
```

### İlk Güncellemeler
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 3. Metasploitable 2 Kurulumu (Hedef Makine)

Metasploitable, bilerek güvenlik açıkları bırakılmış bir eğitim VM'sidir.

### Kurulum
1. **İndirme:** https://sourceforge.net/projects/metasploitable/
2. İndirilen .zip dosyasını çıkartın
3. VirtualBox'ta: New → Linux → Ubuntu (64-bit)
4. RAM: 1 GB
5. "Use an existing virtual hard disk file" seçin → .vmdk dosyasını gösterin
6. Oluşturun ve başlatın

### Varsayılan Giriş Bilgileri
- **Kullanıcı adı:** msfadmin
- **Parola:** msfadmin

> **Uyarı:** Bu VM kesinlikle internete bağlanmamalıdır! Sadece iç ağda kullanın.

---

## 4. Windows Hedef Makine (Opsiyonel)

### Windows Evaluation VM
1. Microsoft'un ücretsiz değerlendirme VM'lerini kullanabilirsiniz
2. **İndirme:** https://www.microsoft.com/en-us/evalcenter/
3. Windows 10/11 Enterprise veya Windows Server evaluation
4. 90 günlük ücretsiz kullanım

---

## 5. Ağ Yapılandırması

### Lab Ağ Topolojisi

```
┌──────────────┐     ┌──────────────┐
│  Kali Linux  │     │ Metasploitable│
│  (Saldırgan) │     │   (Hedef)     │
│  10.0.2.10   │     │  10.0.2.20    │
└──────┬───────┘     └──────┬───────┘
       │                     │
       └──────────┬──────────┘
                  │
          ┌───────┴───────┐
          │  Internal Net  │
          │  10.0.2.0/24   │
          └───────────────┘
```

### VirtualBox Ağ Ayarları

1. **Host-Only Network oluşturun:**
   - File → Host Network Manager
   - "Create" tıklayın
   - DHCP Server'ı etkinleştirin

2. **Her VM için ağ adaptörünü ayarlayın:**
   - VM Settings → Network → Adapter 1
   - "Host-only Adapter" seçin
   - Oluşturduğunuz ağı seçin

3. **İnternet erişimi gerekiyorsa (sadece Kali için):**
   - Adapter 2'yi etkinleştirin
   - "NAT" seçin

### IP Adresi Kontrolü
```bash
# Kali Linux
ip addr show

# Metasploitable
ifconfig

# Bağlantı testi
ping 10.0.2.20
```

---

## 6. Sorun Giderme

### VM Başlamıyor
- BIOS/UEFI'de sanallaştırma desteğini (VT-x/AMD-V) etkinleştirin
- Hyper-V'nin devre dışı olduğundan emin olun (Windows)

### Ağ Bağlantısı Yok
- Ağ adaptörünün doğru modda olduğunu kontrol edin
- VM'yi yeniden başlatın
- `ip addr show` ile IP adresi atanmış mı bakın

### Performans Sorunları
- VM'ye daha fazla RAM verin
- 3D hızlandırmayı devre dışı bırakın
- Ana makinede gereksiz uygulamaları kapatın

### Wireshark İzin Hatası (Linux)
```bash
sudo usermod -aG wireshark $USER
# Oturumu kapatıp açın
```

---

## Kontrol Listesi

Kurulumun tamamlandığını doğrulamak için:

- [ ] VirtualBox/VMware kurulu
- [ ] Kali Linux VM çalışıyor
- [ ] Metasploitable VM çalışıyor
- [ ] İki VM birbirini ping atabiliyor
- [ ] Kali'de Wireshark çalışıyor
- [ ] Kali'de internet erişimi var (NAT adaptörü ile)
- [ ] Metasploitable internete erişemiyor (güvenlik)
