# Askeri Birimlere Yönelik Siber Saldırı Örnekleri

## 1. Stuxnet (2010)

**Hedef:** İran nükleer tesisleri (Natanz uranyum zenginleştirme tesisi)

**Saldırı Türü:** Endüstriyel kontrol sistemi (SCADA/PLC) saldırısı

**Özet:**
- Siemens Step 7 yazılımını hedef alan gelişmiş bir solucan (worm)
- USB sürücüler aracılığıyla air-gapped ağlara sızmayı başardı
- Santrifüjlerin dönüş hızlarını değiştirerek fiziksel hasar verdi
- Operatörlere normal çalışma verileri göstererek tespit edilmeyi geciktirdi

**Alınan Dersler:**
- Air-gapped sistemler bile güvende değildir
- USB cihazlarının kontrolü kritik öneme sahiptir
- Endüstriyel kontrol sistemleri siber saldırılara karşı savunmasızdır

---

## 2. SolarWinds / Sunburst Saldırısı (2020)

**Hedef:** ABD devlet kurumları, askeri birimler, kritik altyapı

**Saldırı Türü:** Tedarik zinciri saldırısı (Supply Chain Attack)

**Özet:**
- SolarWinds Orion yazılımının güncelleme mekanizmasına zararlı kod enjekte edildi
- 18.000'den fazla kuruluş etkilendi
- ABD Savunma Bakanlığı, Hazine Bakanlığı dahil birçok kuruma sızıldı
- Aylarca tespit edilemedi

**Alınan Dersler:**
- Tedarik zinciri güvenliği kritik önem taşır
- Güvenilir yazılım güncellemeleri bile tehlike oluşturabilir
- Ağ segmentasyonu ve sıfır güven (Zero Trust) mimarisi gereklidir

---

## 3. NotPetya (2017)

**Hedef:** Ukrayna altyapısı (askeri ve sivil)

**Saldırı Türü:** Yıkıcı kötü amaçlı yazılım (Destructive Malware / Wiper)

**Özet:**
- Ukrayna'nın M.E.Doc muhasebe yazılımı üzerinden yayıldı
- Fidye yazılımı gibi görünse de asıl amacı veri imhasıydı
- Küresel çapta 10 milyar dolardan fazla hasar
- Maersk, FedEx, Merck gibi büyük şirketler de etkilendi

**Alınan Dersler:**
- Siber silahlar sivil altyapıya yayılabilir
- Yedekleme stratejileri hayati öneme sahiptir
- Yama yönetimi (patch management) ihmal edilmemelidir

---

## 4. Estonya Siber Saldırıları (2007)

**Hedef:** Estonya devlet altyapısı

**Saldırı Türü:** Dağıtık Hizmet Reddi (DDoS)

**Özet:**
- Estonya parlamentosu, bankalar, medya kuruluşları hedef alındı
- Haftalarca süren koordineli DDoS saldırıları
- Ülkenin dijital altyapısı ciddi şekilde etkilendi
- NATO'nun siber savunma politikalarının şekillenmesine neden oldu

**Alınan Dersler:**
- DDoS saldırıları bir ülkeyi felç edebilir
- Siber saldırılar savaş eylemi olarak değerlendirilebilir
- NATO Tallinn Kılavuzu bu olaydan sonra hazırlandı

---

## 5. Fancy Bear / APT28 Operasyonları

**Hedef:** NATO ülkeleri askeri birimleri

**Saldırı Türü:** Gelişmiş Kalıcı Tehdit (APT) / Spear Phishing

**Özet:**
- Rus askeri istihbaratıyla (GRU) ilişkilendirildi
- NATO üyesi ülkelerin askeri personeline hedefli phishing e-postaları
- Ukrayna ordusunun topçu birimlerinin Android uygulamasını trojanlaştırma
- Askeri konum bilgilerinin ele geçirilmesi

**Alınan Dersler:**
- Askeri personel sosyal mühendislik saldırılarına karşı eğitilmelidir
- Mobil cihaz güvenliği askeri operasyonlarda kritiktir
- Uygulama kaynaklarının doğrulanması şarttır

---

## Tartışma Soruları

1. Bu saldırılardan hangisi sizin biriminizi en çok etkileyebilir? Neden?
2. Air-gapped sistemlerin güvenliği nasıl sağlanabilir?
3. Tedarik zinciri saldırılarına karşı hangi önlemler alınabilir?
4. Askeri personelin siber güvenlik farkındalığı nasıl artırılabilir?
