# NASA Turbofan Engine RUL Prediction (FD001)

<p align="center">
  <a href="#english-version">🇬🇧 English Version</a> • 
  <a href="#türkçe-versiyon">🇹🇷 Türkçe Versiyon</a>
</p>

---

<a id="english-version"></a>
## 🇬🇧 English Version

This project focuses on predicting the **Remaining Useful Life (RUL)** of aircraft turbofan engines using the benchmark **NASA C-MAPSS (FD001)** dataset. By leveraging advanced feature engineering, time-series transformations, and state-of-the-art gradient boosting algorithms, this machine learning pipeline achieves highly accurate and explainable prognostic results.

### 🚀 Key Achievements & Architecture
* **Advanced Feature Engineering:** Extracted rolling windows (mean, std) and lagging indicators to capture degradation trends over time instead of relying on single snapshots.
* **Feature Transformation:** Applied **Yeo-Johnson** transformation via PowerTransformer to handle highly skewed sensor data, stabilizing variance across cycles.
* **Piecewise RUL Target Formulation:** Implemented an upper limit threshold of 125 cycles (Piecewise Linear RUL) to accurately model the early wear-free phase of healthy engines, preventing early-stage model confusion.
* **Data Leakage Prevention:** Scaled training and testing data securely by fitting the `StandardScaler` only on the training set and transforming the test set on the final cycle cutoff points.

### 📊 Model Performance Summary
The models were trained on the full lifecycle data and evaluated strictly on the official 100 test engines at their respective last operational cycles:

| Model | R2 Score | MSE | MAE (Cycles) |
| :--- | :---: | :---: | :---: |
| Decision Tree | ~55% - 60% | - | - |
| Random Forest | 71.68% | 488.91 | 16.43 |
| **XGBoost (Champion)** | **80.25%** | **340.98** | **13.33** |

### 🔍 XAI & Model Interpretability (SHAP)
To avoid a "black-box" approach, **SHAP (SHapley Additive exPlanations)** was integrated to achieve full transparency:
1. **Summary Plot:** Identified `s_4_roll_mean_5`, `s_11_roll_mean_5`, and `s_9_roll_mean_5` as the primary catalyst features driving engine degradation. Higher values of these sensors significantly accelerate the drop in RUL.
2. **Dependence Plot:** Uncovered a distinct physical inflection point around the normalized threshold of `0.25` for sensor 4, where the model abruptly shifts from a stable phase to an exponential decay phase.
3. **Force Plot:** Allowed micro-level diagnostics for individual engines to track exactly which telemetry parameters are pushing the system towards critical maintenance thresholds.

---

<a id="türkçe-versiyon"></a>
## 🇹🇷 Türkçe Versiyon

Bu proje, **NASA C-MAPSS (FD001)** veri setini kullanarak uçak turbofan motorlarının **Kalan Ömür (RUL)** tahminini gerçekleştirmektedir. Gelişmiş özellik mühendisliği, zaman serisi dönüşümleri ve gradyan artırma (XGBoost) algoritmaları kullanılarak, yüksek doğrulukta ve açıklanabilir tahmini bakım sonuçları elde edilmiştir.

### 🚀 Öne Çıkan Mühendislik Adımları
* **Zaman Serisi Özellikleri:** Anlık sensör verilerinin gürültüsünü engellemek adına son 5 adımın hareketli ortalamaları (`rolling_mean`), standart sapmaları (`rolling_std`) ve gecikme değerleri (`lag`) türetilmiştir.
* **Varyans Kararlılaştırma:** Çarpık (skewed) dağılıma sahip sensör verilerini normal dağılıma yaklaştırmak için **Yeo-Johnson** dönüşümü uygulanmıştır.
* **Parçalı Ömür (Piecewise RUL) Stratejisi:** Motorların aşınmadığı ilk sağlıklı dönemleri doğru modellemek adına hedef değişken 125 çevrim ile sınırlandırılmıştır.
* **Veri Sızıntısı (Data Leakage) Engeli:** `StandardScaler` terazisi sadece eğitim verisinden öğrenilmiş, test setindeki 100 motorun son satırlarına sızıntı olmadan uygulanmıştır.

### 📊 Model Performans Raporu
Modeller tüm yaşam döngüsüyle eğitilmiş ve sadece test setindeki 100 motorun NASA tarafından kapatıldığı son operasyonel satırlarında test edilmiştir:

| Model | R2 Skoru | MSE | MAE (Hata Çevrimi) |
| :--- | :---: | :---: | :---: |
| Decision Tree | ~%55 - %60 | - | - |
| Random Forest | %71.68 | 488.91 | 16.43 |
| **XGBoost (Şampiyon)** | **%80.25** | **340.98** | **13.33** |

### 🔍 Açıklanabilir Yapay Zeka (SHAP Analizi)
Kara kutu modellerin aksine, sistemin kararları **SHAP** kütüphanesi ile tamamen şeffaf hale getirilmiştir:
1. **Summary Plot (Genel Özet):** `s_4_roll_mean_5`, `s_11_roll_mean_5` ve `s_9_roll_mean_5` sensörlerinin motorun canını okuyan ana unsurlar olduğu belirlenmiştir. Bu değerler arttıkça kalan ömür hızla düşmektedir.
2. **Dependence Plot (Bağımlılık Grafiği):** Sensör 4'ün hareketli ortalaması `0.25` eşiğini geçtiği an motorun stabil durumdan çıkıp küt diye arıza fazına geçtiği (uçurum etkisi) matematiksel olarak kanıtlanmıştır.
3. **Force Plot (Kuvvet Grafiği):** Tek bir motor bazında mikro analizler yapılarak, hangi sensörün motoru ne kadar yıprattığı canlı olarak takip edilebilmektedir.

---

### 🛠️ Installation & Usage / Kurulum ve Kullanım

```bash
# Clone the repository / Depoyu kopyalayın
git clone [https://github.com/yourusername/nasa-turbofan-rul.git](https://github.com/yourusername/nasa-turbofan-rul.git)

# Install dependencies / Gerekli kütüphaneleri yükleyin
pip install -r requirements.txt