# 2019 İstanbul Trafik Verisi
İstanbul Büyükşehir Belediyesi Açık Veri Portalı Trafik İndeksi Raporu İncelemesi

# Top 10

# İstanbul Trafik İndeksi
İstanbul Trafik İndeksi Raporu [İstanbul Büyükşehir Belediyesi Açık Veri Portalı](https://data.ibb.gov.tr/dataset/trafik-indeksi-raporu)'nda yayınlanan bir veri seti. Veri seti nasıl oluşmuş, indeks tam olarak neyi açıklıyor aslında veri setinde yazmıyor. Muhtemelen İBB Cep Trafik uygulmnasında sağ üst kösede yazan Trafik Yoğunluğu verisi buradaki sayı.

## Orijinal format
Portaldaki veri [xlsx formatında](https://data.ibb.gov.tr/dataset/807be791-bc23-4f25-afeb-35ee9a4df43c/resource/e0a9dfd3-1579-4412-ab46-e54fb78e5b4d/download/trafik-indeks-raporu.xlsx). İçinde üç kolon var:

- ID	
- Trafik İndeksi
- Trafik İndeksi Tarihi

**Trafik İndeksi Tarihi** ölçümün yapıldığı zamanı gösteriyor. Bu veri iki değişik formatta. Çoğu "d.mm.yyyy  ss:dd:nn" formatında. Geri kalanı "yyyy-mm-dd ss:dd:nn.nnn" formatında. Bunları okurken aynı formata getirmek lazım (veriyi nasıl temizlediğimin detayları [şurada](Istanbul-Trafik-Veri-Hazirlik.ipynb)). Benim analiz ettiğim veri 1 Ocak 2019 - 8 Ocak 2020 aralığındaydı. Veri güncellendikçe muhtemeelen yeni veriler eklenecektir. Ben analiz için sadece 2019 yılı verisini kullandım.

**Trafik İndeksi** bir tam sayı. Yaklaşık beş dakikada bir yapılan ölçümlerden oluşuyor. Bir gün için tüm ölçümler yapılmışsa günlük 287-289 arası gözlem oluyor. Bu değişkenin en küçük değeri 1, en büyüğü 255. 255 belli ki bir hata değeri. Çünkü sonraki en büyük değer 81. Dolayısıyla 255 olan ölçümleri veriden çıkartarak analiz ettim.

**ID** anahtar olsun diye eklenmişe benziyor. Trafik İndeksi Tarihi varken ihtiyaç yok. Dolayısıyla ben analiz için veriyi alırken kullanmadım.

255'li değerleri çıkarttıktan sonra geriye 86,982 gözlem kalıyor. Bu gözlemler 308 güne ait. Yani 57 günde hiç gözlem yok (tüm günlerin %15.6'sı). En uzun eksik veri serisi Temmuz - Eylül arası (18.07.2019 11:26 - 09.09.2019 09:40). Mart'da da kısa bir veri kaybı görünüyor (22.03.2019 14:00 - 27.03.2019 11:31). Son olarak Mayıs'da sorunlu tek bir gün var: 21 Mayıs. Geri kalan günlerin 204 tanesinde tam veri var (287 ve üstü gözlem), 94 tanesinde de 12 ya da daha az eksik var. Geri kalan 8 tanesindeyse 117 ile 250 arası gözlem var. 24 Ocak biarz sorunlu, 22 gözlem var sadece. 

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

Gözlemlere baktığımızda ortalama trafiğin 26.17, stadanrt sapmanın 18.53 olduğunu görüyoruz. Gözlemler normale benzer bir şekilde dağılmamış. SAnki bir medyanı 40 civarında normal dağılım var bir de Sola yaslanmış ve modu 1-2 olan bir dağılım daha var. Ama kantiller arası mesafe iki standart sapmaya yakın, dolayısıyla eğrisi doğrusuna denk geliyor:)

![](/Figures/O%CC%88lc%CC%A7u%CC%88m%20Baz%C4%B1nda%20Trafik%20Yog%CC%86unlug%CC%86u%20(Dag%CC%86%C4%B1l%C4%B1m).png)

Yılın en yüksek trafik yoğunluğunun (81) gözlemlendiği zamanlar 31 Ocak Perşembe 18:36 - 18:56 arası (yarı yıl tatilinin son Perşembesi) ve 29 Haziran Cumartesi 15:53 - 17:08 arası (Büyükşehir Belediye Başkanlığı Seçiminden önceki gün). Diğer maksimum gözlemleri de 20 Eylül Cuma, 31 Ekim Perşembe ve 11 Aralık Çarşamba günleri 18:00 - 19:00 arası görüyoruz hep.

## Günlük Veriler

Günlük ortalama ve maksimum değerler günden güne ciddi değişiklikler gösteriyor. Yedi günlük kayan ortalamalarla yıllık trendi daha iyi görmek mümkün. Hem günlük ortalama trafik (o günkü gözlemlerin ortalaması) hem de o günkü maksimum trafik (her gün içindeki en yüksek gözlem) Temmuz ortasına kadar yavaş yavaş azalıyor (25'ten 20'ye gibi) sonra yıl sonuna kadar daha hızlı bir şekilde artıyor (20'den 35'e gibi). Temmuz'un ikinci yarısı ve Ağustos ayı verileri olmadığı için bu grafikte trafikteki artış 15 Temmuz'da başlıyormuş gibi gözüküyor ama veri olsa muhtemelen düşüşün Ağustos sonuna kadar devam ettiğini görürdük.

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Gu%CC%88nlu%CC%88k%20Kayan%20Ortalama%20ve%20Maksimum).png)

Günlük ortalama ve maksimm değerlerin dağılımı normal dağılıma yakın. Ortalamaların ortalaması 26.09, standart sapması 5.92. Günlük ortalamaların yaklaşık %27'si bir standart sapmadan daha uzakta ortalamadan. Günlük maksimum değerlerin de ortalaması 58.30, standart sapması 11.72. Maksimum değerlerin de yine yaklaşık %28'i bir standart sapmanın dışında kalıyor.

![](/Figures/Gu%CC%88nlu%CC%88k%20Trafik%20Yog%CC%86unlug%CC%86u%20(Dag%CC%86%C4%B1l%C4%B1m).png)

Günlük ortalamaların en yüksek olduğu beş gün şöyle:

Tarih|Ortalama
-----|--------
4 Aralık | 39.80
26	Aralık | 38.69
1	Kasım | 37.77
20	Eylül | 37.62
27	Aralık | 36.94

Aşağıda bütün yılı gösteren bir ısı haritası var. Her hücre bir günü gösteriyor. Hücre renkleri günlük ortalamaya göre değişiyor. Ortalama yükseldikçe ilgili hücrenin rengi de kızarıyor, düştükçe, yeşilleşiyor, ortalamaya yaklaştıkça beyazlaşıyor. Her hücrenin sol üst köşesinde o gün gözlemlenen maksimum trafik yazıyor, ortasındaysa o günün ortalaması. Haftasonları maviyle, resmi tatiller sarıyla işsaretlenmiş. Okul tatillerine denk gelen hücrelerin altında da sarı bir bant var.

![](/Figures/Trafik%20Yoğunluğu%20(Ay%20ve%20Günlere%20Göre).png)

Isı haritası bize üç (buçuk) şey gösteriyor (öncelikle Temmuz - Ağustos olmayan veriyi gösteriyor tabii:).
* Haftasonları, daha doğrusu Pazar günleri, trafik rahatlıyor. Bunu biliyorduk zaten:)
* Resmi tatillerde de trafik rahatlıyor. Özellikle de Haziran başındaki bayramda hakikaten keyifli olmuş trafik. Ama 19 Mayıs, 15 Temmuz, 23 Nisan da rahatlatmış.
* Yarı yıl tatili trafiği pek rahatlatmıyor. Yarı yıl tatilinin ilk haftası önceki haftaya göre biraz daha iyi olsa da trafik ikinci haftada tekrar yükseliyor, hatta yılın en yoğın zamanlarından biri yaşanıyor. 
* Yaz tatili verisi biraz daha karışık. Haziran seçimlerine kadar geçen zaman zaten tipik bir zaman değil. Bayram haricinde kalan günlerde okul tatil ama yazlığa ve tatile giden muhtemelen seçim olayan yıllara göre daha az. Temmuz'un ilk 15 gününü Ekim'in ilk 15 günüyle karşılaştırdığımızda ortalamar arasında 6.6 puanlık bir fark (23.32, 29.92), 75. kantilleri arasında da 9 puanlık (35, 44) bir fark görüyoruz. Yani Temmuz trafiği Ekim'e göre bayağı daha iyi. Zaten aşağıda ilk 15 günleri karşılaştıran grafikte de görebilrisiniz.

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Temmuz%201-15%2C%20Ekim%201-15%20Kars%CC%A7%C4%B1las%CC%A7t%C4%B1rmas%C4%B1).png)

## Aylar ve Haftanın Günü

Hazır aylara bakmışken bütün aylara bakmakta yarar var. En 

<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th></th>\n      <th>mean</th>\n      <th>max</th>\n      <th>min</th>\n      <th>median</th>\n      <th>std</th>\n      <th>25. Kantil</th>\n      <th>75. Kantil</th>\n    </tr>\n    <tr>\n      <th>Ay</th>\n      <th></th>\n      <th></th>\n      <th></th>\n      <th></th>\n      <th></th>\n      <th></th>\n      <th></th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>Ocak</th>\n      <td>25.587357</td>\n      <td>81.0</td>\n      <td>1.0</td>\n      <td>24.0</td>\n      <td>18.103894</td>\n      <td>9.0</td>\n      <td>38.0</td>\n    </tr>\n    <tr>\n      <th>Şubat</th>\n      <td>26.600398</td>\n      <td>76.0</td>\n      <td>1.0</td>\n      <td>26.0</td>\n      <td>17.882722</td>\n      <td>9.0</td>\n      <td>40.0</td>\n    </tr>\n    <tr>\n      <th>Mart</th>\n      <td>24.780998</td>\n      <td>76.0</td>\n      <td>1.0</td>\n      <td>26.0</td>\n      <td>18.659537</td>\n      <td>6.0</td>\n      <td>40.0</td>\n    </tr>\n    <tr>\n      <th>Nisan</th>\n      <td>26.017552</td>\n      <td>71.0</td>\n      <td>1.0</td>\n      <td>27.0</td>\n      <td>17.751961</td>\n      <td>8.0</td>\n      <td>40.0</td>\n    </tr>\n    <tr>\n      <th>Mayıs</th>\n      <td>22.820428</td>\n      <td>71.0</td>\n      <td>1.0</td>\n      <td>22.0</td>\n      <td>18.024812</td>\n      <td>4.0</td>\n      <td>36.0</td>\n    </tr>\n    <tr>\n      <th>Haziran</th>\n      <td>22.757448</td>\n      <td>81.0</td>\n      <td>1.0</td>\n      <td>20.0</td>\n      <td>17.857419</td>\n      <td>4.0</td>\n      <td>38.0</td>\n    </tr>\n    <tr>\n      <th>Temmuz</th>\n      <td>23.025841</td>\n      <td>56.0</td>\n      <td>1.0</td>\n      <td>26.0</td>\n      <td>14.641085</td>\n      <td>9.0</td>\n      <td>35.0</td>\n    </tr>\n    <tr>\n      <th>Ağustos</th>\n      <td>NaN</td>\n      <td>NaN</td>\n      <td>NaN</td>\n      <td>NaN</td>\n      <td>NaN</td>\n      <td>NaN</td>\n      <td>NaN</td>\n    </tr>\n    <tr>\n      <th>Eylül</th>\n      <td>26.498176</td>\n      <td>80.0</td>\n      <td>1.0</td>\n      <td>29.0</td>\n      <td>18.854179</td>\n      <td>6.0</td>\n      <td>42.0</td>\n    </tr>\n    <tr>\n      <th>Ekim</th>\n      <td>26.798295</td>\n      <td>80.0</td>\n      <td>1.0</td>\n      <td>27.0</td>\n      <td>19.262072</td>\n      <td>8.0</td>\n      <td>42.0</td>\n    </tr>\n    <tr>\n      <th>Kasım</th>\n      <td>30.309482</td>\n      <td>76.0</td>\n      <td>1.0</td>\n      <td>31.0</td>\n      <td>19.240169</td>\n      <td>11.0</td>\n      <td>45.0</td>\n    </tr>\n    <tr>\n      <th>Aralık</th>\n      <td>30.892396</td>\n      <td>80.0</td>\n      <td>1.0</td>\n      <td>31.0</td>\n      <td>19.576070</td>\n      <td>11.0</td>\n      <td>45.0</td>\n    </tr>\n  </tbody>\n</table>





# İBB Açık Veri Portali
