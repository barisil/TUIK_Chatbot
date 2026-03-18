# 📊 TÜİK İstatistik Chatbot

## 🎥 Tanıtım Videosu

[YouTube'da İzle](https://www.youtube.com/watch?v=r3HDQdH1b2w)

OpenAI GPT-4o Mini ve RAGAS performans değerlendirmesi ile geliştirilmiş, Türkiye'nin gençlik, aile ve yaşlı istatistiklerine özel RAG tabanlı chatbot.

---

## 🎯 Özellikler

- **Akıllı Döküman Retrieval:** Yıl ve kategori bazlı filtreleme ile optimize edilmiş arama
- **Metadata Tabanlı Sınıflandırma:** Her chunk otomatik olarak yıl ve kategori bilgisi ile etiketlenir
- **Kavram Uyumsuzluk Kontrolü:** LLM'in yanlış bilgi üretmesini engelleyen guard mekanizması
- **Karşılaştırma Desteği:** Çoklu yıl sorgularında gelişmiş retrieval stratejisi
- **RAGAS Entegrasyonu:** Chatbot performansını ölçmek için kapsamlı değerlendirme sistemi
- **Kalıcı Vektör Veritabanı:** ChromaDB ile embedding'lerin disk üzerinde saklanması

---

## 📁 Proje Yapısı

```
CHATBOT_DERSI/
│
├── .vscode/                # VS Code yapılandırma dosyaları
├── chroma_db/              # Kalıcı vektör veritabanı (otomatik oluşturulur)
├── data/                   # PDF dokümanlar (TÜİK istatistikleri)
├── pages/                  # Streamlit sayfaları
│   └── ragas_evaluation.py # RAGAS değerlendirme sayfası
│
├── .env                    # Ortam değişkenleri (OPENAI_API_KEY)
├── .gitignore              # Git ignore dosyası
├── pdf_gemini_1.py         # Alternatif implementasyon (Gemini)
├── pdf_gpt_2.py            # Alternatif implementasyon (GPT v2)
├── pdf_gpt_3.py            # ⭐ Ana uygulama dosyası
├── requirements.txt        # Python bağımlılıkları
└── README.md               # Bu dosya
```

---

## 🚀 Kurulum

### 1. Gereksinimler

- Python 3.8+
- OpenAI API anahtarı

### 2. Bağımlılıkların Yüklenmesi

```bash
pip install -r requirements.txt
```

#### Ana Kütüphaneler

| Kütüphane | Amaç |
|-----------|------|
| `streamlit` | Web arayüzü |
| `langchain` | RAG pipeline |
| `langchain-openai` | OpenAI entegrasyonu |
| `langchain_google_genai` | Gemini entegrasyonu (karşılaştırma için) |
| `langchain-chroma` | Vektör veritabanı |
| `langchain-community` | Döküman yükleyiciler |
| `pypdf` | PDF okuma |
| `python-dotenv` | Ortam değişkenleri yönetimi |
| `ragas` | Performans değerlendirmesi |
| `datasets` | RAGAS için veri yönetimi |

Tam liste için `requirements.txt` dosyasına bakın.

### 3. Ortam Değişkenlerinin Ayarlanması

Proje kök dizininde `.env` dosyası oluşturun:

```
OPENAI_API_KEY=your_openai_api_key_here
```

### 4. PDF Dokümanların Eklenmesi

`data/` klasörüne TÜİK PDF dokümanlarınızı ekleyin. Dosya isimlendirme formatı:

```
kategori_yıl.pdf
```

Örnekler:

```
genclik_20.pdf  →  2020 Gençlik İstatistikleri
yasli_23.pdf    →  2023 Yaşlı İstatistikleri
aile_18.pdf     →  2018 Aile İstatistikleri
```

---

## 💻 Kullanım

### Ana Uygulamayı Çalıştırma

```bash
streamlit run pdf_gpt_3.py
```

Uygulama `http://localhost:8501` adresinde açılacaktır.

### İlk Çalıştırma

1. Uygulama açıldığında PDF'ler otomatik olarak yüklenecek ve vektör veritabanı oluşturulacaktır
2. Bu işlem ilk seferde birkaç dakika sürebilir
3. Sonraki çalıştırmalarda mevcut veritabanı kullanılacağı için hızlı açılır

### Soru Sorma

Chatbot aşağıdaki türde soruları yanıtlayabilir:

**Basit Sorular:**
```
- 2020 yılında genç nüfus oranı nedir?
- 2023 yılında akraba evliliği oranı nedir?
- 2024 yılında yaşlı nüfus kaç kişidir?
```

**Karşılaştırma Soruları:**
```
- 2014 ile 2024 arasında aile yapısı nasıl değişti?
- Yaşlı nüfus oranı yıllara göre nasıl bir trend gösteriyor?
```

**Kategori Bazlı Sorular:**
```
- En son yıl için gençlik istatistikleri nedir?
- Hangi yıllarda evlilik oranı en yüksekti?
```

---

## 📊 RAGAS Değerlendirmesi

Chatbot performansını ölçmek için RAGAS (Retrieval-Augmented Generation Assessment) entegrasyonu mevcuttur.

### RAGAS Sayfasına Erişim

1. Ana chatbot sayfasında en az bir soru sorun
2. Sağ alt köşedeki **"📈 RAGAS Değerlendirmesine Git"** butonuna tıklayın
3. `pages/ragas_evaluation.py` sayfası açılacaktır

### Değerlendirilen Metrikler

| Metrik | Açıklama |
|--------|----------|
| **Context Precision** | Getirilen bağlamın ne kadar ilgili olduğu |
| **Context Recall** | Doğru cevap için gerekli bilginin ne kadarının getirildiği |
| **Faithfulness** | Cevabın bağlama ne kadar sadık olduğu |
| **Answer Relevancy** | Cevabın soruyla ne kadar ilgili olduğu |

---

## ⚙️ Yapılandırma

`pdf_gpt_3.py` dosyasındaki sabitler:

```python
CHUNK_SIZE = 600    # Her chunk'ın karakter boyutu
CHUNK_OVERLAP = 120 # Chunk'lar arası örtüşme
TOP_K = 4           # Döndürülecek maksimum chunk sayısı
```

### Chunk Size ve Overlap Optimizasyonu

- `CHUNK_SIZE`: 500–800 arası değerler genelde iyi sonuç verir
- `CHUNK_OVERLAP`: %20 oranında overlap (`CHUNK_SIZE`'ın 1/5'i) önerilir
- Daha uzun dokümanlar için chunk size artırılabilir
- Daha hassas sorgular için overlap artırılabilir

---

## 🛡️ Güvenlik Önlemleri

### Kavram Uyumsuzluk Kontrolü (`guard_mismatch`)

LLM'in farklı ama benzer kavramları karıştırmasını önler:

```
❌ "yaşlı nüfus oranı"          ≠  "yaşlı bağımlılık oranı"
❌ "doğuşta beklenen yaşam süresi"  ≠  "65 yaşında beklenen yaşam süresi"
```

Uyumsuzluk tespit edilirse: `"Bu bilgi dokümanlarda bulunmamaktadır."`

---

## 🎨 Özellikler Detayı

### 1. Akıllı Retrieval (`retrieve_docs`)

```python
def retrieve_docs(question: str) -> List[Document]:
    # Metinden yıl çıkarma
    # Yıl bazlı filtreleme
    # Karşılaştırma sorularında k değerini artırma
    # Yedek genel arama
    # Tekrar eden sonuçları temizleme
```

### 2. Metadata Extraction

Her PDF'den otomatik olarak çıkarılır:

- **Kategori:** `genclik`, `yasli`, `aile`
- **Yıl:** Dosya adından (örn: `_20.pdf` → 2020)

### 3. Context Tracking

Her soru-cevap için kaydedilir:

- Kullanıcı sorusu
- Sistem cevabı
- Kullanılan context'ler
- Ground truth (varsa)

---

## 🔧 Sorun Giderme

| Problem | Çözüm |
|---------|-------|
| `"OPENAI_API_KEY bulunamadı"` | `.env` dosyasını kontrol edin ve API anahtarınızı ekleyin |
| `"PDF bulunamadı"` | `data/` klasörüne PDF dosyalarını ekleyin |
| Vektör veritabanı yavaş yükleniyor | İlk yüklemede normaldir; yeniden oluşturmak için `chroma_db/` klasörünü silin |
| Yanlış cevaplar alıyorum | `CHUNK_SIZE`, `CHUNK_OVERLAP` ve `TOP_K` değerlerini ayarlayın; PDF formatını kontrol edin |

---

## 📝 Ground Truth Test Soruları

RAGAS değerlendirmesinde kullanılan test soruları:

1. 2020 yılında genç nüfus oranı nedir?
2. 2023 yılında akraba evliliği oranı nedir?
3. 2014 yılında boşanan çift sayısı kaçtır?
4. 2018 yılında gençlerde işsizlik oranı nedir?
5. 2020 yılında ne eğitimde ne istihdamda olan gençlerin oranı nedir?
6. 2023 yılında internet kullanan gençlerin oranı nedir?
7. 2024 yılında yaşlı nüfus kaç kişidir?

---

## 🤝 Katkıda Bulunma

1. Fork edin
2. Feature branch oluşturun: `git checkout -b feature/amazing-feature`
3. Commit edin: `git commit -m 'Add amazing feature'`
4. Push edin: `git push origin feature/amazing-feature`
5. Pull Request açın

---

## 📄 Lisans

Bu proje eğitim amaçlıdır.

## 📧 İletişim

Sorularınız için issue açabilirsiniz.
