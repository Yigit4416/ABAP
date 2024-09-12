# screen grouptan da bahset twitter'de

### Gün 1
- Şirket oryantasyon programı yapıldı.
- Şirket hakkında ve kendi haklarımız konusunda haklardan bahsedildi.

### Gün 2 - 3
- ABAP dilindeki veri objeleri nelerdir ve nasıl kullanıldığı konusunda bilgi verildi.
- SELECTION-SCREEN'in ne olduğu ve bunu nasıl kullanılacağı öğretildi.
- Burada görülen veri objeleri kullanılarak basit bir hesap makinesi uygulaması yapılmıştır.

### Gün 4 - 5 - 6
- ABAP Dictionary'nin ne olduğu ve nasıl kullanılacağı öğretildi.
- Internal table, structure, table type, database table, data element, domain, search help kavramları anlatıldı.
- Burada anlatılan kavramlar kullanılarak personel bilgi sistemi yapıldı.
- Personel bilgi sisteminin master tablosunda 2 ekranlı bakım ekranı kullanılarak personelin id, adı, soyadı, önceden belirlenen değerlerli kullanarak search help ile cinsiyeti, doğum yeri / tarihi, medeni hali, çocuk sayısı ve uyruğu alındı.
- Sonra personelin iletişim tablosunu tek bakım ekranı kullanarak master tabledeki id'yi search help kullanarak başka id girilmesi önlendi ve iletişim türü ve tanımlayıcısı kullanıcıdan alındı.
- Sonra Aile bilgileri bakım tablosunda 2 ekran kullanıldı aynı iletişim bakım tablosunda kullanıldığı gibi id alanında search help kullanıldı ve kullanıcıdan aile tanıtıcı no, personel adı, soyadı, telefon, sokak, il, ülke bilgileri alındı ve aile tanıtıcı no için önceden belirlenen veriler kullanılarak search help kullanılarak veriler alınmıştır.
- 2 ekranlı bakım tablosu kullanarak eğitim tablosu için bilgileri alırken aynı iletişim bakım ekranında kullanıldığı gibi id alanında search help kullanıldı ve kullanıcıdan eğitim kodu, okul adı, il ve ülke bilgileri alındı. Ülkeyi alırken  T005 isimli ülke tablosundan veriler çekilerek, eğitim kodunda ise önceden belirlenen eğitim kodları kullanılarak search help kullanıldı.
- Tüm bilgiler girildikten sonra SELECTION-SCREEN'de 4 tane radyo buton oluşturuldu ve buradaki seçenekler Master Bilgisi, İletişim bilgisi, aile bilgisi ve eğitim bilgisidir. Seçilen radyo butona göre gerekli tablolar kullanıcıya sunulmuştur.

### Gün 7
- OPEN SQL üzerine çalışmalar yapıldı.
- Database table'de OPEN SQL komutları nasıl kullanılması gerektiği öğrenildi.
- Basit post atma uygulaması yapıldı.
- Kullanıcıdan alınan kullanıcı ID'si ve post ID'si ile post atma, silme, değiştirme ve görüntüleme yapıldı.
- SELECTION-SCREEN'de 4 radyo buton kullanıldı, bu radyo butonları ile istenilen işlemler yapıldı.
- screen-group1'de kullanıcıdan istediğimiz parametrelere MODIF ID vererek screen-group1'de değişiklikler yaparak istenilen radiobutton'da istenilen parametreler gösterilir.

### Gün 8
- Internal tablelerde CURD işlemlerinin nasıl yapıldığı ve ne işe yaradığı anlatıldı.
- Bazı durumlarda database table yerine internal table üzerinden işlemler yapılır bu durumda normal SQL komutları olan SELECT gibi komutlar bu internal tableler üzerinde uygulanamaz.
- Bu durumda READ TABLE, LOOP AT gibi fonksiyonlarla intenal tableler üzerinde işlemler yaparız.

### Gün 9 - 10
- Daha önceden anlatılan konuların pratiği için kütüphane sistemi geliştirildi.
- Bu sistemde Kitap ekleme, güncelleme silme, listeleme, kiralama ve geri alma seçenekleri vardır
- Burada SELECTION-SCREEN'i dinamik biçimde kullanılarak daha kullanıcı dostu bir arayüz oluşturuldu.
- SELECTION SCREEN'nin dinamik özelliklerini:
	- Kitap ekleme radyo butonuna basıldığında kitabın ISBN no, kitabın adı, yazarın adı, yayım yılı ve kategori bölümleri açık kalır.
	- Güncelleme bölümünde ilk basşta ISBN bölümü görünür burad veri tabanını kullanılarak search help oluşturuldu, ISBN'de search help almak istenildiğinde veri tabanınındaki tüm kitapların tüm özellikleri görünüyor ve kullanıcı ona göre ISBN nosunu seçer bunun sonucunda bütün bilgiler SELECTION-SCREEN'de editable olarak görünür ve istenilen bölümler değiştirilir.
	- Kitap silme bölümünde sadece ISBN bölümü görünür aynı güncelleme bölümünde olduğu gibi search help kullanılarak tüm bilgilerin görünmesi sağlandı ve kullanıcının istediği kitabın silinmesi sağlandı.
	- Kitapları listeleme bölümünde kullanıcıya tüm seçenekler açılır ve SELECTION-SCREEN'de yazılan alanlara göre kitaplar filtrelenerek listelenmesi sağlandı.
	- Kitap kiralama bölümünde kiralanacak kitabın ISBN no'su girilir ve ana databaseden kitap kaldırılır ve kiralama databasesine aktarılır ve o anki tarihin üzerine 10 gün ekler.
	- Kitap geri alma bölümünde kitap kiralama databasesinden veriyi kaldırır ve ana databaseye geri döndürür.

### Gün 11 - 12 - 13
- ALV'nin ne olduğu ve ne işe yaradığı üzerine duruludu.
- Şirketlerin istediği tabloların istediği şekilde modifiye edilmesine olanak sağlayan bir teknolojidir.
- En basitinden belli bir alan belli bir boyuttan büyükse o alanın bulunduğu hücre yeşile boyanır.

### Gün 14 - 15
- OO ALV kullanılarak SAT-SAS raporu yapıldı.
- Satın alma talebi ve siparişi arasındaki ilişkiyi ve uyumu değerlendiren bir rapordur.
- SELECTION-SCREEN'de 2 tane radio butonumuz var bunlar:
	- SAT
	- SAS
- SAT radyo butonuna basıldığında ekranın altında satın alma talep numarası ve satın alma belgesi türü girişleri çıkar.
- SAS radyo butonuna basıldığında satın alma belge numarası ve mal grubu girişi çıkar.
- SAT raporunu oluştururken SAP hazır tablolarından olan eban ve ekpo tablolarını inner join yaparak kullanırız.
- Tabloyu bir takım fonksiyonlarla ALV ekranına koymadan önce gs_layout dediğim internal tableye tablonun genel özelliklerini atarız burada değeri belli aralığın dışında olan taleplerin hücrelerinin renklerini yeşil yaparız 'LVC_FIELDCATALOG_MERGE' fonksiyonu ile her kolona has özellikleri ve göstermek istediğimiz kolonları atarız.

### Gün 16 - 17
- Programın performansını arttırma konusunda eğitim verildi.
- SAP sistemleri tablolar üzerine kurulu bir sistem olduğu için hangi noktada internal tableye başvurulması gerektiği, hangi noktada database tableye başvurulması gerektiği konusunda çalışmalar yapıldı.
- Çalışılan müşterilerin genelde çok yüklü miktarda verileri olduğu için bu konu önem teşkil etmektedir.
- Tablolar üzerine genel işlemler yapılırken database tableyi internal tableye aktarılır bunun sebebi ise database table bilgisayarın sabit diskinde yer alırken internal table cache bölümünde yer alır ve bu işlemlerin daha hızlı yapılmasına olanak sağlar.
- Burada örnek olarak turnuva sistemi yapıldı database'ye kaydedilen takımları 4 gruplu turnuvada rastgele seçilmiş 4 takım dağıtılacaktır. Her grupta bir torbadan bir takım olabilir, aynı grupta aynı ülkeden iki takım olamaz şartlarına göre işlemler yapılmıştır.
- Bu işlemi olabildiğince performanslı halde yapılmaya çalışılmıştır.

### Gün 18 - 19
- Adobe Forms'un nasıl kullanılacağı öğrenildi.
- Bazı müşteriler tırlarla ürün taşırken tır şoförlerinin yanlarında ne taşıdıklarına dair belge bulunması lazımdır ve müşteriler zamanla yarıştıkları için bu belgenin otomatik hale getirilmesi lazımdır.
- Örnek olarak müşteri seçtiği tabloyu belgeye aktartabilir bu şekilde şoföre hızlıca belgenin verilmesi sağlanır.
- Bunun çaışılması için airport belge sistemi yapıldı:
	- Burada programın başlangıcında SELECTION-SCREEN'den yetkiliden bir barkod kodu alınır ve SAP'nin hazır tablolarından olan SCARR tablosundan veriler çekilerek tabloya aktarılması sağlanır ve girilen barkod numarasına göre sayfanın sol altında gerekli barkodun görülmesi sağlanır.

### Gün 20 - 21
- Havayolu Bilgi Sistemi 
- Burada ekranın en üstünde havayolu şirketi ID'si için bir input field oluşturldu ve burada girilen değre göre sol taraftaki ALV gridine veriler aktarıldı.
- Eğer \*   girilirse SCARR tablosundaki bütün havayolu şirketleri getirilir öbür türlü ID'si girilen havayolu şirketi sol taraftaki ALV gridinde gösterilir.
- Sol taraftaki ALV gridinde her satırın başında DETAY butonu konuldu bu butona basıldığında sağ taraftaki ALV'ye SFLIGHT tablosundan bu havayolu şirketinin uçakları hakkında bilgiler getiriliyor.

### Gün YAZILMAYACAK
- Hanoi kulesi için algoritma yapıldı ve test edildi.

### Gün 22 - 23
- Depo sistemi
- Kullanıcıdan malzeme numarası, depo yeri ve stok miktarı alınır.
- Sistem kullanıcının girdiği değerleri kontrol eder ve eğer girdiği malzeme hazır SAP tablosu olan makt tablosunda bulunmazsa eklemesine müsade etmez ve sol alt ekranda uyarı mesajı çıkarır.
- Eğer girdiği malzeme tabloda bulunuyorsa ve girdiği değer girdiği depoda bulunmuyorsa o depo yeni ürün olarak ekler ve ALV ekranı yenileyerek o ürünü yeşil hücre ile gösterir.
- Eğer kullanıcı ürün çıkarmak isterse sistem kullanıcın girdiği değerleri kontrol eder eğer böyle bir ürün yoksa pop-up mesajla bu ürünün olmadığını yazar, eğer yeterli ürün yoksa yine aynı şekilde yeterli mesaj olmadığını gösterir, eğer ürün 0'lanırsa sistemden ürün silinir, sıfırlanmazsa ürün kırmızı hücre ile gösterilir ve yine alv ekranı yenilenir.
- Kullanıcı bir ürünün ID'sini girdikten sonra ENTER tuşuna basınca hazır SAP tablosu olan makt tablosunu kontrol eder eğer böyle bir ürün yoksa pop-up mesajla böyle bir ürünün olmadığı belirtilir, ürün varsa da sol taraftaki non-editable text fielda ürünün açıklama bölümü printlenir.

### Gün 24 - 25 - 26 - 27 - 28
- Dinamik ALV
- Bazen müşteriler tabloya istedikleri spesifik şeylerin getirilmesini istiyor bu noktada Dinamik ALV kullanıyoruz.
- Dinamik ALV kullanabilmek için dinamik internal tablel ve dinamik select yapıları kullanılması lazım.
- İlk olarak tabloda kullanılmak istenilen tablo ve alanların isimlerini tablodan çeker ve bir intenal tableye koyarız.
- Sonrasında internal tablede tüm bunları LOOP AT ile tararız ve ona göre tablo adları ve alan adlarını dinamic select sorgusu için CONCONATE ederiz.
- Sonrasında normal alv oluşturmada kullanılan fonksiyonları kullanırız ancak bu kez FILLFIELDCONATINER_MERGE fonksiyonunu kullanamayız çünkü bu fonksiyon static structerler içindir ancak bizim kullandığımız dinamik.
- Bu noktada normal şartlar altında manuel girmemiz gerekir ancak elimizde alan adları ve tablo adları bulunduğu için LOOP AT komutunu kullanarak daha önce oluşturduğumuz internal tableden veri çeker ve tabloya istediğimiz alanları koyarız.
- go_grid->set_table_for_first_display fonksiyonunda hem static hem de dinamic tabloları kabul edebildiği için elemanların yer aldığı tablomuzu buraya yerleştiririz.