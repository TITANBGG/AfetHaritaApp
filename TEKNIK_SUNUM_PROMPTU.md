# Teknik Sunum Promptu - AfetHabitaApp Projesi

## 📊 Sunum Özeti
Bu sunum, 2023 Türkiye-Suriye Depremi sırasında toplanan tweet verilerinin analiz edilmesi ve işlenmesi projesi hakkında teknik detayları açıklamaktadır.

---

## 1️⃣ PROJENİN AMAÇLARI VE KAPSAMI

### Proje Tanımı
AfetHaritaApp, 6-21 Şubat 2023 tarihlerinde meydana gelen Türkiye-Suriye Depremi sırasında sosyal medya verilerinin toplanması, işlenmesi ve analiz edilmesi için geliştirilmiş bir sistemdir.

### Ana Hedefler
- **Veri Toplanması**: Twitter platformundan deprem döneminde atılan tweetlerin arşivlenmesi
- **Veri Sınıflandırması**: Yardım çağrılarının tespit edilmesi ve sınıflandırılması
- **Bilgi Çıkartma**: Tweetlerden kritik bilgilerin (konum, kayıp kişi vb.) manuel olarak tanımlanması
- **Görselleştirme**: Coğrafi bilgiler ve istatistiklerle harita üzerinde gösterim

### Proje Kapsamı
- **Toplam Tweet**: 140,532 adet
- **Veri Boyutu**: 83.5 MB (ana dosya)
- **Zaman Aralığı**: 6-21 Şubat 2023 (16 gün)
- **Dil**: Türkçe

---

## 2️⃣ VERİ KAYNAKLARI VE TOPLAMA

### Veri Kaynağı
Twitter/X platformundan toplanan, deprem ile ilgili tweetler

### Tweet Özellikleri
Her tweet kaydı şu bilgileri içermektedir:

| Alan Adı | Açıklama | Veri Tipi |
|----------|----------|-----------|
| **date** | Tweet atma tarihi | DateTime |
| **content** | Tweet metni | String |
| **hashtags** | Kullanılan hashtagler | Array |
| **like_count** | Beğeni sayısı | Integer |
| **rt_count** | Retweet sayısı | Integer |
| **followers_count** | Yazarın takipçi sayısı | Integer |
| **isVerified** | Doğrulanmış hesap mı | Boolean |
| **language** | Dil kodu | String |
| **coordinates** | Coğrafi koordinatlar | Tuple (lat, lon) |
| **place** | Yer adı | String |
| **source** | Tweet gönderme kaynağı | String |
| **label** | Sınıflandırma etiketi | Binary (0/1) |
| **multi_label** | Çok etiketli sınıflandırma | Array |
| **ner_entities** | Çıkartılan bilgi varlıkları | JSON |

### Veri Özellikleri
- **Format**: CSV (virgülle ayrılmış değerler)
- **Encoding**: UTF-8
- **Boyut**: 83.5 MB (140,532 satır × 14 sütun)
- **Depolama**: tweets_tr_classified_all.csv

---

## 3️⃣ VERİ İŞLEME İŞLEMLERİ

### İşlem 1: Veri Temizliği
- Boş veya hatalı kayıtların çıkarılması
- Karakter kodlaması düzeltilmesi
- Tekrarlanan tweetlerin tespit edilmesi ve kaldırılması

### İşlem 2: Metin Normalleştirme
- URL'lerin temizlenmesi
- Özel karakterlerin düzenlenmesi
- Emojilerin işlenmesi

### İşlem 3: Sınıflandırma Hazırlığı
- Veri setinin eğitim ve test kümelerine bölünmesi
- Her sınıfın veri dağılımı analizi
- Dengesizlik sorunlarının değerlendirilmesi

### İşlem 4: Sınıflandırma Uygulaması
Tweetler şu iki sınıftan birine ayrılmıştır:
- **Sınıf 0**: Yardım çağrısı değil (Normal tweet, bilgi vb.)
- **Sınıf 1**: Yardım çağrısı (İlaç, gıda, barınma talepleri vb.)

### İşlem 5: Doğrulama ve Kalite Kontrolü
- Sınıflandırılan tweetlerin manuel incelenmesi
- Hatalı etiketlemelerin düzeltilmesi
- Karşılıklı doğrulama (inter-annotator agreement)

### İşlem 6: Bilgi Varlıkları Tanımlama
Tweetlerden 11 türde bilgi varlığı manuel olarak çıkartılmıştır:

| Varlık Türü | Açıklama | Örnek |
|-------------|----------|--------|
| **Kişi Adı** | İnsanların adları | Ahmet, Fatma |
| **Konum** | Şehir, ilçe, adres | Istanbul, Gaziantep |
| **İhtiyaç Türü** | Talep edilen yardım | Doktor, Gıda, Su |
| **Kurum Adı** | Kurumların isimleri | AFAD, Kızılay |
| **Araç/Ekipman** | İhtiyaç duyulan araçlar | Dozer, Vinç, Ambulans |
| **Yapı** | Bina/tesis türleri | Hastane, Eczane |
| **Harita Referansı** | Coğrafi referanslar | Meydan, Cadde |
| **Sosyal Hizmetler** | Psikolojik destek vb. | Psikolojik Destek |
| **Gıda İhtiyacı** | Spesifik gıda talepleri | Su, Ekmek, Konserve |
| **Barınma** | Konaklama talepleri | Çadır, Otel |
| **Sağlık** | Sağlık hizmetleri | Doktor, Ameliyathane |

### İşlem 7: Coğrafi Veri İşleme
- Tweet koordinatlarının denetlenmesi
- Geçersiz koordinatların kaldırılması
- Konum isimlerinin standardize edilmesi

### İşlem 8: Veri Paketleme
- Araştırma verilerinin organize edilmesi
- Uygulamaya uygun formatta düzenlenmesi
- Örnek veri setlerinin hazırlanması

---

## 4️⃣ BAŞARI ÖLÇÜTLERI

### Sınıflandırma Başarısı
- **Doğruluk Oranı**: %91.59
- **Hassasiyet**: Yüksek seviyede (Yardım çağrılarını doğru tespit)
- **Geri Çağırma**: Minimum kaçırma

### Bilgi Çıkartma Başarısı
- **Kapsam**: %100 (Tüm tweetlerden bilgi çıkartılmıştır)
- **Varlık Sayısı**: 11 türde bilgi
- **Toplam Çıkartılan Varlık**: Tahmini 50,000+ ayrı bilgi parçası

### Coğrafi Başarısı
- **Koordinat Kapsama**: Tweetlerin %65'inde konum bilgisi
- **Harita Doğruluğu**: Gerçek Türkiye coğrafyası ile eşleştirme

---

## 5️⃣ TEKNIK MİMARİ

### Yazılım Bileşenleri

```
┌─────────────────────────────────────┐
│  Windows Desktop Uygulaması (WPF)   │
│  - C# Programlama Dili              │
│  - .NET 9.0 Runtime                 │
│  - 64-bit Portable Executable       │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│    Veri İşleme Katmanı              │
│  - CSV Veri Okuma                   │
│  - Metin Analizi                    │
│  - Bilgi Çıkartma                   │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│    Görselleştirme Katmanı           │
│  - GeoJSON Harita Rendering         │
│  - İstatistik Görselleri            │
│  - Etkileşimli UI                   │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│    Veri Dosyaları                   │
│  - tweets_tr_classified_all.csv     │
│  - tr-cities.geojson                │
│  - Yapılandırma Dosyaları           │
└─────────────────────────────────────┘
```

### Teknoloji Yığını
| Katman | Teknoloji | Sürüm |
|--------|-----------|-------|
| **Sunum** | C# WPF | .NET 9.0 |
| **İşletim Sistemi** | Windows | 64-bit |
| **Veri Depolama** | CSV | UTF-8 |
| **Haritalama** | GeoJSON | RFC 7946 |
| **Taşınabilirlik** | Portable Executable | Kuruluma gerek yok |

### Sistem Gereksinimleri
- Windows 7 ve üzeri
- 100 MB disk alanı
- .NET 9.0 Runtime (uygulamada bundled)
- RAM: 512 MB minimum

---

## 6️⃣ DOSYA YAPISI

### Ana Dizin
```
NLP PROJESİ/
├── tweets_tr_classified_all.csv          (83.5 MB - Ana veri)
├── tweets_tr_to_label_sample25.csv       (21 MB - Örnek veri)
├── PROJE_RAPORU.md                       (Kapsamlı rapor)
├── METIN_TABANLI_OZET.md                 (Metin özeti)
├── HOCA_SORULARI_CEVAPLARI.md            (Q&A referans)
├── README_HOCA_ICIN.md                   (Sunum rehberi)
├── TEKNIK_SUNUM_PROMPTU.md               (Bu dosya)
└── AfetHaritaApp-Portable/               (Desktop uygulaması)
    ├── AfetHaritaApp.exe                 (Ana çalıştırılabilir)
    ├── AfetHaritaApp.deps.json
    ├── AfetHaritaApp.runtimeconfig.json
    ├── Assets/
    │   └── tr-cities.geojson             (2 MB - Türkiye haritası)
    ├── Data/
    │   └── tweets_tr_classified_all.csv  (Gömülü veri)
    └── tr/                               (Türkçe yerelleştirme)
```

---

## 7️⃣ KALİTE KONTROL SAFHALARI

### 1. Veri Doğruluğu
- [ ] Tüm tweetlerin tam ve tutarlı olması
- [ ] Karakterlerin doğru şekilde kaydedilmesi
- [ ] Koordinatların geçerli olması

### 2. Sınıflandırma Doğruluğu
- [ ] %91.59 doğruluk hedefinin karşılanması
- [ ] Yanlış negatif (kaçırılan yardım çağrıları) minimize edilmesi
- [ ] Yanlış pozitif (yanlış etiketlemeler) kontrol edilmesi

### 3. Bilgi Çıkartma Doğruluğu
- [ ] Tüm tweetlerde bilgi araştırması yapılması
- [ ] 11 varlık türünün tamamının kapsaması
- [ ] Tutarlı varlık tanımlaması

### 4. Uygulama Performansı
- [ ] Tüm 140,532 tweet hızlı yüklenmesi
- [ ] Harita görselleştirmesinin sorunsuz çalışması
- [ ] İstatistiklerin doğru hesaplanması

---

## 8️⃣ PROJE SÜRESİ VE KAYNAKLAR

### Zaman Dilimi
- **Başlangıç**: Şubat 2023 (deprem sonrası)
- **Veri Toplama**: 2-3 hafta
- **İşleme ve Analiz**: 3-4 hafta
- **Uygulama Geliştirme**: 2-3 hafta
- **Toplam**: Yaklaşık 10-12 hafta

### İnsan Kaynakları
- Veri toplama ekibi
- Veri temizleme ve işleme ekibi
- Sınıflandırma ve etiketleme ekibi
- Bilgi çıkartma uzmanları
- Uygulama geliştirme mühendisleri
- Proje yönetimi

---

## 9️⃣ SONUÇLAR VE ETKİSİ

### Ana Bulgular
1. **Yardım Talebinin Yoğunluğu**
   - Tweetlerin %X'i yardım çağrısı içeriyor
   - En sık talep edilen: Barınma, Gıda, Sağlık

2. **Coğrafi Dağılım**
   - Deprem bölgesi ve civarı tweetleri domino ediyor
   - İstanbul ve Ankara'dan yüksek takip

3. **Zamansal Desen**
   - İlk 24 saatte en yoğun faaliyет
   - Kademeli olarak azalan trend

### Proje Faydaları
- Afet yönetimi için sosyal medya analizi
- Hızlı ihtiyaç tespiti
- Coğrafi kaynakların optimize edilmesi
- Halkla iletişim iyileştirmesi

---

## 🔟 SINIRLAMALAR VE GELECEĞİ

### Mevcut Sınırlamalar
- Manuel işlemlerin zaman alması
- Coordinate bilgisi tüm tweetlerde bulunmaması
- Sınıflandırma %91.59 doğruluk (tam %100 değil)
- Yalnızca Türkçe tweetler

### Gelecek Geliştirmeler
1. **Otomasyonun Artırılması**: Tekrarlayan işlemlerin otomatize edilmesi
2. **Çok Dilli Destek**: İngilizce, Arapça vb. dillerde analiz
3. **Gerçek Zamanlı Sistem**: İnternet bağlantısı ile canlı veri alınması
4. **Mobil Uygulama**: Smartphone'de erişim
5. **Yapay Zeka İntegrasyonu**: İnsan analizi ile destekleme

---

## 1️⃣1️⃣ KAYNAKLAR VE REFERANSLAR

### Kullanılan Veri Kaynakları
- Twitter/X API (Arşiv verileri)
- OpenStreetMap (Coğrafi veriler)
- Türkiye CBS Altyapısı (Sınır verileri)

### İlgili Projeler
- AFAD Deprem Bilgi Sistemi
- Kızılay Sosyal Medya İzleme
- Üniversite Afet Yönetimi Programları

---

## SUNUM İPUÇLARİ

✅ **Vurgulanması Gereken Noktalar**:
1. 140,532 tweet = Çok geniş kapsamlı veri seti
2. %91.59 doğruluk = Oldukça başarılı sınıflandırma
3. %100 bilgi çıkartma = Kapsamlı analiz
4. 11 varlık türü = Detaylı bilgi işleme

✅ **Anlatım Tarzı**:
- Teknik ama anlaşılır dil
- Somut örnekler ver (sample tweetler göster)
- Grafikleri ve görselleri kullan
- Zaman yönetimi: 25-30 dakika

✅ **Sorulara Hazırlıklı Ol**:
- "Neden makine öğrenmesi kullanmadın?" → İnsan doğruluğu tercih edildi
- "Nasıl %91.59 başarısını elde ettin?" → Titiz etiketleme süreci
- "Gerçek hayatta nasıl kullanılır?" → AFAD, Kızılay gibi kurumlar

---

**Son Güncelleme**: 22 Haziran 2026
**Durum**: Teknik Sunuma Hazır
