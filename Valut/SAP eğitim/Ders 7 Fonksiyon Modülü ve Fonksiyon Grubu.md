## Function Group
- Function Group birbiriyle ilişkili **function module**leri bir arada tutan kapsayıcıdır. `SE80` ya da `SE37` ile oluşturulur. Aslında bir module pool programıdır yani kendi global verisi ve include yapısı vardır.
- Function group oluşturulduğu zaman otomatik şu nesneleri üretir (Örnek: `ZFG_FLIGHT):
	- `SAPLZFG_FLIGHT`: Ana program
	- `LZFG_FLIGHTTOP`: Global data tanımları
	- `LZFG_FLIGHTUXX`: Functıon module includelerini tutan include
	- `LZFG_FLIGHTF01`: Form rutinleri
- Function Group bir nevi "state tutan" bir yapıya sahiptir.

## Function Module
- Dışarıdan çağırılabilir, tekrar kullanılabilir bir kod parçasıdır. Her FM bir function group içerisinde yaşar ve `SE37` ile oluşturulur.

### Function Module arayüzü şunlardan oluşur
- `IMPORTING`: girdi parametreleri
- `EXPORTING`: çıktı parametreleri
- `CHANGING`: hem girdi hem de çıktı parametreleri
- `TABLES`: Internal Table (eski yöntem bu artık CHANGING/IMPORTING ile tipli table tercih edilir)
- `EXCEPTIONS`: Klasik hata yönetimi (yeni kodda class-based exception tercih edilir)

### FM Kodu
```ABAP
FUNCTION z_get_flights.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IV_CARRID) TYPE  S_CARR_ID
*"  EXPORTING
*"     VALUE(EV_COUNT) TYPE  I
*"  TABLES
*"      ET_FLIGHT STRUCTURE  SFLIGHT
*"  EXCEPTIONS
*"      NO_DATA_FOUND
*"----------------------------------------------------------------------

  SELECT * FROM sflight
    INTO TABLE et_flight
    WHERE carrid = iv_carrid.

  IF sy-subrc <> 0.
    RAISE no_data_found.
  ENDIF.

  ev_count = lines( et_flight ).
ENDFUNCTION.
```

### FM Çağırmak
```ABAP
DATA: gt_flight TYPE STANDART TABLE OF sflight,
	  gv_count TYPE i.

CALL FUNCTION 'Z_GET_FLIGHTS'
	EXPORTING
		iv_carrid = 'LH'
	IMPORTING
		ev_count = gv_count
	TABLES
		et_flight = gt_flight
	EXCEPTIONS
		no_data_found = 1
		OTHERS = 2.

CASE sy-subrc. 
	WHEN 0. 
		WRITE: / 'Kayıt sayısı:', gv_count.
	WHEN 1. 
		WRITE: / 'Veri bulunamadı'. 
	WHEN OTHERS. 
		WRITE: / 'Beklenmeyen hata'. 
ENDCASE.
```

> **Dikkat**: Çağıran taraftaki `EXPORTING` parametresi, FM tarafında `IMPORTING` olur. Yani yön, bakış açısına göre tersine döner.
> Kolaylık için SE37'de **Pattern** butonuyla `CALL FUNCTION` iskeletini otomatik üretebilirsin.