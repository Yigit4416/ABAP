```ABAP
DATA: gt_scarr TYPE TABLE OF scarr,
      gs_scarr TYPE scarr.

FIELD-SYMBOLS: <gfs_scarr> TYPE scarr.

START-OF-SELECTION.

  SELECT * FROM scarr
    INTO TABLE gt_scarr.

  LOOP AT gt_scarr ASSIGNING <gfs_scarr>.
    IF <gfs_scarr>-carrid EQ 'LH'.
      <gfs_scarr>-carrname = 'Burak Kocaaslan'.
    ENDIF.
  ENDLOOP.

  BREAK-POINT.
```

> You can think this as `pointer`
> We don't have to put it in a structure and than get the necessary data and use `modify gt_scarr from gs_scarr`.