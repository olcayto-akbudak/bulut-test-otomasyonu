# Bulut Test Otomasyonu

Playwright + pytest kullanılarak web tabanlı ERP ve ticari uygulamaların UI/E2E testlerini otomatikleştirmek için hazırlanmış, **Page Object Model (POM)** tabanlı test otomasyon framework'üdür.

Proje; giriş, cari kart ve fatura gibi kritik iş akışlarının tekrar edilebilir şekilde test edilmesini, test sonuçlarının Excel ve iş takip sistemlerine aktarılabilecek özet olarak üretilmesini ve hata anında ekran görüntüsü, video ve Playwright Trace alınmasını amaçlar.

> Bu repository demo/test amaçlıdır. Gerçek müşteri verisi, üretim erişimi veya gizli şirket konfigürasyonu içermez.

## Özellikler

- Playwright + pytest
- Page Object Model
- JSON tabanlı test verisi
- Pozitif / negatif / iş kuralı testleri
- `smoke`, `regresyon`, `edonusum` marker'ları
- Playwright `storage_state`
- Test ortamı URL koruması
- Screenshot, video ve trace
- Excel test raporu
- İş takip sistemlerine aktarılabilecek özet
- Demo uygulama
- e-Dönüşüm senaryolarına genişletilebilir yapı

## Teknolojiler

Python · pytest · Playwright · JSON · openpyxl · python-dotenv

## Mimari

```text
Test Senaryosu / İş Kuralı
          │
          ▼
      tests/*.py
          │
          ▼
     pages/*.py
          │
          ▼
       Playwright
          │
          ▼
       Test Ortamı
          │
          ├── Screenshot
          ├── Video
          └── Trace
                    │
                    ▼
              pytest sonuçları
                    │
                    ├── Excel
                    └── İş takip özeti
```

Detay: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)

## Kurulum

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

```bash
pip install -r requirements.txt
playwright install chromium
```

`.env.example` dosyasını `.env` olarak kopyala.

## Demo

Terminal 1:

```bash
python -m http.server 8765 -d demo_site
```

Terminal 2:

```bash
pytest -v
```

Demo kullanıcı:

```text
Kullanıcı: demo
Şifre: demo
```

## Test çalıştırma

```bash
pytest -m smoke -v
pytest -m regresyon -v
pytest -k CARI -v
```

e-Dönüşüm testleri yalnızca yetkili test ortamında:

```bash
GIB_TEST_ORTAMI=1 pytest -m edonusum -v
```

## Hata analizi

Başarısız testlerde screenshot, video ve `trace.zip` üretilebilir.

```bash
playwright show-trace test-results/<klasor>/trace.zip
```

## Gerçek test ortamına uyarlama

1. `BASE_URL` test ortamını göstermeli.
2. Kimlik bilgileri `.env` üzerinden verilmelidir.
3. Gerçek locator'lar doğrulanmalıdır.
4. Test verileri sentetik olmalıdır.
5. e-Dönüşüm gönderimleri yalnızca resmi test ortamında çalıştırılmalıdır.

Detay: [`docs/REAL_ENVIRONMENT.md`](docs/REAL_ENVIRONMENT.md)

## Test stratejisi

Testler UI etkileşiminin yanında kritik iş kurallarını da doğrular.

Örneğin:

```text
Miktar × Birim Fiyat
        │
        ▼
Satır Tutarı
        │
        ▼
KDV
        │
        ▼
Beklenen Genel Toplam
```

Beklenen sonuç mümkün olduğunca test verisinden bağımsız hesaplanır.

Detay: [`docs/TEST_STRATEGY.md`](docs/TEST_STRATEGY.md)

## Senaryolar

Örnek senaryolar:

- geçerli giriş
- yanlış şifre
- tüzel cari oluşturma
- şahıs cari oluşturma
- geçersiz VKN/TCKN
- boş ünvan
- tek satırlı e-Arşiv fatura
- farklı KDV oranları
- e-Dönüşüm gönderim akışı

Detay: [`docs/SCENARIO_CATALOG.md`](docs/SCENARIO_CATALOG.md)

## Güvenlik

Public repository'de:

- `.env` commit edilmez
- parola/token paylaşılmaz
- `.auth/` paylaşılmaz
- gerçek müşteri verisi kullanılmaz
- üretim URL'si eklenmez
- kişisel/gizli veri AI araçlarına gönderilmez

Detay: [`docs/SECURITY.md`](docs/SECURITY.md)

## AI destekli geliştirme

AI; test senaryosu üretimi, negatif/sınır senaryolarının bulunması, Page Object refactoring, Trace analizi, hata sınıflandırması ve dokümantasyon için yardımcı olarak kullanılabilir.

Parola, token, session cookie, canlı URL veya gerçek müşteri verisi AI sistemlerine gönderilmemelidir.

## Katkı

Yeni ekran ve senaryo standartları: [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)

## Lisans

Bu proje MIT License altında lisanslanmıştır.
