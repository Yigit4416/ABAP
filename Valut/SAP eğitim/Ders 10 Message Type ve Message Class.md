## Message Class
- Message class ilişkili mesaj metinlerini bir arada tutan bir kapsayıcıdır. SE91 ile oluşturulur. Her message class'ın içinde 000-999 arası numaralanmış mesajlar bulunur.
- Mesaj kodun içinde sabit yazmak yerine message class'ta tutmanın avantajları:
	- Metin tek yerden değiştirilir.
	- Çoklu dil desteği.
	- `&` ile parametre kullanılabilir (&1, &2, &3).
	- Where-used list ile hangi mesajın nerede kullanıldığı izlenir.

Örneğin SE91'de `ZFLIGHT` message class'ı:

| No  | Metin                               |
| --- | ----------------------------------- |
| 001 | Havayolu kodu boş olamaz            |
| 002 | `&1` havayolu için kayıt bulunamadı |
| 003 | `&1` kayıt başarıyla kaydedildi     |
## Message Type
- Message type mesajın ne tür bir mesaj olduğunu ve programın akışını nasıl etkileceğini belirleyen tek harflik koddur.

| Tip   | Anlamı      | Davranış                                                                                                     |
| ----- | ----------- | ------------------------------------------------------------------------------------------------------------ |
| **S** | Success     | Bir sonraki ekranın alt kısmında (status bar) gösterilir, akış devam eder                                    |
| **I** | Information | Popup açılır, kullanıcı onaylayınca akış devam eder                                                          |
| **W** | Warning     | Status bar’da gösterilir, kullanıcı Enter ile devam edebilir (selection screen’de alan tekrar girişe açılır) |
| **E** | Error       | Status bar’da gösterilir, **akış durur**, kullanıcı düzeltmek zorundadır (davranış bağlama göre değişir)     |
| **A** | Abort       | Popup açılır, **işlem sonlandırılır** (transaction kapanır)                                                  |
| **X** | Exit        | Kısa dump (runtime error) üretir, genelde debug için                                                         |
> E tipi her yerde aynı davranmaz.
> `AT SELECTION-SCREEN` içinde selection screen geri döner, alanlar düzeltilebilir.
> `START-OF-SELECTION` içinde program sonlandırılır.
> Dynopro PAI'da ilgili alan(lar) girişe açılır.

## Mesajları kodda kullanma
```ABAP
REPORT zdemo_msg MESSAGE-ID zflight.

PARAMETERS p_carr TYPE s_carr_id.

AT SELECTION-SCREEN.
	IF p_carr IS INITIAL.
		MESSAGE e001. " Tip E, mesaj no 001
	ENDIF.

START-OF-SELECTION.
	SELECT * FROM sflight INTO TABLE @DATA(lt_flight)
		WHERE carrid = @p_carr.
	
	IF sy-subrc <> 0.
		MESSAGE e002 WITH p_carr.
	ENDIF.
	
	MESSAGE s003 WITH lines(lt_flight).
```
- `MESSAGE-ID zflight`: Programın varsayılan message class’ını belirler.
- `MESSAGE e001`: **Tip + numara** birleşik yazılır (`e` + `001`).
- `WITH`: `&1`, `&2` yerine geçecek değerler (en fazla 4 tane).

### MESSAGE-ID kullanmadan:
```ABAP
MESSAGE e001(zflight). " MESSAGE-ID kullanmadan
MESSAGE-ID 'Z_FLIGHT' TYPE 'E' NUMBER 001. " Dinamik
```

### Message Class olmadan
```ABAP
MESSAGE 'Bir hata oluştu' TYPE 'E'.
MESSAGE |{lv_count} kayıt bulundu| TYPE 'S'.
```
Hızlı ve pratik ama çoklu dil desteği yoktur, metin kodun içine gömülür. Deneme için uygundur, üretim kodunda message class tercih edilir.

### Tip değiştirme: DISPLAY LIKE
```ABAP
MESSAGE s002 WITH p_carr DISPLAY LIKE 'E'.
```

Mesaj `s` gibi davranir ve akışı; durmaz ama E gibi gorunur. Hata gostermek ancak akisi durdurmamak icin kullanilir.
## Class/method içinde mesaj
Class içinde `MESSAGE-ID` yerine class tanımında balirtirsin
```ABAP
CLASS lcl_demo DEFINITION MESSAGE-ID zflight.
	PUBLIC SECTION.
		METHODS check IMPORTING iv_carrid TYPE s_carr_id.
ENDCLASS.

CLASS lcl_demo IMPLEMENTATION.
	METHOD check.
		IF iv_carrid IS INITIAL.
			MESSAGE e001.
		ENDIF.
	ENDMETHOD.
ENDCLASS	
```

## SY alanları
Mesaj gösterilince sistem şu alanları doldurur:

| Alan                      | İçerik              |
| ------------------------- | ------------------- |
| `sy-msgid`                | Message class       |
| `sy-msgty`                | Message type        |
| `sy-msgno`                | Mesaj numarası      |
| `sy-msgv1` ... `sy-msgv4` | Parametre değerleri |
Bunlar FM'lerin `RAISE ... MESSAGE` yapısında işe yarar:
```ABAP
CALL FUNCTION 'Z_GET_FLIGHTS'
	EXPORTING ...
	EXCEPTIONS
		OTHERS = 1.

IF sy-subrc <> 0.
	MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
	WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
ENDIF.
```
- Bu kalıp FM içinde fırlatılan mesajı olduğu gibi çağıran tarafta yeniden göstermek için çok kullanılır.
- FM içinde çıkan hata FM ekrana basamdığı için gösterilemez ama bununla ekrana basılır. (FM ekrana basabilir ama basmamyı seçiyor onu unutma)