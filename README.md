## ÍNDICES BIOCLIMÁTICOS Y ESPECTRALES DE LA PROVINCIA DE PISCO

**ALUMNOS** |  **CÓDIGO** | 
----------------------| ----------------------|
*LIMAS CERNA ANTONIO JESUS* | 19160180 
*MALLQUI GUZMÁN ROCÍO ZAYDA* | Texto 4
*VILCA TORRES DIEGO RENZO* | Texto 2


### ÍNDICE DE ARIDEZ DE MARTONNE DE LA PROVINCIA DE PISCO

**Permite una primera identificación fitoclimática del mundo, aunque es especialmente
efectivo en zonas tropicales y subtropicales.**

-  Instalación de las librerías
```{librerías}
library(raster)
library(tmap)
library(rcartocolor)
library(sf)
library(rgdal)
library(raster)
library(dplyr)
library(RColorBrewer)
library(tidyverse)
```
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

### ÍNDICE DE ARIDEZ DE MARTONNE DE LA PROVINCIA DE PISCO

**Permite una primera identificación fitoclimática del mundo, aunque es especialmente
efectivo en zonas tropicales y subtropicales.**

-  Instalación de las librerías
```{librerías}
library(raster)
library(tmap)
library(rcartocolor)
library(sf)
library(rgdal)
library(raster)
library(dplyr)
library(RColorBrewer)
library(tidyverse)
```
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

