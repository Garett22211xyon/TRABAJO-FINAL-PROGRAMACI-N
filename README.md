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


### ÍNDICE DE ARIDEZ DE MARTONNE DE LA PROVINCIA DE PISCO

**Permite una primera identificación fitoclimática del mundo, aunque es especialmente
efectivo en zonas tropicales y subtropicales.**

-  Cargando raster de precipitación
```{raster}
grids <- list.files('C:/Users/USUARIO/Documents/TAREA PROGRAMACION/DATOS/2014-2018 PRECIPITACION/2014', full.names = TRUE, pattern = "*.tif")
```
-  Raster stack
```{raster}
prp_stack <- stack(grids)
```
-  Suma de precipitación anual
```{raster}
prp_anual <- sum(prp_stack)
plot(prp_anual)
```
-  Para la provincia de Pisco
```{raster}
prov <- readOGR("E:/TAREA BIOGEO/Materiales/PROVINCIAS.shp")
plot(prov)

pisco <- prov[prov$PROVINCIA == "PISCO",]
plot(pisco)
```
- Guardando el raster
```{raster}
writeRaster(prp_anual_pisco,filename = "Precipitacion anual pisco 2014",format = "GTiff",overwrite =TRUE)
```
-  Hallando la temperatura minima anual
```{raster}
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
```{raster}
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
```{raster}
IA_2014 <- prp_anual_pisco/(tmd + 10)
plot(IA_2014, main = "Indice de Aridez de Martonne 2014")
plot(pisco, add = TRUE)
```
- Cortando
```{raster}
IA_2014_f <- mask(IA_2014,pisco)
plot(IA_2014_f, main = "Indice de Aridez de Martonne 2014")
plot(pisco, add = TRUE)
```
- Procedemos a guardar en formato tiff
```{raster}
writeRaster(IA_2014,filename = "Indice de Aridez de Martonne",format = "GTiff",overwrite = TRUE)
```

### ÍNDICE DE DANTIN-REVENGA DE LA PROVINCIA DE PISCO

**Este índice permite calificar el ambiente fitoclimático y se basa en la precipitación y la
temperatura.**

-  Cargando raster de precipitación
```{raster}
grids <- list.files('D:/TRABAJO FINALPROGRAMACIÓN/datos/PRECIPITACION 2016', full.names = TRUE, pattern = "*.tif")
```
-  Raster stack
```{raster}
prp_stack <- stack(grids)
```
-  Suma de precipitación anual
```{raster}
prp_anual <- sum(prp_stack)
plot (prp_anual)
```
-  Para la provincia de Pisco
```{raster}
prov <- readOGR("D:/TRABAJO FINALPROGRAMACIÓN/MATERIALES/PROVINCIAS.shp")
plot(prov)

pisco <- prov[prov$PROVINCIA == "PISCO",]
plot(pisco)
```
- Cortando la provicnia de Pisco
```{raster}
prp_anual_pisco <- crop(prp_anual,pisco)
plot(prp_anual_pisco)
plot(pisco, add = TRUE)
```
- Guardando el raster
```{raster}
writeRaster(prp_anual_pisco,filename = "Precipitacion anual pisco 2016",format = "GTiff",overwrite = TRUE)
```
-  Hallando la temperatura minima anual
```{raster}
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
```{raster}
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
```{raster}
tmd <- (tmin_prom_anual_pisco + tmax_prom_anual_pisco )/2
writeRaster(tmd,filename = "Temperatura promedio anual 2016",format = "GTiff",overwrite = TRUE)
```
- Ploteando el raster
```{raster}
danreven <- (tmd/prp_anual_pisco)*100
plot(danreven)
plot(pisco, add = TRUE)
```

