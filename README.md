# 2019 İstanbul Trafik Verisi
İstanbul Büyükşehir Belediyesi Açık Veri Portalı Trafik İndeksi Raporu İncelemesi

# Top 10

1. Trafik hafta başından hafta sonuna doğru her geçen gün gittikçe kötüleşiyor.
2. Akşamlar yoğunluğu sabah yoğunluğuna göre çok daha kötü.
3. Hatta öğleden sonra trafiği bile sabah trafiğinden daha kötü.
4. Cumartesi günleri hiç masum değil. Öğleden sonra trafik hafta içini aratmıyor.
5. Trafikten en az etkilenmek istiyorsanız sabah 10 gibi evden çıkıp akşam da sekiz gibi işten çıkmak en iyisi.
6. Kasım ve Aralık trafiğin en kötü olduğu aylar.
7. Bayramlar trafiği gerçekten rahatlatıyor. Resmi tatiller de. Ama bayram öncesinde yılın en kötü trafikleri de yaşanıyor.
8. Yarıyıl tatilinin trafiğe pek faydası yok.
9. Dün trafik çok kötüydü, millet bugün çıkmamış gözlemi, dün trafik endeksi 40'ların üzerine çıkmışsa doğru.
10. [İBB Açık Veri Portali](https://data.ibb.gov.tr/) tüm eksikliklerine rağmen gayet iyi bir proje. Daha iyi olmasını dört gözle bekliyorum.

# İstanbul Trafik İndeksi
İstanbul Trafik İndeksi Raporu [İstanbul Büyükşehir Belediyesi Açık Veri Portalı](https://data.ibb.gov.tr/dataset/trafik-indeksi-raporu)'nda yayınlanan bir veri seti. Veri seti nasıl oluşmuş, indeks tam olarak neyi açıklıyor aslında veri setinde yazmıyor. Muhtemelen İBB Cep Trafik uygulamasında, sağ üst köşede yazan Trafik Yoğunluğu sayısı buradaki veriyle aynı.

## Orijinal format
Portaldaki veri [xlsx formatında](https://data.ibb.gov.tr/dataset/807be791-bc23-4f25-afeb-35ee9a4df43c/resource/e0a9dfd3-1579-4412-ab46-e54fb78e5b4d/download/trafik-indeks-raporu.xlsx). İçinde üç kolon var:

- ID	
- Trafik İndeksi
- Trafik İndeksi Tarihi

**Trafik İndeksi Tarihi** ölçümün yapıldığı zamanı gösteriyor. Bu veri iki değişik formatta. Çoğu "d.mm.yyyy  ss:dd:nn" formatında. Geri kalanı "yyyy-mm-dd ss:dd:nn.nnn" formatında. Bunları okurken aynı formata getirmek lazım (veriyi nasıl temizlediğimin detayları [şurada](Istanbul-Trafik-Veri-Hazirlik.ipynb)). Benim analiz ettiğim veri 1 Ocak 2019 - 8 Ocak 2020 aralığındaydı. Veri güncellendikçe muhtemeelen yeni veriler eklenecektir. Ben analiz için sadece 2019 yılı verisini kullandım.

**Trafik İndeksi** bir tam sayı. Yaklaşık beş dakikada bir yapılan ölçümlerden oluşuyor. Bir gün için tüm ölçümler yapılmışsa günlük 287-289 arası gözlem oluyor. Bu değişkenin en küçük değeri 1, en büyüğü 255. 255 belli ki bir hata değeri. Çünkü sonraki en büyük değer 81. Dolayısıyla 255 olan ölçümleri veriden çıkartarak analiz ettim.

**ID** anahtar olsun diye eklenmişe benziyor. Trafik İndeksi Tarihi varken ihtiyaç yok. Dolayısıyla ben analiz için veriyi alırken kullanmadım.

255'li değerleri çıkarttıktan sonra geriye 86,982 gözlem kalıyor. Bu gözlemler 308 güne ait. Yani 57 günde hiç gözlem yok (tüm günlerin %15.6'sı). En uzun eksik veri serisi Temmuz - Eylül arası (18.07.2019 11:26 - 09.09.2019 09:40). Mart'da da kısa bir veri kaybı görünüyor (22.03.2019 14:00 - 27.03.2019 11:31). Son olarak Mayıs'da sorunlu tek bir gün var: 21 Mayıs. Geri kalan günlerin 204 tanesinde tam veri var (287 ve üstü gözlem), 94 tanesinde de 12 ya da daha az eksik var. Geri kalan 8 tanesindeyse 117 ile 250 arası gözlem var. 24 Ocak biraz sorunlu, 22 gözlem var sadece. 

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

Gözlemlere baktığımızda ortalama trafiğin 26.17, standart sapmanın 18.53 olduğunu görüyoruz. Gözlemler normale benzer bir şekilde dağılmamış. Sanki  medyanı 40 civarında bir normal dağılım var, bir de sola yaslanmış ve modu 1-2 olan bir dağılım daha var. Ama kantiller arası mesafe iki standart sapmaya yakın, dolayısıyla eğrisi doğrusuna denk geliyor:)

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

Aşağıda bütün yılı gösteren bir ısı haritası var. Her hücre bir günü gösteriyor. Hücre renkleri günlük ortalamaya göre değişiyor. Ortalama yükseldikçe ilgili hücrenin rengi de kızarıyor, düştükçe, yeşilleşiyor, ortalamaya yaklaştıkça beyazlaşıyor. Her hücrenin sol üst köşesinde o gün gözlemlenen maksimum trafik yazıyor, ortasındaysa o günün ortalaması. Hafta sonları maviyle, resmi tatiller sarıyla işaretlenmiş. Okul tatillerine denk gelen hücrelerin altındaysa sarı bir bant var.

![](/Figures/Trafik%20Yoğunluğu%20(Ay%20ve%20Günlere%20Göre).png)

Isı haritası bize dört (hatta üç buçuk:) şey gösteriyor (öncelikle Temmuz - Ağustos olmayan veriyi gösteriyor tabii:).
1. Haftasonları, daha doğrusu Pazar günleri, trafik rahatlıyor. Bunu biliyorduk zaten:)
2. Resmi tatillerde de trafik rahatlıyor. Özellikle de Haziran başındaki bayramda hakikaten keyifli olmuş trafik. Ama 19 Mayıs, 15 Temmuz, 23 Nisan da rahatlatmış.
3. Yarı yıl tatili trafiği pek rahatlatmıyor. Yarı yıl tatilinin ilk haftası önceki haftaya göre biraz daha iyi olsa da trafik ikinci haftada tekrar yükseliyor, hatta yılın en yoğın zamanlarından biri yaşanıyor.
4. Yaz tatili verisi biraz daha karışık. Haziran seçimlerine kadar geçen zaman zaten tipik bir zaman değil. Bayram haricinde kalan günlerde okul tatil ama yazlığa ve tatile giden muhtemelen seçim olayan yıllara göre daha az. Temmuz'un ilk 15 gününü Ekim'in ilk 15 günüyle karşılaştırdığımızda ortalamalar arasında 6.6 puanlık bir fark (23.32, 29.92), 75. kantilleri arasında da 9 puanlık (35, 44) bir fark görüyoruz. Yani Temmuz trafiği Ekim'e göre bayağı daha iyi. Bunu aşağıda bu ayların ilk 15 günlerini karşılaştıran grafikte de görebilrisiniz.

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Temmuz%201-15%2C%20Ekim%201-15%20Kars%CC%A7%C4%B1las%CC%A7t%C4%B1rmas%C4%B1).png)

## Aylar

Hazır aylara bakmışken bütün aylara bakabiliriz.

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Ay</th>
      <th>Ortalama</th>
      <th>Maksimum</th>
      <th>Minimum</th>
      <th>Medyan</th>
      <th>Standart Sapma</th>
      <th>25. Kantil</th>
      <th>75. Kantil</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ocak</th>
      <td>25.59</td>
      <td>81.0</td>
      <td>1.0</td>
      <td>24.0</td>
      <td>18.109</td>
      <td>9.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>Şubat</th>
      <td>26.60</td>
      <td>76.0</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>17.889</td>
      <td>9.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>Mart</th>
      <td>24.79</td>
      <td>76.0</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>18.66</td>
      <td>6.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>Nisan</th>
      <td>26.02</td>
      <td>71.0</td>
      <td>1.0</td>
      <td>27.0</td>
      <td>17.75</td>
      <td>8.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>Mayıs</th>
      <td>22.82</td>
      <td>71.0</td>
      <td>1.0</td>
      <td>22.0</td>
      <td>18.02</td>
      <td>4.0</td>
      <td>36.0</td>
    </tr>
    <tr>
      <th>Haziran</th>
      <td>22.76</td>
      <td>81.0</td>
      <td>1.0</td>
      <td>20.0</td>
      <td>17.86</td>
      <td>4.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>Temmuz</th>
      <td>23.03</td>
      <td>56.0</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>14.64</td>
      <td>9.0</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>Ağustos</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Eylül</th>
      <td>26.50</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>29.0</td>
      <td>18.85</td>
      <td>6.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>Ekim</th>
      <td>26.80</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>27.0</td>
      <td>19.26</td>
      <td>8.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>Kasım</th>
      <td>30.31</td>
      <td>76.0</td>
      <td>1.0</td>
      <td>31.0</td>
      <td>19.24</td>
      <td>11.0</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>Aralık</th>
      <td>30.89</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>31.0</td>
      <td>19.58</td>
      <td>11.0</td>
      <td>45.0</td>
    </tr>
  </tbody>
</table>

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Aylara%20Go%CC%88re).png)

En kötü aylar Kasım ve Aralık. Eylül ve Ekim onların altonda bir başka seviye. Şubat, Mart ve Nisan ortalama açısından Eylül ve Ekime benzese de 75. kantilleri daha düşük bir grup oluşturuyor. Ocak bu gruptan da daha düşük. Mayıs-Haziran en düşük grubu oluşturuyorlar. Temmuz verisi eksik olduğu için ay bazında karşılaştırmak çok anlamlı deği. Bu durumda sanki ilk dört ay ortalamanın altı, sonraki dört ay düşük, sonra gelen Eylül ve Ekim yüksek ve son olarak Kasım ve Aralık en yüksek gibi bir sıralama yapabiliriz sanki.

##  Haftanın Günü ve Saat

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Haftan%C4%B1n%20Gu%CC%88nu%CC%88%20ve%20Saatlere%20Go%CC%88re).png)

Veriye haftanın günleri ve saatler bazında baktığımızda da göze çarpan birkaç şey var:

1. Yoğun trafik hafta içi sabah yedide başlıyor, akşam sekizde bitiyor. Cuma akşam dokuza kadar devam ediyor.
2. Pazartesi'den Cuma'ya bu yoğun saatlerde her geçen gün trafik daha da kötüleşiyor.
3. Tek istinası Cuma 13:00 - 15:00 arası. Sanırım Cuma namazı etkisi.
4. Cumartesi saat 11'e kadar trafik sakin, ama 11^den sonra hafta içi 16:00 - 17:00 trafiğine benzeyen bir seviyeye çıkıyor ve akşam sekize kadar böyle devam ediyor.
5. Pazar günleri trafik Cumartesiye göre iki saat daha geç başlıyor. Saat 14:00 gibi. Sonra akşam sekize kadar hafta içi 13:00 - 15:00 arası trafik seviyesinde devam ediyor.
6. Akşam dokuzla geceryarısı arası trafik de Pazartesi'den başlayarak her gün yükseliyor. Bu saatlerde Pazar gecesi Cumartesi'den pek farklı değil. Sadece gece 23:00 geceyarısı arasında Pazarlar daha düşük.
7. Geceyarısından sonra trafik hızla düşüyor ve saat sabah altıdan sonra tekrar yükselmeye başlıyor. Cumartesi, Pazar ve Pazartesi geceyarısıyla sabah bir arasında trafik diğer gecelere göre daha fazla.

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Gün</th>
      <th>Ortalama</th>
      <th>Maksimum</th>
      <th>Minimum</th>
      <th>Medyan</th>
      <th>Standart Sapma</th>
      <th>25. Kantil</th>
      <th>75. Kantil</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cuma</th>
      <td>30.20</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>31.0</td>
      <td>19.79</td>
      <td>11.0</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>Perşembe</th>
      <td>28.56</td>
      <td>81.0</td>
      <td>1.0</td>
      <td>31.0</td>
      <td>19.18</td>
      <td>9.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <th>Çarşamba</th>
      <td>27.87</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>31.0</td>
      <td>18.54</td>
      <td>9.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>Salı</th>
      <td>27.26</td>
      <td>76.0</td>
      <td>1.0</td>
      <td>31.0</td>
      <td>18.85</td>
      <td>8.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>Cumartesi</th>
      <td>25.89</td>
      <td>81.0</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>17.65</td>
      <td>9.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>Pazartesi</th>
      <td>24.75</td>
      <td>72.0</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>17.64</td>
      <td>8.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>Pazar</th>
      <td>18.52</td>
      <td>65.0</td>
      <td>1.0</td>
      <td>15.0</td>
      <td>15.26</td>
      <td>4.0</td>
      <td>31.0</td>
    </tr>
  </tbody>
</table>

![](/Figures/Trafik%20Yog%CC%86unlug%CC%86u%20(Haftan%C4%B1n%20Gu%CC%88nu%CC%88ne%20Go%CC%88re).png)

## Vakitler

Teker teker saatler yerine günüparçalara bölerek de bakmak mümkün. Buna vakitler diyelim (daha iyi bir isim bulamadım). Ben yaptığım basit we araştırmasında trafikle ilgili bu konuda bir standart sınıflama bulamadım. Onun için buradaki sınıflama bir İstanbullu olarak benim deneyimlerime ve verinin kendisine biraz dayanıyor. Vakitler şöyle:

Vakit|Saatler
-----|-------
Sabah Yoğunluğu|07:00-09:59 
Öğleden Önce|10:00-12:59 
Öğlen|13:00-14:59 
Öğleden Sonra|15:00-16:59 
Akşam Yoğunluğu|17:00-19:59 
Gece|20:00-22:59 
Gece Yarısı|23:00-01:59 
Sabaha Karşı|02:00-04:59 
Sabah|05:00-06:59

Aşağıdaki grafikte ve tabloda da göreceğiniz gibi, Akşam Yoğunluğu en yoğun vakit. Şaşırtıcı bir şekilde ikinci en yoğun vakit Sabah Yoğunluğu değil. Sabah Yoğunluğunun ortalama ve medyan trafiği Akşam Yoğunluğuna kadar olan vakitlere çok yakın ya da altında. Tabii Sabah Yoğunluğu kendini izleyen bu diğer vakitlere göre daha geniş bir dağılıma sahip. Yani ortalama belki düşük ama bazı günler çok fazla trafik olma ihtimali de var. Bir de ilginç olan Gece Yarısı ve Sabaha Karşı vakitlerindeki yüksek değerler. Örneğin 26 ve 27 Mayıs geceyarısında trafik indeksi bayağı yükselmiş. Benzer bir şekilde 1 Nisan ve 5 Ekim tarihlerinde de sabaha karşı bayağı yoğun trafik görüyoruz.

![](/Figures/Gu%CC%88nu%CC%88n%20Vakitleri%20Dag%CC%86%C4%B1l%C4%B1m%20Kars%CC%A7%C4%B1las%CC%A7t%C4%B1rmas%C4%B1.png)

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Vakit</th>
      <th>Ortalama</th>
      <th>Maksimum</th>
      <th>Minimum</th>
      <th>Medyan</th>
      <th>Standart Sapma</th>
      <th>25. Kantil</th>
      <th>75. Kantil</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Akşam Yoğunluğu 17:00-19:59</th>
      <td>50.71</td>
      <td>81.0</td>
      <td>2.0</td>
      <td>53.0</td>
      <td>13.40</td>
      <td>44.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>Öğleden Sonra 15:00-16:59</th>
      <td>42.19</td>
      <td>81.0</td>
      <td>2.0</td>
      <td>42.0</td>
      <td>9.58</td>
      <td>36.0</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>Öğlen 13:00-14:59</th>
      <td>34.70</td>
      <td>65.0</td>
      <td>2.0</td>
      <td>35.0</td>
      <td>9.34</td>
      <td>29.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>Sabah Yoğunluğu 07:00-09:59</th>
      <td>31.93</td>
      <td>72.0</td>
      <td>1.0</td>
      <td>36.0</td>
      <td>17.23</td>
      <td>17.0</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>Öğleden Önce 10:00-12:59</th>
      <td>31.62</td>
      <td>62.0</td>
      <td>1.0</td>
      <td>35.0</td>
      <td>10.99</td>
      <td>27.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>Gece 20:00-22:59</th>
      <td>23.58</td>
      <td>63.0</td>
      <td>1.0</td>
      <td>22.0</td>
      <td>10.54</td>
      <td>17.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>Gece Yarısı 23:00-01:59</th>
      <td>10.65</td>
      <td>47.0</td>
      <td>1.0</td>
      <td>9.0</td>
      <td>7.92</td>
      <td>4.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>Sabah 05:00-06:59</th>
      <td>7.75</td>
      <td>36.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>6.14</td>
      <td>2.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>Sabaha Karşı 02:00-04:59</th>
      <td>4.71</td>
      <td>42.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>3.92</td>
      <td>2.0</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>

# Vakitler Birbirini Ne Kadar Etkiliyor?
Vakitlerin birbirini etkilemesini bekliyor muyuz? Yani sabah çok tafik varsa sonraki saatlerde de trafiğin yoğun olmasını ya da tam tesini bekliyor muyuz? Aşağıda aynı gün içindeki vakitlerin birbiriyle korelasyonu var. Bire yakın sayılar iki vaktin tafiklerinin birbiriyle pozitif olarak çok ilişkili olduğunu (yani biri yüksekken öbürünün de yüksek olduğunu), eksi bire yakın sayılarsa negatif olarak çok ilişkili olduğunu (yani biri yüksekken diğerinin düşük olduğunu) gösteriyor. Korelasyon sıfıra yaklaştıkça iki vaktin yoğunluğunun birbileriyle bir ilişkisi olmadığı anlamına geliyor (korelasyonları daha detaylı görmek isterseniz [şuradan bakabilirsiniz](https://github.com/skurmus/2019istanbultrafikverisi/blob/master/Figures/Gu%CC%88nu%CC%88n%20Vakitleri%20Trafik%20Yog%CC%86unluk%20Korelasyonu%20(Scatter%20ve%20Fit).png)).

![](/Figures/Gu%CC%88nu%CC%88n%20Vakitleri%20Trafik%20Yog%CC%86unluk%20Korelasyonu%20(2).png)

Korelasyonların ısı haritasına baktığımda gözüme şunlar çarpıyor:

- Gece Yarısı, Sabaha Karşı ve Sabah yoğunlukları günün geri kalanıyle pek ilişkili değil.
- Sabah Yoğunluğu en çok ve ciddi anlamda kendinden sonra gelen Öğleden Önce trafiğini etkiliyor. Akşam Yoğunluğunu da aynı oranda olmasa bile etkiliyor. Yani sabah trafik kötüyse genelde akşam da kötü oluyor.
- Öğleden Önce, Akşam Yğunluğunu anlamak için Sabah Yoğunluğundan daha iyi bir gösterge.
- Öğlen trafiğine bakarak sadece Öğleden Sonra trafiğiyle ilgili bir şeyler söylemek mümkün. Başka vakitlere pek ilgisi yok.
- Öğleden Sonra trafiği de Akşam Yoğunluğunu tahmin etmek için fena değil. Ama Sabah ya da Öğleden Önce trafiği kadar etkili değil.
- Akşam Yoğunluğu ve Gece trafiklerine bakarak kendilerinden sonra gelen vakitlerle ilgili pek bir şey söylemek mümkün değil.

# "Dün Trafik Çok Kötüydü Bugün Millet Çıkmamış" Diye Bir Şey Gerçekten Var mı?

Bu İstanbul'da çok sık yapılan bir geyik: dün trafik çok kötüydü, bugün kimse çıkmamış; ya da tam tersi, dün trafik iyi olunca bugün herkes çıkmış. Acaba doğru mu? Aşağıda iki ardışık günün Sabah Yoğunluğu ve Akşam Yoğunluğu değerlerinin birbirleriyle ve ilgili günlerin en yüksek ve ortalama değerleriyle ilişkisi var.

![](/Figures/Ard%C4%B1s%CC%A7%C4%B1k%20Gu%CC%88nlerinin%20Vakitlerinin%20Trafik%20Yog%CC%86unluk%20Korelasyonu.png)

Günlerin kendi içinde yukarıda bahsettiğim ilişkiler tabii burada da görünüyor ama düne bakıp bugünle ilgili pek bir şey söylemek mümkün değil pek. Ama burada atladığımız bir şey var: haftanın bazı günleri kendinden önce ya da sonra gelen günlerden çok farklı. Yani pazara bakıp pazartesiyi anlamaya çalışmak, ya da cumaya bakıp cumartesiyi tahmin etmek çok anlamlı değil. Onun için yukarıdaki korelasyon ısı haritasına bir de sadece salı, çarşamba, perşembe ve cuma için bakalım.

![](/Figures/Ard%C4%B1s%CC%A7%C4%B1k%20Gu%CC%88nlerin%20(Sal%C4%B1%20-%20Cuma)%20Vakitlerinin%20Trafik%20Yog%CC%86unluk%20Korelasyonu.png)

Tüm günleri aldığımız korelasyonlara göre daha yüksek bu günlerdeki korelasyonlar ama halen çok da etkili değil. Üstelik negatif ilişkiler beklerken halen pozitif ilişkiler görüyoruz. Yani veri bu şekliyle İstanbulluların inançlarını pek desteklemiyor.

Bu kadar İstanbul'lu yanlış değildir diyerek biraz daha kurcalayalım. Ardışık günler tabii ki birbiriyle ilgili. Sonuç olarak aynı aylar, mevsimler, okul zamanları, bayramlar içindeler. Dolayısıyla birisinin bir vakti yüksekken öbürünün de vakitleri yüksekolabilir. Belki vakitler arasındaki farklara bakmak hipotezimizi destekler. Yani vakitlerin trafik yoğunluğu ile vakitler arasındaki farklara bakalım. Beklediğimiz şey, örneğin dün sabah trafik yoğunluğu yüksekse, bu sabahın trafik yoğunluğunun dün sabahın yoğunluğundan daha düşük olması. Yani günlük mutlak değerlerle vakitlerin farklılıkların arasındaki korelasyonların negatif olması. 

![](/Figures/Ard%C4%B1s%CC%A7%C4%B1k%20Gu%CC%88nlerin%20(Sal%C4%B1%20-%20Cuma)%20Vakitlerinin%20%20ve%20Farklar%C4%B1n%C4%B1n%20Korelasyonu.png)

Veeee, çok güçlü olmasa da, beklediğimiz ilişkiyi görüyoruz (dün trafik yüksekse, bugün aynı vakitte trafiğin daha düşük olacağını bekliyorduk). Ama bu ilişki çok da güçlü değil. Biraz daha detaylı bakınca bakalım bir şey çıkacak mı? Önce sabahlara daha detaylı bakalım.

![](/Figures/Du%CC%88n%20Sabahla%20Bu%20Sabah%20Fark%C4%B1%20-%20Du%CC%88n%20Sabah%20Yog%CC%86unluk%20I%CC%87lis%CC%A7kisi.png)

Y aksı dünle olan farkı gösteriyor, X aksı ise dünkü trafiği. Farkın çok büyük olduğu tarihler 1 Mayıs, 2 Mayıs, 30 Ekim, 24 Nisan. Bunlar öncesinde ya da sonrasında resmi tatil olan günler (halen Salı-Cuma versiine bakıyoruz bu arada). Y aksındaki bu ekstrem veriler haricinde, X aksında da düşük trafikli ve düşük farklı bir grubu var. Kalanlar için korelasyon matrisinde gördüğümüz negatif ilişkiyi gözle bile görmek mümkün. Daha iyi görmek için basit bir regresyon bile yapabiliriz (neden yoralım gözümüzü:).

![](/Figures/Du%CC%88n%20Sabahla%20Bu%20Sabah%20Fark%C4%B1%20-%20Du%CC%88n%20Sabah%20Yog%CC%86unluk%20I%CC%87lis%CC%A7kisi%20(Lineer%20Regresyon).png)

Lineer regresyon da bu ilişkinin çok güçlü olmadığını gösteriyor. Ama çok güçlü olmaması için akla yatkın bir açıklama var. Trafiğin genel seviyesi düşükken "dün biraz trafik vardı, yarın arabayı çıkarmayayım" diyen çok da yoktur. Sürücüler bu kararı muhtemelen belirli bir trafik seviyesinin üzerinde bir trafikle karşılaştıklarında veriyorlardır. O zaman bir başka metodla tekrar aradaki ilişkiyi göreselleştirmeyi denyelim. Bu metodun adı Loess ya da Lowess: [Locally Weighted Linear Regression](https://xavierbourretsicotte.github.io/loess.html). Kabaca birbirine yakın noktalar için bir fonksiyon tahmin edip, sonra bu tahminleri birleştirerek çalışıyor. Loess sonucu şöyle:

![](/Figures/Du%CC%88n%20Sabahla%20Bu%20Sabah%20Fark%C4%B1%20-%20Du%CC%88n%20Sabah%20Yog%CC%86unluk%20I%CC%87lis%CC%A7kisi%20(Lowess).png)

Bu biraz daha beklediğimiz gibi. Dünkü sabah trafiği 40'lara gelene kadar bugünkü trafiği etkilemiyor ama sonrasında etkiliyor. Hatta şu tatil öncesi sonrası ekstrem değerleri çıkartırsak bunu daha da açık görebiliriz:

![](/Figures/Du%CC%88n%20Sabahla%20Bu%20Sabah%20Fark%C4%B1%20-%20Du%CC%88n%20Sabah%20Yog%CC%86unluk%20I%CC%87lis%CC%A7kisi%20(Lowess%20-%20Limitli).png)

Ve görüyoruz:) Tabii bunu görmek için ille de Lowess gibi yeni moda şeyler şart değil. Üçüncü dereceden bir polinominal regresyonla da benzer bir ilişkiyi görmek mümkün (bu arada kullandığım [grafik paketi](https://seaborn.pydata.org/)  regresyonlardaki denklemleri göstermiyor, başka bir istatistik paketiyle hesaplayıp eklemeye de üşendim, merak ediyorsanız kendiniz hesaplayabilirsiniz).

![](/Figures/Du%CC%88n%20Sabahla%20Bu%20Sabah%20Fark%C4%B1%20-%20Du%CC%88n%20Sabah%20Yog%CC%86unluk%20I%CC%87lis%CC%A7kisi%20(Poly%20Reg%20-%20Limitli).png)

Burada da ekstrem değerler dışarıda ve sadece Salı-Cuma versine bakıyoruz.

Akşam trafiği için de bu ölçüde olmasa da aynı şeyleri söylemek mümkün. Bu grafiklerin akşam için olanlarını bu git içindeki [Figures klasöründe](https://github.com/skurmus/2019istanbultrafikverisi/tree/master/Figures) ya da daha düşük çözünürlüklü olanları [Analiz Jupyter Notebook'u](https://github.com/skurmus/2019istanbultrafikverisi/blob/master/Istanbul-Trafik-Veri-Analiz.ipynb) içinde görebilirsiniz.

# Başka Ne Yapılabilir
Trafik İndeksi verisinin bu haliyle yapılacak başka işler de var. Zamanınız olup da bunları ya da başklarını yaparsanız lütfen bu repoya ekleyin. benim ilk aklıma gelenler:

- Hava durumu verisi ile birlikte bakıp hem hava durumunun etkisine bakmak, hem de bu etkiden arındırılmış ilişkilere bakmak ilginç olabilir. Bu arada etrafta doğru dürüst günlük ya da saatlik İstanbul hava durumu verisi ben bulamadım. Bulursanız eminim başka ihtiyacı olanlar da vardır.
- İyi bir istatiskçi ya da veri bilimcisi bu veriden yola çıkarak sabah ya da akşam trafiğini tahmin edebilecek modeller geliştirebilir. Benim burada yaptıklarım basit ilişkileri gösteriyor.
- Tüm İstanbul için tek indeks aslında veriyi çok basitleştiriyor. Bölgesel bazda indekslerle daha ilginç şeyler yapılabilir. Ben İBB Açık Veri Portalindeki [veri taleplerinden birine](https://data.ibb.gov.tr/datarequest/1f79628a-a633-4649-95eb-1fd2caef6bcc) yorum olarak ekledim. Siz de isterseniz belki olur.



