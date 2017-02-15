
# Mapa de Abastecimiento
## 15 febrero 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa la poblacion de cada AGEB que no cuenta con agua entubada dentro de la vivienda.

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
Personas en viviendas sin agua entubada en el ámbito de la vivienda
Los datos corresponden al censo de 2010 (INEGI) se normalizaron y se categorizaron en 5 categorías usando la ley de Weber-Fechner

**Proceso en GRASS**  
Usando una hoja de cálculo y a partir de los datos del censo de 2010 (SINCE 2010) se obtuvo de la resta del número de personas en viviendas con agua entubada y la población total de AGEB, con esos datos se genero un archivo de reclasificación.

Se reclasifica la capa de AGEB  
```r.reclass i=AGEB_edos o=Pob_saent rules=/home/marco/CVM_SIG/Shells/pob_sin_aent.rec --o```  

La diferencia entre la población más alta y la más baja es grande (min=0 max=13011) para aumentar la heterogeneidad de la clasificación se clasificaran directamente con el valor 1 las AGEB que tengan más de 2884 habitantes en viviendas sin agua entubada, ese valor es el promedio de habitantes por AGEB  

```r.mapcalc 'AGEB_abast = float(Pob_saent)' --o```  
se normaliza  
```r.mapcalc 'AGEB_abast = if(AGEB_abast > 1884,1,(AGEB_abast/2884))' --o```  
Se reclasifica  
```r.mapcalc 'AGEB_abast = if(AGEB_abast<=0.0625,0.0625,if(AGEB_abast<=0.125,0.125,if(AGEB_abast<=0.25,0.25,if(AGEB_abast<=0.5,0.5,1.00))))' --o```

Se crea la capa de abastecimiento  
```r.mapcalc 'ABASTECLCS17a = AGEB_abast'```


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
