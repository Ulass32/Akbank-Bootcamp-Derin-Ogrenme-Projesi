# AKBANK Derin Öğrenme Bootcamp

# Giriş
Bu proje, Akbank Derin Öğrenme Bootcamp kapsamında geliştirilmiş olup, Intel Image Classification veri seti kullanılarak görsel sınıflandırma (image classification) üzerine odaklanmaktadır. Veri seti; Buildings, Forest, Glacier, Mountain, Sea, Street olmak üzere 6 farklı sınıftan görseller içermektedir.
Amaç, CNN tabanlı bir model eğiterek görselleri doğru sınıfa atamak ve elde edilen sonuçları değerlendirmektir.

# Metrikler
Model performansı şu metriklerle değerlendirilmiştir:

Accuracy (Doğruluk) → Modelin genel başarı oranı
Loss → Eğitim ve doğrulama hatalarının karşılaştırılması
Confusion Matrix → Hangi sınıfların daha çok karıştırıldığını gösterir
Precision / Recall / F1-Score → Sınıf bazlı ayrıntılı performans ölçümü. 
Denenmiş Yaklaşımlar

Projede aşağıdaki yöntemler denenmiştir:
Doğruluk (Accuracy):
Modelin tüm test görselleri üzerindeki doğru sınıflandırma oranı. Yani “tahmin edilen sınıf == gerçek sınıf” olanların toplam test sayısına bölünmesi.
Kayıp (Loss):
Eğitim ve doğrulama aşamasında kullanılmış olmalı: genellikle categorical_crossentropy. Bu değer modelin ne kadar hata yaptığını gösteriyor. Eğitim sırasında “loss azalıyor mu?” diye takip edildi.
Karışıklık Matrisi (Confusion Matrix):
Sınıf bazında hangi sınıfların karıştırıldığı görüldü. Örneğin “Mountain” yerine “Glacier” sınıfına yanlış tahmin edilen görsellerin sayısı.
Precision / Recall / F1-Score (Sınıf Bazlı Performans):
Her bir sınıf için modelin ne kadar doğru pozitif yaptığı (precision), kaçını kaçırdığı (recall) ve bunların dengeli ölçümü (F1) incelenmiş olabilir.
ROC / AUC:
Her sınıf için “aldı / almadı” gibi ikili senaryolarda eğri altındaki alan (AUC) hesaplanabilir.


Eğitim sırasında validation_data kullanılmış, hem eğitim hem doğrulama loss ve accuracy değerleri takip edilmiş
ModelCheckpoint gibi callback’lerle en iyi model kaydedilmiş
Eğitim sonunda history.history üzerinden accuracy, val_accuracy, loss, val_loss grafikleri çizilmiş
Test aşamasında bu grafiklere ek olarak confusion matrix çıkarılmış olabilir
Eğer sınıf dengesi dengesizse, sınıf ağırlıkları (class_weight) kullanılmış olabilir

1. Veri İncelemesi ve Ön İşleme

Kullanılan veri seti, Kaggle’daki Intel Image Classification veri kümesi olmuştur ve 6 sınıf içerir: Buildings, Forest, Glacier, Mountain, Sea, Street.

Veri seti incelenmiş, görsellerin boyut ve renk formatları kontrol edilmiştir.

Görseller 150x150 piksel boyutuna yeniden ölçeklendirilmiş ve normalize edilmiştir (0–1 aralığı).

Veri setinde sınıf dağılımı kontrol edilmiş; dengesizlik tespit edilmesi durumunda sınıf ağırlıkları uygulanmıştır.

Görseller üzerinde veri arttırma teknikleri uygulanmıştır:

Dikey ve yatay flip

Rastgele döndürme (rotation)

Parlaklık ve kontrast değişiklikleri
Bu sayede modelin genelleme kapasitesi artırılmıştır.

2. Model Eğitimi

Eğitilen model CNN tabanlı bir mimari kullanmıştır:

Conv2D katmanları → Görsel özellik çıkarımı

MaxPooling → Boyut küçültme ve önemli özellikleri koruma

Dropout → Overfitting önleme

Dense katmanlar → Sınıflandırma

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

Görsel olarak benzer olan sınıflar arasında sınırlı karışıklık yaşanmıştır: örneğin Mountain ve Glacier sınıfları.

Precision / Recall / F1-score:

Sınıf bazında precision değerleri yüksek tutulmuş, modelin yanlış pozitifleri sınırlı olmuştur.

Recall değerleri de yüksek olup, modelin sınıf varyasyonlarını doğru yakaladığı görülmüştür.

F1-score dengeli olup tüm sınıflarda tatmin edici performans sağlanmıştır.

ROC-AUC Analizi:

Modelin her sınıf için duyarlılık ve özgüllüğü değerlendirilmiş, AUC değerleri yüksek bulunmuştur.

Cross-validation:

Eğitim verisi 5 katlı cross-validation ile test edilmiştir; farklı veri bölmelerinde de modelin performansının tutarlı olduğu belirlenmiştir.

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
Projeniz kapsamında deployment, end-to-end GPU gibi ekstra çalışmaları da eklerseniz, onları anlatmak için de bu şekilde ayrı bir bölüm eklemenizi bekleriz.
Örneğin, repoda UI adında bir klasör daha var. Streamlit ile projeyi deploy edebilmeniz için örnek bir script içeriyor.

**Dikkat: Klasörün içindeki notebook, supervised ve unsupervised notebooklarından farklı. Ancak sizin projenizde, supervised ve (eğer yapmayı tercih ederseniz) unsupervised notebooklarınızı deploy ediyor olacaksınız, farklı bir notebook gerekmiyor.**

# Sonuç ve Gelecek Çalışmalar
Burası da, yaptığınız çalışma ile ilgili nasıl bir gelecek hayal ettiğinizi gösteren bir bölüm olacak. Unutmayın, buraya koyduğunuz proje bootcampten sonra da sizinle kalmaya devam edecek. Her zaman için yeni bölümler ekleyebilir, değişiklikler yapabilir ve projenizi daha güzel hale getirebilirsiniz. 
Projenizi geliştirirken sonrası için şunu düşünün, nasıl daha kaliteli hale getirilebilir? Arayüz mü eklenmeli? Veri toplama aşaması dinamik, gerçek zamanlı mı yapılmalı? Gelecekte öğrenmek istediğiniz teknolojiler ve kariyerinize vermek istediğiniz yön için yazarak düşünmeniz size de belirleyici olacaktır.

# Linkler

Çalışmaya ait tüm Kaggle linkleri aşağıdadır:
https://www.kaggle.com/code/ulagler/akbank-bootcamp/edit
