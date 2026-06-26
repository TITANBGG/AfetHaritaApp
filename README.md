1
BMÜ 460 Doğal Dil İşleme Dersi
Dönem Sonu Proje Raporu

Proje Başlığı: Afet Durum Analizi

Proje Ekibindeki Kişiler:  Ali Nebi ER,  Ahmet Dağıstanlı,  Mir Bedirhan 
Erkmen,  Ahmet Melik Yıldırım


1. Amaç ve Hedefler
Bu projede, afet dönemlerinde Twitter/X üzerinde paylaşılan Türkçe tweetleri daha düzenli okunabilecek bir 
yapıya dönüştürmeyi amaçladık. Deprem gibi kriz anlarında yardım çağrıları, ihtiyaç bildirimleri, bağış 
duyuruları, haber paylaşımları ve kişisel yorumlar aynı akış içinde yer alıyor. Bu yoğunluk, acil bilgiye 
ulaşmayı zorlaştırıyor.
Çalışmanın çıkış noktası bu sorundu. Önce tweetleri anlamlı sınıflara ayırdık. Ardından metinlerin içinden il, 
ilçe, adres, kişi, kişi durumu, ihtiyaç ve telefon gibi alanları çıkarmaya çalıştık. Böylece ham sosyal medya 
verisi, afet yönetimi açısından daha kolay incelenebilecek bir veri haline geldi.

3. Veri Seti Hazırlığı
Veri tarafında üç dosya üzerinden ilerledik. İlk dosyada Kaggle’dan alınan ham Türkçe tweetler yer aldı. İkinci 
dosya olan sample25, eğitimde kullandığımız etiketli örnek veri setiydi. Üçüncü dosyada ise model çıktıları ve 
kural düzeltmeleri birlikte yer aldı.
Bu ayrım çalışırken işi kolaylaştırdı. Ham veri, eğitim verisi ve uygulamada kullanılacak son veri birbirinden 
ayrılınca hem model eğitimi hem de uygulama geliştirme süreci daha takip edilebilir oldu.
Dosya Satır Kolon Kullanım amacı
tweets_tr.csv 104.637 11 Ham Türkçe ana veri
tweets_tr_to_label_sample25.csv 35.133 14
Model eğitimi ve etiketli 
örneklem
tweets_tr_classified_all.csv 140.532 14
Uygulamada kullanılan 
veri seti
Tablo 1. Projede kullanılan veri dosyaları
2.1. Etiketleme Yapısı
Binary etiketleme kısmında yalnızca somut yardım çağrılarını pozitif sınıf olarak aldık. Bir tweetin 1 olması 
için enkaz altında kalma, kayıp olma, ulaşılamama ya da mahsur kalma gibi bir durumun açıkça geçmesi 
gerekiyordu. Bunun yanında kişi adı, adres, bina bilgisi veya telefon gibi destekleyici ayrıntılar da aradık.
Genel yardım ifadelerini, dua veya duygu içeren paylaşımları, kurtarma haberlerini ve sadece malzeme ihtiyacı 
bildiren tweetleri 0 sınıfında tuttuk. Çoklu sınıflandırmada beş grup kullandık: Yardım Çağrısı, İhtiyaç, Bağış 
ve Gönüllülük, Bilgi Paylaşımı ve Konu Dışı. NER bölümünde ise il, ilçe, mahalle veya köy, cadde veya 
sokak, bina veya site, kapı/daire numarası, kişi, kişi durumu, yer tarifi, ihtiyaç ve telefon numarası alanlarına 
odaklandık.
2
2.2. Sample25 Çoklu Sınıf Dağılımı
Sınıf Adet Oran
Bilgi Paylaşımı 16.034 %45,64
Bağış ve Gönüllülük 6.029 %17,16
Konu Dışı 5.692 %16,20
Yardım Çağrısı 4.671 %13,30
İhtiyaç 2.707 %7,71
2.3. Sample25 Binary Dağılımı
Sınıf Adet Oran
0.0 31.488 %89,63
1.0 3.645 %10,37
4. Model Eğitimi
Model eğitimlerini Google Colab ortamında yürüttük. Temel model olarak Türkçe metinlerde yaygın 
kullanılan dbmdz/bert-base-turkish-cased modelini seçtik. Veri setini sınıf dağılımını koruyacak şekilde yüzde 
80 eğitim ve yüzde 20 test olarak ayırdık.
Eğitimlerde maksimum token uzunluğunu 128 olarak belirledik. Epoch sayısını 3, batch size değerini 16 ve 
learning rate değerini 2e-5 aldık. Bu ayarlar, veri boyutu ve Colab çalışma süresi açısından dengeli bir 
başlangıç sağladı.
3.1. Binary Model
Binary modelin görevi, bir tweetin gerçek bir yardım çağrısı içerip içermediğini ayırmaktı. Confusion matrix 
sonucunda modelin özellikle ilgisiz paylaşımları ayırmada başarılı olduğu görüldü. Pozitif sınıf daha az örneğe 
sahipti; buna rağmen model yardım çağrılarını büyük ölçüde yakalayabildi.
Bu model, uygulamada yardım tweeti sayısını şehir bazında göstermek için temel alanlardan biri oldu. Bu 
nedenle sadece genel afet içeriklerini değil, gerçekten aksiyon gerektiren çağrıları ayırması önemliydi.
Şekil 1. Binary model eğitim grafiği
3
Şekil 2. Binary model confusion matrix
3.2. Kategori Modeli
Kategori modeli tweetleri beş afet iletişim sınıfına ayırdı. Sonuçlarda Bilgi Paylaşımı, Bağış ve Gönüllülük, 
Konu Dışı ve Yardım Çağrısı sınıflarında yüksek doğru tahmin sayıları elde ettik.
Hataların bir kısmı birbirine yakın içeriklerden kaynaklandı. Örneğin bazı tweetler hem bilgi verme hem de 
yardım duyurusu niteliği taşıyordu. Bu yüzden model çıktısından sonra belirli kurallarla son veri setini tekrar 
kontrol ettik.
Şekil 3. Kategori modeli eğitim grafiği
4
Şekil 4. Kategori modeli confusion matrix
3.3. NER Modeli
NER modelini tweetlerde geçen konum ve adres parçalarını ayırmak için denedik. Model, O etiketi dışında il 
ve ilçe gibi varlıkları da öğrenebildi. Buna karşın tweetlerdeki yazım hataları, kısaltmalar ve eksik adresler 
NER tarafını diğer modellere göre daha zor hale getirdi.
Bu nedenle nihai veri setinde NER çıktısını tek başına bırakmadık. Kural tabanlı kontrollerle il, ilçe, ihtiyaç ve 
telefon gibi alanları destekledik. Sonuçları da uygulamanın okuyabileceği dictionary formatına çevirdik.
Şekil 5. NER modeli eğitim grafiği
5
Şekil 6. NER modeli etiket dağılımı
Şekil 7. NER modeli confusion matrix
5. Nihai Sınıflandırma Sonuçları
Nihai veri dosyasında 140.532 satır yer aldı. Bu dosyada binary etiketler, çoklu sınıf etiketleri ve NER alanları 
birlikte tutuldu. Model tahminlerini afet alanı için belirlediğimiz kurallarla tekrar gözden geçirdik.
Bu aşamadan sonra veri, C# uygulamasının doğrudan okuyabileceği hale geldi. Böylece uygulama şehir, sınıf 
ve ihtiyaç bilgilerini ayrı ayrı hesaplayabildi.
6
4.1. Çoklu Sınıf Dağılımı
Sınıf Adet Oran
Bilgi Paylaşımı 78.396 %55,79
Konu Dışı 28.439 %20,24
Bağış ve Gönüllülük 13.734 %9,77
Yardım Çağrısı 11.816 %8,41
İhtiyaç 8.147 %5,80
4.2. Binary Dağılımı
Sınıf Adet Oran
0 128.716 %91,59
1 11.816 %8,41
4.3. NER Alanları
NER alanlarından en az biri dolu olan satır sayısı 88.152 oldu. Bu sayı, nihai veri setinin yaklaşık 
%62,73’ünde en az bir varlık bilgisinin çıkarılabildiğini gösteriyor.
En sık dolan alanlar il ve ilçe bilgileri oldu. Telefon, yer tarifi ve kişi bilgisi gibi alanlarda sayı daha düşük 
kaldı; bu durum tweetlerin çoğunda açık adres veya iletişim bilgisinin bulunmamasından kaynaklandı.
NER alanı Dolu satır
il 67.447
ilçe 31.506
kişi durumu 18.793
no 15.748
mahalle veya köy 15.095
ihtiyaç 13.924
apartman veya bina veya site 12.175
bulvar veya cadde veya sokak 10.615
kişi 10.574
telefon numarası 4.427
yer 4.290
Tablo 2. NER alanlarına göre dolu satır sayıları
6. C# Masaüstü Uygulaması
Projenin son kısmında C# WPF ile bir masaüstü izleme uygulaması geliştirdik. Uygulama sınıflandırılmış CSV 
dosyasını okuyup şehir bazlı istatistik üretiyor. Türkiye haritası için GeoJSON il sınırlarını kullandık; bu 
sayede harita gerçek şehir sınırlarına göre dinamik renklendiriliyor.
7
Arayüzü sade tutmaya çalıştık. Üst bölümde sınıf filtreleri ve arama alanı yer alıyor. Harita bölümünde şehirler 
yoğunluğa göre renklendiriliyor. Alt kısımdaki kartlarda toplam tweet, yardım tweeti, ihtiyaç, bağış ve 
bilgi/konu dışı sayıları özetleniyor.
Şekil 8. Afet Harita uygulaması arayüzü
Tablo 3. Uygulama bileşenleri
7. Sonuç
Proje sonunda Türkçe afet tweetlerini sınıflandıran, metinlerden temel varlıkları çıkaran ve sonuçları Türkiye 
haritası üzerinde gösteren çalışan bir prototip hazırladık. Eğitim grafikleri ve confusion matrix sonuçları, 
modellerin genel olarak tutarlı çalıştığını gösterdi.
Uygulama tarafında kullanıcı, sınıflara göre filtreleme yaparak hangi şehirlerde yardım çağrısı, ihtiyaç veya 
bilgi paylaşımı yoğunluğunun arttığını hızlıca inceleyebiliyor. Bu çalışma gerçek bir afet yönetim sistemi 
yerine geçmez; ancak sosyal medya verisinin nasıl işlenip görselleştirilebileceğini gösteren uygulanabilir bir 
örnek sunar.
Bileşen Görev
CsvTweetLoader.cs CSV okuma, NER ayrıştırma ve şehir istatistikleri
GeoJsonProvinceMap.cs Türkiye il sınırlarını WPF Geometry nesnelerine 
dönüştürme
MainWindow.xaml Harita, filtreler, arama, şehir kartı ve metrik kartları
AfetHaritaApp.csproj Data/Assets dosyalarını publish paketine ekleme ve 
sadece Türkçe kaynakları tutma
8
8. Kaynaklar
[1] Tripathi, S. (2023). Turkey and Syria Earthquake Tweets [Veri seti]. Kaggle. 
https://www.kaggle.com/datasets/swaptr/turkey-earthquake-tweets
[2] MDZ Digital Library Team. (2020). dbmdz/bert-base-turkish-cased: BERTurk cased Turkish BERT model 
[Ön eğitilmiş dil modeli]. Hugging Face. https://huggingface.co/dbmdz/bert-base-turkish-cased
[3] alpers. (t.y.). Turkey-Maps-GeoJSON [Türkiye il sınırları GeoJSON verisi]. GitHub. 
https://github.com/alpers/Turkey-Maps-GeoJSON
[4] Microsoft. (2026). Windows Presentation Foundation documentation. Microsoft Learn. 
https://learn.microsoft.com/dotnet/desktop/wpf/
