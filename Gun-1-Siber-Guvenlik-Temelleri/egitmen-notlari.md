# Eğitmen Notları - Gün 1

**Eğitmen:** Halil Yaşarsoy
**Konu:** Siber Güvenlik Temelleri ve Ağ Savunması

---

## Genel Hazırlık

### Eğitim Öncesi Kontrol Listesi
- [ ] Lab ortamı hazır mı? (VM'ler çalışıyor mu?)
- [ ] Wireshark tüm bilgisayarlarda kurulu mu?
- [ ] Ağ bağlantısı aktif mi?
- [ ] Projeksiyon/ekran çalışıyor mu?
- [ ] Örnek pcap dosyaları USB'ye kopyalandı mı?
- [ ] Kahoot quiz'i oluşturuldu mu?
- [ ] Katılımcı listesi hazır mı?
- [ ] Senaryo çıktıları basıldı mı?

### Katılımcı Profili
- Askeri personel (farklı rütbe ve branşlar)
- Teknik bilgi seviyesi: Başlangıç - Orta
- Motivasyon: Birim güvenliğini artırma

---

## Modül 1: Siber Güvenlik ve Askeri Sistemler (09:00-11:00)

### Zaman Planı
| Süre | Aktivite |
|------|----------|
| 15 dk | Tanışma ve eğitim hedeflerinin açıklanması |
| 30 dk | Siber güvenliğin askeri operasyonlardaki önemi (sunum) |
| 25 dk | Gerçek saldırı örneklerinin incelenmesi |
| 20 dk | Güvenlik açıklarının kaynakları |
| 30 dk | Pratik senaryo çalışması (grup çalışması) |

### Önemli Noktalar
- Katılımcıların dikkatini çekmek için güncel ve askeri odaklı örneklerle başlayın
- Stuxnet örneği askeri operasyonlara etkiyi göstermek için etkili
- Grup çalışmasında 4-5 kişilik gruplar oluşturun
- Her gruba farklı bir senaryo verin (phishing, USB, insider threat, parola)

### Dikkat Edilecekler
- Gizlilik dereceli bilgilerden bahsetmeyin
- Gerçek birim isimleri yerine hayali isimler kullanın
- Saldırı tekniklerini anlatırken savunma odaklı olun
- Katılımcıların kendi deneyimlerini paylaşmasını teşvik edin

---

## ARA (11:00-11:15)

---

## Modül 2: Ağ Güvenliği ve Trafik İzleme (11:15-13:00)

### Zaman Planı
| Süre | Aktivite |
|------|----------|
| 20 dk | TCP/IP temelleri (sunum) |
| 15 dk | Ağ cihazları güvenliği |
| 25 dk | Wireshark demo (canlı gösterim) |
| 20 dk | Normal vs anormal trafik paternleri |
| 25 dk | Lab çalışması: Ağ trafiği yakalama |

### Wireshark Demo Adımları
1. Wireshark'ı açın ve arayüzü tanıtın
2. Yakalama başlatın → bir web sitesine girin → durdurun
3. TCP üçlü el sıkışmayı gösterin
4. HTTP vs HTTPS farkını gösterin (HTTP'de veri okunabilir)
5. DNS sorgularını filtreleyin
6. İstatistik menüsünü gösterin

### Lab Çalışması Talimatları
- Katılımcılar kendi bilgisayarlarında Wireshark açar
- Adım adım yönlendirin
- `tcp`, `dns`, `http` filtrelerini denetin
- "Follow TCP Stream" özelliğini gösterin
- Zaman kalırsa örnek pcap dosyasını inceletin

### Sık Sorulan Sorular
- **S:** Wireshark ile başkalarının trafiğini görebilir miyiz?
  - **C:** Promiscuous modda aynı ağdaki trafik görülebilir, ancak switched ağlarda bu kısıtlıdır ve yasal olmadan yapılmamalıdır
- **S:** HTTPS trafiğini okuyabilir miyiz?
  - **C:** İçerik şifreli olduğu için doğrudan okunamaz, ancak metadata (hangi siteye gidildiği) görülebilir

---

## ÖĞLE ARASI (13:00-14:00)

---

## Modül 3: Firewall ve Erişim Kontrolü (14:00-16:00)

### Zaman Planı
| Süre | Aktivite |
|------|----------|
| 25 dk | Firewall mantığı ve prensipleri (sunum) |
| 20 dk | Temel firewall kuralları oluşturma |
| 15 dk | Port filtreleme ve servis kontrolü |
| 20 dk | DMZ kavramı ve askeri ağlarda kullanımı |
| 40 dk | Pratik senaryo: Firewall kuralları tasarlama |

### Demo: iptables Örneği
- Basit bir iptables kural setini canlı gösterin
- Kural sırasının önemini vurgulayın
- "Deny all" kuralının neden sonda olması gerektiğini açıklayın

### Pratik Senaryo Yönetimi
- Senaryo dokümanını dağıtın
- Gruplar halinde çalıştırın
- Her grup kendi kural tablosunu oluşturmalı
- Sonuçları tahtada karşılaştırın
- En yaygın hataları tartışın

### Sık Yapılan Hatalar (Beklenen)
1. Kural sırasını yanlış yapma (deny all'u başa koyma)
2. Stateful inspection'ı unutma (ESTABLISHED,RELATED)
3. DNS çözümlemesine izin vermeden HTTP'ye izin verme
4. Loglama kurallarını eklemeyi unutma

---

## ARA (16:00-16:15)

---

## Modül 4: Günlük Değerlendirme (16:15-17:00)

### Zaman Planı
| Süre | Aktivite |
|------|----------|
| 10 dk | Günün özetlenmesi |
| 15 dk | Soru-cevap oturumu |
| 15 dk | Kahoot quiz |
| 5 dk | Kapanış ve ertesi gün planı |

### Kahoot Quiz Yönetimi
1. Kahoot hesabınıza giriş yapın
2. Quiz'i başlatın ve PIN kodunu paylaşın
3. Katılımcıların telefonlarından bağlanmasını bekleyin
4. 20 soru - yaklaşık 15 dakika sürecek
5. Sonuçları ekranda gösterin
6. İlk 3'e küçük ödüller verin (opsiyonel)

### Kapanış
- Günün ana mesajlarını 3 maddede özetleyin:
  1. Siber güvenlik askeri operasyonlar için kritik
  2. Ağ trafiğini izlemek tehditleri tespit etmenin ilk adımı
  3. Firewall yapılandırması savunmanın temel taşı
- Katılımcılardan geri bildirim alın
- Ertesi günün konularına kısa bakış

---

## Yedek İçerikler (Süre Kalırsa)
- Ek siber saldırı örnekleri (WannaCry, Colonial Pipeline)
- Nmap ile basit port tarama demosu
- Parola kırma demosu (hashcat/john temel demo)

## Acil Durum Planı (Teknik Sorunlar)
- **İnternet yoksa:** Önceden kaydedilmiş pcap dosyalarını kullanın
- **Wireshark çalışmıyorsa:** tcpdump ile komut satırından gösterim
- **Projeksiyon bozulursa:** Whiteboard kullanarak şema çizin
- **Kahoot erişilemezse:** Basılı quiz formlarını dağıtın
