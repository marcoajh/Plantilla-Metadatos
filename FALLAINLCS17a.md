
# Mapa de falla de infraestructura
## 13 abril 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa el nivel de falla de la infraestructura de abastecimiento de agua, se basa en la antigüedad de la infraestructura (antigüedad de la mancha urbana) y la velocidad de subsidencia (cm/año)

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
Los datos de antigüedad del crecimiento de la macha urbana fueron proporcionado por el equipo de urbasnismo a cargo del Mtro. Sergio Flores.    
Los datos de subsidencia porvienen de un estudio contratado por la Subdirección General Técnica, Gerencia de Aguas Subterráneas de la CONAGUA (Contrato: CNA-SGT-GAS-19-2011 "Estudio de impacto por la subsidencia ocasionada por la extracción, uso y manejo de los recursos hídricos en los acuíferos Texcoco y Chalco-Amecameca, Distrito Federal y Estado de México”.) Consultor: Centro GEO (CONACYT) Año: 2011

**Proceso en GRASS**     
Se importa la capa de antigüedad de la mancha urbana    
 ```v.import input=/home/marco/CVM_SIG/Urbano_Crecim_Hist/UTM/Crec_Urb.shp layer=Crec_Urb output=Crec_Urb```   
Se convierte a raster
 ```v.to.rast input=Crec_Urb@Marco output=Crec_Urb_hist use=attr attribute_column=CLAVE_EDAD label_column=EDAD```

Se reclasifica el mapa de AGEB para asignarle a cada AGEB un dato de antigüedad     
```r.stats -lan AGEB_edos,Crec_Urb_hist separator=, > AGEB_ant.csv```  

Se asigna la categoría con mayor extensión para cada AGEB usando una hoja de cálculo, se obtiene un archivo de reclasificación, dicha reclasificación de basa en la ley de Weber-Fechner para siete categorías con factor de progresión = 2

Se reclasifica la capa de AGEB    
```r.reclass i=AGEB_edos o=AGEB_antiguedad rules=/home/marco/CVM_SIG/Shells/ageb_ant.rec --o```      
El comando de reclasificación no reconoce decimales, por ello se efectua la siguiente operación     
```r.mapcalc 'AGEB_antiguedad = AGEB_antiguedad*0.000001' --o```    

Se normaliza la capa de velocidad de subsidencia por AGEB    
```MIN=`r.stats -1n AGEB_subsidencia | gawk 'NR==1 { MIN=$1; next }  $1 < MIN { MIN=$1 } END{ print MIN }'````    

```MAX=`r.stats -1n AGEB_subsidencia | gawk 'NR==1 { MAX=$1; next } $1 > MAX { MAX=$1 } END{ print MAX }'````    

```r.mapcalc "subsid = ((AGEB_subsidencia - "$MIN")/("$MAX" - "$MIN"))" --o```     

Se genera la capa de falla    
```r.mapcalc 'AGEB_falla = (AGEB_antiguedad*0.5)+(subsid*0.5)' --o```     
Se normaliza    
```MIN=`r.stats -1n AGEB_falla | gawk 'NR==1 { MIN=$1; next }  $1 < MIN { MIN=$1 } END{ print MIN }'````    
```MAX=`r.stats -1n AGEB_falla | gawk 'NR==1 { MAX=$1; next } $1 > MAX { MAX=$1 } END{ print MAX }'````   
```r.mapcalc "AGEB_falla = ((AGEB_falla - "$MIN")/("$MAX" - "$MIN"))" --o```   
```r.mapcalc 'FALLAINLCS17a = AGEB_falla'```


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
