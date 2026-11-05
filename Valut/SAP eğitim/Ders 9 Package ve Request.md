## Package
- İlişkili geliştirme nesnelerini (program, class, tablo, function group, data element vs.) **bir arada tutan bir kapsayıcıdır** eskiden buna "Development Class" denilirdi.
- Her repository nesnesi (Z'li program, class, tablo...) mutlaka bir package'ye ait olmak zorundadır. Nesneyi oluştururken karşına çıkan "Package" alanı budur.

### Neden var?
- Nesneleri düzenli gruplandırmak (örneğin `ZFLIGHT` paketı içinde uçuş raporuyla ilgili her şey)
- Hangi nesnelerin birlikte taşınacağına karar vermek.
- Yetki bağımlılık ve kapsülleme kontrolü.
### $TMP: Local Object
Nesneyi `$TMP` packagesine ya da "Local Object" butonuyla kaydedersen:
- Nesne sadece o sistemde (Örneğin DEV) kalır.
- Transport request'e girmez, başka sisteme taşınamaz.
- Deneme, öğrenme, test için idealdir.
### Package'ın transport layer'ı
Gerçek (transportable) bir package bir transport layer'a bağlıdır. Bu nesnelerin hangi sistem hattında (DEV -> QAS -> PRD) taşınacağını belirler. Pratikte bunu Basis ekibi ayarlar biz genelde hazır bir package'a nesne ekleriz.

## Request
- Request, DEV sisteminde yaptığımız değişiklikleri QAS ve PRD gibi diğer sistemlere taşımak için kullanılan pakettir. Bir nevi "değişiklik seperi" denebilir.
- Transportable bir package'a nesne kaydettiğimizde sistem bir request ister. Nesne o request'e kaydedilir. Sonra request release edilir Basis ekibi (ya da yetkimiz varsa biz) onu sıradaki sisteme import ederiz.

### Request türleri
| Tür                     | Ne için                                                                      |
| ----------------------- | ---------------------------------------------------------------------------- |
| **Workbench Request**   | Repository nesneleri: program, class, tablo, FM, data element (cross-client) |
| **Customizing Request** | Client’a bağlı ayarlar (SPRO ile yapılan konfigürasyon)                      |
### Request ve Task ilişkisi
Bir request'in altında her geliştirici için ayrı tasklar bulunur:
```
Request: DEVK900123  (ana request, sahibi: lider/sorumlu)
 ├─ Task: DEVK900124  (geliştirici A)
 └─ Task: DEVK900125  (geliştirici B)
```
- Nesneyi kaydedince biz kendi taskımıza yazılır.
-  Önce task release edilir (bizim işimiz bitti demek)
- Tğm taskler release olunca request release edilir ve transport başlar.
#### Akış (baştan sona)
1. SE80'de nesnemizi oluşturur package gireriz (`ZFLIGHT`)
2. Sistem request sorar, mevcut bir request seçeriz ya da yeni oluştururuz.
3. Geliştirmeyi bitirince SE09/SE10 ile taskımızı release ederiz.
4. Request'i release ederiz.
5. Basis request'i QAS'a, test sonrası PRD'ye import eder.