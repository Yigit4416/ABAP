- Başlangıç INCLUDEleri:
```ABAP
INCLUDE zint_yo_top. " Değişkenleri tanımlarız
INCLUDE zint_yo_pbo. " Screenlerin açılmadan önce modüllerinin toplandığı yerdir  
INCLUDE zint_yo_pai. " Oluşturduktan sonra ekranların yakalnadığı yer
INCLUDE zint_yo_frm. " Formları oluşturudğumuz yapı
```

- Şimdi bir 'screen'e ihtiyacımız var. Bunun için Programa sağ tık yapıp Create bölümünden Screen seçeneği seçer ve screen numbere 0100 gireriz.
- Sonrasında Screen'de Flow Logic bölümüne girer orada da her kısmı uncomment eder ve orada görünün modülleri çift tıklayarak oluştururuz.
- STATUS'lu kısmı PBO'de oluştururuz.
- USER_COMMAND'li kısmı PAI'da oluştururuz.
- PBO'da da uncommentlersin sonrasında PF-STATUS ve SET TITLEBAR ister.
- bunlar için yine Create bölümüne girer bunların yine GUI status ve GUI title kısımlarını oluşturusun.

- SSonrasında PAI'ye gelirsin ve oluşturduğun buton fonksiyonlarını yazmaya başlarsın mesela ben sadece '&BACK' yaptım o zaman şöyle yazmamız lazım.
```ABAP
MODULE user_command_0100 INPUT.  
  CASE sy-ucomm.  
    WHEN '&BACK'.  
      SET SCREEN 0. " Ekrandan çıkmamızı sağlar.
    ENDCASE.  
ENDMODULE.
```
- Senin yazdığın sadece CASE kısmı geri kalanı otomatik oluşuyor.
- Bu şekilde direkt programı çalıştırmaya çalışırsan bir şey çıkmaz bunun için istediğimiz ekranı START-OF-SELECTION'dan CALL etmemiz lazım bunun için de şunu yazarı:
```ABAP
START-OF-SELECTION.  
  
CALL SCREEN 0100.
```

---

- Kolon ve satırları oluşturmak içn ve onlara özellikler atamak için FRM include programımıza şunları atabiliriz:
```ABAP
FORM set_fcat .  
CLEAR: gs_fcat.  
gs_fcat-fieldname = 'CARRID'.  
gs_fcat-scrtext_s = 'Havayolu T.'.  
gs_fcat-scrtext_m = 'Havayolu Tanımı'.  
gs_fcat-scrtext_l = 'Havayolu Şirketi Tanımı'.  
gs_fcat-col_pos   = 4.  
gs_fcat-key       = abap_true.  
APPEND gs_fcat TO gt_fcat.  
  
CLEAR: gs_fcat.  
gs_fcat-fieldname = 'CARRNAME'.  
gs_fcat-scrtext_s = 'Havayolu A.'.  
gs_fcat-scrtext_m = 'Havayolu Şirketi A.'.  
gs_fcat-scrtext_l = 'Havayolu Şirketinin Adı'.  
gs_fcat-col_pos   = 2.  
gs_fcat-edit      = abap_true.  
APPEND gs_fcat TO gt_fcat.  
  
CLEAR: gs_fcat.  
gs_fcat-fieldname = 'CURRCODE'.  
gs_fcat-scrtext_s = 'H. P. B.'.  
gs_fcat-scrtext_m = 'Havayolu Şirketi P. B.'.  
gs_fcat-scrtext_l = 'Havayolu Şirketinin Ulusal Para Birimi'.  
gs_fcat-col_pos   = 3.  
gs_fcat-hotspot   = abap_true.  
APPEND gs_fcat TO gt_fcat.  
  
CLEAR: gs_fcat.  
gs_fcat-fieldname = 'URL'.  
gs_fcat-scrtext_s = 'HY URL'.  
gs_fcat-scrtext_m = 'Havayolu URL'.  
gs_fcat-scrtext_l = 'Havayolu Şirketi URLsi'.  
gs_fcat-col_opt   = abap_true.  
gs_fcat-col_pos   = 1.  
*gs_fcat-ref_table = 'SCARR'.  
*gs_fcat-ref_field = 'URL'.  
* Üstteki 2 satır sayesinde bu alanın gerçekte olması gerektiği şekilde kolonunu ayarlıyor. Mesela SCARR tablesinden  
* URL fieldının domaini ile ilgili her şeyi çek diyor adeta.  
*gs_fcat-no_out    = abap_true. " You can dissapear this colon  
*gs_fcat-outputlen = 100. " How many carachters do you want?  
APPEND gs_fcat TO gt_fcat.  
ENDFORM.
```
- Tabi bunu ana programda da yapabilirsin ama böyle daha derli toplu oluyor (PERFORM şeklinde oluşturup FRM dosyasına atarak).

- Bunun daha kısa yolu şudur (Tek kod bloğuna atamak):
```ABAP
CALL FUNCTION 'LVC_FIELDCATALOG_MERGE'  
 EXPORTING  
   I_STRUCTURE_NAME             = 'SCARR'  
*   I_INTERNAL_TABNAME           =  
  CHANGING  
    ct_fieldcat                  = gt_fcat  
 EXCEPTIONS  
   INCONSISTENT_INTERFACE       = 1  
   PROGRAM_ERROR                = 2  
   OTHERS                       = 3  
          .
```
- Burada bütün kolonlar için geçerli olacak değişiklikleri yapabilirsin.
- İlla SCAR tablosuyla aynı olmak zorunda değil SCARR tablosuyla aynı structureyi oluşturan bir structure yap (MANDT'siz yaptım ben) sonrasında bunu SCARR yazan yerin yerine koy kod aşağıda:
```ABAP
CALL FUNCTION 'LVC_FIELDCATALOG_MERGE'  
 EXPORTING  
*   I_STRUCTURE_NAME             = 'SCARR'  
   I_STRUCTURE_NAME             = 'ZINT_YO_S_OO_ALV'  
*   I_INTERNAL_TABNAME           =  
  CHANGING  
    ct_fieldcat                  = gt_fcat  
 EXCEPTIONS  
   INCONSISTENT_INTERFACE       = 1  
   PROGRAM_ERROR                = 2  
   OTHERS                       = 3
```

---
- Kolon rengi belirleme:
	- ![[Pasted image 20240801100049.png]]
- Yukarıdaki mantığa göre doldurulur.
- Kodu:
```ABAP

```