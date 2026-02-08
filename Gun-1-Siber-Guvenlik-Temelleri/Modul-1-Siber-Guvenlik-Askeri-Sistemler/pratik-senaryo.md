# Pratik Senaryo: Askeri Birim Güvenlik Zafiyet Analizi

## Senaryo Açıklaması

Siz bir askeri birimin siber güvenlik sorumlususunuz. Aşağıdaki senaryoda birliğinize yönelik çeşitli güvenlik tehditleri tanımlanmıştır. Her bir tehdidi analiz edin ve uygun savunma stratejilerini belirleyin.

---

## Senaryo 1: Phishing Saldırısı

### Durum
Birliğinizdeki bir personel, "Savunma Bakanlığı Personel Sistemi - Şifre Güncelleme" başlıklı bir e-posta almıştır. E-posta, resmi görünümlü bir bağlantıya tıklanmasını istemektedir.

### E-posta İçeriği
```
Konu: ACİL - Şifre Güncelleme Zorunluluğu
Gönderen: bilgi@savunma-bakanIigi.gov.tr  (dikkat: 'l' yerine büyük 'I')

Sayın Personel,

Sistemimizde güvenlik güncellemesi yapılmıştır. 24 saat içinde
şifrenizi güncellemeniz gerekmektedir. Aksi halde hesabınız
askıya alınacaktır.

Şifre güncellemek için tıklayın: http://savunma-guncelleme.xyz/login
```

### Analiz Edilecek Noktalar
- [ ] Gönderen adresindeki şüpheli unsurları tespit edin
- [ ] URL'deki anormallikleri belirleyin
- [ ] E-postadaki aciliyet baskısını değerlendirin
- [ ] Bu saldırıyı önlemek için hangi teknik kontroller uygulanabilir?

### Beklenen Cevaplar
1. **Domain spoofing:** `savunma-bakanIigi` - büyük I harfi kullanılmış
2. **Sahte URL:** `.xyz` uzantılı alan adı resmi değil
3. **Sosyal mühendislik:** Aciliyet yaratılarak düşünme süresi kısaltılmış
4. **Önlemler:** E-posta filtreleme, URL sandboxing, personel farkındalık eğitimi

---

## Senaryo 2: USB Tabanlı Saldırı

### Durum
Birlik karargahının otoparkında markalı bir USB bellek bulunmuştur. Üzerinde "Tatbikat Planları - GİZLİ" etiketi vardır.

### Analiz Edilecek Noktalar
- [ ] USB belleğin potansiyel tehlikelerini listeleyin
- [ ] Doğru müdahale prosedürünü belirleyin
- [ ] Benzer saldırıları önlemek için politika önerileri geliştirin

### Beklenen Cevaplar
1. **Tehlikeler:**
   - USB Rubber Ducky (otomatik komut çalıştırma)
   - BadUSB (firmware seviyesinde zararlı yazılım)
   - Autorun malware
   - Veri sızdırma aracı
2. **Doğru prosedür:**
   - Asla doğrudan bilgisayara takılmamalı
   - Güvenlik birimine teslim edilmeli
   - İzole ortamda analiz edilmeli
3. **Politikalar:**
   - USB portlarının fiziksel/yazılımsal olarak devre dışı bırakılması
   - Yalnızca onaylı cihazların kullanılması
   - Personel farkındalık eğitimi

---

## Senaryo 3: İç Tehdit (Insider Threat)

### Durum
Bir personelin mesai saatleri dışında yetkisiz dosya erişimi yaptığı ve büyük miktarda veriyi harici bir servise yüklemeye çalıştığı loglardan tespit edilmiştir.

### Log Kayıtları
```
[2025-01-15 23:45:12] USER: sgm_mehmet ACCESS: /classified/operations/plan-2025.docx
[2025-01-15 23:47:33] USER: sgm_mehmet ACCESS: /classified/personnel/roster.xlsx
[2025-01-15 23:52:01] USER: sgm_mehmet UPLOAD: cloud-storage.example.com 45MB
[2025-01-15 23:55:18] USER: sgm_mehmet ACCESS: /classified/comms/freq-table.pdf
[2025-01-16 00:01:44] USER: sgm_mehmet UPLOAD: cloud-storage.example.com 12MB
```

### Analiz Edilecek Noktalar
- [ ] Log kayıtlarındaki anormal davranışları belirleyin
- [ ] Hangi acil müdahale adımları atılmalı?
- [ ] Bu tür tehditleri önlemek için hangi kontroller gerekli?

### Beklenen Cevaplar
1. **Anormal davranışlar:**
   - Mesai dışı erişim (gece 23:45)
   - Gizli belgelere ardışık erişim
   - Harici servise veri yükleme
   - Farklı kategorilerdeki gizli belgelere erişim
2. **Acil müdahale:**
   - Hesabın derhal askıya alınması
   - Ağ erişiminin kesilmesi
   - Adli bilişim (forensic) incelemesi başlatılması
   - Komuta kademesinin bilgilendirilmesi
3. **Önleyici kontroller:**
   - DLP (Data Loss Prevention) çözümleri
   - UEBA (User Entity Behavior Analytics)
   - En az yetki prensibi (Least Privilege)
   - Mesai dışı erişim için ek onay mekanizması

---

## Senaryo 4: Zayıf Parola Güvenliği

### Durum
Birlik ağındaki parola denetimi sırasında aşağıdaki parolalar tespit edilmiştir:

| Kullanıcı | Parola |
|-----------|--------|
| bnb_ali | Turkiye123 |
| yzb_veli | 12345678 |
| astgm_hasan | hasan1990 |
| svs_ayse | qwerty |

### Analiz Edilecek Noktalar
- [ ] Her bir parolanın zayıf yönlerini belirleyin
- [ ] Güçlü parola politikası önerileri oluşturun
- [ ] MFA (Multi-Factor Authentication) uygulamasını planlayın

### Beklenen Cevaplar
1. **Zayıf yönler:**
   - `Turkiye123`: Yaygın kelime + basit sayı
   - `12345678`: Sıralı sayılar
   - `hasan1990`: Kişisel bilgi (isim + doğum yılı)
   - `qwerty`: Klavye deseni
2. **Parola politikası:**
   - Minimum 12 karakter
   - Büyük/küçük harf, sayı, özel karakter zorunluluğu
   - Kişisel bilgi içermeme kuralı
   - 90 günde bir değişim zorunluluğu
   - Son 12 parola tekrar kullanılamaz
3. **MFA planı:**
   - Askeri kimlik kartı + PIN
   - Donanım token (YubiKey vb.)
   - Biyometrik doğrulama (parmak izi)

---

## Değerlendirme

Her senaryo için gruplar halinde çalışılacaktır:
- **Süre:** Her senaryo için 15 dakika
- **Sunum:** Her grup bulgularını 5 dakika içinde sunacaktır
- **Tartışma:** Sınıf genelinde en iyi uygulamalar tartışılacaktır
