# Data Science Project 26 — EduPlatform Analytics

**Modül**: Power BI Temelleri (Module 301) • **Süre**: 3-4 saat
**Dersler**: Ders 3 (PowerBI Özellikleri II) sonrası — DAX

---

## 🎯 Proje Senaryosu

**EduPlatform** isimli online eğitim şirketinde **junior data analyst**'isin. CEO sabah toplantıda şöyle dedi:

> "Hangi kategoriler kazandırıyor? Hangi planlar daha kâr getiriyor? Hangi öğrenciler tamamlamadan vazgeçiyor? Bana bunları gösteren bir Power BI dashboard'u hazırla — DAX measure'larla."

Bu projede **Ders 3'te öğrendiğimiz DAX formüllerini** (COUNT, MIN, MAX, AVG, SUM, IF, DATEDIFF) kullanarak **calculated measure**'lar oluşturacaksın.

---

## 📊 Veri Setleri (3 tablo — Star Schema)

`veri-setleri/` klasöründe 3 CSV var:

| Dosya | Satır | Tip | Açıklama |
|-------|-------|-----|----------|
| `ogrenciler.csv` | 50 | Dim | Öğrenci profili (yaş, şehir, üyelik, plan) |
| `kurslar.csv` | 20 | Dim | Kurs katalogu (kategori, eğitmen, fiyat, süre) |
| `kayitlar.csv` | 500 | **Fact** | Kayıt tarihi, ödeme, tamamlanma, durum |

**İlişki Yapısı**:
```
ogrenciler ──→ kayitlar ←── kurslar
   (1:N)                       (1:N)
```

**İlişkileri kur**:
- `kayitlar[Ogrenci_ID]` ↔ `ogrenciler[Ogrenci_ID]` (Many → One)
- `kayitlar[Kurs_ID]` ↔ `kurslar[Kurs_ID]` (Many → One)

**Önemli alanlar**:
- `ogrenciler.Plan`: Free / Pro / Premium (Pro %20 indirim, Premium %50 indirim)
- `kayitlar.Durum`: Tamamlandı / Devam Ediyor / İptal / Süresi Doldu
- `kayitlar.Sertifika_Aldi`: Evet / Hayır
- `kurslar.Seviye`: Başlangıç / Orta / İleri

---

## 📋 Görevler — DAX Measure'lar + Dashboard

### 1. **DAX Measure'lar** — `kayitlar` tablosuna ekle

Aşağıdaki **8 measure'u** DAX ile yaz:

| # | Measure Adı | Formül (örnek) |
|---|-------------|----------------|
| 1 | `Toplam Kayit` | `COUNTROWS(kayitlar)` |
| 2 | `Toplam Gelir` | `SUM(kayitlar[Odeme_Tutari_TL])` |
| 3 | `Ortalama Kurs Fiyati` | `AVERAGE(kurslar[Fiyat_TL])` |
| 4 | `En Yuksek Odeme` | `MAX(kayitlar[Odeme_Tutari_TL])` |
| 5 | `En Dusuk Odeme` | `MIN(kayitlar[Odeme_Tutari_TL])` |
| 6 | `Tamamlanma Orani %` | `DIVIDE(CALCULATE(COUNTROWS(kayitlar), kayitlar[Durum]="Tamamlandı"), [Toplam Kayit]) * 100` |
| 7 | `Sertifika Veren mi?` (calc col on kayitlar) | `IF(kayitlar[Sertifika_Aldi]="Evet" && kayitlar[Tamamlanma_Yuzdesi]>=90, "Evet", "Hayır")` |
| 8 | `Üyelik Süresi (gün)` (calc col on ogrenciler) | `DATEDIFF(ogrenciler[Uyelik_Tarihi], TODAY(), DAY)` |

### 2. **Dashboard** — DAX measure'larını görselleştir

Tek sayfada şu görseller olsun:

1. **4 KPI Kartı** (üst sıra):
   - Toplam Kayıt
   - Toplam Gelir
   - Ortalama Kurs Fiyatı
   - Tamamlanma Oranı %

2. **Bar Chart** — Kategori bazlı toplam gelir
   - X: kurslar.Kategori, Y: Toplam Gelir

3. **Line Chart** — Aylık kayıt trendi
   - X: kayitlar.Kayit_Tarihi (ay), Y: Toplam Kayıt

4. **Donut Chart** — Plan dağılımı
   - Dilimler: ogrenciler.Plan (Free/Pro/Premium)

5. **Stacked Bar** — Şehir × Plan → öğrenci sayısı
   - Y: ogrenciler.Sehir, Stack: ogrenciler.Plan

6. **Tablo Görsel** — En çok kayıt yaptıran 5 kurs
   - Sütunlar: Kurs Adı, Kategori, Toplam Kayıt, Toplam Gelir, Tamamlanma Oranı

7. **Funnel** — Kayıt durumu hunisi
   - Toplam Kayıt → Devam Eden → Tamamlanan → Sertifika Alan

### 3. **Slicer'lar** — En az 2 tane filtre

- Kategori (kurslar.Kategori) — dropdown
- Plan (ogrenciler.Plan) — buton listesi
- (Opsiyonel) Tarih (kayitlar.Kayit_Tarihi) — date range

---

## 📦 Proje Kurulumu

```bash
git clone <your-fork-url>
cd data-science-project-26
```

Power BI Desktop'a yükleme:
1. **Get Data → Text/CSV** → 3 CSV'yi tek tek yükle
2. Transform Data → tipleri doğrula (Tarih → Date, Tutar/Yuzde → Number)
3. **Model view** → 3 tablo arası ilişkileri kur (yukarıdaki star schema)
4. **Modelling tab → New Measure** ile yukarıdaki 8 measure'u yaz
5. **Report view** → görselleri yerleştir

---

## 📤 Submission — Pumble'dan Ekran Görüntüsü Gönder

1. **Dashboard'u tek sayfada topla**
2. **2 ekran görüntüsü al**:
   - `dashboard.png` — tüm dashboard
   - `dax-measures.png` — DAX measure listesi (Modelling tab)
3. **Pumble'a gönder** → kanal: `#proje-teslim`
4. Mesaj formatı:
   ```
   DS-26 Submission
   Repo: <senin GitHub fork URL'in>
   Ek: dashboard.png, dax-measures.png
   ```
5. Eğitmen kontrol edip "geçti" işaretler — Kaizu'da skor 100 görünür

---

## ✅ Başarı Kriterleri

Eğitmen kontrol edecek:
- [ ] 8 DAX measure doğru yazılmış (mantıken)
- [ ] Star schema ilişkileri kurulmuş
- [ ] 4 KPI kartı + 6+ görsel + 2 slicer var
- [ ] Tüm görseller doğru measure'ları kullanıyor
- [ ] Slicer'lar görselleri etkiliyor (interaktif)
- [ ] Premium öğrencilerin daha çok kurs aldığı görsel olarak anlaşılır

---

## 💡 İpuçları

- **Tarih hiyerarşisi**: Power BI Kayit_Tarihi'nden otomatik Year/Quarter/Month hiyerarşisi yapar — line chart drill-down olarak kullan
- **CALCULATE**: Filter context değiştirmek için kritik. Tamamlanma oranı için şart.
- **Conditional formatting**: KPI kartlarında eşik koy — Tamamlanma %80'in altındaysa kırmızı
- **Bookmark**: Kategori filtresi için bookmark ekle (Programlama / Veri Bilimi / Tasarım vb.)

---

## 🚫 Dikkat

- `veri-setleri/` içindeki CSV'leri değiştirme
- `.pbix` dosyasını commit etmene gerek yok (.gitignore'da exclude)
- DAX measure'larını **doğru naming** ile yaz (yukarıdaki tabloya yakın)
- İlişkiler **otomatik** algılanmazsa Modelling → Manage Relationships'ten manuel kur
