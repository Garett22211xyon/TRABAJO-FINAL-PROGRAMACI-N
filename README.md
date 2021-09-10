# ÍNDICES BIOCLIMÁTICOS Y ESPECTRALES DE LA PROVINCIA DE PISCO

**ALUMNOS** |  **CÓDIGO** | 
----------------------| ----------------------|
LIMAS CERNA ANTONIO JESUS | 19160180 
MALLQUI GUZMÁN ROCÍO ZAYDA | 19160169
VILCA TORRES DIEGO RENZO | 19160038


## LIBRERÍAS QUE SE USARON

```library(raster)```: Leer, escribir, manipular, analizar y modelar datos espaciales. El paquete implementa funciones básicas y de alto nivel para datos ráster y para operaciones de datos vectoriales como intersecciones.

```library(tmap)```: Los mapas temáticos son mapas geográficos en los que se visualizan distribuciones de datos espaciales. Este paquete ofrece un enfoque flexible, basado en capas y fácil de usar para crear mapas temáticos, como coropletas y mapas de burbujas.

```library(rcartocolor)```: Proporciona esquemas de color para mapas y otros gráficos diseñados por 'CARTO' como se describe en [https://carto.com/carto-colors/]
. Incluye cuatro tipos de paletas: agregación, divergente, cualitativa y cuantitativa.

```library(sf)```: Soporte para funciones simples, una forma estandarizada de codificar datos vectoriales espaciales. Se une a 'GDAL' para leer y escribir datos, a 'GEOS' para operaciones geométricas y a 'PROJ' para conversiones de proyección y transformaciones de datum. Utiliza por defecto el paquete 's2' para operaciones de geometría esférica en coordenadas elipsoidales (largo / latitud).

```library(rgdal)```: Proporciona enlaces a la biblioteca de abstracción de datos 'geoespacial' ('GDAL') (> = 1.11.4) y acceso a operaciones de proyección / transformación desde la biblioteca 'PROJ'.

```library(dplyr)```: Una herramienta rápida y consistente para trabajar con marcos de datos como objetos, tanto en memoria como fuera de ella.

```library(RColorBrewer)```: Proporciona esquemas de color para mapas (y otros gráficos) diseñados por Cynthia Brewer como se describe en [http://colorbrewer2.org]

```library(tidyverse)```: El 'tidyverse' es un conjunto de paquetes que funcionan en armonía porque comparten representaciones de datos comunes y diseño de 'API'. Este paquete está diseñado para facilitar la instalación y la carga de varios paquetes 'tidyverse' en un solo paso. Obtenga más información sobre el 'tidyverse' en [https://www.tidyverse.org].

```library(pacman)```: Herramientas para realizar de manera más conveniente las tareas asociadas con los paquetes complementarios. pacman envuelve convenientemente las funciones relacionadas con la biblioteca y el paquete y las nombra de una manera intuitiva y coherente. Busca combinar la funcionalidad de funciones de nivel inferior que pueden acelerar el flujo de trabajo.

```library(ggplot2)```: Un sistema para crear gráficos 'declarativamente', basado en "La gramática de los gráficos". Usted proporciona los datos, le dice a 'ggplot2' cómo asignar variables a la estética, qué primitivas gráficas usar y se encarga de los detalles. 


### ÍNDICE DE ARIDEZ DE MARTONNE DE LA PROVINCIA DE PISCO

**Permite una primera identificación fitoclimática del mundo, aunque es especialmente
efectivo en zonas tropicales y subtropicales.**

-  Cargando raster de precipitación
```
grids <- list.files('C:/Users/USUARIO/Documents/TAREA PROGRAMACION/DATOS/2014-2018 PRECIPITACION/2014', full.names = TRUE, pattern = "*.tif")
```
-  Raster stack
```
prp_stack <- stack(grids)
```
-  Suma de precipitación anual
```{raster}
prp_anual <- sum(prp_stack)
plot(prp_anual)
```
-  Para la provincia de Pisco
```
prov <- readOGR("E:/TAREA BIOGEO/Materiales/PROVINCIAS.shp")
plot(prov)

pisco <- prov[prov$PROVINCIA == "PISCO",]
plot(pisco)
```
- Guardando el raster
```
writeRaster(prp_anual_pisco,filename = "Precipitacion anual pisco 2014",format = "GTiff",overwrite =TRUE)
```
-  Hallando la temperatura minima anual
```
tmn <- list.files('C:/Users/USUARIO/Documents/TAREA PROGRAMACION/DATOS/2014-2018tmax/2014', full.names = TRUE, pattern = "*.tif")
tmin_stack <- stack(tmn)
tmin_anual <- sum(tmin_stack)
tmin_anual_prom <- tmin_anual/10
tmin_prom_anual_pisco <- crop(tmin_anual_prom,pisco)
plot(tmin_prom_anual_pisco)
plot(pisco, add = TRUE)
writeRaster(tmin_prom_anual_pisco,filename = "Temperatura promedio minima 2014",format = "GTiff",overwrite = TRUE)
```
- Hallando la temperatura maxima anual
```
tmax <- list.files('C:/Users/USUARIO/Documents/TAREA PROGRAMACION/DATOS/2014-2018tmax/2014', full.names = TRUE, pattern = "*.tif")
tmax_stack <- stack(tmax)
tmax_anual <- sum(tmax_stack)
tmax_anual_prom <- tmax_anual /10
tmax_prom_anual_pisco <- crop(tmax_anual_prom ,pisco)
plot(tmax_prom_anual_pisco)
plot(pisco, add = TRUE)
writeRaster(tmax_prom_anual_pisco,filename = "Temperatura promedio maxima 2014",format = "GTiff",overwrite = TRUE)
```
- Hallando la temperatura media anual
```{raster}
tmd <- (tmin_prom_anual_pisco + tmax_prom_anual_pisco )/2
```
- Realizando las operaciones correspondientes
```
IA_2014 <- prp_anual_pisco/(tmd + 10)
plot(IA_2014, main = "Indice de Aridez de Martonne 2014")
plot(pisco, add = TRUE)
```
- Cortando
```
IA_2014_f <- mask(IA_2014,pisco)
plot(IA_2014_f, main = "Indice de Aridez de Martonne 2014")
plot(pisco, add = TRUE)
```
- Procedemos a guardar en formato tiff
```
writeRaster(IA_2014,filename = "Indice de Aridez de Martonne",format = "GTiff",overwrite = TRUE)
```

### ÍNDICE DE DANTIN-REVENGA DE LA PROVINCIA DE PISCO

**Este índice permite calificar el ambiente fitoclimático y se basa en la precipitación y la
temperatura.**

-  Cargando raster de precipitación
```
grids <- list.files('D:/TRABAJO FINALPROGRAMACIÓN/datos/PRECIPITACION 2016', full.names = TRUE, pattern = "*.tif")
```
-  Raster stack
```
prp_stack <- stack(grids)
```
-  Suma de precipitación anual
```
prp_anual <- sum(prp_stack)
plot (prp_anual)
```
-  Para la provincia de Pisco
```
prov <- readOGR("D:/TRABAJO FINALPROGRAMACIÓN/MATERIALES/PROVINCIAS.shp")
plot(prov)

pisco <- prov[prov$PROVINCIA == "PISCO",]
plot(pisco)
```
- Cortando la provincia de Pisco
```
prp_anual_pisco <- crop(prp_anual,pisco)
plot(prp_anual_pisco)
plot(pisco, add = TRUE)
```
- Guardando el raster
```
writeRaster(prp_anual_pisco,filename = "Precipitacion anual pisco 2016",format = "GTiff",overwrite = TRUE)
```
-  Hallando la temperatura minima anual
```
tmn <- list.files('D:/TRABAJO FINALPROGRAMACIÓN/datos/T° MINIMA 2016/2016', full.names = TRUE, pattern = "*.tif")
tmin_stack <- stack(tmn)
tmin_anual <- sum(tmin_stack)
tmin_anual_prom <- tmin_anual/12
tmin_prom_anual_pisco <- crop(tmin_anual_prom,pisco)
plot(tmin_prom_anual_pisco)
plot(pisco, add = TRUE)
writeRaster(tmin_prom_anual_pisco,filename = "Temperatura promedio minima 2016",format = "GTiff",overwrite = TRUE)
```
- Hallando la temperatura maxima anual
```
tmax <- list.files('D:/TRABAJO FINALPROGRAMACIÓN/datos/T° MAXIMA 2016/2016', full.names = TRUE, pattern = "*.tif")
tmax_stack <- stack(tmax)
tmax_anual <- sum(tmax_stack)
tmax_anual_prom <- tmax_anual /12
tmax_prom_anual_pisco <- crop(tmax_anual_prom ,pisco)
plot(tmax_prom_anual_pisco)
plot(pisco, add = TRUE)
writeRaster(tmax_prom_anual_pisco,filename = "Temperatura promedio maxima 2016",format = "GTiff",overwrite = TRUE)
```
- Hallando la temperatura media anual
```
tmd <- (tmin_prom_anual_pisco + tmax_prom_anual_pisco )/2
writeRaster(tmd,filename = "Temperatura promedio anual 2016",format = "GTiff",overwrite = TRUE)
```
- Ploteando el raster
```
danreven <- (tmd/prp_anual_pisco)*100
plot(danreven)
plot(pisco, add = TRUE)
```

### ÍNDICE DE VEGETACIÓN DIFERENCIADA NORMALIZADA DE LA PROVINCIA DE PIURA (NDVI)

**Se suponía que era de Pisco, pero hubo una confusion con Piura profesor, disculpe. Pero los comandos como tal funcionan para cualquier departarmento, la cuestión es descargar la data :")**

- Descomprimiendo archivos LANDSAT
```
ZIPfile1 <- 'LC08_L1TP_011064_20161229_20170314_01_T1.tar.gz'
untar(ZIPfile1, exdir = ZIPfile1 %>% strsplit('.tar') %>% sapply('[',1))

ZIPfile2 <- 'LC08_L1TP_011063_20161229_20170314_01_T1.tar.gz'
untar(ZIPfile2, exdir = ZIPfile2 %>% strsplit('.tar') %>% sapply('[',1))

ZIPfile3 <- 'LC08_L1TP_010064_20161222_20180130_01_T1.tar.gz'
untar(ZIPfile3, exdir = ZIPfile3 %>% strsplit('.tar') %>% sapply('[',1))

ZIPfile4 <- 'LC08_L1GT_010063_20161222_20180130_01_T2.tar.gz'
untar(ZIPfile4, exdir = ZIPfile4 %>% strsplit('.tar') %>% sapply('[',1))
```
- Lectura de datos LANDSAT
```
mtlfile1 <- 'LC08_L1TP_011064_20161229_20170314_01_T1/LC08_L1TP_011064_20161229_20170314_01_T1_MTL.txt'
MTL1 <- readMeta(mtlfile1)

mtlfile2 <- 'LC08_L1TP_011063_20161229_20170314_01_T1/LC08_L1TP_011063_20161229_20170314_01_T1_MTL.txt'
MTL2 <- readMeta(mtlfile2)

mtlfile3 <- 'LC08_L1TP_010064_20161222_20180130_01_T1/LC08_L1TP_010064_20161222_20180130_01_T1_MTL.txt'
MTL3 <- readMeta(mtlfile3)

mtlfile4 <- 'LC08_L1GT_010063_20161222_20180130_01_T2/LC08_L1GT_010063_20161222_20180130_01_T2_MTL.txt'
MTL4 <- readMeta(mtlfile4)
```
- Lectura de datos vectoriales
```
peru <- st_read("D:/NDVI/provincias/provincias.shp")
piura <- peru[peru$PROVINCIA == "PIURA",]
sisref <- '+proj=utm +zone=17 +datum=WGS84 +units=m +no_defs'
sisref2 <- '+proj=utm +zone=17 +south +datum=WGS84 +units=m +no_defs +ellps=WGS84 +towgs84=0,0,0'
```
- Lectura de imagen espectral del landsat 
```
lsat1 <- stackMeta(mtlfile1) %>% crop(piura %>% st_transform(sisref))
lsat2 <- stackMeta(mtlfile2) %>% crop(piura %>% st_transform(sisref))
lsat3 <- stackMeta(mtlfile3) %>% crop(piura %>% st_transform(sisref))
lsat4 <- stackMeta(mtlfile4) %>% crop(piura %>% st_transform(sisref))
```

- Cálculo de la reflectividad aparente
```
lsat_coat1 <- radCor(lsat1,MTL1,"dos",atmosphere = "clear",clamp = T)[[c(1:7)]]
lsat_coat1 <- lsat_coat1 %>% projectRaster(crs = crs(sisref2))
lsat_coat2 <- radCor(lsat2,MTL2,"dos",atmosphere = "clear",clamp = T)[[c(1:7)]]
lsat_coat2 <- lsat_coat2 %>% projectRaster(crs = crs(sisref2))
lsat_coat3 <- radCor(lsat3,MTL3,"dos",atmosphere = "clear",clamp = T)[[c(1:7)]]
lsat_coat3 <- lsat_coat3 %>% projectRaster(crs = crs(sisref2))
lsat_coat4 <- radCor(lsat4,MTL4,"dos",atmosphere = "clear",clamp = T)[[c(1:7)]]
lsat_coat4 <- lsat_coat4 %>% projectRaster(crs = crs(sisref2))
```

- Cálculo NDVI
```
ndvi1 <- (lsat_coat1[[4]]-lsat_coat1[[3]])/(lsat_coat1[[4]]+lsat_coat1[[3]])
ndvi2 <- (lsat_coat2[[4]]-lsat_coat2[[3]])/(lsat_coat2[[4]]+lsat_coat2[[3]])
ndvi3 <- (lsat_coat3[[4]]-lsat_coat3[[3]])/(lsat_coat3[[4]]+lsat_coat3[[3]])
ndvi4 <- (lsat_coat4[[4]]-lsat_coat4[[3]])/(lsat_coat4[[4]]+lsat_coat4[[3]])

piura<- CRS(sisref2)
```














