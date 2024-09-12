### Matematiksel işlemler:

```ABAP
DATA gv_degis1 TYPE i.
DATA gv_degis2 TYPE n LENGTH 10.

* Şöyle bir şey de yapılabilir hız için:

DATA: gv_degis3 TYPE i,
	  gv_degis4 TYPE n LENGTH 10,
	  gv_sonuc TYPE i,
	  gv_metin TYPE string.

gv_degis3 = 10.
WRITE gv_degis3.
* output = 10

gv_degis4 = 10
WRITE gv_degis4
* output = 0000000010

* Üstteki gibi yaparsan veriler yan yana olur alt satıra geçirebilmek için:
WRITE / gv_degis4.

gv_sonuc = gv_degis3 + gv_degis4.
WRITE / gv_sonuc.

gv_metin = 'Toplamı: '
gv_sonuc = gv_degis3 - gv_degis4.
WRITE: / gv_metin, gv_sonuc.

* Şöyle de print edebilrisin
WRITE: / 'Toplamı: ', gv_sonuc. 
```

### IF / ELSE / ELSEIF döngüleri:

```ABAP
IF gv_degis3 > gv_degis4.
	WRITE: 'Birinci sayı büyüktür'.
ELSEIF gv_degis4 > gv_degis3.
	WRITE: 'İkinci sayı büyüktür'.
ELSE.
	WRITE: 'İki sayı eşittir'.
ENDIF.
```

### CASE kullanımı:

```ABAP
CASE gv_degis3.
	WHEN 1.
		WRITE: 'gv_degis3 = 1'
	WHEN 2.
		WRITE: 'gv_degis3 = 2'
	WHEN 3.
		WRITE: 'gv_degis3 = 3'
	WHEN 4.
		WRITE: 'gv_degis3 = 4'
	WHEN OTHERS.
		WRITE: 'Hiçbiri değil'
```

### DO kullanımı:

```ABAP
DATA gv_degis TYPE i.
gv_degis = 1.
DO 10 TIMES.
	gv_degis = gv_degis + 1.
	WRITE: / 'DO döngüsü', gv_degis, 'defa çalıştı.'.
ENDDO.
```

### WHILE döngüsü:

```ABAP
WHILE gv_degis < 10.
	gv_degis = gv_degis + 1.
ENDWHILE.
```