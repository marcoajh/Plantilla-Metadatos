
# Mapa de Peticion de delegaciones
## 10 febrero 2017


#### Datos generales del mapa.
###### Cita oficial:
Texto aquí

###### Resumen:
En este mapa se expresa la importancia de las peticiones de servicio o infraestructura de las Delegaciones a SACMEX, basados en el nivel socioeconómico de las AGEB

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

El equipo de urbanismo a cargo del Mtro.  Sergio Flores envió una capa de nivel socioeconomico para usarse com o proxy de peticion delegaciones. Sólo contiene datos para Ciudad de México (DF)  
Se asignó un valor numérico a cada categoría (originalmente eran letras)  

**Proceso en GRASS**  

Se importa la capa vectorial a GRASS  
```v.import input=/home/marco/CVM_SIG/Niv_sociec_AGEB/UTM/Niv_SocioEc_AGEB.shp layer=Niv_SocioEc_AGEB output=Niv_SocioEc_AGEB```  
Se convierte a ráster
Se usan los datos mas recientes (2008)  
```v.to.rast --overwrite input=Niv_SocioEc_AGEB@Marco output=Niv_SE_AGEB_08 use=attr attribute_column=BIM_08 label_column=BIMSA2008```

Se asignan valores según Weber-Fechner para seis categorías con factor de progresión = 2 (x 100000 porque r.reclass no trabaja con decimales)   
```r.reclass i=Niv_SE_AGEB_08 o=Niv_SE_AGEB_08r rules=/home/marco/CVM_SIG/Shells/niv_SE_ageb.rec --o```

Se crea la tabla de reclasdificación para AGEBs  
```r.stats -ln AGEB_edos,Niv_SE_AGEB_08r |
gawk '{printf("%i %s %i\n", $1, "=", $3)}' > /home/marco/CVM_SIG/Shells/AGEB_niv_SE.rec```  
Se reclasifica  
```r.reclass i=AGEB_edos o=AGEB_niv_SE rules=/home/marco/CVM_SIG/Shells/AGEB_niv_SE.rec --o```  
Se convirete a decimal   
```r.mapcalc 'AGEB_niv_S E= AGEB_niv_SE*0.000001' --o```   
```r.colors map=AGEB_niv_SE color=rainbow```   
Se crea el mapa  
```r.mapcalc 'PETDELSLCS17a = AGEB_niv_SE'```   


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
