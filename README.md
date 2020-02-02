# 2019 İstanbul Trafik Verisi
İstanbul Büyükşehir Belediyesi Açık Veri Portalı Trafik İndeksi Raporu İncelemesi

# Top 10

# İstanbul Trafik İndeksi
İstanbul Trafik İndeksi Raporu [İstanbul Büyükşehir Belediyesi Açık Veri Portalı](https://data.ibb.gov.tr/dataset/trafik-indeksi-raporu)'nda yayınlanan bir veri seti. Veri seti nasıl oluşmuş, indeks tam olarak neyi açıklıyor aslında veri setinde yazmıyor. Muhtemelen İBB Cep Trafik uygulmnasında sağ üst kösede yazan Trafik Yoğunluğu verisi.

## Orijinal format
Portaldaki veri [xlsx formatında](https://data.ibb.gov.tr/dataset/807be791-bc23-4f25-afeb-35ee9a4df43c/resource/e0a9dfd3-1579-4412-ab46-e54fb78e5b4d/download/trafik-indeks-raporu.xlsx). İçinde üç kolon var:

- ID	
- Trafik İndeksi
- Trafik İndeksi Tarihi

Trafik İndeksi Tarihi ölçümün yapıldığı zamanı gösteriyor. Bu veri iki değişik formatta. Çoğu "d.mm.yyyy  ss:dd:nn" formatında. Geri kalanı "yyyy-mm-dd ss:dd:nn.nnn" formatında. Bunları okurken aynı formata getirmek lazım (veriyi nasıl temizlediğimin detayları [şurada](Istanbul-Trafik-Veri-Hazirlik.ipynb). Benim analiz ettiğim veri 1 Ocak 2019 - 8 Ocak 2020 aralığındaydı. Veri güncellendikçe muhtemeelen yeni veriler eklenecektir. Ben analiz için sadece 2019 yılı verisini kullandım.

Trafik indeksi bir tam sayı. Yaklaşık beş dakikada bir yapılan ölçümlerden oluşuyor. Bir gün için tüm ölçümler yapılmışsa günlük 287-289 arası gözlem oluyor. Bu değişkenin en küçük değeri 1, en büyüğü 255. 255 belli ki bir hata değeri. Çünkü sonraki en büyük değer 81. Dolayısıyla 255 olan ölçümleri veriden çıkartarak analiz ettim.

ID anahtar olsun diye eklenmişe benziyor. Trafik İndeksi Tarihi varken ihtiyaç yok. Dolayısıyla ben analiz için veriyi alırken kullanmadım.

255'li değerleri çıkarttıktan sonra geriye 86,982 gözlem kalıyor. Bu gözlemler 308 güne ait. Yani 57 günde hiç veri yok (tüm günlerin %15.6'sı). Temmuz - Eylül arası uzun bir süre veri yok (18.07.2019 11:26 - 09.09.2019 09:40 arası). Bir De Mart'da bir veri kaybı görünüyor (22.03.2019 14:00 - 27.03.2019 11:31 arası). Son olarak Mayıs'da sorunlu bir gün var: 21-05-2019. Geri kalan günlerin 204 tanesinde tam veri var (287 ve üstü), 94 tanesinde de 12 ya da daha az eksik var. Geri kalan 8 tanesindeyse 117 ile 250 arası gözlem var. Tek bir günde 22 gözlem var: 24 Ocak. 

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Gu%CC%88nlu%CC%88k%20Go%CC%88zlem%20Say%C4%B1s%C4%B1).png)

# Analiz

## Gözlem Bazında
- Gözlem: 86,982
- Ortalama: 26.17
- Standart Sapma: 18.53
- Minimum: 1.00
- 25%: 8.00
- 50%: 26.00
- 75%: 40.00
- Maksimum: 81.00

Gözlemlere baktığımızda ortalama trafiğin 26,17, stadanrt sapmanın 18.53 olduğunu görüyoruz. Gözlemler normale yakın 

Yılın en yüksek trafik yoğunluğunun (81) gözlemlendiği zamanlar 31 Ocak Perşembe 18:36 - 18:56 arası (yarı yıl tatilinin son Perşembesi) ve 29 Haziran Cumartesi 15:53 - 17:08 arası (Büyükşehir Belediye Başkanlığı Seçiminden önceki gün).

Günlük ortalama ve maksimum değerler günden güne ciddi değişiklikler gösteriyor. Yedi günlük kayan ortalamalarla yıllık trendde daha iyi görünüyor.
![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Gu%CC%88nlu%CC%88k%20Kayan%20Ortalama%20ve%20Maksimum).png)




# İBB Açık Veri Portali
