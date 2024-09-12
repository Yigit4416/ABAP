- Bir tablenin verilerini başka bir tableye atmak.
```ABAP
MOVE lt_material TO lt_material_buffer.
```
---
 - Bir tablenin sadece gerekli kısımlarını başka tableye aktarmak.
```ABAP
MOVE-CORRESPONDING lt_material TO lt_material_0.
```
---
- Sorted internal table oluşturmak.
```ABAP
DATA: lt_material_kırtasiye TYPE SORTED TABLE OF lty_material WITH UNIQUE KEY matnr.
```
---
- Kodun çalışma süresini ölçmek için.
```ABAP
DATA: lv_start_time TYPE i,
      lv_end_time TYPE i,
      lv_elapsed_time TYPE i.

* Başlangıç zamanını al
GET RUN TIME FIELD lv_start_time.

* Ölçmek istediğiniz kod bloğu
DO 1000 TIMES.
  " Zaman alıcı işlemler burada yapılır.
ENDDO.

* Bitiş zamanını al
GET RUN TIME FIELD lv_end_time.

* Geçen süreyi hesapla
lv_elapsed_time = lv_end_time - lv_start_time.

WRITE: / 'Geçen süre:', lv_elapsed_time, 'mikrosaniye'.
```
---
- COLLECT komutu aynı anahtara sahip satırları birleştirerek sayısal alanları toplar örnek olarak:
```ABAP
DATA: BEGIN OF itab OCCURS 0,
        field1 TYPE c LENGTH 5,
        field2 TYPE i,
      END OF itab.

itab-field1 = 'A'.
itab-field2 = 10.
COLLECT itab INTO itab.

itab-field1 = 'B'.
itab-field2 = 20.
COLLECT itab INTO itab.

itab-field1 = 'A'.
itab-field2 = 30.
COLLECT itab INTO itab.

LOOP AT itab.
  WRITE: / itab-field1, itab-field2.
ENDLOOP.

```
- Sonuç olarak:
```ABAP
A 40
B 20
```
