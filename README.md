# Coffe_Order_SQL

<p align="center">
  <h1 align="center">☕ Kahve Satış Operasyonları: Uçtan Uca Veri Modelleme ve Analizi</h1>
</p>

<p align="center">
  <a href="https://www.postgresql.org/">
    <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
  </a>
  <a href="https://www.python.org/">
    <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/Data_Engineering-FF6F00?style=for-the-badge&logo=databricks&logoColor=white" alt="Data Engineering"/>
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/ETL_Pipeline-0052CC?style=for-the-badge&logo=apacheairflow&logoColor=white" alt="ETL"/>
  </a>
</p>

<p align="center">
  <strong>Karmaşık ham verileri, stratejik iş kararlarına dönüştüren SQL tabanlı ETL mimarisi.</strong><br>
  Bu proje, tutarsız verilerin temizlenerek 3 katmanlı (Staging, Clean, Mart) bir veri ambarı modeline dönüştürülmesini ve iş zekası (BI) süreçlerine hazır hale getirilmesini kapsar.
</p>

---

## 📑 İçindekiler
1. [Proje Mimarisi (ETL Süreci)](#-proje-mimarisi-etl-süreci)
2. [Temel Performans Göstergeleri (KPIs)](#-temel-performans-göstergeleri-kpis)
3. [Veri Analizi ve İş İçgörüleri](#-veri-analizi-ve-i̇ş-i̇çgörüleri)
4. [Teknik Çözümler ve Veri Kalitesi](#-teknik-çözümler-ve-veri-kalitesi)
5. [Aksiyon Planı](#-aksiyon-planı)
6. [Dosya Yapısı](#-dosya-yapısı)

---

## 🏗 Proje Mimarisi (ETL Süreci)

Veri modelleme süreci, kurumsal en iyi pratikler (best practices) gözetilerek **3 ana katmanda** tasarlanmıştır:

| Katman | Açıklama | İşlev |
| :--- | :--- | :--- |
| **1. Staging Layer** | Ham Veri Katmanı | `order_id`, `city` gibi ham CSV verilerinin hiçbir dönüşüm yapılmadan dış sistemlerden doğrudan RDBMS'e aktarıldığı katmandır. |
| **2. Clean Layer** | Dönüşüm Katmanı | Veri tiplerinin (CAST/CONVERT) düzeltildiği, ayırıcıların (`,` yerine `_`) standardize edildiği ve eksik verilerin (NULL handling) yönetildiği katmandır. |
| **3. Mart Layer** | İş Zekası Katmanı | Dashboard (Power BI vb.) araçlarına bağlanmaya hazır, JOIN ve aggregation işlemlerinin yapıldığı View'ların bulunduğu katmandır. |

---

## 📊 Temel Performans Göstergeleri (KPIs)

Verilerin ETL süreçlerinden geçirilmesinin ardından işletmenin genel sağlığını gösteren temel metrikler:

| Metrik | Değer | Durum/Not |
| :--- | :--- | :--- |
| **Net Gelir** | 10.896,00 TL | *İptal ve iadeler hariç net ciro.* |
| **Toplam Sipariş** | 100 | *İncelenen periyottaki başarılı işlem hacmi.* |
| **Ortalama Sepet (AOV)**| 119,74 TL | *Müşteri başına düşen ortalama harcama tutarı.* |
| **İade Oranı** | %4,0 | ⚠️ *Hedeflenen oran %2. Kurye süreçleri incelenmeli.* |

---

## 💡 Veri Analizi ve İş İçgörüleri

Python görselleştirmeleri ve SQL sorguları sonucunda elde edilen stratejik bulgular:

### 📍 Şehir Bazlı Performans
> **İstanbul operasyonun kalbidir.** Toplam cironun **%33'ü** İstanbul'dan gelmektedir. Ankara ve İzmir stabil bir seyir izlerken, Antalya ve Bursa pazarlarında henüz ulaşılamamış ciddi bir büyüme potansiyeli mevcuttur.

### ⏱️ Operasyonel Yoğunluk ve Darboğazlar (Bottlenecks)
> Siparişler sabah **09:00** (Kahvaltı) ve **13:00-15:00** (Mola saatleri) arasında pik yapmaktadır. Đặcellikle saat **14:00**, mutfak ve kurye operasyonlarında hazırlık sürelerinin (prep_minutes) uzadığı en yüksek stres noktasıdır.

### ⭐ Müşteri Memnuniyeti Analizi
> **Kritik Eşik: 45 Dakika.** Teslimat süresi bu süreyi aştığında, müşteri değerlendirmelerinde (Rating) sert bir düşüş yaşanmaktadır. Müşteri şikayetlerinin odak noktası **'Cold' (Soğuk)** ve **'Late' (Gecikmiş)** teslimatlardır.

---

## 🛠 Teknik Çözümler ve Veri Kalitesi

Projeyi geliştirirken veri kalitesini (Data Quality) sağlamak adına uygulanan müdahaleler:
- **Veri Bütünlüğü (Data Integrity):** Hatalı matematiksel hesaplamalar barındıran kayıtlar (Örn: *Sipariş 1036*) SQL mantıksal kontrolleri (Flags) ile tespit edilip ayrıştırıldı.
- **Veri Standardizasyonu:** Tarih (Datetime) formatlarındaki tutarsızlıklar ve tutar (Currency) sembolleri veri ambarı standartlarına göre normalize edildi.
- **Ölçeklenebilirlik:** Kod blokları, gelecekteki veri artışına dayanacak şekilde Stored Procedure ve View'lar kullanılarak modüler hale getirildi.

---

## 🚀 Aksiyon Planı

Veriden elde edilen bulgular ışığında yönetime sunulan eylem önerileri:
1. **Pazarlama:** İstanbul şubelerinde ciro liderliğini korumak için yüksek harcama yapan müşterilere *Gold Tier* sadakat programı başlatılmalı.
2. **Operasyon:** Saat 14:00 darboğazını çözmek için öğleden sonra vardiyalarına ek kurye planlaması yapılmalı; >40 dk rotalar için ısı yalıtımlı çantalar zorunlu tutulmalı.
3. **Teknoloji:** Siparişlerdeki manuel matematiksel hataları önlemek adına POS ve sipariş sistemlerinde otomasyona geçilmeli.

---

## 📁 Dosya Yapısı

Aşağıdaki yapı, projenin modüler olarak nasıl kurgulandığını göstermektedir:

```text
├── data/
│   ├── Kahve_Siparis_RawData.csv      # İşlenmemiş ham veriler
│   └── Veri_Sozlugu.md                # Veri seti sütun açıklamaları
├── sql/
│   ├── 01_Staging_Tables.sql          # Tablo oluşturma ve BULK INSERT işlemleri
│   ├── 02_Clean_Transform.sql         # Veri temizleme fonksiyonları ve prosedürleri
│   └── 03_Data_Mart_Views.sql         # Analitik raporlama View'ları
├── notebooks/
│   └── EDA_ve_Gorsellestirme.ipynb    # Python tabanlı veri analizi
├── sunum/
│   └── Coffee_Order_SQL_Sunum.pptx    # Yönetici özeti sunumu
└── README.md
