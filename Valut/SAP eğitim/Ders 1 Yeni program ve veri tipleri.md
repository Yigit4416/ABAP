### Program oluşturma:
- Sol üstteki input bölümüne kodu yazman gerekir.
- Bu kod SE38
### Yeni program adı:
- Baş harfi z ya da y ile başlamalıdır.
- SAP normal kendi uygulamalarıyla sonradan yapılmış uygulamaların farkını anlamak için böyle bir formalite koymuş.
### SAP data tipleri:
- DEC = decimal
- INT 1/2/4 = byte uzunluğuna göre isimlendiriliyor
- NUMC = Numeric text
- CHAR = Karakter
- STRING = String
### Bunlar nasıl tanımlanır:

```ABAP
Başlangıçta gv olmasının nedeni sonra anlatılacak

DATA gv_degisken1 TYPE p DECIMALS 2.
* 2. kullanılmasının sebebi virgülden sonra 2 değer tutulmak istenilmesinden dolayı
* gv = Global Variable
DATA gv_degisken2 TYPE int4.
DATA gv_degisken3 TYPE NUMC 10. 
* Bu sondaki 10 sayısı bu değişkenin uzunluğu

DATA gv_degisken4 TYPE c. 
* Bu char
DATA gv_degisken5 TYPE string.
```

Değer ataması da aynı diğer dillerde olduğu gibidir:

```ABAP
gv_degisken1 = '34.54'.
gv_degisken1 = '41.5265'. 
* Bunu yuvarlar

gv_degisken2 = 1234.

gv_degisken3 = 1234567890.

gv_degisken4 = 'A' 
* Buna bir length verilmediği için tek karakter girişi yapılır.

gv_degisken5 = 'Bu bir string'.
```

