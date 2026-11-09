> [!note]
> Bu Ders 15'in belli kisminin kopyalanip uzerinden devamidir.

```ABAP
* Main Program
INCLUDE zygt_egt_0030_top. " Degiskenler tanimlanir
INCLUDE zygt_egt_0030_pbo. " Screenlerin acilmadan onceki modullerinin toplandigi yer
INCLUDE zygt_egt_0030_pai. " Olusturduktan sonra butonlarin yakalandigi yer
INCLUDE zygt_egt_0030_frm. " Geri kalan formlarin olusuturldugu yapi

START-OF-SELECTION.
	PERFORM get_data.
	PERFORM set_fcat.
	PERFORM set_layout.
	
	CALL SCREEN 0100. " Ekrani cagirir
```

```ABAP
* zygt_egt_0030_top
DATA: go_alv TYPE REF TO cl_gui_alv_grid,
	  go_cont TYPE REF TO cl_gui_custom_container.

DATA: gt_scarr  TYPE TABLE OF scarr,
	  gt_fcat   TYPE lvc_t_fcat,  " Fieldcatalog icin kullanilacak internal table
	  gs_cat    TYPE lvc_s_fcat,  " Bu da onunun structi
	  gs_layout TYPE lvc_s_layo
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
		INTO TABLE gt_scarr.
ENDFORM.

FORM set_fcat.
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'CARRID'.
	gs_fcat-scrtext_s = 'Short description'.
	gs_fcat-scrtext_m = 'Medium description'.
	gs_fcat-scrtext_l = 'Long description'.
	gs_fcat-col_pos   = 2.
	gs_fcat-key       = abap_true. " It will make its color different so user will understand that this is a key value
	APPEND gs_fcat TO gt_fcat.
	
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'CARRNAME'.
	gs_fcat-scrtext_s = 'Short description'.
	gs_fcat-scrtext_m = 'Medium description'.
	gs_fcat-scrtext_l = 'Long description'.
	gs_fcat-col_pos   = 3.
	gs_fcat-edit      = abap_true. " It will allow to make changes from the table (but only visually for its functions we need to do other things (which we won't for now))"
	APPEND gs_fcat TO gt_fcat.
	
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'CURRCODE'.
	gs_fcat-scrtext_s = 'Short description'.
	gs_fcat-scrtext_m = 'Medium description'.
	gs_fcat-scrtext_l = 'Long description'.
	gs_fcat-col_pos   = 4.
	gs_fcat-hotspot   = abap_true. " That part will be clickable (we will need other functions as well)
	APPEND gs_fcat TO gt_fcat.
	
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'URL'.
	gs_fcat-scrtext_s = 'Short description'.
	gs_fcat-scrtext_m = 'Medium description'.
	gs_fcat-scrtext_l = 'Long description'.
	gs_fcat-col_pos   = 1.
* Column optimization (it will manage itself width auto) for example normaly it tries to make width same as column name but with this it can go wider acording to its rows
	gs_fcat-col_opt   = abap_true.
	gs_fcat-no_out    = abap_true. " This field won't shown in ALV
	gs_fcat-outputlen = 100        " It will have fixed width form the beginning
	APPEND gs_fcat TO gt_fcat.
	
* For example if we don't want to bother tings like this and if the fields have allready made we can just make use of this thing (ofcourse we need to get rid of the thing on top
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'URL'.
	gs_fcatref_table  = 'SCARR'.
	gs_fcat-ref_field = 'URL'.
	APPEND gs_fcat TO gt_fcat.
ENDFORM.

FORM set_layout.
	CLEAR: gs_layout.
	gs_layout-cwidt_opt  = abap_true. " With this all columns will have `column optimization`
	gs_layout-edit       = abap_true. " With this all columns will be allowed to make changes from the table 
	gs_layout-no_toolbar = abap_true. " It will remove toolbar
	gs_layout-zebra      = abap_true. " It will make rows look like zebra colors
ENDFORM.
```