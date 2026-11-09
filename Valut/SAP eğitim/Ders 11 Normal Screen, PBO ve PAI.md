## PBO (Process Before Output)
- Title, üstteki butonlar bunların hepsi buradan düzenlenir.
- Bunları yapmak için `GUI title` ve `GUI status` oluştururuz ekstradan.
- Oluşturunca bunlar için module oluşturulur ve bunun üzerinden değişiklikler yapılır.
```ABAP
MODULE status_0100 OUTPUT.
	SET PF-STATUS 'STATUS_0100'.
	SET TITLEBAR 'TITLE_0100'.
ENDMODULE.

MODULE user_command_0100 INPUT.
	IF sy-ucomm EQ '&BACK'. " &BACK başka menü üzerinden değişiklik yapılarak elde edilir öyle default bir değer değildir (GUI status oluştururken karşımıza çıkar)
		LEAVE TO SCREEN 0.
	ENDIF.
```

## PAI (Process After Input)
- Screen üzerinde işlemler yaptıktan sonra burada veri çekme falan filan bunların hepsi burada gerçekleşir.

## Normal Screen
- Normal Screen ekran tasarımı (arayüz) yaptığımız ekrandır.
```ABAP
START-OF-SELECTION.
	CALL SCREEN 0100. " (0100 oluşturduğumuz dynopro ekran (normal screen))
```
- Ekrana yeni bir buton eklemek için de ilk olarak bir variable oluşturup sonrasında da styler şeyinde bunu bulup oraya yerleştirmek.
- Radio buton oluşturmak için `xfeld` tipinde bir variable oluşturup onu kullanman lazım. Bir de bunları eklerken `Apply as` kısmında Radio buton yazmayı unutmaman lazım. Bunları da yine grup haline getirebilirsin onları topluca seçip sonrasında da sağ tık yapıp oradaki menülerden birini kullanarak yapabilirsin.
> Checkli olup olmadığını test ederken şunu yaparsın:
```ABAP
IF gv_var1 EQ abap_true.
ENDIF.
```

### Screen Dropdown
- For that we need to define key values and normal variable itself here is hwo it works out
```abap
DATA: gv_yas TYPE i.

DATA: gv_key TYPE vrm_id,
	  gt_value TYPE vrm_values, " This is a table
	  gs_value TYPE vrm_value.  " This is a struct (the thing that gets single value)
	  
START-OF-SELECTION
	MODULE status_0100 OUTPUT.
		gv_id = 'GV_YAS'.
		gs_value-yas = '1'.
		gs_value-text = '18 yas'.
		APPEND gs_value TO gt_values.
		
		gs_value-yas = '2'.
		gs_value-text = '19 yas'.
		APPEND gs_value TO gt_values.
		
		CALL FUNCTION 'VRM_SET_VALUES'
			EXPORTING 
				id = gv_id
				values = gt_values.
	ENDMODULE.
```
> When we select it it will send the `key` to `gv_yas`.

### Sub Screens
- To create those things i screen creation tool we need to use `subscreen` option not `normal`.
- When we want it we can call it like this (it needs to be under a normal screen)
```abap
PROCESS BEFORE OUTPUT
	MODULE status_0100
		CALL SUBSCREEN sub1 INCLUDING sy-repid '0101'
```
> sub1 is the the subscreens area id.
> sy-repid is that current programs name. it is just convenience so we don't have to write program by hand and cause any issues.
> And 0101 is subscreens given number

