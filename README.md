# AfetHarita NLP Projesi

Kısa açıklama: Deprem sırasında toplanan Türkçe tweet'lerin sınıflandırılması, NER çıkarımı ve coğrafi haritalama ile acil durum tespiti.

Hızlı başlangıç

1. Sanal ortam oluşturun ve etkinleştirin:

```bash
python -m venv .venv
source .venv/bin/activate   # macOS / Linux
.venv\Scripts\Activate.ps1 # Windows PowerShell
```

2. Bağımlılıkları yükleyin:

```bash
pip install -r requirements.txt
```

3. Sunum oluşturun (örnek):

```bash
python create_presentation.py
```

Notlar
- Büyük ikili dosyalar ve veri kümeleri (`AfetHaritaApp-Portable/`, `tweets_tr_classified_all.csv`) `.gitignore` içine alındı. Eğer bu dosyaları GitHub'da saklamak isterseniz `git lfs` kullanmanızı öneririm.

Eğer GitHub'a yüklememi isterseniz, uzaktan depo oluşturma ve yönergeler konusunda yardımcı olabilirim.
