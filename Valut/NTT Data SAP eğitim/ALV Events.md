```ABAP
*&---------------------------------------------------------------------*  
*& Include          ZINT_YO_DNMC_EVENTS  
*&---------------------------------------------------------------------*  
  
CLASS gcl_event_receiver DEFINITION.  
  PUBLIC SECTION.  
  
  METHODS handle_top_of_page  
    FOR EVENT top_of_page OF cl_gui_alv_grid  
    IMPORTING  
      e_dyndoc_id  
      table_index.  
  
  METHODS handle_hotspot_click  
    FOR EVENT hotspot_click OF cl_gui_alv_grid  
    IMPORTING  
      e_row_id  
      e_column_id.  
  
  METHODS handle_double_click  
    FOR EVENT double_click OF cl_gui_alv_grid  
    IMPORTING  
      e_row  
      e_column  
      es_row_no.  
  
  METHODS handle_data_changed  
    FOR EVENT data_changed OF cl_gui_alv_grid  
    IMPORTING  
      er_data_changed  
      e_onf4  
      e_onf4_before  
      e_onf4_after  
      e_ucomm.  
  
  METHODS handle_e_onf4  
    FOR EVENT onf4 OF cl_gui_alv_grid  
    IMPORTING  
      e_fieldname  
      e_fieldvalue  
      es_row_no  
      er_event_data  
      et_bad_cells  
      e_display.  
  
ENDCLASS.  
  
CLASS gcl_event_receiver IMPLEMENTATION.  
  METHOD handle_top_of_page.  
    BREAK D_YIOZDEMIR.  
  ENDMETHOD.  
  
  METHOD handle_hotspot_click.  
    BREAK D_YIOZDEMIR.  
  ENDMETHOD.  
  
  METHOD handle_double_click.  
    BREAK D_YIOZDEMIR.  
  ENDMETHOD.  
  
  METHOD handle_data_changed.  
    BREAK D_YIOZDEMIR.  
  ENDMETHOD.  
  
  METHOD handle_e_onf4.  
    BREAK D_YIOZDEMIR.  
  ENDMETHOD.  
ENDCLASS.
ENDCLASS.
```

- Bu ve bunun gibi şeyleri kullanarak verileri handle edebilirsin.
- Kullanacağımız zaman ise şöyle yapacağız:

```ABAP
CREATE OBJECT go_event_receiver.  
SET HANDLER go_event_receiver->handle_hotspot_click FOR go_grid.
```
- displa_alv bölümünde set_teable bilmem neyinin hemen üstünde yapmak istediğim şeyi böyle belirtiyoruz set handler kısmında da artık hangi handleri kullanmak istersek onu kullanırız.