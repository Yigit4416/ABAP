- Object Oriented mantığına istinaden ilk olarak includeleri oluştururuz.
- Ekranlari olustururuz.
```ABAP
* Main Program
INCLUDE zygt_egt_0030_top. " Degiskenler tanimlanir
INCLUDE zygt_egt_0030_pbo. " Screenlerin acilmadan onceki modullerinin toplandigi yer
INCLUDE zygt_egt_0030_pai. " Olusturduktan sonra butonlarin yakalandigi yer
INCLUDE zygt_egt_0030_frm. " Geri kalan formlarin olusuturldugu yapi

START-OF-SELECTION.
	PERFORM get_data.
	
	CALL SCREEN 0100. " Ekrani cagirir
```

```ABAP
* zygt_egt_0030_top
DATA: go_alv TYPE REF TO cl_gui_alv_grid,
	  go_cont TYPE REF TO cl_gui_custom_container.

DATA: gt_scarr TYPE TABLE OF scarr.
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
* Burada go_cont (olusturdugumuz container) yerine full ekran yapmak istersek `cl_gui_container=>screen0` ile yapabiliriz
* Ve bu noktada go_cont'u da silebiliriz...
			i_parent         = go_cont  " Parent container (Ekrana basacagimiz ALV'nin hangi container'de olmasi gerektigini belirtmemiz gerekiyor)
			.
	
	CALL METHOD go_alv->set_table_for_firts_display " Bununla bu ALV'yi ekrana basariz
		EXPORTING
			i_structure_name = 'SCARR'  " Internal Output Table Structure Name
		CHANGING
			it_outtab        = gt_scarr " Output Table
		.
ENDFORM

FORM get_data.
	SELECT * FROM scarr
		INTO TABLE gt_scarr.
ENDFORM
```