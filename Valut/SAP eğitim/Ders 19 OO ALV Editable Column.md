> [!note]
> Bu Ders 18'in belli kisminin kopyalanip uzerinden devamidir.

> [!tip]
> Bunun icin save butonunu aktive etmen lazim

```ABAP
* Main Program
REPORT zygt_egt_0030.

INCLUDE zygt_egt_0030_top.  " Degiskenler tanimlanir
INCLUDE zygt_egt_0030_pbo.  " Ekranlar acilmadan once calisan modullerin toplandigi yer
INCLUDE zygt_egt_0030_pai.  " Ekran olustuktan sonra butonlarin yakalandigi yer
INCLUDE zygt_egt_0030_frm.  " Geri kalan formlarin olusturuldugu yer

START-OF-SELECTION.
  PERFORM get_data.
  PERFORM set_layout.
  PERFORM set_fcat.

  CALL SCREEN 0100.  " Ekrani cagirir
```

```ABAP
* zygt_egt_0030_top

* Tipler, kullanildiklari DATA tanimlarindan once gelmeli
TYPES: BEGIN OF gty_scarr,
         carrid     TYPE s_carr_id,
         carrname   TYPE s_carrname,
         currcode   TYPE s_currcode,
         url        TYPE s_carrurl,
         cost       TYPE int4,
       END OF gty_scarr.

DATA: go_alv  TYPE REF TO cl_gui_alv_grid,
      go_cont TYPE REF TO cl_gui_custom_container.

DATA: gt_scarr  TYPE TABLE OF gty_scarr,
	  gs_scarr  TYPE gty_scarr,
      gt_fcat   TYPE lvc_t_fcat,   " Fieldcatalog icin kullanilacak internal table
      gs_cat    TYPE lvc_s_fcat,   " Yukaridakinin structure'i
      gs_layout TYPE lvc_s_layo.

DATA: gs_cell_color TYPE lvc_s_scol.

FIELD-SYMBOLS: <gfs_fc>    TYPE lvc_s_fcat,
               <gfs_scarr> TYPE gty_scarr.
```

```ABAP
* zygt_egt_0030_pbo

MODULE status_0100 OUTPUT.
  SET PF-STATUS '0100'.  " Status bar atamalari
  SET TITLEBAR '0100'.   " Title bar atamasi
  PERFORM display_alv.   " ALV ekran acilmadan hazirlanmali, o yuzden PBO icinde cagirdik
ENDMODULE.
```

```ABAP
* zygt_egt_0030_pai
* Unutma dinamik olarak yapilan degisiklikler burada yapilmasi lazim mesela birinin costuna gore bir sey yaparken degisiklikten sonra buraya dusecek ve mesela ortalamadan buyukse o zaman oranin iconu burada duzenlenmeli yine ayni sekilde get_total_sum burada cunku degisiklik yapiliyor sonrasinda da SAP PAI'ye atiyor ve boylece 

MODULE user_command_0100 INPUT.
  CASE sy-ucomm.
    WHEN '&BACK'.              " &BACK butonuna basilinca
	    SET SCREEN 0.          " Ekrandan cikilir
    WHEN '&SAVE'.
		PERFORM get_total_sum.
  ENDCASE.
ENDMODULE.
```

```ABAP
* zygt_egt_0030_frm

FORM display_alv.
* PBO her calistiginda tekrar tekrar olusturmamak icin sadece ilk seferde olustur
  IF go_alv IS INITIAL.

    CREATE OBJECT go_cont
      EXPORTING
        container_name = 'CC_ALV'.  " Screen painter'da container'a verdigimiz isim

* Full ekran istersek go_cont yerine `cl_gui_container=>screen0` kullanabiliriz
* (statik oldugu icin => ile). Bu durumda go_cont'u da silebiliriz.
    CREATE OBJECT go_alv
      EXPORTING
        i_parent = go_cont.  " ALV'nin hangi container'a basilacagini belirtir

* ALV'yi ekrana basar. Fieldcatalog verdigimiz icin i_structure_name'e gerek yok.
    CALL METHOD go_alv->set_table_for_first_display
      EXPORTING
*       i_structure_name = 'SCARR'
        is_layout       = gs_layout
      CHANGING
        it_outtab       = gt_scarr  " Output table
        it_fieldcatalog = gt_fcat.  " Kolon bazli duzenlemelerin yapildigi yapi
	
*	CALL METHOD go_alv->register_edit_event.
*	  EXPORTING
*	    i_event_id = cl_gui_alv_grid=>mc_evt_modified. " bununla herhangi bir degisiklik yapildigi zaman otomatik degisikligi gerceklestir der
	    
	CALL METHOD go_alv->register_edit_event.
	  EXPORTING
	    i_event_id = cl_gui_alv_grid=>mc_evt_enter. " bununla enter'e bastigimiz zaman degisikligi kaydeder
  ENDIF.
ENDFORM.

FORM get_data.
  SELECT * FROM scarr
    INTO CORRESPONDING FIELDS OF TABLE gt_scarr.
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
	
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'COST'.
	gs_fcat-scrtext_s = 'Short description'.
	gs_fcat-scrtext_m = 'Medium description'.
	gs_fcat-scrtext_l = 'Long description'.
	gs_fcat-col_pos   = 4.
	gs_fcat-edit      = abap_true.
	APPEND gs_fcat TO gt_fcat.
	
* For example if we don't want to bother tings like this and if the fields have allready made we can just make use of this thing (ofcourse we need to get rid of the thing on top
	CLEAR: gs_fcat.
	gs_fcat-fieldname = 'URL'.
	gs_fcatref_table  = 'SCARR'.
	gs_fcat-ref_field = 'URL'.
	APPEND gs_fcat TO gt_fcat.
ENDFORM.

FORM set_layout.
  CLEAR gs_layout.
  gs_layout-cwidth_opt = abap_true.  " Tum kolonlara column optimization uygular
  gs_layout-edit       = abap_true.  " Tum kolonlar tablo uzerinden duzenlenebilir olur
  gs_layout-no_toolbar = abap_true.  " Toolbar'i kaldirir
ENDFORM.

FORM get_total_sum.
	DATA: lv_ttl_sum   TYPE int4,
		  lv_ttl_sum_c TYPE char10,
		  lv_mess      TYPE char200.
	
	LOOP AT gt_scarr INTO gs_scarr.
		lv_ttl_sum = lv_ttl_sum + gs_scarr-cost.
	ENDLOOP.
	
	lv_ttl_sum_c = lv_ttl_sum.
	
	CONCANATE `Tum satirlarin toplami ` lv_ttl_sum_c INTO lv_mess.
	
	MESSAGE lv_mess TYPE 'I'.
ENDFORM.
```