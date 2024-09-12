```ABAP
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"D grubu için

ELSEIF count EQ 3.

  

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

" A torbası için

  

lv_length = LINES( lt_torba_a ).

  

IF lv_length > 0.

lv_random = cl_abap_random_int=>create( seed = cl_abap_random=>seed( )

min = 1

max = lv_length )->get_next( ).

ENDIF.

  

READ TABLE lt_torba_a INDEX lv_random INTO ls_group.

APPEND ls_group TO lt_grup_d.

  

clear: lv_length, lv_random.

  

" TODO: Seçilen ülkeyi sil

DELETE: lt_torba_a WHERE takim EQ ls_group-takim.

  

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"B torbası için

  

LOOP AT lt_torba_b INTO ls_temp.

  

READ TABLE lt_grup_d WITH KEY ulke = ls_temp-ulke TRANSPORTING NO FIELDS.

IF sy-subrc EQ 0.

  

ELSE.

APPEND ls_temp TO lt_temp_b.

ENDIF.

  

ENDLOOP.

  

lv_length = LINES( lt_temp_b ).

  

IF lv_length > 0.

lv_random = cl_abap_random_int=>create( seed = cl_abap_random=>seed( )

min = 1

max = lv_length )->get_next( ).

ENDIF.

  

READ TABLE lt_temp_b INDEX lv_random INTO ls_group.

APPEND ls_group TO lt_grup_d.

  

clear: lv_length, lv_random, ls_temp, lt_temp_b.

  

DELETE: lt_torba_b WHERE takim EQ ls_group-takim.

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"C torbası için

  

LOOP AT lt_torba_c INTO ls_temp.

  

READ TABLE lt_grup_d WITH KEY ulke = ls_temp-ulke TRANSPORTING NO FIELDS.

IF sy-subrc EQ 0.

  

ELSE.

APPEND ls_temp TO lt_temp_c.

ENDIF.

  

ENDLOOP.

  

lv_length = LINES( lt_temp_c ).

  

IF lv_length > 0.

lv_random = cl_abap_random_int=>create( seed = cl_abap_random=>seed( )

min = 1

max = lv_length )->get_next( ).

ENDIF.

  

READ TABLE lt_temp_c INDEX lv_random INTO ls_group.

APPEND ls_group TO lt_grup_d.

  

clear: lv_length, lv_random, ls_temp, lt_temp_c.

  

DELETE: lt_torba_c WHERE takim EQ ls_group-takim.

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"D Torbası için

  

LOOP AT lt_torba_d INTO ls_temp.

  

READ TABLE lt_grup_d WITH KEY ulke = ls_temp-ulke TRANSPORTING NO FIELDS.

IF sy-subrc EQ 0.

  

ELSE.

APPEND ls_temp TO lt_temp_d.

ENDIF.

  

ENDLOOP.

  

lv_length = LINES( lt_temp_d ).

  

IF lv_length > 0.

lv_random = cl_abap_random_int=>create( seed = cl_abap_random=>seed( )

min = 1

max = lv_length )->get_next( ).

ENDIF.

  

READ TABLE lt_temp_d INDEX lv_random INTO ls_group.

APPEND ls_group TO lt_grup_d.

  

clear: lv_length, lv_random, ls_temp, lt_temp_d.

count += 1.

  

DELETE: lt_torba_d WHERE takim EQ ls_group-takim.
```
















|                   |                    |                        |                    |
| ----------------- | ------------------ | ---------------------- | ------------------ |
| Liverpool-EN++    | Manchester City-EN | Borussia Dortmund-DE++ | AEK-GR++           |
| Bayern Münih-DE++ | PSV-NE++           | Galatasaray-TR++       | Roma-IT++          |
| İnter-IT          | Porto-PO++         | Marsilya-FR++          | Steaua Bükreş-RO++ |
| PGS-FR++          | Real Madrid-ES++   | Ajax-NE                | Atletico Madrid-ES |