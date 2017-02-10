
# Mapa de Presion de medios
## 10 febrero 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa la presión en medios de comunicación basada en la cantidad de Unidades Económicas en cada AGEB

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

Con datos del sistema SINCE de INEGI, se creó una capa de puntos combinando las unidades económicas de DF, Edo Mex, Hidalgo y Tlaxcala. Con ésta capa y la capa de AGEB, se usó el geoalgoritmo de qgis "Contar puntos en polígono" para obtener un campo con el número total de unidades económicas en cada AGEB. Posteriormente se importó la capa vectorial a GRASS

**Proceso en GRASS**   
Se import la capa
```v.import input=/home/marco/CVM_SIG/AGEB_UE/AGEB_UE.shp layer=AGEB_UE output=AGEB_UE```   
Se convirete a raster  
```v.to.rast --overwrite input=AGEB_UE@Marco output=AGEB_UE use=attr attribute_column=NUMPOINTS```   
```r.mapcalc 'AGEB_UE = float(AGEB_UE)' --o```   

Los datos de las AGEB correspondientes a la Central de abastos y al mercado de la Merced son muy altos, se clasifican directamente con valor de 1 y se usa el valor mayor después de éstos (2565) para normalizar.  
```r.mapcalc 'AGEB_UE = if(AGEB_UE>2565,1,(AGEB_UE/2565))'```  
Se relasifican en 5 categorías según la ley de Weber-Fechner   
```r.mapcalc 'AGEB_UE = if(AGEB_UE<=0.0625,0.0625,if(AGEB_UE<=0.125,0.125,if(AGEB_UE<=0.25,0.25,if(AGEB_UE<=0.5,0.5,1.00))))' --o```   
```r.colors map=AGEB_UE color=rainbow```   
Se crea la capa   
```r.mapcalc 'PRESMEDLCS17a = AGEB_UE'```

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
