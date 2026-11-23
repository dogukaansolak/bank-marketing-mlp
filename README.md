# Bank Marketing MLP

Üsküdar Üniversitesi Yapay Zekâ Mühendisliği Yüksek Lisans Programı · Yapay Sinir Ağları Ara Sınav Projesi

Bu projede, bir banka müşterisinin vadeli mevduat aboneliği yapıp yapmayacağını tahmin etmek için PyTorch ile çok katmanlı bir algılayıcı (MLP) geliştirdim. Altı manuel ve altı otomatik hiperparametre adayını aynı çapraz doğrulama bölmelerinde değerlendirdim. Sonuçları Logistic Regression ve Dummy modelleriyle karşılaştırdım.

**[Teknik rapor](report.pdf)** · [Test sonuçları](results/test_metrics.csv) · [Grafikler](figures)

## Veri ve deney düzeni

| Özellik | Değer |
| --- | --- |
| Veri seti | UCI Bank Marketing · `bank-additional.csv` |
| Kayıt sayısı | 4.119 |
| Geliştirme / bağımsız test | 3.296 / 823 kayıt |
| Çapraz doğrulama | 3 dış kat × 3 iç kat |
| Model seçim metriği | Average Precision (AP) |
| Temel seed | `31060` |
| Seçilen MLP | `M1` · AdamW, 32 nöronlu tek gizli katman, ReLU |

Görüşme süresi (`duration`), görüşme başlamadan bilinmediği için model girdilerinden çıkarıldı. Hiperparametre seçimi geliştirme verisinde yapıldı. Bağımsız test metrikleri, [kilitli tahmin dosyası](results/test_predictions_locked.csv) üzerinden hesaplandı.

## Bağımsız test sonuçları

| Model | AP | ROC-AUC | Duyarlılık | F1 |
| --- | ---: | ---: | ---: | ---: |
| Ana MLP | **0,3955** | 0,7576 | 0,1848 | 0,2810 |
| Logistic Regression | 0,3795 | **0,7607** | **0,2391** | **0,3284** |
| Dummy | 0,1118 | 0,5000 | 0,0000 | 0,0000 |

Ana MLP bağımsız testte en yüksek AP’yi elde etti. `0,50` karar eşiğinde ise Logistic Regression 92 pozitif vakanın **22’sini**, ana MLP **17’sini** yakaladı. Bootstrap güven aralıkları ve üç farklı başlangıç seed’iyle yapılan karşılaştırma [teknik raporda](report.pdf) yer alıyor.

## Nasıl çalıştırılır?

[UCI Bank Marketing arşivinden](https://doi.org/10.24432/C5K306) `bank-additional.csv` dosyasını çıkarıp proje köküne koyun. Deneyler Google Colab’da CPU üzerinde çalıştırıldı. Paket sürümleri [`requirements.txt`](requirements.txt) dosyasında kayıtlıdır.

1. [`01_eda.ipynb`](01_eda.ipynb) — Veri bölme ve keşifsel veri analizi
2. [`02_nested_cv.ipynb`](02_nested_cv.ipynb) — Ön işleme, hiperparametre araması ve nihai model seçimi
3. [`03_final_test.ipynb`](03_final_test.ipynb) — Tahmin dosyası doğrulaması, test metrikleri ve bootstrap analizi

Nihai tahminleri yeniden üretmek için `02_nested_cv.ipynb` içindeki tahmin üretme adımını ayrı bir çalışma kopyasında çalıştırın. Kayıtlı tahmin dosyasının SHA-256 özeti [`test_lock_manifest.json`](results/test_lock_manifest.json) içinde yer alır.
