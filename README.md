# 🚨 Afet Durum Analizi

> Deprem döneminde Twitter/X'te paylaşılan Türkçe tweetleri sınıflandıran, konum ve ihtiyaç bilgilerini çıkaran ve sonuçları Türkiye haritası üzerinde görselleştiren bir NLP sistemi.

**BMÜ 460 — Doğal Dil İşleme Dönem Sonu Projesi | Fırat Üniversitesi**

---

## 👥 Ekip

| Öğrenci No | İsim |
|---|---|
|  | Ali Nebi ER |
|  | Ahmet Dağıstanlı |
|  | Mir Bedirhan Erkmen |
|  | Ahmet Melik Yıldırım |

---

## 🎯 Proje Özeti

Deprem gibi kriz anlarında Twitter akışı; yardım çağrıları, ihtiyaç bildirimleri, bağış duyuruları ve kişisel yorumların iç içe geçtiği gürültülü bir ortama dönüşür. Bu proje, o gürültüyü anlamlı veriye çevirmeyi hedefler.

**Üç temel NLP görevi:**

1. **Binary Sınıflandırma** — Tweet gerçek bir yardım çağrısı mı, değil mi?
2. **Çoklu Sınıf Sınıflandırma** — Tweet hangi kategoriye giriyor? (Yardım / İhtiyaç / Bağış / Bilgi / Konu Dışı)
3. **NER (Varlık Tanıma)** — Metinden il, ilçe, adres, kişi, telefon gibi alanları çıkar.

Çıktı, C# WPF masaüstü uygulamasında Türkiye haritası üzerinde şehir bazlı yoğunluk olarak görselleştirilir.

---

## 📊 Veri Seti

| Dosya | Satır | Kolon | Kullanım |
|---|---|---|---|
| `tweets_tr.csv` | 104.637 | 11 | Ham Türkçe ana veri |
| `tweets_tr_to_label_sample25.csv` | 35.133 | 14 | Model eğitimi ve etiketli örneklem |
| `tweets_tr_classified_all.csv` | 140.532 | 14 | Uygulamada kullanılan nihai veri seti |

**Kaynak:** [Turkey and Syria Earthquake Tweets — Kaggle](https://www.kaggle.com/datasets/swaptr/turkey-earthquake-tweets)

### Etiket Şeması

**Binary:**
- `1` → Enkaz altında kalma, kayıp olma, ulaşılamama, mahsur kalma + destekleyici ayrıntı (ad, adres, bina, telefon)
- `0` → Genel yardım ifadesi, dua, duygu, kurtarma haberi, salt malzeme bildirimi

**Çoklu sınıf:**

| Sınıf | Adet (Sample25) | Oran |
|---|---|---|
| Bilgi Paylaşımı | 16.034 | %45,64 |
| Bağış ve Gönüllülük | 6.029 | %17,16 |
| Konu Dışı | 5.692 | %16,20 |
| Yardım Çağrısı | 4.671 | %13,30 |
| İhtiyaç | 2.707 | %7,71 |

---

## 🤖 Modeller

**Temel model:** [`dbmdz/bert-base-turkish-cased`](https://huggingface.co/dbmdz/bert-base-turkish-cased) (BERTurk)

**Eğitim ortamı:** Google Colab

**Hiperparametreler:**

| Parametre | Değer |
|---|---|
| Max Token Length | 128 |
| Epoch | 3 |
| Batch Size | 16 |
| Learning Rate | 2e-5 |
| Train/Test Split | %80 / %20 |

### Binary Model
Gerçek yardım çağrılarını diğer içeriklerden ayırır. Sınıf dengesizliğine (yalnızca %10,37 pozitif) rağmen yardım tweetlerinin büyük çoğunluğunu yakalamayı başardı.

### Kategori Modeli
Tweetleri beş afet iletişim sınıfına ayırır. İçerik yakınlığından kaynaklanan sınır hatalarını azaltmak için model çıktısı kural tabanlı kontrol adımından geçirildi.

### NER Modeli
Il, ilçe, mahalle/köy, cadde/sokak, bina/site, kapı/daire no, kişi, kişi durumu, yer tarifi, ihtiyaç ve telefon numarası alanlarını çıkarmak için eğitildi. Tweetlerdeki yazım hataları, kısaltmalar ve eksik adresler nedeniyle kural tabanlı kontrollerle desteklendi.

---

## 📍 Nihai Sınıflandırma Sonuçları

140.532 tweetlik nihai veri setinde:

| Sınıf | Adet | Oran |
|---|---|---|
| Bilgi Paylaşımı | 78.396 | %55,79 |
| Konu Dışı | 28.439 | %20,24 |
| Bağış ve Gönüllülük | 13.734 | %9,77 |
| Yardım Çağrısı | 11.816 | %8,41 |
| İhtiyaç | 8.147 | %5,80 |

**NER kapsamı:** 88.152 tweet (%62,73) en az bir varlık alanı içeriyor.

En sık doldurulan NER alanları: `il` (67.447) · `ilçe` (31.506) · `kişi durumu` (18.793) · `no` (15.748) · `mahalle/köy` (15.095)

---

## 🖥️ C# WPF Masaüstü Uygulaması

Sınıflandırılmış veriyi Türkiye haritası üzerinde görselleştiren masaüstü izleme uygulaması.

**Özellikler:**
- Şehir bazlı yoğunluk renklendirmesi (GeoJSON il sınırları)
- Sınıf filtreleri: Yardım / İhtiyaç / Bağış / Bilgi / Konu Dışı
- Şehir arama
- Özet metrik kartları: Toplam Tweet, Yardım Tweeti, İhtiyaç, Bağış, Bilgi

**Bileşenler:**

| Dosya | Görev |
|---|---|
| `CsvTweetLoader.cs` | CSV okuma, NER ayrıştırma, şehir istatistikleri |
| `GeoJsonProvinceMap.cs` | Türkiye il sınırlarını WPF Geometry'ye dönüştürme |
| `MainWindow.xaml` | Harita, filtreler, arama, şehir ve metrik kartları |
| `AfetHaritaApp.csproj` | Asset yönetimi, Türkçe kaynak konfigürasyonu |

---

## 🛠️ Teknoloji Yığını

```
NLP / ML          Python · HuggingFace Transformers · BERT · Google Colab
Masaüstü Uygulama C# · WPF · .NET
Harita            GeoJSON (Türkiye il sınırları)
Veri              CSV · Pandas
```

---

## ⚠️ Sınırlılıklar

- Bu sistem gerçek bir afet yönetim platformunun yerini tutmaz; akademik bir prototiptir.
- NER performansı yazım hataları ve eksik adres bilgisi içeren tweetlerde düşmektedir.
- Veri seti 2023 Kahramanmaraş depremiyle sınırlıdır; farklı afet türleri için yeniden etiketleme gerekebilir.

---

## 📚 Kaynaklar

1. Tripathi, S. (2023). [Turkey and Syria Earthquake Tweets](https://www.kaggle.com/datasets/swaptr/turkey-earthquake-tweets). Kaggle.
2. MDZ Digital Library Team. (2020). [dbmdz/bert-base-turkish-cased](https://huggingface.co/dbmdz/bert-base-turkish-cased). Hugging Face.
3. alpers. [Turkey-Maps-GeoJSON](https://github.com/alpers/Turkey-Maps-GeoJSON). GitHub.
4. Microsoft. (2026). [Windows Presentation Foundation documentation](https://learn.microsoft.com/dotnet/desktop/wpf/). Microsoft Learn.
