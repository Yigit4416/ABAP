> [!note]
> Bu Ders 17'in belli kisminin kopyalanip uzerinden devamidir.

```ABAP
* Main Program
INCLUDE zygt_egt_0030_top. " Degiskenler tanimlanir
INCLUDE zygt_egt_0030_pbo. " Screenlerin acilmadan onceki modullerinin toplandigi yer
INCLUDE zygt_egt_0030_pai. " Olusturduktan sonra butonlarin yakalandigi yer
INCLUDE zygt_egt_0030_frm. " Geri kalan formlarin olusuturldugu yapi

START-OF-SELECTION.
	PERFORM get_data.
*	PERFORM set_fcat. " With set_fieldcatalog_merge we don't need this anymore
	PERFORM set_layout. 
	PERFORM set_fieldcatalog_merge.
	
	CALL SCREEN 0100. " Ekrani cagirir
```

```ABAP
* zygt_egt_0030_top
DATA: go_alv TYPE REF TO cl_gui_alv_grid,
	  go_cont TYPE REF TO cl_gui_custom_container.

DATA: gt_scarr  TYPE TABLE OF gty_scarr,
	  gt_fcat   TYPE lvc_t_fcat,  " Fieldcatalog icin kullanilacak internal table
	  gs_cat    TYPE lvc_s_fcat,  " Bu da onunun structi
	  gs_layout TYPE lvc_s_layo

TYPES: BEGIN OF gty_scarr,
		   carrid      TYPE s_carr_id,
		   carrname    TYPE s_carrname,
		   currcode    TYPE s_currcode,
		   url         TYPE s_carrurl,
		   mess        TYPE char200,
		   line_color  TYPE char4,       " This is necessary for line and cell color
		   cell_color  TYPE lvc_t_scoll,
	   END OF gty_scarr.

DATA: gs_cell_color TYPE lvc_s_scoll.

FIELD-SYMBOLs: <gfs_fc>    TYPE lvc_s_fcat,
			   <gfs_scarr> TYPE gty_scarr.
```

```ABAP
* zygt_egt_0030_pbo
  
MODULE status_0100 OUTPUT.
	SET PF-STATUS '0100'. " Status bar icin atamalar yapilir.
	SET TITLEBAR '0100'.  " Title bar icin atama yapilir.
	PERFORM display_alv.  " Ekran acilmadan ALV'nin hazirlanmasi lazim bu yuzden PBO icine bunu yerlestirdik.
ENDMODULE.
```

```ABAP
* zygt_egt_0030_pai
  
MODULE user_command_0100 INPUT.
	CASE sy-ucomm.
		WHEN '&BACK'. " &BACK isaretli bir noktaya basilinca
			SET SCREEN 0. " Ekrandan cikilir
	ENDCASE.
ENDMODULE.
```

```ABAP
* zygt_egt_0030_frm

FORM display_alv.
	CREATE OBJECT go_cont
		EXPORTING
			container_name   = 'CC_ALV' " Name of the container in style page (we gave this name which we call ID)
			.

	CREATE OBJECT go_alv
		EXPORTING
* Burada go_cont (olusturdugumuz container) yerine full ekran yapmak istersek `cl_gui_container=>screen0` ile yapabiliriz (=> yaptik cunku bu statik...)
* Ve bu noktada go_cont'u da silebiliriz...
			i_parent         = go_cont  " Parent container (Ekrana basacagimiz ALVnin hangi containerda olmasi gerektigini belirtmemiz gerekiyor)
			.
	
	CALL METHOD go_alv->set_table_for_firts_display " Bununla bu ALV'yi ekrana basariz
* Burada i_structure_name'e ihtiyac yok
		EXPORTING
			is_layout        = gs_layout
*			i_structure_name = 'SCARR'  " Internal Output Table Structure Name
		CHANGING
			it_outtab        = gt_scarr " Output Table
			it_fieldcatalog  = gt_fcat  " Bu kolon bazli duzenlemeleri yaptigimiz yapi
		.
ENDFORM.

FORM get_data.
	SELECT * FROM scarr
		INTO CORRESPONDING FIELD OF TABLE gt_scarr.
		
	LOOP AT gt_scarr ASSIGNING <gfs_scarr>.
		CASE <gfs_scarr>-currcode.
			WHEN 'USD'.
				<gfs_scarr>-line_color = 'C710'. " Renk kodu C ile baslar sonra 3 bitlik bir alan var gibi dusun 7 rengi 1 background nasil olacak (acik koyu) 0 da yazi koyu olacka mi olmayacak mi onu belirler.
			WHEN 'JPY'.
				<gfs_scarr>-line_color = 'C501'.
			WHEN 'EUR'.
				CLEAR: gs_cell_color.
				gs_cell_color-name  = 'URL'.
				gs_cell_color-color = '3'. " Bu alttaki 3 alan bu C310 ile ayni olayi goruyor ama bu structure oldugu icin boyle yapmamiz gerekiyor
				gs_cell_color-int   = '1'.
				gs_cell_color-inv   = '0'.
				APPEND gs_cell_color TO <gfs_scarr>-cell_color.
			WHEN 'ZAR'.
				CLEAR: gs_cell_color.
				gs_cell_color-name  = 'URL'.
				gs_cell_color-color = '3'. " Bu alttaki 3 alan bu C310 ile ayni olayi goruyor ama bu structure oldugu icin boyle yapmamiz gerekiyor
				gs_cell_color-int   = '0'. " Acik renkli olacak
				gs_cell_color-inv   = '0'. " Yazinin rengi siyah sari degil
				APPEND gs_cell_color TO <gfs_scarr>-cell_color.
		ENDCASE.
	ENDLOOP.
ENDFORM.

FORM set_fcat.
* Look previous lessons for this field (it is before 18)
ENDFORM.

* With this we don't have to make things one by one it will just create one for us
FORM set_fieldcatalog_merge.
	CALL FUNCTION 'LVC_FIELDCATALOG_MERGE'.
		EXPORTING
			i_structure_name       = 'SCARR'
*			i_internal_tabname     =        " If you don't want to give table you can give structure instead
* If the given structure has different field name from table than it will get previous columns rows and it will paste into there so be careful!
		CHANGING
			ct_fieldcat            = gt_fcat  
		EXCEPTIONS
			INCONSISTENT_INTERFACE = 1
			PROGRAM_ERROR          = 2
			OTHERS                 = 3
			.
	
	IF sy-subrc <> 0.
* you will implement your errors in here
	ENDIF.
	
* If we have a field MESS in gt_fcat than we will make that thign editable (only visual for now)
	READ TABLE gt_fcat ASSIGNING <gfs_fc> WITH KEY fieldname = 'MESS'.
	IF sy-subrc EQ 0.
		<gfs_fc>-edit = abap_true.
	ENDIF.
ENDFORM.

FORM set_layout.
	CLEAR: gs_layout.
	gs_layout-cwidt_opt  = abap_true.    " With this all columns will have `column optimization`
	gs_layout-edit       = abap_true.    " With this all columns will be allowed to make changes from the table 
	gs_layout-no_toolbar = abap_true.    " It will remove toolbar
*	gs_layout-zebra      = abap_true.    " It will make rows look like zebra colors
	gs_layout-info_fname = 'LINE_COLOR'. " We need to specify column name for that
	gs_layout-ctab_fname = 'CELL_COLOR'.
ENDFORM.
```