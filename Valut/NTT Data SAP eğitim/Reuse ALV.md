- Bunu kullanabilmek için öncelikle şu fonksiyonu çağırman lazım:
```ABAP
CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
```

- Çalışma mantığı şudur:
	- Kendi belirlediğimiz 3 tane kolon var diyelim.
	- Sonrasında bunları basabilmek için *FIELDCATALOG* diye bir şeye ihtiyacımız var.
	- Bunun altında en önemli kolonlardan biri olan *fieldname*'nin altına belirlediğimiz kolonları satır satır ekleriz.
```ABAP
TYPES: BEGIN OF gty_list,
		ebeln TYPE EBELN,
		ebelp TYPE EBELP,
		bstyp TYPE EBSTYP,
		bsart TYPE ESART,
		matnr TYPE MATNR,
		menge TYPE BSTMG,
	   END OF gty_list.

  

DATA: gt_list TYPE TABLE OF gty_list,
	  gs_list TYPE gty_list.

  

DATA: gt_fieldcatalog TYPE slis_t_fieldcat_alv,
	  gs_fieldcatalog TYPE slis_fieldcat_alv.

START-OF-SELECTION.

SELECT
	ekko~ebeln,
	ekpo~ebelp,
	ekko~bstyp,
	ekko~bsart,
	ekpo~matnr,
	ekpo~menge
	FROM ekko
	INNER JOIN ekpo ON ekpo~ebeln EQ ekko~ebeln
	INTO TABLE @gt_list.

CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'EBELN'.
APPEND gs_fieldcatalog TO gt_fieldcatalog.


CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'EBELNP'.
APPEND gs_fieldcatalog TO gt_fieldcatalog.


CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'BSTYP'.
APPEND gs_fieldcatalog TO gt_fieldcatalog.


CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'BSART'.
APPEND gs_fieldcatalog TO gt_fieldcatalog.


CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'MATNR'.
APPEND gs_fieldcatalog TO gt_fieldcatalog.


CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'MENGE'.
APPEND gs_fieldcatalog TO gt_fieldcatalog.
```

- Aynen burada olduğu gibi teker teker gt_fieldcatalog'a ekleriz sonrasında da bu tableyi çağırdığımız fonksiyonda it_fieldcat alanına atarız.
- Verilerimizin bulunduğu tableyi de aynı fonksiyonun TABLES kısmındaki t_outtab kısmına atarız.

- Key olduğunu belirtmek için şu kodu ekleriz:
```ABAP
gs_fieldcatalog-key = abap_true.
```

- Tam kod örneği:
```ABAP
CLEAR: gs_fieldcatalog.
gs_fieldcatalog-fieldname = 'EBELN'.
gs_fieldcatalog-seltext_s = 'SAS No.'.
gs_fieldcatalog-seltext_m = 'SAS Numarası'.
gs_fieldcatalog-seltext_l = 'SAS Numarası'.
gs_fieldcatalog-key = abap_true.
APPEND gs_fieldcatalog TO gt_fieldcatalog.
```
- Gördüğün gibi rengi değiştirdi:
	- ![[Pasted image 20240731115133.png]]

- Hangi kolonun kaçıncı pozisyonda olacağına şöyle karar verirsin:
```ABAP
gs_fieldcatalog-col_pos = 3.
```
- Mesela burada hangi kolonla beraber kullanıldıysa onu 3. pozisyona alacak.

- İstediğin kolonun uzunluğunu değiştirmek için şunu yaparsın:
```ABAP
gs_fieldcatalog-outputlen = 40.
```

- Bir kolonu hotspot yapmak için:
```ABAP
gs_fieldcatalog-hotspot = abap_true.
```

- Bir kolonun editlenebilir yapmak için:
```ABAP
gs_fieldcatalog-edit = abap_true.
```

- Bir kolonun değerleri toplamını alabilmek için:
```ABAP
gs_fieldcatalog-do_sum = abap_true.
```

- Title bar belirlemek için:
```ABAP
gs_layout-window_titlebar = 'Reuse ALV Title'.
```

- Zebra gibi bir koyu bir açık yapabilmek için: 
```ABAP
gs_layout-zebra = abap_true.
```

- Tüm satır ve kolonları düzenlenebilr hale getirmek için:
```ABAP
gs_layout-edit = abap_true.
```

- 