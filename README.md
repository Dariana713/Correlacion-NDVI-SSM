# Correlacion-NDVI-SSM
# MES DE ABRIL

# Ubicación 
setwd("~/Análisis de Tesis en Rstudio y SAGA GIS/Variables/Vegetación/NDVI_1km_v2_2020/4_Abril")

# Unificar archivos
s <- stack(list.files(pattern = ".nc"))

# Cargar libreria Map
library(maps)
plot(extent(s))
#Colocar el mapa mundial
maps:: map("world", add =TRUE)

# colección de rasters de un subconjunto de datos

#Extención del area de investigación
ex <- extent(-10.13393, 6.169643, 35.48214, 43.91964)
#
ESP_NDVI_A <- stack(crop(s, ex))

# Data frame 
#df <- as.data.frame(ESP_NDVI_A , xy=TRUE)
plot(ESP_NDVI_A )

writeRaster(ESP_NDVI_A, file="ESP_NDVI_A.tif")

# Datos de SSM Abril

SSM_media_Abril <- stack(list.files("C:/Users/Usuario/Documents/Análisis de Tesis en Rstudio y SAGA GIS/Variables/Humedad/SSM_Bir  2020/4_Abril/Imagen correcta", full.names = TRUE))

# ?resample
# Ajustar los datos para el mismo mes (cambio de tamaño de tamaño de pixel a uno mismo), es la misma capa Ajuste_AbrilNDVI, para que quede en el mismo tamaño ssm
Ajuste_AbrilNDVI <- resample(ESP_NDVI_A, SSM_media_Abril, method='ngb')

# Combinación_ contiene los datos de ssm y ndvi, en un mismo tamaño
# Se reviso este link "https://hakimabdi.com/blog/test-pixelwise-correlation-between-two-time-series-of-gridded-satellite-data-in-r"  (correlación por píxeles entre dos series de tiempo de datos satelitales en cuadrícula en R)

CombinaciónAbril <- stack(Ajuste_AbrilNDVI, SSM_media_Abril)

# guardar como writeRaster


# HAY QUE ELIMINAR LOS "na", de la función, no es muy acorde con los -9999
