- Başlangıç basit kısımları anlatmıyorum onları zaten biliyorsun ve öbür taraflarda notlar bulunuyor.
- Şimdi ilk olarak:
	- Class includesine giriyorsun ve orada classları tanımlamaya başlıyorsun.
	- Oluşturman gereken metodlar şunlar:
		- get_data            
		- fill_fiealdcatalog  
		- set_layout          
		- display_data

```ABAP
METHOD get_data.  
    free: gt_alv.  
      
    SELECT vbeln,  
           posnr,  
           matnr,   
           erdat,  
           erzet,  
           ernam  
      from vbap  
      into table gt_alv.  
  
  ENDMETHOD.
```

```ABAP

```