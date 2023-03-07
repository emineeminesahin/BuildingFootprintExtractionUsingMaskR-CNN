Deep learning model - https://www.arcgis.com/home/item.html?id=a6857359a1cd44839781a4f113cd5934

ArcGIS Living Atlas of the World'den alınan ABD bina çıkarımı derin öğrenme modelinin yüksek çözünürlüklü uydu görüntüsünde test edilmiştir. Manuel olarak yapılırsa, bina çıkarımı işlemi karmaşık ve zaman alan bir iştir. Derin öğrenme, bu görevi önemli ölçüde optimize etmek ve otomatikleştirmek için kullanılabilir. Bu bina çıkarımı derin öğrenme modeli, yüksek çözünürlüklü uydu görüntülerinden bina çıkarımı için önceden eğitilmiş, kullanıma hazır bir derin öğrenme modelidir. Bu model olduğu gibi kullanılabilir veya kendi verilerimize / coğrafyamıza uyum sağlamak için ince ayar yapılabilir. 
Uygulamada yazılım olarak ArcGIS Pro kullanılmaktadır. ABD Bina çıkarımı derin öğrenme modelinde girdi olarak raster veri kümeleri, tarama ürünleri, mozaik veri kümeleri kullanılabilir. Kullanılan görüntüler optimum sonuç için 30-50 cm yüksek çözünürlüklü uydu görüntüsü, 8 bit, multispektral görüntülerden elde edilmiş ortofotolar olmalıdır. Nadir ile açısı yüksek olan görüntüler uygun sonuç vermeyecektir. Bu uygulamada çalışma alanı Amerika Birleşik Devletleri'nin Kaliforniya eyaletinde yer alan ve Riverside ilçesinde yer alan bir Lake Elsinore seçilmiştir.

![image](https://user-images.githubusercontent.com/114474881/223489936-00c27b17-a7a8-418f-84fb-021b835162d4.png)

 Preprocessing İşlemi
 
 ![image](https://user-images.githubusercontent.com/114474881/223490137-347d1dd0-65b4-40c9-ae3a-e00bd49d69aa.png)

ArcGIS Pro > Contents bölmesinde raster görüntüye ulaşılır, ‘‘.ımd ’’ dosyası açılır ve “Pansharpen” katmanı eklenir.

![image](https://user-images.githubusercontent.com/114474881/223490336-d65f81ce-d7f0-427f-9e31-e61a90739014.png)

“Pansharpening” katmanına sağ tıklanıp “Edit Function Chain” seçilir burada “Function Chain” penceresi açılır, “Strech Button” düğmesine tıklanır ve özellikleri 8 bite dönüştürmek için düzenleme yapılır. “Strech Properties” aşağıdaki gibi değiştirilir ve değişiklikler uygulanır.

![image](https://user-images.githubusercontent.com/114474881/223490711-0f4c4e81-2555-4cb1-bacb-9f65ae9d4677.png)

Bina Çıkarımı

GitHub’ dan alınan Esri derin öğrenme ArcGIS Pro’ ya kurulduktan sonra model açılır.

![image](https://user-images.githubusercontent.com/114474881/223490979-0b4b3f61-9791-4269-ada7-0dccba478c42.png)

![image](https://user-images.githubusercontent.com/114474881/223491041-931eb0e7-ec1e-4008-9fe1-5d0cb9d6b5c8.png)

Analysis > Tools > Geoprocessing > Image Analyst Tools > Detect Objects Using Deep Learning adımlarından sonra girdi 
olarak raster veriden “.tif” dosya çıkartılır.

![image](https://user-images.githubusercontent.com/114474881/223491317-a8a57c94-93c6-4e95-966d-6bae8b250cb7.png)

![image](https://user-images.githubusercontent.com/114474881/223491365-3d3ab189-3581-4972-8188-38b69783158e.png)

Model Definition’ a ArcGIS Living Atlas of the World > Building Footprint Extraction – USA ’den indirilen “usa_building_footprints.dlpk” dosyası açılır. Model için parametreler kullanılacak olan sınırlı GPU belleğine sahip dizüstü bilgisayara göre Şekil 19’daki gibi ayarlanır. Örtüşen algılamalardan kurtulmak için de Environments > Processing Extent > As Spesified Below seçilir. Hızlı çalıştırmak istenildiği için piksel boyutu 0.3 m olarak belirlenir. Bu model CPU’ da da çalışabilir fakat GPU daha hızlı olacağı için GPU seçilir ve çalıştırılır.

![image](https://user-images.githubusercontent.com/114474881/223491527-c2710d95-cd72-4e5b-992f-10da51935a9b.png)

Model çalıştığında bina sınırlarının çıkarımı yapılmış olur. “Map” katmanına “Buildings” katmanı eklenir.

![image](https://user-images.githubusercontent.com/114474881/223491682-fd0326f1-6da4-4423-8b63-23438e6fea5a.png)

![image](https://user-images.githubusercontent.com/114474881/223491750-27c4626e-1600-44b8-a630-6ac38cb068f1.png)

Şekil 22’de görüldüğü gibi bina sınırları köşeli değil yuvarlatılmış şekildedir. Bu sınırların düzeltilmesi gerekir.

![image](https://user-images.githubusercontent.com/114474881/223491937-228824c5-4d55-41ec-b717-840a9f68b19b.png)

Geoprocessing > Regularize Building Footprint’ de düzenlenecek katman “Buildings” ve metot “Right Angles” seçilir, tolerans ve hassasiyet de ayarlandıktan sonra çıktı olarak “Buildings_RegularizeBuilding” Feature Class’ ı “Map” katmanının altında oluşacaktır.

![image](https://user-images.githubusercontent.com/114474881/223492692-fc8a5949-c8a6-4bf3-87b1-78a6c2f33a81.png)

![image](https://user-images.githubusercontent.com/114474881/223492843-625f4e6b-340c-49f0-8318-5654aa13a8cb.png)

![image](https://user-images.githubusercontent.com/114474881/223492890-f2b5928c-238b-48e6-bf27-41bf6eb4aa05.png)

Oluşturulan bina sınırlarının gösterimini son oluşturulan düzenlenmiş bina sınırlarının bulunduğu “Buildings_RegularizeBuilding” katmanında “Sembology” den değiştirilebilir. Burada dolgu rengi kaldırılmış ve bina sınırları kırmızı ile gösterilmiştir.

![image](https://user-images.githubusercontent.com/114474881/223493110-31623faf-07be-4a9e-91a1-3e228014e615.png)

Sonuç ürün Şekil 27’de gösterilmiştir.

![image](https://user-images.githubusercontent.com/114474881/223493300-2f86debd-25a7-4aeb-9116-9966ec58e4ac.png)

Model genelde sorunsuz çalışmıştır. (Şekil 28-(a), (b), (c))

![image](https://user-images.githubusercontent.com/114474881/223493415-c2bbf936-e41f-4b68-aa8f-25d29e41841b.png)

![image](https://user-images.githubusercontent.com/114474881/223493464-0661ab54-ecc8-4f94-8a7d-b79f392a669e.png)

![image](https://user-images.githubusercontent.com/114474881/223493644-5004d434-b87d-4357-a83a-d27704f0b8a7.png)

Bazı bölgelerde ise bina olmayan yerleri de bina olarak algılamıştır. (Şekil 29- (a),(b))

![image](https://user-images.githubusercontent.com/114474881/223493794-0d37e671-7799-4f6f-acd0-3edf56bfc593.png)

![image](https://user-images.githubusercontent.com/114474881/223493853-4f4a855e-56d8-45ff-abb0-d48a26cb94c6.png)

SONUÇLAR VE ÖNERİLER 
Günümüzde uzaktan algılama popüler bir alandır. Bu nedenle, güvenlik, savunma, ziraat, afet tespiti, şehir gelişimi, elektrik ve gaz dağıtımı gibi amaçlarla yaygın olarak kullanılmaktadır. Derin öğrenme için kullanılan algoritmalarda, çok güçlü bir bilgisayar donanımına ihtiyaç duyduğu gözlemlenmiştir. Derin öğrenme geleneksel makine öğrenmesi algoritmalarına göre en büyük avantajı olan otomatik özellik/öznitelik çıkarımı test edilmiştir. Bu yöntemin daha çok tercih edilme nedeni insandan bağımsız ve hızlı bir şekilde verilen görevi yerine getirmesi olarak sunulabilir. Çünkü bunun temelinde insanı taklit eden bir algoritma mevcuttur. Derin öğrenme yöntemlerinden Mask R CNN modelinde diğer diğer modeller gibi kargaşa giderme, öznitelik çıkarma, boyut azalma gibi algoritmalara ihtiyaç duyulmaması sebebiyle işlem yükü azalmakta ancak doğrudan görüntü üzerinden model eğitimleri gerçekleştiğinden eğitim süreleri uzun olmaktadır. Obje tespiti için Faster R-CNN ile karşılaştırıldığında bu yöntem hem ürettiği nesnelik puanı hem de hesaplama süresi açısından daha başarılı sonuçlar verir. Mask R-CNN’de, görüntülerdeki sınırlayıcı kutulara ilave olarak gerçek görünümlerine daha yakın maskelerle işaretlenmektedir. Bu modelde bulunan ve diğer bölgeye dayalı yöntemlerde bulunmayan, nesne konumunun daha doğru şekilde belirlenmesini sağlayan RoI katmanı yardımıyla daha yüksek doğruluk sağlamaktadır. Bu çalışmada olduğu gibi seçilen veri kümesinin dağınık dağılımlı olmamasıyla; derin öğrenme algoritmaları, makine öğrenmesinden daha iyi sonuçlar vermektedir. İş gücünü büyük ölçüde azaltmasına rağmen mükemmel doğrulukta olmadığını göstermektedir. Dağınık bir dağılımda zorlaştırıcı ve uğraştırıcı olabilir. Bu doğrultuda her şeye rağmen manuel bina çıkarımına gerek kalmaması açısından önerilen yöntem daha da geliştirilebilir. Ayrıca uydu görüntülerinden yollar, su ve diğer özelliklerin çıkarılması için de kullanılabilir hale getirilebilir.
