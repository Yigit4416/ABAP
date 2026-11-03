## Class nedir
Class, **veri (attribute) ve davranışı (method)** tek bir yapıda toplayan şablondur. Function group’a benzer ama önemli bir farkla: function group’ta tek bir “instance” vardır, class’tan ise istediğin kadar **nesne (object)** üretebilirsin ve her birinin kendi verisi olur.
- ABAP'ta class 2 yapidan oluşur
	1. DEFINITION
	2. IMPLEMENTATION
```ABAP
CLASS lcl_flight DEFINITION
	PUBLIC SECTION.
		METHODS: constructor IMPORTING iv_carrrid TYPE s_carr_id,
				 get_flights RETURNING VALUE(rt_flight) TYPE tt_flight,
				 get_count RETURNING VALUE(rv_count) TYPE i.
	PRIVATE SECTION.
		DATA: mv_carrid TYPE s_carr_id,
			  mt_flight TYPE tt_flight.
ENDCLASS.

CLASS lcl_flight IMPLEMENTATION
	METHOD constructor.
		mv_carrid = iv_carrid.
	ENDMETHOD
	
	METHOD get_flights.
		SELECT carrid connid fldate price
			FROM sflight
			INTO TABLE mt_flight
			WHERE carrid = mv_carrid. 
		rt_flight = mt_flight. 
	ENDMETHOD.
	
	METHOD get_count.
		rv_count = lines(mt_flight).
	ENDMETHOD.
ENDCLASS.
```

## Görünürlük Bölümleri
| Bölüm               | Kimle Eşleşir                                    |
| ------------------- | ------------------------------------------------ |
| `PUBLIC SECTION`    | Her yerden erişilir                              |
| `PROTECTED SECTION` | Class’ın kendisi ve alt class’ları (inheritance) |
| `PRIVATE SECTION`   | Sadece class’ın kendisi                          |

## Nesne Oluşturma ve Method Çağırma

```ABAP
DATA: go_flight TYPE REF TO lcl_flight
	  gt_flight TYPE tt_flight

START-OF-SELECTION
	go_flight = NEW lcl_flight(iv_carrid = 'LH')
	gt_flight = go_flight->get_flights()
	
	WRITE: / go_flight->get_count()
```

## Instance ve Static üyeler
```ABAP
CLASS lcl_util DEFINITION.
  PUBLIC SECTION.
    CLASS-METHODS add IMPORTING iv_a TYPE i iv_b TYPE i
                      RETURNING VALUE(rv_sum) TYPE i.
ENDCLASS.

CLASS lcl_util IMPLEMENTATION.
  METHOD add.
    rv_sum = iv_a + iv_b.
  ENDMETHOD.
ENDCLASS.

" Nesne oluşturmadan çağrılır
lv_sum = lcl_util=>add( iv_a = 5 iv_b = 3 ).
```

## Error Yönetimi

```ABAP
METHODS get_flights RAISING cx_no_data.

" Çağıran taraf
TRY.
    go_flight->get_flights( ).
  CATCH cx_no_data INTO DATA(lx_error).
    WRITE: / lx_error->get_text( ).
ENDTRY.
```

## Inheritence

```ABAP
CLASS lcl_special_flight DEFINITION INHERITING FROM lcl_flight.
  PUBLIC SECTION.
    METHODS get_count REDEFINITION.
ENDCLASS.
```

## Local vs Global Class

- **Local class** (`lcl_`): Program içinde `CLASS ... DEFINITION` ile yazılır, sadece o programda görünür.
- **Global class** (`zcl_`): SE24 ya da SE80 ile oluşturulur, her yerden kullanılabilir, DDIC’te durur.