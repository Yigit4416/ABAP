- SALV tablo sayfası oluşturma:
```ABAP
DATA: gt_sbook TYPE TABLE OF sbook,
	  go_salv TYPE REF TO cl_salv_table.

START-OF-SELECTION.

SELECT * UP TO 20 ROWS
	FROM sbook
	INTO TABLE gt_sbook.

cl_salv_table=>factory(
IMPORTING
	r_salv_table = go_salv
CHANGING
	t_table = gt_sbook
).

go_salv->display( ).
```

---

- Bir header belirlemek için:
```ABAP
DATA: gt_sbook TYPE TABLE OF sbook,
	  go_salv TYPE REF TO cl_salv_table.
  
START-OF-SELECTION.
  
SELECT * UP TO 20 ROWS
FROM sbook
INTO TABLE gt_sbook.
  
cl_salv_table=>factory(
	IMPORTING
		r_salv_table = go_salv
	CHANGING
		t_table = gt_sbook

).
  
DATA: lo_display TYPE REF TO cl_salv_display_settings.
  
lo_display = go_salv->get_display_settings( ).
lo_display->set_list_header( 'SALV Eğitim' ).
  
go_salv->display( )
```
- Burada yaptığımız işlem ilk oalrak lo_displayi tanımlayıp sonra onu get_display_settings'e bağlayıp headeri oluşturur.

---

- Satırların birini koyu birini açık yapmak için: 
```ABAP
DATA: gt_sbook TYPE TABLE OF sbook,
	  go_salv TYPE REF TO cl_salv_table.
  
START-OF-SELECTION.
  
SELECT * UP TO 20 ROWS
FROM sbook
INTO TABLE gt_sbook.
  
cl_salv_table=>factory(
	IMPORTING
		r_salv_table = go_salv
	CHANGING
		t_table = gt_sbook

).
  
DATA: lo_display TYPE REF TO cl_salv_display_settings.
  
lo_display = go_salv->get_display_settings( ).
lo_display->set_list_header( 'SALV Eğitim' ).
lo_display->set_striped_pattern( 'X' ).
  
go_salv->display( )
```
- lo_display_->set_striped_pattern( 'X' ). kodunu kullanırsın.

---

- Kolonları minimum noktaya çekmek için:
```ABAP
DATA: lo_cols TYPE REF TO cl_salv_columns.

lo_cols = go_salv->get_columns( ).
lo_cols->set_optimize( value = 'X' ).
```
- Bu kodu eklersin ve kolonlar olabildiğince incelir.
- Tam kod örneği:
```ABAP
DATA: gt_sbook TYPE TABLE OF sbook,
	  go_salv TYPE REF TO cl_salv_table.
  
START-OF-SELECTION.
  
SELECT * UP TO 20 ROWS
FROM sbook
INTO TABLE gt_sbook.
  
cl_salv_table=>factory(
	IMPORTING
		r_salv_table = go_salv
	CHANGING
		t_table = gt_sbook

).
  
DATA: lo_display TYPE REF TO cl_salv_display_settings.
  
lo_display = go_salv->get_display_settings( ).
lo_display->set_list_header( 'SALV Eğitim' ).
lo_display->set_striped_pattern( 'X' ).

DATA: lo_cols TYPE REF TO cl_salv_columns.

lo_cols = go_salv->get_columns( ).
lo_cols->set_optimize( value = 'X' ).

go_salv->display( )
```

---

- Bazı kolonların adını değiştirmek için:
```ABAP
DATA: lo_cols TYPE REF TO cl_salv_columns.

lo_cols = go_salv->get_columns( ).
lo_cols->set_optimize( value = 'X' ).

DATA: lo_col TYPE REF TO cl_salv_column.
lo_col = lo_cols->get_column( columnname = 'INVOICE' ).
lo_col->set_long_text(' Yeni Fatura Düzenleyicisi ').
lo_col->set_medium_text(' Yeni Fatura D. ').
lo_col->set_short_text(' Yeni F. ').
```
- Burada farklı olarak go_salv ile bağlamak yerine lo_cols ile bağlarız.
- Sonrasında istediğimiz kısımları düzenleriz.

- Ancak insanlık hatasından dolayı olmayan bir kolonu adını yazarsak dump yeriz bunu önlemek için TRY-CATCH kullanırız burada da kod örneği var:

```ABAP
TRY.
	  lo_col = lo_cols->get_column( columnname = 'INVOICE' ).
	  lo_col->set_long_text(' Yeni Fatura Düzenleyicisi ').
	  lo_col->set_medium_text(' Yeni Fatura D. ').
	  lo_col->set_short_text(' Yeni F. ').
	CATCH cx_salv_not_found.
ENDTRY.
```

- Tam kod örneği:
```ABAP
DATA: gt_sbook TYPE TABLE OF sbook,
	  go_salv TYPE REF TO cl_salv_table.
  
START-OF-SELECTION.
  
SELECT * UP TO 20 ROWS
FROM sbook
INTO TABLE gt_sbook.
  
cl_salv_table=>factory(
	IMPORTING
		r_salv_table = go_salv
	CHANGING
		t_table = gt_sbook

).
  
DATA: lo_display TYPE REF TO cl_salv_display_settings.
  
lo_display = go_salv->get_display_settings( ).
lo_display->set_list_header( 'SALV Eğitim' ).
lo_display->set_striped_pattern( 'X' ).

DATA: lo_cols TYPE REF TO cl_salv_columns.

lo_cols = go_salv->get_columns( ).
lo_cols->set_optimize( value = 'X' ).

DATA: lo_col TYPE REF TO cl_salv_column.
lo_col = lo_cols->get_column( columnname = 'INVOICE' ).
lo_col->set_long_text(' Yeni Fatura Düzenleyicisi ').
lo_col->set_medium_text(' Yeni Fatura D. ').
lo_col->set_short_text(' Yeni F. ').

go_salv->display( )
```

---

- İstediğimiz kolonların görünmemesini sağlamak:
```ABAP
lo_col = lo_cols->get_column( columnname = 'MANDT' ).
lo_col->set_visible( value = if_salv_c_bool_sap=>false ).
```
- Yine aynı kodu kullanarak deva ederiz.
- Üst seçenekte belirtilen dump olayı burada da olabilir yine aynı mantıkla yazılır buraya yazmaya üşendim.
- Tam kod örneği:
```ABAP
DATA: gt_sbook TYPE TABLE OF sbook,
	  go_salv TYPE REF TO cl_salv_table.
  
START-OF-SELECTION.
  
SELECT * UP TO 20 ROWS
FROM sbook
INTO TABLE gt_sbook.
  
cl_salv_table=>factory(
	IMPORTING
		r_salv_table = go_salv
	CHANGING
		t_table = gt_sbook

).
  
DATA: lo_display TYPE REF TO cl_salv_display_settings.
  
lo_display = go_salv->get_display_settings( ).
lo_display->set_list_header( 'SALV Eğitim' ).
lo_display->set_striped_pattern( 'X' ).

DATA: lo_cols TYPE REF TO cl_salv_columns.

lo_cols = go_salv->get_columns( ).
lo_cols->set_optimize( value = 'X' ).

DATA: lo_col TYPE REF TO cl_salv_column.
lo_col = lo_cols->get_column( columnname = 'INVOICE' ).
lo_col->set_long_text(' Yeni Fatura Düzenleyicisi ').
lo_col->set_medium_text(' Yeni Fatura D. ').
lo_col->set_short_text(' Yeni F. ').

lo_col = lo_cols->get_column( columnname = 'MANDT' ).
lo_col->set_visible( value = if_salv_c_bool_sap=>false ).

go_salv->display( )
```

---

- Tool bar oluşturma:
```ABAP
DATA: lo_func TYPE REF TO cl_salv_functions.
lo_func = go_salv->get_functions( ).
lo_func->set_all( abap_true ).
```
- Tool box aşağıdaki şeydir:
	- ![[Pasted image 20240730170831.png]]
	- Bunlarla filtreleme sıralama gibi kritik işlemleri yapabilirsin.

---

- Top header oluşturma:
```ABAP
DATA: lo_header   TYPE REF TO cl_salv_form_layout_grid,
	  lo_h_label  TYPE REF TO cl_salv_form_label,
	  lo_h_flow   TYPE REF TO cl_salv_form_layout_flow.

CREATE OBJECT lo_header.

lo_h_label = lo_header->create_label( row = 1 column = 1 ).
lo_h_label->set_text( value = 'Başlık İlk Satır' ).

lo_h_flow = lo_header->create_flow( row = 2 column = 1 ).
lo_h_flow->create_text(
EXPORTING
text = 'Başlık 2. Satır' )

go_salv->set_top_of_list( value = lo_header ).
```
- Burada 3 tane DATA oluşturuyoruz.
-  Sonrasında ilk olarak lo_header objesini oluşturuyoruz.
- lo_header objesine bağlı olarak flow ve label oluşturuyoruz.
- lo_headeri go_salv'ye set_top_of_list ile bağlarız.
- Ve bu şekil ortaya çıkar:
	- ![[Pasted image 20240730172811.png]]
	- Renkten dolayı anlaşılmıyor ama en tepede gerekli header oluşuyor.

---

- Normal cl_demo_output'taki gibi popup gibi görünmesi için:
```ABAP
go_salv->set_screen_popup(
	EXPORTING
		start_column = 10
		end_column = 75
		start_line = 5
		end_line = 25
).
```
- ![[Pasted image 20240730173109.png]]

---
