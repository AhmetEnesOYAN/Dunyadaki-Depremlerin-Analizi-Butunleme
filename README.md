# Deprem Büyüklüğü Tahmin Projesi

## Proje Açıklaması

Bu proje, deprem verilerini kullanarak deprem büyüklüğünü tahmin etmek amacıyla çeşitli makine öğrenmesi modelleri kullanmaktadır. Proje, veri ön işleme adımlarından model eğitimi ve değerlendirmeye kadar bir dizi aşamayı kapsamaktadır.

## Genel Bakış

Bu projede, makine öğrenmesi regresyon modelleri kullanarak deprem büyüklüklerini tahmin etmeye çalıştık. Değerlendirilen modeller şunlardır:
- **Linear Regression (Doğrusal Regresyon)**
- **Random Forest Regressor (Rastgele Orman Regresyonu)**
- **Support Vector Machine (Destek Vektör Makinesi)**
- **Gradient Boosting Regressor (Gradyan Artırma Regresyonu)**
- **K-Nearest Neighbors (K-En Yakın Komşular)**

Veri seti, deprem büyüklüğü, derinlik, enlem, boylam ve diğer çeşitli özellikleri içermektedir. Bu projenin amacı, bu özelliklere dayalı olarak deprem büyüklüğünü tahmin etmektir.

## Veri Seti

Veri seti, depremlerle ilgili çeşitli özellikleri içermektedir:
- **Deprem büyüklüğü** (`mag`)
- **Coğrafi konum** (enlem, boylam)
- **Deprem derinliği** (`depth`)
- **Zaman ve tarih bilgileri** (`time`)
- **Ölçüm hataları ve diğer sayısal değişkenler** (örneğin, `nst`, `gap`, `dmin` vb.)

Veri seti CSV formatında sağlanmıştır ve projede kullanılan bazı özellikler şunlardır:
- **Sayısal Özellikler**: Deprem büyüklüğü, derinlik, enlem, boylam vb.
- **Zaman Özellikleri**: Yıl, ay, Unix zaman damgası (`time_unix`), vb.

## Veri Seti ve Ön İşleme

Veri setindeki veriler, aşağıdaki işlemlerle temizlendi ve hazır hale getirildi:

1. **Eksik Verilerin Düzenlenmesi**: Veri setindeki eksik değerler, her sütunun medyanı ile dolduruldu.
2. **Aykırı Değerlerin Temizlenmesi**: Aykırı değerler, IQR (Interquartile Range) metoduyla temizlendi.
3. **Tarihsel Özelliklerin Çıkarılması**: Zamanla ilgili özellikler çıkarıldı ve Unix zaman damgasına dönüştürüldü.

### Eksik Veriler
Eksik veriler, medyan değeri ile doldurulmuştur. Özellikle sayısal sütunlarda eksik veriler bulunmuş ve her bir sütun için medyan (orta değer) kullanılmıştır. Aşağıda eksik verilerle ilgili bazı detaylar yer almaktadır:

| Özellik             | Eksik Veri Sayısı |
|---------------------|-------------------|
| `nst`               | 5313              |
| `gap`               | 324               |
| `dmin`              | 702               |
| `rms`               | 26                |
| `horizontalError`   | 1218              |
| `depthError`        | 209               |
| `magError`          | 2231              |
| `magNst`            | 2067              |

### Aykırı Değerler
Aykırı değerler, IQR (Interquartile Range) yöntemiyle temizlenmiştir. Bu adım, modelin doğruluğunu artırmak için gereklidir.

### Tarih Dönüşümü
Zaman verileri (`time`), tarih formatına dönüştürülüp, Unix zaman damgası (`time_unix`), yıl ve ay gibi yeni özellikler eklenmiştir.

### Kategorik Verilerin Sayısallaştırılması
Kategorik veriler, sayısal verilere dönüştürülmüştür. One-hot encoding veya etiket kodlama kullanılarak bu işlem gerçekleştirilmiştir.

## Modelleme ve Sonuçlar

### Kullanılan Modeller
Aşağıdaki 5 farklı regresyon modeli, deprem büyüklüğünü tahmin etmek için eğitilmiştir:

1. **Linear Regression** (Doğrusal Regresyon)
2. **Random Forest Regressor**
3. **Support Vector Regression** (SVR)
4. **Gradient Boosting Regressor**
5. **K-Nearest Neighbors** (KNN)

### Model Değerlendirmesi
Modellerin başarısı, test verisi üzerinde tahminler yapılarak **Mean Squared Error (MSE)** metriği ile ölçülmüştür. MSE, tahmin edilen değerlerle gerçek değerler arasındaki farkın karesinin ortalamasıdır. Bu metrik, modelin doğruluğunu anlamamıza yardımcı olur.

Aşağıdaki tabloda, her bir modelin test verisi üzerinde hesaplanan MSE değeri yer almaktadır:

| Model                    | MSE       |
|--------------------------|-----------|
| Linear Regression         | 0.11      |
| Random Forest             | 0.10      |
| Support Vector Machine    | 0.11      |
| Gradient Boosting         | 0.09      |
| K-Nearest Neighbors       | 0.11      |

### Sonuçların Değerlendirilmesi

#### **Linear Regression**:
- Basit ve hızlı bir modeldir.
- Genellikle karmaşık veri setlerinde doğrusal olmayan ilişkileri yakalamakta zorluk çeker.
- MSE değeri 0.11 ile diğer modellere benzer seviyededir, ancak doğrusal olmayan ilişkileri modelleme kapasitesi sınırlıdır.

#### **Random Forest**:
- Ensemble (toplu) bir yöntemdir ve genellikle karmaşık veri setlerinde daha iyi sonuçlar verir.
- Karar ağaçlarının birleşiminden oluşur ve MSE değeri 0.10 ile oldukça düşük olmuştur.
- Yüksek doğruluk ve az overfitting riski ile sonuçlanır.

#### **Support Vector Regression (SVR)**:
- Doğrusal olmayan ilişkilere dayalı tahminler yapar.
- Parametre ayarları doğru yapılmadığında, düşük performans gösterebilir. MSE değeri 0.11 ile Linear Regression ile benzer seviyededir.

#### **Gradient Boosting**:
- Zayıf öğrenicilerin ardı ardına eklenmesiyle güçlü bir model oluşturur.
- Genellikle diğer modellere göre daha iyi sonuçlar verir. MSE değeri 0.09 ile en düşük değeri göstermiştir.
- Daha fazla hesaplama kaynağı gerektirse de bu modelin güçlü tahmin yeteneği ortaya çıkmaktadır.

#### **K-Nearest Neighbors (KNN)**:
- Veri noktalarına olan uzaklıklarına göre tahminler yapar.
- Veri seti büyüdükçe yavaşlayabilir ve komşu sayısının doğru ayarlanması önemlidir. MSE değeri 0.11 ile Linear Regression ve SVR ile aynı seviyededir.

## Sonuçların Karşılaştırılması

Model performansları karşılaştırıldığında, **Gradient Boosting** modeli en düşük MSE değeri olan **0.09** ile en iyi performansı göstermektedir. Bu, modelin güçlü bir şekilde doğrusal olmayan ilişkileri öğrenebilme yeteneği ile açıklanabilir. 

**En iyi sonuçlar**:
- **Gradient Boosting** en düşük MSE değeri ile en iyi sonuçları vermiştir.

**Genel Sonuç**:
- Bu projede **Gradient Boosting** modeli, en iyi sonuçları vermiştir. Ancak, **Random Forest** ve **Linear Regression** gibi daha basit modeller de oldukça benzer performanslar sergilemiştir (0.10 - 0.11 MSE aralığında).
- **KNN** ve **SVR** modelleri, basit modellerin gerisinde kalmış ve MSE değerleri daha yüksek olmuştur.

## Gereksinimler

Bu projeyi çalıştırmak için aşağıdaki Python kütüphanelerine ihtiyacınız vardır:
- pandas
- numpy
- seaborn
- matplotlib
- scikit-learn

## Yazar

Bu projeyi **Ahmet Enes OYAN** geliştirmiştir.
