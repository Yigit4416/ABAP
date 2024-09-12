```ABAP
TABLES: eban,
ekpo.


DATA: gt_field_cat TYPE lvc_t_fcat,
	gt_alv_sat TYPE TABLE OF zint_mb_s_sat_alv,
	gt_alv_sas TYPE TABLE OF zint_mb_s_sas_alv,
	gs_layout TYPE lvc_s_layo.
  

SELECTION-SCREEN BEGIN OF BLOCK block1 WITH FRAME TITLE TEXT-001.
SELECT-OPTIONS : s_sat FOR Eban-banfn MODIF ID sat. "Select-options for sas
SELECT-OPTIONS : s_sas FOR ekpo-ebeln MODIF ID sas. "Select-options for sat

  
PARAMETERS : p_stdot TYPE eban-bsart MODIF ID pdt. "Parameter Document Type
PARAMETERS : p_ssmgr TYPE ekpo-matkl MODIF ID pmg. "Parameter Material group
SELECTION-SCREEN END OF BLOCK block1.


SELECTION-SCREEN BEGIN OF BLOCK b2 WITH FRAME TITLE TEXT-002.
PARAMETERS: p_sat RADIOBUTTON GROUP g1 DEFAULT 'X' USER-COMMAND act,
p_sas RADIOBUTTON GROUP g1.
SELECTION-SCREEN END OF BLOCK b2.
```

SAP'ın `EKPO` tablosunda belirttiğiniz bilgileri bulmak için aşağıdaki alan adlarını kullanabilirsiniz:

1. **SAS No:** `EBELN` (Purchase Document Number)
2. **SAS Kalem No:** `EBELP` (Item Number of Purchasing Document)
3. **SAS Malzeme Kısa Metni:** `TXZ01` (Short Text)
4. **SAS Malzeme Miktarı:** `MENGE` (Quantity)
5. **Ölçü Birimi:** `MEINS` (Unit of Measure)

Bu alan adlarını kullanarak EKPO tablosundan gerekli bilgileri çekebilirsiniz.

---

```ABAP
FREE: gt_alv_sat.

SELECT DISTINCT
	eban~banfn,
	eban~bnfpo,
	eban~bsart,
	eban~matkl,
	eban~menge,
	eban~meins
	FROM eban
	INNER JOIN ekpo ON ekpo~banfn = eban~banfn "AND eban-bnfpo = ekpo-bnfpo
	WHERE eban~banfn IN @s_sat " AND bsart EQ @p_stdot
	INTO TABLE @DATA(lt_sat).
SORT lt_sat ASCENDING BY banfn bnfpo.
gt_alv_sat = CORRESPONDING #( lt_sat ).
```
---
```ABAP
FORM build_report.

BREAK D_YIOZDEMIR.

  

  

DATA(lv_count) = 0.

DATA: lv_fieldname TYPE zint_yo_e_fieldname.

  

DATA: lo_descr TYPE REF TO cl_abap_tabledescr,

lo_struct_descr TYPE REF TO cl_abap_structdescr.

  

FIELD-SYMBOLS: <fs_1>.

DO.

  

READ TABLE gt_alv_ctrl INTO gs_alv_ctrl INDEX lv_count.

lv_fieldname = gs_alv_ctrl-fieldname.

  

SELECT *

FROM ekko

INNER JOIN ekpo ON ekko~ebeln = ekpo~ebeln

INTO TABLE @DATA(gt_ekko)

UP TO 100 ROWS.

  

READ TABLE gt_ekko INTO DATA(gs_ekko) INDEX 1.

CLEAR: gs_ekko.

  

LOOP AT gt_ekko INTO gs_ekko.

ASSIGN COMPONENT lv_fieldname OF STRUCTURE <fs_dyn_wa> TO <fs_1>.

<fs_1> = CORRESPONDING <fs_dyn_wa>( gs_ekko ).

ENDLOOP.

  

APPEND <fs_dyn_wa> to <fs_dyn_table>.

  

lv_count += 1.

IF lv_count EQ LINES( gt_alv_ctrl ).

EXIT.

ENDIF.

ENDDO.

  

ENDFORM.
```