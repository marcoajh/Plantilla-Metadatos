
# Mapa de escasez de agua
## 10 febrero 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa el nivel de escasez de agua (horas de entrega / semana) a nivel de AGEB.

######  Nivel de avance:
Aún en procesamiento

#### Ubicación geográfica.
###### Área geográfica:
Cuenca del valle de México


###### Coordenadas extremas:

xMín 348621.68    
yMín 2054434.61    
xMáx 644398.35    
yMáx 2344228.00    

#### Calidad de los datos.
###### Metodología:
Los datos de tandeo se obtuvo de los planes de ¿?, dichos datos se convirtieron a horas de servicio por semana, se normalizaron y se categorizaron en 5 categorías usando la ley de Weber-Fechner

**Proceso en GRASS**
Se usa como proxy el tandeo, es un dato por colonia expresado en horas de agua por semana
se obtiene la tabla de recasificación    
```r.stats -ln AGEB_edos,tandeo |
gawk '{printf("%i %s %i\n", $1, "=", $3)}' > /home/marco/CVM_SIG/Shells/ageb_tand.rec```    
Se rclasifica la capa de AGEB    
```r.reclass i=AGEB_edos o=AGEB_tand rules=/home/marco/CVM_SIG/Shells/ageb_tand.rec --o```    
Se convirte a float   
```r.mapcalc 'AGEB_tand = float(AGEB_tand)' --o```

Se normaliza   
```MIN=`r.stats -1n AGEB_tand | gawk 'NR==1 { MIN=$1; next }  $1 < MIN { MIN=$1 } END{ print MIN }'´```         
```MAX=`r.stats -1n AGEB_tand | gawk 'NR==1 { MAX=$1; next } $1 > MAX { MAX=$1 } END{ print MAX }'´ ```

```r.mapcalc "AGEB_tand = (AGEB_tand - "$MIN")/("$MAX" - "$MIN")" --o```   
El concepto es inverso, la escasez es mayor cuando tienen menos horas de agua
```r.mapcalc 'AGEB_tand =  1 - AGEB_tand' --o```   
Se reclasifica    
```r.mapcalc 'AGEB_tand = if(AGEB_tand<=0.0625,0.0625,if(AGEB_tand<=0.125,0.125,if(AGEB_tand<=0.25,0.25,if(AGEB_tand<=0.5,0.5,1.00))))' --o```     
Se aplica paleta de color     
```r.colors map=AGEB_tand color=rainbow```     

```r.mapcalc 'ESCASEZLCS17a = AGEB_tand'```

#### Datos espaciales.
###### Estructura del dato:
Vector

###### Tipo de datos:
Polígonos

#### Proyección geográfica.
###### Sistema de coordenadas:
Universal Transversa de Mercator

###### Proyección:
WGS 84 / UTM zone 14N

###### Paralelos estándar:
Greenwich

###### Esferoide:
--

###### Datum:
World Geodetic System 1984

###### Elipsoide:
WGS84

#### Atributos del mapa.

 Nombre del campo | CVEGEO
------------ | -------------
Tipo | Texto
Unidades | --
Descripción | Clave alfanumérica para cada AGEB, definida por el INEGI
Observaciones | --

Nombre del campo | AGEB_ID
------------ | -------------
Tipo | Entero
Unidades | --
Descripción | Identificador numérico para cada AGEB
Observaciones | Texto

Nombre del campo | VALUE
------------ | -------------
Tipo | Real
Unidades | --
Descripción | Función de valor del atributo
Observaciones | --
