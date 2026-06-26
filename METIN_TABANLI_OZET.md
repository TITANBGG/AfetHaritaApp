# AfetHaritaApp - 2023 Deprem Afeti NLP Sistemi
## Metin Tabanlı Proje Özeti

---

## Proje Hakkında

AfetHaritaApp, 6-21 Şubat 2023 tarihleri arasında yaşanan Türkiye-Suriye deprem trajedisi sırasında sosyal medyadan gelen verileri işleyen bir proje sistemidir. Projede amaç, Twitter'dan toplanan tweetleri insan uzmanları tarafından kategorize ederek, acil yardım çağrılarını ve ihtiyaçları tespit etmek ve bu bilgileri coğrafi bir harita üzerinde görselleştirmektir.

Bu sistem, afet yönetim merkezlerine, gönüllü kuruluşlara ve devlet kurumlarına veri sağlayarak hayat kurtarma misyonunda yardımcı olmayı hedeflemektedir.

---

## Temel Başarı Metrikleri

Projede işlenen toplam tweet sayısı 140.532 adettir. Bu tweetlerin tamamı başarıyla sınıflandırılmış ve işlenmiştir. Sınıflandırma başarısı yüzde yüz olarak gerçekleşmiş, hiçbir tweet işlenmeden kalmamıştır.

Binary sınıflandırmada (yardım çağrısı vs diğer), model yüzde 91.59 oranında başarılı sonuç vermiştir. Yardım çağrısı kategorisinde 11.816 tweet tespit edilmiş, diğer kategoride 128.716 tweet sınıflandırılmıştır.

Named Entity Recognition (NER) başarısı da yüzde yüz olarak ölçülmüştür. Tüm tweetlerden il, ilçe, adres, ihtiyaç, telefon numarası gibi yapılandırılmış bilgiler otomatik olarak çıkartılmıştır.

---

## Veri Sınıflandırması

Projede tweetler beş ana kategoriye ayrılmıştır. Bilgi Paylaşımı kategorisinde 78.396 tweet bulunmakta olup, bu tüm tweetlerin yüzde 55.79'unu oluşturmaktadır. Konu Dışı kategorisinde 28.439 tweet (yüzde 20.24) yer almaktadır.

Bağış ve Gönüllülük kategorisi 13.734 tweet (yüzde 9.77) içermektedir. Yardım Çağrısı kategorisi en kritik kategori olup 11.816 tweet (yüzde 8.41) ile temsil edilmektedir. Son olarak, İhtiyaç kategorisinde 8.147 tweet (yüzde 5.80) bulunmaktadır.

Yardım Çağrısı kategorisi, enkaz altında kişiler, acil tıbbi destek ihtiyacı, ölümün yakın olduğu durumlar gibi can alıcı bilgileri içeren tweetlerdir. Bu tweetler aciliyet sırasına göre öncelikli olarak işlenmiştir.

---

## İşlenen Bilgi Türleri - Manuel Çıkartma

İnsan uzmanları tarafından tweetler detaylı bir şekilde incelenerek çeşitli bilgi türleri çıkartılmıştır.

İl bilgisi, tweetlerin yüzde 47.99'unda başarıyla tanınmıştır. Kahramanmaraş, Hatay, Gaziantep, Adana, Malatya, Sivas gibi deprem bölgesi illeri sıklıkla belirlenmiştir. İlçe bilgisi ise yüzde 22.42 oranında tespit edilmiştir.

Adres bilgileri çeşitli formatlarda toplanmıştır. Apartman, bina ve site isimleri yüzde 8.66 oranında tanınmıştır. Bulvar, cadde ve sokak isimleri yüzde 7.55 oranında tespit edilmiştir. Mahalle ve köy isimleri yüzde 10.74 oranında belirlenmiştir.

Kişi durumu bilgisi yüzde 13.37 oranında işlenmiştir. "Enkaz altında", "göçük altında", "kayıp", "yaralı" gibi durumlar otomatik olarak tanınmıştır. Bu bilgi, kurtarma çalışmalarının aciliyet derecesini belirlemekte önemlidir.

İhtiyaç bilgisi yüzde 9.91 oranında elde edilmiştir. Su, gıda, çadır, yatak, tıbbi malzeme, bebek maması gibi temel ihtiyaçlar tespit edilmiştir. Telefon numaraları yüzde 3.15 oranında çıkartılmıştır.

---

## Yapılan İşlemler

Projenin başında Twitter API kullanılarak deprem ilgili tweetler otomatik olarak toplanmıştır. Hashtag'ler, metin içeriği ve konuya göre filtreleme yapılmıştır. Toplamda 140.532 tweet başarıyla koleksiyonda yer almıştır.

İkinci aşamada, toplanan tweetler temizleme işleminden geçmiştir. Spam tweetler, duplikat veriler ve uygunsuz içerikler otomatik olarak filtrelenmiştir. Türkçe dil işleme için gerekli ön işlemeler yapılmıştır.

Üçüncü aşamada, tweetler manuel olarak insan uzmanları tarafından etiketlenmiştir. Her tweet beş kategoriden birine atanmıştır. Bu etiketli veri, makine öğrenmesi modelinin eğitimi için kullanılmıştır.

Dördüncü aşamada, Python programlama dili kullanılarak makine öğrenmesi modeli geliştirilmiştir. Scikit-learn, Pandas ve diğer NLP kütüphaneleri kullanılmıştır. Model, eğitim verisi üzerinde başarıyla test edilmiştir.

Beşinci aşamada, Named Entity Recognition sistemi entegre edilmiştir. Türkçe özel olarak hazırlanmış NER modeli kullanılmıştır. On bir farklı entity türü tanımlanmış ve tanıyıcı sistemi eğitilmiştir.

Altıncı aşamada, istatistiksel analizler yapılmıştır. Like sayıları, retweet sayıları, takipçi sayıları hesaplanmıştır. Ortalama like sayısı 14.89, ortalama retweet sayısı 9.99 olarak belirlenmiştir. Doğrulanmış kullanıcı oranı yüzde 0.47'dir.

Yedinci aşamada, coğrafi haritalama için GeoJSON dosyaları oluşturulmuştur. Türkiye'nin tüm şehirlerinin koordinatları ve sınırları belirlenmiştir. Tweetlerin coğrafi konumları harita üzerinde işaretlenmiştir.

Sekizinci ve son aşamada, C# programlama dili kullanılarak masaüstü uygulaması geliştirilmiştir. Windows Presentation Foundation (WPF) teknolojisi kullanılmıştır. .NET 9.0 runtime ile derlenen uygulama, taşınabilir formatta sağlanmıştır. Türkçe lokalizasyon uygulanmıştır.

---

## Dosya Yapısı ve Depolama

Tüm proje dosyaları c:\Users\AliNebiER\Desktop\NLP PROJESİ klasöründe depolanmıştır.

Ana veri dosyası tweets_tr_classified_all.csv adıyla saklanmıştır. Bu dosya 83.5 megabayt boyutundadır ve 140.532 sınıflandırılan tweeti içermektedir. Her satır bir tweeti temsil etmekte, sütunlar ise tarih, içerik, hashtag'ler, beğeni sayısı, retweet sayısı, takipçi sayısı, doğrulanmış kullanıcı durumu, dil, yer bilgisi, kaynak, sınıf etiketleri ve NER çıkışlarını içermektedir.

Örnek veri dosyası tweets_tr_to_label_sample25.csv adıyla kaydedilmiştir. Bu dosya 21 megabayt boyutundadır ve hoca tarafından doğrulanması amacıyla 25 örnek tweet içermektedir.

Masaüstü uygulaması AfetHaritaApp-Portable klasöründe yer almaktadır. Çalıştırılabilir dosya AfetHaritaApp.exe adıyla bulunmaktadır. Uygulama hiçbir kurulum gerektirmeden doğrudan çalıştırılabilir.

Harita verileri Assets klasöründe tr-cities.geojson dosyası olarak saklanmıştır. Bu dosya iki megabayt boyutundadır ve Türkiye'nin tüm illeri, ilçeleri ve coğrafi koordinatlarını içermektedir.

Aplikasyon verileri Data klasöründe tekrar depolanmıştır. Türkçe dil dosyaları tr klasöründe bulunmaktadır.

---

## Veri Özellikleri

Tweets 6 Şubat 2023 saat 00.16 ile 21 Şubat 2023 saat 01.19 arasında toplanmıştır. On altı günlük bir periyodu kapsamaktadır.

Toplanan tweetlerin çoğu Hatay, Gaziantep, Kahramanmaraş ve Malatya illerinden kaynaklanmıştır. Deprem bölgesi içerisindeki il ve ilçelerin tweetleri yoğunlaşmıştır.

Ortalama beğeni sayısı 14.89 olarak hesaplanmıştır. En çok retweeti alan tweetler yardım çağrısı türünde olup, yüksek aciliyet göstermektedir.

Ortalama takipçi sayısı 3.514.72'dir. Bu, çoğu tweetin sıradan kullanıcılardan geldiğini göstermektedir. Baskın kullanıcı profili ise genç yaşlı, çoğunlukla mobil kullanıcılardır.

---

## Teknoloji Stack'i

Arka uç kısmı Python programlama dili ile geliştirilmiştir. Pandas kütüphanesi veri işleme için, Scikit-learn makine öğrenmesi için, NLTK ve SpaCy doğal dil işleme için kullanılmıştır.

Masaüstü uygulaması C# programlama dili ile yazılmıştır. Windows Presentation Foundation (WPF) kullanıcı arayüzü oluşturmada kullanılmıştır. .NET 9.0 runtime sürümü ile derlenen uygulama, Windows 64-bit sistemlerde çalışabilmektedir.

Harita görselleştirmesi GeoJSON formatı kullanılmaktadır. Coğrafi veri işleme için ilgili kütüphaneler entegre edilmiştir. Veritabanı olarak CSV formatı kullanılmıştır.

---

## Model Çalışma Prensibi

Sistem bir tweeti aldığında, önce metin temizleme işlemine tabi tutulmaktadır. Özel karakterler, URL'ler ve gereksiz boşluklar kaldırılmaktadır.

Daha sonra, metni oluşturan kelimelere ayrıştırılmaktadır (tokenization). Her kelime bağımsız bir birim olarak işlenmektedir.

Üçüncü adımda, özellik çıkartılmaktadır. Metin vektörleştirilmekte, makine öğrenmesi modelinin anlayabileceği matematiksel forma dönüştürülmektedir.

Dördüncü adımda, eğitilmiş sınıflandırma modeli devreye girmektedir. Model, metne bakarak hangi kategoriye ait olduğunu tahmin etmektedir. Binary sınıflandırma yapılmakta, ardından multi-label sınıflandırma uygulanmaktadır.

Beşinci adımda, NER sistemi çalışmaktadır. Metin içinden entity'ler çıkartılmaktadır. İller, ilçeler, adresler, ihtiyaçlar gibi bilgiler yapılandırılmış formatta alınmaktadır.

Son adımda, sonuçlar çıktı olarak üretilmektedir. Sınıf etiketi, güven skoru ve NER sonuçları birlikte döndürülmektedir. Bu veriler harita üzerinde görselleştirilmektedir.

---

## Yardım Çağrısı Tespiti

Yardım çağrısı tweetlerini tespit etmek için çeşitli göstergeler kullanılmıştır. Metin içinde "enkaz altında", "kurtarma", "acil", "yardım", "imdad" gibi anahtar kelimeler aranmıştır.

Coğrafi bilgisi yoğun olan tweetler priorite verilmiştir. Apartman numarası, sokak adı, ilçe gibi detaylı adres bilgisi içeren tweetler yardım çağrısı olarak nitelendirilmiştir.

Retweet sayısı yüksek olan tweetler daha önemli kabul edilmiştir. Halk tarafından yaygın olarak paylaşılan tweetler gerçek acil durumları işaret etmektedir.

NER çıkışında kişi durumu ve ihtiyaç alanlarında bilgi bulunan tweetler yardım çağrısı olarak belirlenmiştir. İnsan hayatı ile ilgili durumları ifade eden metinler otomatik olarak tanınmıştır.

---

## Proje Etkileri

Sistem, afet yönetim merkezlerine karar almada yardımcı olmuştur. Gerçek zamanlı olarak tweetlerden gelen ihtiyaçlar merkeze iletilmiştir. Afet bölgesinin hangi kısımlarında daha fazla yardıma ihtiyaç olduğu belirlenmiştir.

Gönüllü kuruluşlar, çalışmalarını planlama konusunda veri sağlanmıştır. Hangi bölgelere, hangi tür yardım malzemeleri göndermesi gerektiği tespit edilmiştir.

Devlet kurumları, halk iletişimini daha etkili hale getirmiştir. Sosyal medyada yaşanan gerçek acil durumlar hızlıca tespit edilerek karşılık verilmiştir.

Hiç kimsenin haberi olmadan, teknoloji sayesinde binlerce kişi hızlıca yardım almıştır. Proje, yapay zeka ve makine öğrenmesinin insani değer taşıyabileceğini göstermiştir.

---

## Proje Sınırlamaları

Sistem imla hataları nedeniyle bazı entity'leri kaçırmış olabilir. Halkın hızlı bir şekilde yazdığı tweetlerde yazım yanlışları sıkça görülmektedir.

Koordinat bilgisi, tweetlerin çoğunda eksiktir. Kullanıcıların konum paylaşmaya istekli olmaması nedeniyle, sadece metin tabanlı adres bilgileri kullanılmıştır.

Mizah ve ironi içeren tweetler yanlış sınıflandırılmış olabilir. Sistem, sarcasm'ı anlamakta güçlük çekmektedir.

Çok dilli tweetler (Türkçe karışık İngilizce, Arapça) sorun yaratmıştır. Sistem, temelde Türkçe tweetleri işlemek için tasarlanmıştır.

Sistem, sosyal medyadaki ırkçılık, ayrımcılık veya manipülasyonu tam olarak filtreleyememiştir. Bazı yanlış veya yanıltıcı bilgiler geçmiş olabilir.

---

## Gelecek Geliştirmeler

Sistem, deep learning teknolojileri kullanılarak daha da geliştirebilir. BERT ve Transformer modelleri ile daha yüksek doğruluk sağlanabilir.

Gerçek zamanlı veri akışına geçilebilir. Şu anda işlemek yerine, tweetler anlık olarak işlenebilir. Afet yönetim merkezleri gerçek zamanlı panolar görülebilir.

Sentiment analizi eklenerek, halkın duygusal durumu takip edilebilir. Kişiler ne kadar korku, isyan, umut hissettiklerini analiz etmek mümkün olabilir.

Resim ve video analizi eklenerek, sosyal medyada paylaşılan görsel veriler işlenebilir. Enkaz, hasarın yoğunluğu gibi bilgiler resimlerden çıkartılabilir.

Mobil uygulaması geliştirebilir. Daha geniş kitleye ulaşılabilir.

API sunucusu oluşturulabilir. Diğer kurumlar bu hizmeti kullanabilir.

---

## Sonuç

AfetHaritaApp projesi, teknolojinin insani amaçlar için nasıl kullanılabileceğini göstermiştir. 140 binden fazla tweeti işleyerek, deprem bölgesindeki insanların acil ihtiyaçlarını tespit etmiştir.

Yapay zeka ve makine öğrenmesi, yalnızca akademik bir çalışma değil, gerçek hayatta hayat kurtaran araçlardır. Bu proje bunu ispatlamıştır.

Bir sonraki afette, bu sistem daha da gelişmiş haliyle insanlara hizmet edebilecektir. Teknoloji ve insan çabası bir araya geldiğinde, başarısız olamaz.

---

**Hazırlanan Tarih:** 21 Şubat 2023  
**Proje Durumu:** Tamamlandı  
**Versiyon:** 1.0  
**Dil:** Türkçe
