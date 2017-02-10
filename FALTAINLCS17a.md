
# Mapa de falta de infraestructura
## 10 febrero 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa el nivel de falta de infraestructura de abastecimiento de agua, se refiere al número de viviendas sin agua entubada en el ámbito de la vivienda en cada AGEB, los datos corresponden al censo de población y vievienda de 2010

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

Falta  
**Proceso en GRASS**  
viviendas sin agua entubada en el ámbito de la viviendas
Se obtuvo de la resta del numero de viviendas habitadas y las viviendas con agua entubada en el ámbito de la viviendas   
Se reclasifica el mapa de AGEB    
```r.reclass i=AGEB_edos o=Viv_saent rules=/home/marco/CVM_SIG/Shells/viv_sin_aent.rec --o```   
Se convierte a float   
```r.mapcalc 'AGEB_falta = float(Viv_saent)' --o```    
Se normaliza   
``` MIN=`r.stats -1n AGEB_falta | gawk 'NR==1 { MIN=$1; next }  $1 < MIN { MIN=$1 } END{ print MIN }'` ```    
``` MAX=`r.stats -1n AGEB_falta | gawk 'NR==1 { MAX=$1; next } $1 > MAX { MAX=$1 } END{ print MAX }'` ```     
``` r.mapcalc "AGEB_falta = (AGEB_falta - "$MIN")/("$MAX" - "$MIN")" --o ```    
``` r.colors map=AGEB_falta color=rainbow```    

Los datos de algunas AGEB superan las 1000 viviendas, alguna llega a 2009, para aumentar la heterogeneidad de la clasificación se clasificaran directamente con el valor 1 las AGEB que tengan más de 750 viviendas sin agua entubada ese valor es el promedio de viviendas por AGEB

```r.mapcalc 'AGEB_falta = float(Viv_saent)' --o```
```r.mapcalc 'AGEB_falta = if(AGEB_falta > 750,1,(AGEB_falta/750))' --o```  
Se relasifican en 5 categorías según la ley de Weber-Fechner   
```r.mapcalc 'AGEB_falta = if(AGEB_falta<=0.0625,0.0625,if(AGEB_falta<=0.125,0.125,if(AGEB_falta<=0.25,0.25,if(AGEB_falta<=0.5,0.5,1.00))))' --o```

```r.colors map=AGEB_falta color=rainbow```
Se crea el mapa
```r.mapcalc 'FALTAINLCS17a = AGEB_falta'```
 

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
