### Sonra başlıkları oluşturursun burada sadece kodları yazarsın

```ABAP
SELECT SINGLE banfn
	FROM eban
	WHERE banfn NE @space
	INTO @gv_banfn.

cl_demo_output=>write( gv_banfn ).
```
- eban tablosundaki banfn değerinden sadece 1 adet alıyor ve bu boşluk olmayan ilk satırdır, sonrasında bunu gv_banfn'ye atar.
- Bazen veriler istenilen kısma göre sıralı gelmez bu noktada komutlarla istenilen biçimde sıralanır ve istenilen veri alınır.
---
```ABAP
SELECT SINGLE bsart
	FROM eban
	WHERE banfn = '0010000023'
	INTO @data(lv_bsart).

cl_demo_output=>write( lv_bsart ).
```
- Bu da başka bir seçme şekli.
- banfn'nin istenilen şekilde olan bsart'ı seçiliyor.
---
```ABAP
SELECT SINGLE bsart
	FROM eban
	WHERE banfn = '0010000023'
	AND bnfpo = '0020'
	INTO @lv_bsart.

cl_demo_output=>write( lv_bsart ).
```
- Birden fazla seçenek de 'AND' komutuyla verebilirsin.
- 'se16' ile maintenance ekran olmadan direkt verilere keyler ile erişebilirsin.
---
```ABAP
SELECT SINGLE id
	FROM zint_yo_t_persm
	WHERE dogum_tarihi GT '20000101'
	INTO @DATA(lv_dogum_gunu).

cl_demo_output=>write( lv_dogum_gunu ).
```
- tarih formatlarında 'YYYYMMDD' şeklinde formatlanır.
- 'GT' komutu daha büyüktür anlamında kullanılır.
---
```ABAP
SELECT SINGLE banfn, bnfpo
	FROM eban
	WHERE bsart EQ 'NB'
	INTO CORRESPONDING FIELDS OF @gs_eban.

cl_demo_output=>write( gs_eban ).
```
- Burada 'INTO CORRESPONDING FIELDS OF' kısmı üstte belirttiğimiz şeyleri gs_eban structına uygun biçimde verilerin yerleştirilmesini sağlar.
- @DATA(gs_eban) tarzı bir kullanım yaptığında 'INTO CORRESPONDING FIELDS OF' yazmana gerek olmaz çünkü türü ve şekli orada zaten karar verilmiş oluyor.
---
```ABAP
SELECT *
	FROM zint_yo_t_persm
	WHERE ad LIKE 'Y%'
	INTO TABLE @DATA(lt_persm).

cl_demo_output=>write( lt_persm ).
```
- Burada yapılan şey 'ad' parametresi 'Y' ile başlayan tüm verileri 'gt_persm' tablesine aktarmaktır.
---
```ABAP
DATA: lt_range TYPE TABLE OF rsv_range.
APPEND VALUE #( sign = 'I' option = 'BT' low = '001' high = '016' ) TO lt_range.
APPEND VALUE #( sign = 'E' option = 'EQ' low = '010' ) TO lt_range.

SELECT * FROM some_table
  WHERE some_field IN lt_range.

```
- Burada 'APPEND VALUE #( sign = 'I' option = 'BT' low = '001' high = '016' ) TO lt_range.' yaptığı şey şudur:
	- sign = 'I' olduğunda dahil et olarak kullanılır 001-016 arasındaki değerleri bana getir anlamıdna kullanır
- 'APPEND VALUE #( sign = 'E' option = 'EQ' low = '010' ) TO lt_range.' yaptığı şey ise:
	- sign = 'E' kullanımının amacı 010 değeri dışındakileri getir anlamında kullanılır.
```ABAP
DATA: lt_range TYPE RANGE OF zint_yo_t_persm-id.

APPEND VALUE #( sign = 'I' option = 'BT' low = '001' high = '002') TO lt_range.

SELECT id, ad
	FROM zint_yo_t_persm
	WHERE id IN @lt_range
	INTO TABLE @DATA(lt_main).

cl_demo_output=>write( lt_main ).
```
- Bunun sayesinde belli aralıktaki verileri alabiliyoruz.
-----------
```ABAP
SELECT *
	FROM zint_yo_t_persm
	WHERE id GT 2
	INTO TABLE @lt_main
	UP TO 1 ROWS.
```
- Burada belirtilen kriterlerde max kaç tane row alınacağını gösterir.
------------
```ABAP
SELECT *
	FROM mara
	INNER JOIN makt ON makt~matnr = mara~matnr
		AND makt~spras = @sy-langu
	WHERE mara~matnr = '000000000000000074'
	INTO TABLE @DATA(lt_mara).

cl_demo_output=>write( lt_mara ).
```
- Bu işlem sayesinde 'JOIN' yaparız.
- JOIN işlemlerinde kısıtlamaları JOIN ile beraber yapılması daha doğru olur ama istenirse WHERE'den sonra da yazabilirsin.
----------
```ABAP
SELECT *
	FROM mara
	LEFT JOIN makt AS makt1 ON makt1~matnr = mara~matnr
		AND makt1~spras = @sy-langu
	LEFT JOIN makt AS makt2 ON makt2~matnr = mara~matnr
		AND makt2~spras = 'T'
	WHERE mara~matnr = '000000000000000074'
	INTO TABLE @DATA(lt_mara1).
```
- Burada aynı tabloyu 1'den çok 'JOIN' edilebildiği görünüyor.
- NOT: 2 id var ise id'lerden biri aynı olabilir hata alabilmek için ikisinin de aynı olması lazım biri aynı öbürü farklı olmaz.
-----------
```ABAP
DELETE FROM zint_yo_t_persm
	WHERE id = 004.
COMMIT WORK AND WAIT.

IF sy-subrc = 0.
	cl_demo_output=>write( 'Success' ).
ELSE.
	cl_demo_output=>write( 'Error' ).
ENDIF.
```
- Burada yapılan şey belirtilen noktadaki verilerin silinmesini sağlıyor
- NOT: COMMIT WORK AND WAIT komutu yapılan değişikliklerin database'de kaydedilmesini sağlıyor.
--------------
```ABAP
UPDATE zint_yo_t_persm SET ad = 'Tarık'
	WHERE id = 002.
COMMIT WORK AND WAIT.

IF sy-subrc = 0.
	cl_demo_output=>write( 'Success' ).
ELSE.
	cl_demo_output=>write( 'Error' ).
ENDIF.
```
- Belirtilen noktadaki değerin değiştirilmesini sağlar.
---
- NOT: Her şeyi include etmek için * kullanıyorsun ama çok önerilen bir şey değil (nedeninini bilmiyorum) her şeyi teker teker yaz. 
---
```ABAP
SELECT *
FROM zint_yo_t_persm
WHERE id = 003
INTO TABLE @DATA(lt_main_in_db).
  
DATA lt_main_modify TYPE TABLE OF zint_yo_t_persm.

LOOP AT lt_main_in_db INTO DATA(ls_main_in_db).
APPEND VALUE #( id = ls_main_in_db-id
	ad           = 'Hasan'
	soyad        = 'Tek'
	cinsiyet     = ls_main_in_db-cinsiyet
	dogum_yeri   = ls_main_in_db-dogum_yeri
	dogum_tarihi = ls_main_in_db-dogum_tarihi
	medeni_hal   = ls_main_in_db-medeni_hal
	cocuk_sayisi = ls_main_in_db-cocuk_sayisi
	uyrug        = ls_main_in_db-uyrug ) TO lt_main_modify.
ENDLOOP.
MODIFY zint_yo_t_persm FROM TABLE lt_main_modify.
IF sy-subrc = 0.
	COMMIT WORK AND WAIT.
	cl_demo_output=>write( 'Success' ).
ELSE.
	cl_demo_output=>write( 'Error' ).
ENDIF.
```
- Bu işi loop içerisinde yapılmasının sebebi birden fazla aynı id'den olma ihtimaline karşın (Bu tabloda yok ama eğitimde geçiyor diye ben de böyle yaptım).
- MODIFY'ı kullanırken belirtilen id ile ilgili kayıt olmasa bile yeni kayıt atarak yine o işlemi yaptırır.
---
### Internal Tableler

```ABAP
TYPES: BEGIN OF ty_material,
		number TYPE i,
		matnr TYPE mara-matnr,
		matkl TYPE mara-matkl,
	END OF ty_material,
	tt_material TYPE TABLE OF ty_material.
DATA: ls_material TYPE ty_material,
	  lt_material TYPE tt_material.

ls_material-number = 1.
ls_material-matnr = '000000000000000074'.
ls_material-matkl = 'L003'.
APPEND ls_material TO lt_material.
CLEAR ls_material.

cl_demo_output=>write( lt_material ).
```
- Internal tablede yapılan işlemlerden biri çok açıklamaya da gerek yok her şey ortada.
---
```ABAP
LOOP AT lt_material ASSIGNING FIELD-SYMBOL(<ls_main>).
	<ls_main>-number = 5.
ENDLOOP.

cl_demo_output=>write( lt_material ).
```
- Burada lt_material interal tablesinin bütün numberlerini 5 yapar.
- UNUTMA: field-symboller hep '<>' aralığında gösterilir.
---
```ABAP
LOOP AT lt_material REFERENCE INTO DATA(ls_ref_main).
	ls_ref_main->number = 6.
ENDLOOP.

cl_demo_output=>write( lt_material ).
```
- Yukarıdakinin farklı bir versiyonu.
---
```ABAP
SORT lt_material DESCENDING BY number.
cl_demo_output=>write( lt_material ).
```
- lt_material'i numarası azalana göre sıralar.
- ![[Pasted image 20240723172432.png]]
---
```ABAP
SORT lt_material ASCENDING BY number.
cl_demo_output=>write( lt_material ).
```
- Üsttekinin tam tersi.
- ![[Pasted image 20240723172643.png]]
---
```ABAP
READ TABLE lt_material INTO ls_material WITH KEY matnr = lv_matnr BINARY SEARCH.
```
- 'READ TABLE' ile ilgili kod örneği üstte.
- Binary search kullanılmasının sebebi daha da hızlı olmasıdır.
- Genelde kullanım amacı bir tabloyu döndürürken başka bir tabloyu okuma amacıdır.

---
```ABAP
SELECT SINGLE(*) FROM mara
WHERE MATNR = '0000000000000001390'
IF sy-subrc = 0.
ENDIF.
```
- Eğer bir verinin olup olmadığını kontrol etmek istiyorsan ama o veriyle de başka işin yoksa bu yöntemi kullanabilirsin.

---
```ABAP
LOOP AT lt_matnr ASSIGNING <ls_matnr>.

	READ TEABLE lt_maktx TRANSPORTING NO FIELDS WITH KEY matrn = <ls_matnr>-matnr BINARY SEARCH.
	IF sy-subrc = 0.
	ENDIF.
ENDLOOP.
```
- Yine read table için de böyle yaparız.

---
