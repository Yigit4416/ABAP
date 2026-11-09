### CORRESPONDING FIELDS OF
```ABAP
DATA: gt_scarr TYPE TABLE OF scarr,
	  gt_scarr2 TYPE TABLE OF scarr.

START-OF-SELECTION.

SELECT * FROM scarr
	INTO TABLE gt_scarr.

SELECT carrid carrname FROM scarr
	INTO CORRESPONDING FIELDS OF TABLE gt_scarr2.
```

> `CORRESPONDING FIELDS OF` is tells that if we have that field get it but here is the thing if we did not used that for example it will take carrid and put that into `MANDT` field but with this it will look for `carrid` and it will put into right place.

### MOVE-CORRESPONDING
```ABAP
TYPES: BEGIN OF gty_type1,
	     col1 TYPE char10,
	     col2 TYPE char10,
	     col3 TYPE char10,
	     col4 TYPE char10,
	   END OF gty_type1.

TYPES: BEGIN OF gty_type2,
	     col2 TYPE char10,
	     col3 TYPE char10,
	   END OF gty_type2.

DATA: gs_st1 TYPE gty_type1,
	  gs_st3 TYPE gty_type1,
	  gt_ty2 TYPE gty_type2.

START-OF-SELECTION.

* Burada verilerin konulmasi falan var usendim acikcasi
* Eger farklilar ise bunu kullaniriz
	MOVE-CORRESPONDING gs_st1 TO gs_st2.

* Obur turlu sunu da kullanabiliriz
	MOVE gs_st1 TO gs_st3.
```