## Event Blocklar
Elimizdeki bloklar ve görevleri:
- **LOAD-OF-PROGRAM**: Program ön belleğe yüklendiği zaman
- **INITIALIZATION**: Selection screen gösterilmeden önce (default değer atama burada yapılıyor)
- **AT SELECTION-SCREEN OUTPUT**: Ekran gösterilemden önce (PBO (Process Before Output) gibi ekranı dinamik değiştirmek için)
- **AT SELECTION-SCREEN**: Kullanıcı girdiği değerleri onayladığı zaman
- **START-OF-SELECTION**: Ana işlem mantığı, veri çekme
- **END-OF-SELECTION**: START-OF-SELECTION bittikten sonra tetiklenen kod bloğu
- **TOP-OF-PAGE**: Liste çıktısında yeni sayfa başlığı
- **AT LINE-SELECTION**: Kullanıcı listede satıra çift tıkladığında tetiklenir
> Not: Kodda hangi sırada yazdığımız önemli değildir...

```ABAP
REPORT zdemo_events.

PARAMETERS p_bukrs TYPE bukrs.

INITIALIZATION.
  p_bukrs = '1000'.

AT SELECTION-SCREEN.
  IF p_bukrs IS INITIAL.
    MESSAGE 'Company code boş olamaz' TYPE 'E'.
  ENDIF.

START-OF-SELECTION.
  WRITE: / 'Veri çekiliyor...'.

END-OF-SELECTION.
  WRITE: / 'Bitti.'.
```
> Not: Eğer START-OF-SELECTION yazılmazsa END-OF-SELECTION otomatik çalıştırılır.

## Form (Subroutines)
- Kod parçalara bölmek ve tekrar kullanmak üzere oluşturulur. `FORM ... ENDFORM` olarak kullanılır ve `PERFORM ...` ile çağırılır.
```ABAP
START-OF-SELECTION
	DATA: lv_a TYPE i VALUE 3,
		  lv_b TYPE i VALUE 4,
		  lv_sum TYPE i.
	
	PERFORM add_number USING lv_a lv_b CHANGING lv_sum.
	WRITE: / lv_sum
	
FORM add_number USING iv_x TYPE i
					  iv_y TYPE i
				CHANGING cv_result TYPE i.
	cv_result = iv_x + iv_y.
ENDFORM.
```

## Include
- Kodu ayrı dosyalara bölmek için kullanılan bir kaynak kod modülüdür. Kendi başına çalıştırılamaz parametre alamaz. Derleme sırasında ana programa kopyalanır gibi düşün.

```ABAP
REPORT zdemo_main.

INCLUDE zdemo_top. " global veri tanimlari
INCLUDE zdemo_sel. " selection screen
INCLUDE zdemo_f01. " FORM rutinleri

START-OF-SELECTION
	PERFORM get_data.
	PERFORM display_data.
```

### Standart İsimlendirme
- `_TOP`: Global data tanımları
- `_SEL` veya `_SRC`: Selection screen
- `_F01`. `_F02`: FORM rutinleri
- `_PBO` / `_PAI`: Dynpro modülleri
- `_CLS`: Lokal class tanımları

