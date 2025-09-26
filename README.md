# AKBANK Derin Öğrenme Bootcampi
Bu repo, Global AI Hub bootcamplerinde template olarak kullanmanız amacıyla tasarlanmıştır. Bootcampler dahilinde oluşturacağınız reponun bunun gibi görünmesi gerekmektedir. Ek olarak, projenizin görünürlüğünü GitHub üzerinde **Public** olarak ayarlamalısınız.

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
