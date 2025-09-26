# AKBANK Derin Öğrenme Bootcamp

# Giriş
Bu proje, Akbank Derin Öğrenme Bootcamp kapsamında geliştirilmiş olup, Intel Image Classification veri seti kullanılarak görsel sınıflandırma (image classification) üzerine odaklanmaktadır. Veri seti; Buildings, Forest, Glacier, Mountain, Sea, Street olmak üzere 6 farklı sınıftan görseller içermektedir.

Amaç, CNN tabanlı bir model eğiterek görselleri doğru sınıfa atamak ve elde edilen sonuçları değerlendirmektir.

# Metrikler
1. Veri İncelemesi ve Ön İşleme

Kullanılan veri seti, Kaggle’daki Intel Image Classification veri kümesi olmuştur ve 6 sınıf içerir: Buildings, Forest, Glacier, Mountain, Sea, Street.

Veri seti incelenmiş, görsellerin boyut ve renk formatları kontrol edilmiştir.

Görseller 150x150 piksel boyutuna yeniden ölçeklendirilmiş ve normalize edilmiştir (0–1 aralığı).

Veri setinde sınıf dağılımı kontrol edilmiş; dengesizlik tespit edilmesi durumunda sınıf ağırlıkları uygulanmıştır.

Görseller üzerinde veri arttırma teknikleri uygulanmıştır:

1. Dikey ve Yatay Flip:

Görsellerin dikey (üst-alt) veya yatay (sol-sağ) olarak çevrilmesidir.
Modelin farklı yönlerdeki varyasyonları tanımasını sağlar ve veriyi çoğaltır. Örneğin bir ağaç veya bina resmi sağa çevrilmiş veya tersine çevrilmiş olsa da sınıf doğru tanınabilir.

2. Rastgele Döndürme (Rotation):

Görsellerin belirli bir açı aralığında rastgele döndürülmesidir (ör. -30° ile +30°).
Modelin nesneleri farklı açılarda tanımasını sağlar ve verinin çeşitliliğini artırır. Bu sayede model, görsellerin sadece belirli açılardaki versiyonlarına bağımlı kalmaz.

3. Parlaklık ve Kontrast Değişiklikleri:

Görsellerin ışık ve kontrast seviyelerinin rastgele değiştirilmesidir.
Farklı ışık koşullarında çekilmiş görsellerle modelin daha dayanıklı ve genelleyici olmasını sağlar. Örneğin bir fotoğraf güneşli veya bulutlu bir ortamda çekilmiş olsa da sınıf doğru tanınabilir.
Bu sayede modelin genelleme kapasitesi artırılmıştır.

2. Model Eğitimi

Eğitilen model CNN tabanlı bir mimari kullanmıştır:
Conv2D Katmanları → Görsel Özellik Çıkarımı

Görsellerin kenar, renk, doku ve şekil gibi özelliklerini yakalamak için kullanılır.
Filtreler (kernel) aracılığıyla görseldeki önemli desenler çıkarılır.

2. MaxPooling → Boyut Küçültme ve Önemli Özellikleri Koruma

Özellik haritalarının boyutunu küçültür, hesaplamayı hafifletir.
Aynı zamanda en baskın özellikleri koruyarak modelin daha sağlam öğrenmesini sağlar.

3. Dropout → Overfitting Önleme

Eğitim sırasında bazı nöronlar rastgele kapatılır, böylece model belirli özelliklere fazla bağımlı olmaz.
Bu, modelin genelleme kabiliyetini artırır ve overfitting’i azaltır.

4. Dense Katmanlar → Sınıflandırma

Tüm öğrenilen özellikleri birleştirir ve sınıflar arasında tahmin yapar.
Genellikle CNN’in son katmanlarıdır ve modelin çıktısını oluşturur.

Modelin eğitiminde Adam optimizer ve categorical_crossentropy loss fonksiyonu kullanılmıştır.
Eğitim sürecinde EarlyStopping uygulanarak validation loss değeri artmaya başladığında eğitim durdurulmuştur.
En iyi model ModelCheckpoint ile kaydedilmiştir.

3. Grad-CAM ve Açıklanabilirlik Analizi

Modelin karar mekanizmasını incelemek için Grad-CAM heatmap yöntemi uygulanmıştır.
Her test görseli için heatmap oluşturularak modelin hangi görsel bölgelere odaklandığı gözlemlenmiştir.

Bulgular:

Modelin doğru sınıflandırdığı görsellerde önemli özelliklerin (ör. binalar, dağ zirveleri, deniz yüzeyi) üzerinde yüksek dikkat olduğu belirlenmiştir.

Yanlış sınıflanan görsellerde ise modelin dikkatinin farklı bölgelerde toplandığı ve sınıf karışıklığının bu nedenle gerçekleştiği görülmüştür.

4. Model Değerlendirme ve Metrikler

Accuracy (Doğruluk): Modelin test verisi üzerindeki doğruluk oranı %88–%92 aralığında elde edilmiştir.

Loss (Kayıp): Hem eğitim hem validation loss değerleri düzenli olarak azalmış, eğitim boyunca stabil bir düşüş gözlemlenmiştir.

Confusion Matrix:

Çoğu sınıf doğru tahmin edilmiştir.
Görsel olarak benzer olan sınıflar arasında sınırlı karışıklık yaşanmıştır: örneğin Mountain ve Glacier.

Precision / Recall / F1-score:

Sınıf bazında precision değerleri yüksek tutulmuş, modelin yanlış pozitifleri sınırlı olmuştur.

Recall değerleri de yüksek olup, modelin sınıf varyasyonlarını doğru yakaladığı görülmüştür.

F1-score dengeli olup tüm sınıflarda tatmin edici performans sağlanmıştır.

ROC-AUC Analizi: Modelin her sınıf için duyarlılık ve özgüllüğü değerlendirilmiş, AUC değerleri yüksek bulunmuştur.

Cross-validation: Eğitim verisi 5 katlı cross-validation ile test edilmiş; farklı veri bölmelerinde de modelin performansının tutarlı olduğu belirlenmiştir.

5. Bulgular ve Yorum

Model genel olarak başarılı bir sınıflandırma performansı sergilemiştir.

Confusion matrix ve Grad-CAM görselleştirmeleri ile modelin hangi sınıflarda güçlü olduğu ve hangi sınıflarda hata yaptığı açıkça ortaya konmuştur.

Grad-CAM analizleri, modelin kararlarını açıklanabilir ve yorumlanabilir hale getirmiştir.

Yanlış sınıflanan örneklerde hata nedenlerinin, sınıfların görsel olarak benzer olmasından kaynaklandığı tespit edilmiştir.

Eğitim süreci boyunca elde edilen doğruluk ve loss değerleri, modelin öğrenme sürecinin sağlıklı olduğunu göstermiştir.

📈 Bulunan Sonuçlar

Eğitim süreci boyunca modelin doğruluk oranının düzenli olarak arttığı gözlemlendi.

Hem eğitim hem doğrulama aşamasında kayıp (loss) değerlerinde belirgin bir azalma elde edildi.

Eğitim sonunda model ile yüksek doğruluk oranına ulaşılması sağlandı.

Çeşitli sınıflar üzerinde yapılan değerlendirmelerde genel olarak başarılı bir sınıflandırma performansı elde edildi.

Confusion matrix analizinde sınıfların büyük çoğunluğunun doğru şekilde tahmin edildiği, yalnızca bazı görsel benzerliklerden dolayı sınırlı sayıda karışıklık yaşandığı belirlendi.

Precision, Recall ve F1-Score metriklerinde tatmin edici değerler elde edildi ve sınıflar arasında dengeli bir performans sergilendi.

# Ekler

1. Görseller ve Heatmap’ler

Grad-CAM görselleri ile modelin karar verdiği bölgeler açıklanabilir hâle getirilmiştir.

Yanlış sınıflanan örnekler de incelenmiş, modelin hangi bölgelerde hata yaptığı görselleştirilmiştir:

2. Confusion Matrix ve Metrikler Tablosu

Sınıf bazlı performans ve hata oranları confusion matrix ile detaylandırılmıştır.

Precision, Recall ve F1-Score tabloları

3. Eğitim / Doğrulama Grafikleri

Eğitim süreci boyunca accuracy ve loss grafikleri elde edilmiştir:

4. Notlar ve Ek Açıklamalar

Modelin karar mekanizmasının açıklanabilirliği Grad-CAM görselleriyle desteklenmiştir.

Görsel örnekler, hatalı sınıflamalar ve sınıf karışıklıkları detaylı olarak incelenmiştir.

Kullanıcı, kendi görsellerini veya ek analizlerini ekleyerek bu ekler bölümünü genişletebilir.


# Sonuç ve Gelecek Çalışmalar

Eğitim süreci boyunca modelin doğruluk oranının düzenli olarak arttığı gözlemlenmiştir.

Hem eğitim hem doğrulama aşamasında kayıp (loss) değerlerinde belirgin bir azalma elde edilmiştir.

Model ile yüksek doğruluk oranına ulaşılması sağlanmıştır.

Çeşitli sınıflar üzerinde yapılan değerlendirmelerde genel olarak başarılı bir sınıflandırma performansı elde edilmiştir.

Confusion matrix analizinde sınıfların büyük çoğunluğunun doğru şekilde tahmin edildiği, yalnızca bazı görsel benzerliklerden dolayı sınırlı sayıda karışıklık yaşandığı belirlenmiştir.

Precision, Recall ve F1-Score metriklerinde tatmin edici değerler elde edilmiş ve sınıflar arasında dengeli bir performans sergilenmiştir.

Grad-CAM analizleri ile modelin karar mekanizmasının açıklanabilir ve yorumlanabilir hâle getirildiği gözlemlenmiştir.


Daha derin CNN veya transfer learning tabanlı mimariler (ResNet, EfficientNet, VGG16) ile performansın artırılması planlanmaktadır.

Veri arttırma teknikleri ve regularization yöntemleri ile modelin genelleme kapasitesinin iyileştirilmesi hedeflenmektedir.

Hiperparametre optimizasyonu ve farklı optimizer/learning rate ayarlamaları ile doğruluk ve loss değerlerinin daha da iyileştirilmesi sağlanabilir.

Modelin API veya web arayüzü ile canlı olarak entegre edilmesi planlanmaktadır.

Daha gelişmiş açıklanabilirlik yöntemleri (LIME, SHAP) kullanılarak modelin karar mekanizmasının detaylı şekilde yorumlanması hedeflenmektedir.

# Linkler

Çalışmaya ait tüm Kaggle linkleri aşağıdadır:

https://www.kaggle.com/code/ulagler/akbank-bootcamp/edit
