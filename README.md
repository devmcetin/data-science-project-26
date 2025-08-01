🔍 Power BI Chart Uygulamaları İçin Soru Örnekleri
🟩 Power BI Chart Uygulama Soruları:

1) Bar Chart
Görev: Karakterlerin toplam öldürme sayılarını gösteren bar chart hazırla.
Alanlar: Character, Kills

2) Column Chart
Görev: Her meslekteki karakter sayısını göster.
Alanlar: Profession, count of Character

3) Pie Chart / Donut Chart
Görev: Karakterlerin tanıtım yıllarına göre dağılımını pasta grafikle göster.
Alan: Year_Introduced

4) Clustered Column Chart
Görev: Karakter başına Kills ve Episodes_Appeared verilerini yan yana göster.

5) Line Chart
Görev: Yıllara göre toplam öldürme sayısını gösteren çizgi grafik hazırla.

6) Ribbon Chart
Görev: Zamanla karakterlerin bölüm sayılarındaki değişimi göster (örnekleme).

7) Scatter Plot
Görev: Her karakterin bölüm başına düşen öldürme sayısını karşılaştır.
X: Episodes_Appeared, Y: Kills

8) Gauge Chart
Görev: En yüksek öldürme yapan karakterin yüzde oranını göster.

9) Toplam Kills adında bir Mesure oluştur:
   SUM fonksiyonunu kullanarak toplamda ne kadar kill yapıldığını tutsun

10) Öldürme Ortalamasını tutmak için AverageKillPerEpisode isminde bir measure oluştur.
    öldürme sayısı toplamını ve Episode_Appeared toplamını bulup bu iki değeri birbirine bölerek bu değeri bulmalısın(DIVIDE)
    
11) TopCharacter isminde bir measure oluştur. En Çok Görünen Karakteri bulmk için Episodes_Appeared kolonuna bakmalısın.
    TOPN fonksiyonunu kullanabilirsin.

12) IntroCount isminde bir measure oluştur. Yıllara Göre Tanıtılan Karakter Sayısını bulmaya çalışıyoruz. Year_Introduced kolonunu kullanabilirsin.
    Bu işlem için kullanabileceğin bir diğer fonksiyon COUNTAX fonksiyonu.
