
# Mapa de Subsidencia
## 10 febrero 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa la presencia del fenómeno de subsidencia a nivel de AGEB

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

Se usó la capa de Hundimientos, 1996. Instituto Nacional de Ecología, SEMARNAT - Instituto de Geografía, UNAM. escala 1:1,000,000

**Proceso en GRASS**    

Se importa la capa a GRASS   
```v.import input=/home/marco/CVM_SIG/Hundimientos/INE03_VA1M.shp layer=INE03_VA1M output=hundimientos```   
Se convierte a raster   
```v.to.rast input=hundimientos output=hundimientos use=attr attribute_column=HUNDIM_NUM label_column=HUNDIM_LEY```  
Se crea una capa binaria   
```r.mapcalc 'hund = if(hundimientos==0,0 1)'```   
Se obtiene una tabla de reclasificación binaria señalando en cuales AGEB hay subsidencia y en cuales no la hay.   
```r.stats -lan AGEB_edos,hund |
gawk '{printf("%i %s %i\n", $1, "=", $3)}' > /home/marco/CVM_SIG/Shells/ageb_hund.rec ```  
Se reclasifica la capa de AGEB   
```r.reclass i=AGEB_edos o=AGEB_hund rules=/home/marco/CVM_SIG/Shells/ageb_hund.rec --o```   
Se convierte a float   
```r.mapcalc 'AGEB_hund = float(AGEB_hund)' --o```   
Se crea la capa   
```r.mapcalc 'SUBSIDELCS17a = AGEB_hund'```


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
