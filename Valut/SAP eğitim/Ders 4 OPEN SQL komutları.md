- Struct dediğimiz şey satırdır. Yani tek data tutar
- Variable dediğimiz şey sütundur.
- Tablo birden fazla structın tutulduğu yapıdır.
- gv'deki v variable yerine geçer.
- gs'deki s structure yerine geçer.

```ABAP
* zbk_persi_de data elementi temsil eder.
* Alttakiler seçtiğimiz tabledeki data elementlerdir.
DATA: gv_persid TYPE zbk_persi_de,
      gv_persad TYPE zbk_persad_de,
      gv_perssoyad TYPE zbk_perssoyad_de,
      gv_perscins TYPE zbk_perscins_de,
      "Alttaki çekmek istediğimiz table onun altındaki de internal table
      gv_pers_t TYPE zbk_pers_t,
      gt_pers_t TYPE TABLE OF zbk_pers_t.
```

### Öğrenilecek şeyler
- SELECT
- UPDATE
- INSERT
- MODIFY

- Tablo çekmek istersek:
```ABAP
* Eleman seçmek
SELECT * FROM zbk_pers_t 
	INTO TABLE gt_pers_t.
```

- Tablodan yalnızca tek eleman çekmek istersen:
```ABAP
SELECT SINGLE banfn
FROM eban
WHERE bsart EQ 'NB'
INTO @gv_banfn.
```

- Struct çekmek istersek:
```ABAP
SELECT SINGLE * FROM zbk_per_t 
	INTO gs_pers_t.
```

- Bir kolon çekmek istersek:
```ABAP
SELECT gs_persid_ed FROM zbk_per_t
	INTO gv_persid.
```

- Tablodan sadece belirli elemanları seçmek:
```ABAP
SELECT * FROM zbk_pers_t 
	INTO TABLE gt_pers_t
	WHERE gs_perid EQ 1.
```

- Tabloyu güncelleme:
```ABAP
UPDATE zbk_pers_t SET pers_ad = 'HAKAN'
	WHERE pers_id EQ 1.
```

- Eleman ekleme:
```ABAP
gs_pers_t-pers_id = 1.
gs_pers_t-pers_ad = 'yiğit'.
gs_pers_t-pers_soyad = 'özdemir'.
gs_pers_t-pers_cins = 'E'.
INSERT zbk_pers_t FROM gs_pers_t.
```

- Eleman silme:
```ABAP
DELETE FROM zbk_pers_t
	WHERE pers_id EQ 1.
```

- Eleman düzenleme:
- Burada belirlenen key'de istenen eleman varsa elemanı değiştirir yoksa insert mantığıyla o elemanı ekler.

```ABAP
gs_pers_t-pers_id = 2.
gs_pers_t-pers_ad = 'yiğit'.
gs_pers_t-pers_soyad = 'özdemir'.
gs_pers_t-pers_cins = 'E'.
MODIFY zbk_pers_t FROM gs_pers_t
```