# Bitcoin Time-Series Forecasting with RNN, LSTM and GRU

Türkçe açıklamalı bir Jupyter Notebook: Bitcoin saatlik fiyat verisi üzerinde tekrarlayan sinir ağlarını karşılaştırır ve tahmin sonuçlarını zaman serisi doğrulama yöntemleriyle inceler.

> **Not:** Bu proje eğitim ve araştırma amaçlıdır; yatırım tavsiyesi değildir. Geçmiş veride iyi görünen sonuçlar gelecekteki performansı garanti etmez.

## Proje amacı

Bu çalışmanın amacı yalnızca düşük tahmin hatası elde etmek değil; modelleri adil bir baseline ile karşılaştırmak, zaman sıralamasını korumak, tahminleri doğru zaman damgalarıyla eşleştirmek ve basit bir strateji simülasyonunun sınırlarını incelemektir.

## Notebook içeriği

1. **Ortamı hazırlama** — Gerekli Python paketleri ve çalışma ortamı.
2. **Veriyi alma** — KaggleHub üzerinden veri indirme; gerekirse CSV'yi elle yükleme.
3. **Veri temizliği ve saatlik OHLCV** — Zaman damgalarını ve sütunları kontrol etme, saatlik seriyi hazırlama.
4. **Tahmin problemini tanımlama** — Geçmiş pencere ve kronolojik eğitim/test ayrımı.
5. **Baseline** — Son gözlenen fiyatı bir sonraki fiyat olarak kullanma.
6. **SimpleRNN, LSTM ve GRU** — Tek değişkenli modelleri karşılaştırma.
7. **Sonuçların yorumlanması** — Çıktıları ve baseline farkını değerlendirme.
8. **Log getiri ile modelleme** — Fiyat seviyesinden log getiri hedefine geçiş.
9. **Log getiri deneyinin değerlendirilmesi** — Tahminleri fiyat ölçeğine ve zaman indeksine geri eşleme.
10. **Çok değişkenli GRU ve özellik mühendisliği** — Getiri, volatilite ve teknik göstergeler gibi girdiler.
11. **Çok değişkenli model sonuçları** — Ortak test örneklerinde karşılaştırma.
12. **Backtest ve portföy analizi** — Basit sinyal stratejisi, işlem maliyeti varsayımları ve performans metrikleri.
13. **Walk-forward doğrulama ve kalite kapıları** — Zaman içinde ilerleyen test blokları, güçlü baseline'lar, GRU doğrulaması ve temel tutarlılık kontrolleri.

Her ana bölümün öncesinde, önceki aşamada ne öğrenildiğini ve sonraki aşamanın neden gerekli olduğunu açıklayan bir geçiş notu bulunur.

## Başlarken

### Google Colab ile

1. Bu depodaki `bitcoin-timeseries-rnn-lstm-gru.ipynb` dosyasını aç.
2. Notebook'u Google Colab'a yükle veya GitHub önizlemesindeki **Open in Colab** bağlantısını kullan (bağlantı eklenmişse).
3. Hücreleri yukarıdan aşağıya sırayla çalıştır. Tüm notebook için Colab menüsünden **Runtime → Run all** seçeneğini kullanabilirsin.
4. KaggleHub indirmesi ortamda kimlik doğrulaması isterse, veri indirme hücresinin açıklamasındaki manuel CSV yükleme alternatifini kullan.

### Yerel ortamda

Python 3.10 veya 3.11 önerilir. Sanal ortam oluşturup paketleri kur:

```bash
python -m venv .venv
```

Windows Git Bash üzerinde sanal ortamı etkinleştir:

```bash
source .venv/Scripts/activate
```

Linux/macOS üzerinde:

```bash
source .venv/bin/activate
```

Paketleri kur:

```bash
pip install -r requirements.txt
```

Jupyter Notebook'u başlat:

```bash
jupyter notebook
```

Notebook Colab odaklı `/content` yollarını da desteklediğinden, yerel ortamda veri yolu veya Kaggle erişimi gerektiğinde ilgili hücreleri kontrol et.

## Veri

Varsayılan veri kaynağı Kaggle'daki [`mczielinski/bitcoin-historical-data`](https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data) veri setidir. Veri setinin sürümü ve kapsamı zaman içinde değişebilir. Alternatif olarak, notebook'un beklediği zaman damgası ve kapanış fiyatı sütunlarını içeren uyumlu bir CSV kullanılabilir. Notebook, mevcut veri dosyalarını bulmaya ve sütun adlarındaki olası farklılıkları ele almaya çalışır.

## Yöntem ve değerlendirme

- Veri kronolojik sırada ele alınır; gelecek bilgisi geçmişe taşınmamalıdır.
- Ölçekleyiciler yalnızca ilgili eğitim bölümünde fit edilmelidir.
- Modeller, mümkün olduğunda aynı test zaman damgaları üzerinde karşılaştırılır.
- Baseline karşılaştırması, yalnızca sinir ağları arasındaki sıralamaya bakmaktan daha anlamlıdır.
- Walk-forward testleri, sonuçların tek bir test dönemine bağımlılığını azaltmaya yardımcı olur.
- Backtest çıktıları işlem maliyetleri, kayma, sinyalin uygulanma zamanı ve piyasa varsayımlarına duyarlıdır.

**Sınırlar:** Notebook'taki kontroller bir araştırma başlangıç noktasıdır; profesyonel işlem altyapısı, kapsamlı istatistiksel anlamlılık testi veya canlı piyasa doğrulaması yerine geçmez. Sonuçlar çalıştırma anındaki veri ve yazılım sürümlerine göre değişebilir. Bu depodaki notebook çıktıları temizlenmiştir; grafikler ve metrikler yeniden çalıştırıldığında üretilir.

## Teknolojiler

- Python
- pandas, NumPy
- TensorFlow / Keras
- scikit-learn
- Matplotlib
- KaggleHub
- Jupyter Notebook / Google Colab

## Proje durumu

Bu sürüm; temel RNN ailesi karşılaştırmasını, log getiri yaklaşımını, çok değişkenli GRU deneyini, basit backtest'i ve walk-forward doğrulama adımlarını bir araya getirir. Sonraki iyileştirme alanları; daha geniş walk-forward dönemleri, çoklu rastgelelik tohumları, işlem maliyeti duyarlılık analizi, piyasa rejimlerine göre hata analizi ve deney konfigürasyonlarının kaydedilmesidir.

## Lisans

Henüz bir lisans belirtilmemiştir. Açık kaynak lisansı eklemeden önce paylaşım ve yeniden kullanım koşullarını belirle.
