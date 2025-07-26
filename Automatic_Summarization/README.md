# 🔗 Proje Adresi

👉 GitHub Linki: [https://github.com/mlkenrcydn/Turkcell_GYK_NLP_Project/tree/main/nlp-news-summarization](https://github.com/mlkenrcydn/Turkcell_GYK_NLP_Project/tree/main/nlp-news-summarization)

> Bu proje, T5-Small modelini CNN/DailyMail veri seti ile özetler üretmeyi amaçlamaktadır.

---

# 📝 T5-Small ile Metin Özetleme Projesi

Bu proje, [CNN/DailyMail](https://huggingface.co/datasets/cnn_dailymail) veri seti kullanılarak **T5-Small** modeline metin özetleme görevi için **ince ayar (fine-tuning)** yapmayı amaçlamaktadır.

---

## 📌 Projeye Genel Bakış

Bu depo, bir metin özetleme modeli oluşturmak ve eğitmek için gereken tüm adımları içermektedir:

### 1. ✅ Veri Yükleme ve Ön İşleme
- **CNN/DailyMail** veri seti yüklenir.
- Aşağıdaki işlemleri yapan özel ön işleme fonksiyonları uygulanır:
  - HTML etiketlerinin kaldırılması
  - URL'lerin temizlenmesi
  - Hashtag, özel karakter ve fazladan boşlukların silinmesi

### 2. 🔤 Tokenizasyon
- Metin ve özetler, **T5-Small tokenizer** ile sayısal token'lara dönüştürülür.
- Maksimum uzunluk sıralamaları auarlanır.
- Veriler, model girişi için uygun forma getirilir.

### 3. 🧠 Model Eğitimi
- Hugging Face `transformers` kütüphanesi kullanılarak T5-Small modeli eğitilir.
- Eğitim süreci, `Seq2SeqTrainer` ile gerçekleştirilir.

### 4. 📊 Değerlendirme
- Modelin başarımı, **ROUGE** metrikleri ile ölçülür.
- Oluşturulan özetler, referans özetlerle karşılaştırılır.


---

## ⚙️ Eğitim Parametreleri

`Seq2SeqTrainingArguments` altında belirlenen temel parametreler ve açıklamaları:

| Argüman | Açıklama |
|--------|----------|
| `output_dir="./results"` | Model çıktılarının kaydedileceği dizin |
| `per_device_train_batch_size=4` | Eğitim sırasında her cihaz için batch boyutu |
| `per_device_eval_batch_size=4` | Değerlendirme sırasında batch boyutu |
| `num_train_epochs=3` | Eğitim dönemi sayısı |
| `learning_rate=2e-5` | Öğrenme oranı |
| `logging_dir="./logs"` | Log dosyalarının saklanacağı dizin |
| `logging_steps=10` | Her 10 adımda bir log kaydı oluştur |
| `evaluation_strategy="epoch"` | Her epoch sonunda model değerlendirmesi yapılır |
| `save_strategy="epoch"` | Her epoch sonunda model kaydedilir |
| `report_to="none"` | Harici log aracı kullanılmaz |
| `predict_with_generate=True` | Değerlendirme sırasında özet üretimi yapılır |
| `load_best_model_at_end=True` | Eğitim sonunda en iyi model yüklenir |
| `metric_for_best_model="rouge1"` | En iyi modeli belirlemek için kullanılan metrik |
| `greater_is_better=True` | Daha yüksek `rouge1` puanı daha iyidir |
| `# fp16=torch.cuda.is_available()` | (İsteğe bağlı) Mixed precision training için önerilir |

---

## 🧪 Test Örnekleri

Aşağıda modelin eğitildikten sonra gerçek haber metinleri üzerinde ürettiği 5 örnek ve özetleri verilmiştir:

### Örnek 1
**Girdi Metni:**
> 

**Üretilen Özet:**
> 

---

### Örnek 2
**Girdi Metni:**
> 

**Üretilen Özet:**
> 

---

### Örnek 3
**Girdi Metni:**
> 

**Üretilen Özet:**
> 

---

### Örnek 4
**Girdi Metni:**
> 

**Üretilen Özet:**
> 

---

### Örnek 5
**Girdi Metni:**
> 

**Üretilen Özet:**
> 

---

## 📊 ROUGE Değerlendirme Sonuçları

Modelin test setindeki özetleme başarımı aşağıdaki gibidir:

| Metrik | Skor |
|--------|------|
| **ROUGE-1** | `` |
| **ROUGE-2** | `` |
| **ROUGE-L** | `` |

> ROUGE-1 ve ROUGE-L skorlarının yüksekliği, modelin anlam bütünlüğü olan özetler üretebildiğini göstermektedir.

---

## ✅ Sonuç



---

