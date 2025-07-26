# 🔗 Proje Adresi

👉 GitHub Linki: [https://github.com/mlkenrcydn/Turkcell_GYK_NLP_Project/tree/main/Automatic_Summarization](https://github.com/mlkenrcydn/Turkcell_GYK_NLP_Project/tree/main/Automatic_Summarization)

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
  - Küçük harfe çevirme

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

Aşağıda, `Seq2SeqTrainingArguments` altında belirlenen eğitim parametreleri yer almaktadır:

| Argüman | Açıklama |
|--------|----------|
| `output_dir="./results"` | Model ve log çıktılarının kaydedileceği dizin |
| `evaluation_strategy="epoch"` | Her epoch sonunda model değerlendirmesi yapılır |
| `save_strategy="epoch"` | Her epoch sonunda model kaydedilir |
| `learning_rate=2e-5` | Öğrenme oranı |
| `per_device_train_batch_size=4` | Eğitim sırasında her cihaz için batch boyutu |
| `per_device_eval_batch_size=4` | Değerlendirme sırasında batch boyutu |
| `num_train_epochs=5` | Eğitim dönemi (epoch) sayısı |
| `weight_decay=0.01` | Ağırlık azalması (overfitting’i önlemeye yardımcı olur) |
| `save_total_limit=1` | En fazla 1 model checkpoint’i saklanır (en yenisi tutulur) |
| `load_best_model_at_end=True` | Eğitim sonunda en iyi model yüklenir |
| `metric_for_best_model="eval_loss"` | En iyi modeli belirlemek için kullanılan metrik |
| `greater_is_better=False` | Daha düşük `eval_loss` değeri daha iyidir |
| `logging_dir="./logs"` | Log dosyalarının saklanacağı dizin |
| `logging_steps=100` | Her 100 adımda bir log kaydı oluşturulur |

---

## 🧪 Test Örnekleri

Aşağıda modelin eğitildikten sonra gerçek haber metinleri üzerinde ürettiği 5 örnek ve özetleri verilmiştir:

### Örnek 1
**Gerçek Özet:**
>Membership gives the ICC jurisdiction over alleged crimes committed in Palestinian territories since last June .
Israel and the United States opposed the move, which could open the door to war crimes investigations against Israelis . 

**Üretilen Özet:**
> the 123rd member of the international Criminal Court is a step that gives the court jurisdiction over alleged crimes in palestinians. the ICC opened a preliminary examination into the situation in Palestinian territories.

**İki Özet Karşılaştırması:**
>Gerçek özet, İsrail aleyhine açılabilecek savaş suçu soruşturmalarına dikkat çekerken; üretilen özet yalnızca mahkemenin yetki kazanmasına odaklanmış.
İçerik bakımından örtüşme olsa da üretilen özet, bağlamın siyasi boyutunu eksik aktarmış.
---

### Örnek 2
**Gerçek Özet:**
>  Theia, a bully breed mix, was apparently hit by a car, whacked with a hammer and buried in a field .
"She's a true miracle dog and she deserves a good life," says Sara Mellado, who is looking for a home for Theia .

**Üretilen Özet:**
> Theia is a friendly white-and-black bully breed mix now named theia. she was found four days after being hit by a car and buried in a field. the dog has been receiving care at the Veterinary Teaching Hospital. she suffered dislocated jaw, leg injuries and a caved-in sinus cavity.

**İki Özet Karşılaştırması:**
>Gerçek özet, Theia’nın başına gelen trajik olaylara ve onun hayatta kalma mucizesine odaklanıyor.
Üretilen özet ise daha çok tıbbi durumunu ve tedavi sürecini anlatıyor; duygusal yön eksik.
---

### Örnek 3
**Gerçek Özet:**
> Mohammad Javad Zarif has spent more time with John Kerry than any other foreign minister .
He once participated in a takeover of the Iranian Consulate in San Francisco .
The Iranian foreign minister tweets in English .

**Üretilen Özet:**
> Mohammad Javad Zarif is the Iranian foreign minister. he was nominated by former president Mahmoud Ahmadinejad's successor, Hassan Rouhami. he has gone a long way to bring in from the cold and allow it to rejoin the international community.

**İki Özet Karşılaştırması:**
>Gerçek özet, Zarif’in kişisel özelliklerine ve geçmişine odaklanmışken; üretilen özet diplomatik pozisyonu ve İran’ın küresel ilişkileriyle ilgileniyor.
Bilgi kesişmesi düşük; içerikler farklı yönleri vurguluyor.
---

### Örnek 4
**Gerçek Özet:**
> Student is no longer on Duke University campus and will face disciplinary review .
School officials identified student during investigation and the person admitted to hanging the noose, Duke says .
The noose, made of rope, was discovered on campus about 2 a.m.

**Üretilen Özet:**
> "This is no Duke we want. This is not the Duke we're here to experience," a student says. the incident is one of several recent racist events to affect college students.

**İki Özet Karşılaştırması:**
>Gerçek özet olayın detaylarını ve okulun tepkisini verirken, üretilen özet olayın öğrenciler üzerindeki psikolojik etkisine odaklanmış.
Üretilen özet, olayın nedenini veya sonucunu açıkça belirtmiyor.
---

### Örnek 5
**Gerçek Özet:**
> 17 Americans were exposed to the Ebola virus while in Sierra Leone in March .
Another person was diagnosed with the disease and taken to hospital in Maryland .
National Institutes of Health says the patient is in fair condition after weeks of treatment .

**Üretilen Özet:**
> Five Americans who were monitored for three weeks have been released. one of the five had a heart-related issue on Saturday. none developed the deadly virus.

**İki Özet Karşılaştırması:**
>Gerçek özet, virüsle temas eden kişiler ve tedavi gören hastayı detaylı şekilde açıklıyor.
Üretilen özet ise sadece 5 kişilik bir grubun durumuna değinmiş; kapsamı dar ve bilgi eksikliği var.
---

## 📊 ROUGE Değerlendirme Sonuçları

Modelin test setindeki özetleme başarımı aşağıdaki gibidir:

| Metrik | Skor |Açıklama|
|--------|------|--------|
| **ROUGE-1** | `0.3342826888502629` |Modelin, gerçek özetle aynı kelimeleri ne kadar doğru kullandığını gösterir.|
| **ROUGE-2** | `0.13014166959372436` |Modelin, gerçek özetle aynı iki kelimelik ardışık dizileri (bigrams) ne kadar doğru kullandığını gösterir.|
| **ROUGE-L** | `0.2439524660081373` |	Modelin, gerçek özetle en uzun ortak alt dizi (Longest Common Subsequence) üzerinden yapısal benzerliğini ölçer.|

> ROUGE-1 ve ROUGE-L skorlarının yüksekliği, modelin anlam bütünlüğü olan özetler üretebildiğini göstermektedir.

---





---

