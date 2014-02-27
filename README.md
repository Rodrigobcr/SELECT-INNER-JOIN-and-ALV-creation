SELECT INNER JOIN and ALV creation.
==========================

This ABAP program was created for teaching purposes. His explanation are into this video:

<a href="http://www.youtube.com/watch?v=qqQZsC0npgk&feature=c4-overview&list=UU1m92lTepCpEYDu08QYCSrw" target="_blank">Using SELECT INNER JOIN and showing results in ALV</a>




Installation
------------

Copy the coding and paste to your ABAP environment.



FOLLOW CODING BELLOW:

<div class><pre>

TYPES:
 BEGIN OF ty_flight,
  carrid   TYPE sflight-carrid,
  connid   TYPE sflight-connid,
  fldate   TYPE sflight-fldate,
  price    TYPE sflight-price,
  cityfrom TYPE spfli-cityfrom,
  cityto   TYPE  spfli-cityto,
  airpfrom TYPE spfli-airpfrom,
  airpto   TYPE spfli-airpto,
END OF ty_flight.

DATA it_flight TYPE TABLE OF ty_flight.

SELECTION-SCREEN BEGIN OF BLOCK B1 WITH FRAME TITLE TEXT-001.
PARAMETERS p_carrid TYPE sflight-carrid.
PARAMETERS p_connid TYPE sflight-connid.
SELECTION-SCREEN END OF BLOCK B1.


IF p_connid IS NOT INITIAL.
SELECT a~carrid a~connid a~fldate a~price b~cityfrom b~cityto b~airpfrom b~airpto
  INTO TABLE it_flight
  FROM sflight AS a INNER JOIN spfli AS b
  ON a~carrid = b~carrid AND a~connid = b~connid
  WHERE a~carrid = p_carrid AND a~connid = p_connid.

  ELSE.
    SELECT a~carrid a~connid a~fldate a~price b~cityfrom b~cityto b~airpfrom b~airpto
  INTO TABLE it_flight
  FROM sflight AS a INNER JOIN spfli AS b
  ON a~carrid = b~carrid
  WHERE a~carrid = p_carrid.


ENDIF.


* """If you want show results in loop, you must create a work area""""
*-----------------------------------------SHOW IN LOOP-------------------------------------------------

*  LOOP AT it_flight INTO wa_flight.
*    WRITE: / 'Resultado',
*     'Companhia:',            wa_flight-carrid,
*     'Código do voo:',        wa_flight-connid,
*     'Preço:',                wa_flight-price,
*     'Cidade de partida:',    wa_flight-cityfrom,
*     'Cidade de destino',     wa_flight-cityto,
*     'Aeroporto de partida:', wa_flight-airpfrom,
*     'Aeroporto de destino:', wa_flight-airpto.
*
*  ENDLOOP.



*  ----------------------------------SHOW IN ALV-----------------------------------------

  DATA r_grid TYPE REF TO cl_gui_alv_grid.
  DATA: wa_fieldcat TYPE slis_fieldcat_alv,
        wa_layout   TYPE slis_layout_alv,
        it_fieldcat TYPE slis_t_fieldcat_alv.

  PERFORM MONTAR_CAMPOS.
  PERFORM MONTAR_LAYOUT.
  PERFORM EXIBIR_ALV.


  FORM MONTAR_CAMPOS.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'CARRID'.
    wa_fieldcat-tabname   = 'SFLIGHT'.
    wa_fieldcat-seltext_m = 'Companhia'.
    APPEND wa_fieldcat TO it_fieldcat.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'CONNID'.
    wa_fieldcat-tabname   = 'SFLIGHT'.
    wa_fieldcat-seltext_m = 'Cod. do Voo'.
    APPEND wa_fieldcat TO it_fieldcat.

     CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'FLDATE'.
    wa_fieldcat-tabname   = 'SFLIGHT'.
    wa_fieldcat-seltext_m = 'Data do voo'.
    APPEND wa_fieldcat TO it_fieldcat.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'PRICE'.
    wa_fieldcat-tabname   = 'SFLIGHT'.
    wa_fieldcat-seltext_m = 'Preço'.
    APPEND wa_fieldcat TO it_fieldcat.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'CITYFROM'.
    wa_fieldcat-tabname   = 'SPFLI'.
    wa_fieldcat-seltext_m = 'Cidade de partida'.
    APPEND wa_fieldcat TO it_fieldcat.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'CITYTO'.
    wa_fieldcat-tabname   = 'SPFLI'.
    wa_fieldcat-seltext_m = 'Cidade de destino'.
    APPEND wa_fieldcat TO it_fieldcat.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'AIRPFROM'.
    wa_fieldcat-tabname   = 'SPFLI'.
    wa_fieldcat-seltext_m = 'Aeroporto de partida'.
    APPEND wa_fieldcat TO it_fieldcat.

    CLEAR wa_fieldcat.
    wa_fieldcat-fieldname = 'AIRPTO'.
    wa_fieldcat-tabname   = 'SPFLI'.
    wa_fieldcat-seltext_m = 'Aeroporto de destino'.
    APPEND wa_fieldcat TO it_fieldcat.


ENDFORM.


 FORM MONTAR_LAYOUT.
wa_layout-zebra = 'X'.
wa_layout-colwidth_optimize = 'X'.
ENDFORM.


FORM EXIBIR_ALV.

  CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
   EXPORTING
     I_CALLBACK_PROGRAM                = SY-REPID
     I_GRID_TITLE                      = 'Dados dos Voos'
     IS_LAYOUT                         = wa_layout
     IT_FIELDCAT                       = it_fieldcat
    TABLES
      t_outtab                          = it_flight.


ENDFORM.
</pre></div>
___________

