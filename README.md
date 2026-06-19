# INF-497-2026-1
Ramo Analisis de Datos Espaciales.

# Patrones espaciales y temporales de delitos de alto impacto en la Ciudad de México (2019–2024)

**Autor:** Ignacio Muñoz Sánchez — Rol: 202173620-4
**Curso:** INF-497 — Análisis de Datos Espaciales
**Entrega:** Proyecto Final — Entrega 2

## Descripción del proyecto

Este repositorio contiene el pipeline de procesamiento y el análisis espacial exploratorio para responder la pregunta: *¿Existen patrones espaciales y temporales en la distribución de delitos de alto impacto en la Ciudad de México, y estos varían según el tipo de delito?*

## Datos

Dado el tamaño de los datos utilizados, estos no se incluyen en este repositorio. Se encuentran disponibles en el Drive que se encuentra en el informe.

Los datos provienen de la Fiscalía General de Justicia de la Ciudad de México, el portal de datos abiertos de la CDMX, INAI e INEGI.

El Drive contiene:
- Un archivo `.zip` con las fuentes originales en formato GeoJSON (incidencias, colonias, datos demográficos, cámaras y aglomeraciones).
- Una carpeta `resultados/` con los archivos ya procesados (`.gpkg` y `.csv`) generados por los notebooks de este repositorio.

## Estructura del repositorio

```
├── H2_Procesamiento_de_Datos.ipynb   # Limpieza, unión y construcción de variables
├── H2_Analisis.ipynb                 # Análisis exploratorio espacial
└── README.md
```

## Cómo ejecutar

1. Descargar el `.zip` con las fuentes originales desde el Drive y descomprimirlo en la misma carpeta donde se ejecutarán los notebooks (deben quedar accesibles los archivos `.geojson` referenciados al inicio del primer notebook).

2. Instalar las dependencias necesarias:
   ```
   pip install pandas geopandas matplotlib seaborn numpy libpysal esda mapclassify mgwr
   ```

3. Ejecutar **`H2_Procesamiento_de_Datos.ipynb`** de principio a fin. Este notebook:
   - Carga y limpia los reportes de incidencia, filtrando por delitos de alto impacto y por el período 2019–2024.
   - Construye el dataset consolidado por colonia, uniendo incidencias, geometría y datos demográficos del Censo 2010.
   - Genera las variables derivadas: tasa de delitos por 100.000 habitantes, densidad de delitos por km², densidad poblacional y tasas por tipo de delito.
   - Exporta los resultados a la carpeta `resultados/` en formato GeoPackage (`incidencias.gpkg`, `colonias.gpkg`, `demografico.gpkg`).

4. Ejecutar **`H2_Analisis.ipynb`** de principio a fin. Este notebook:
   - Carga los archivos generados por el notebook de procesamiento.
   - Justifica la unidad de análisis (colonia) frente a la alternativa de alcaldía.
   - Genera mapas coropléticos de la tasa de delitos, total y por tipo.
   - Analiza la distribución temporal de los delitos por hora del día.
   - Calcula la autocorrelación espacial global (Moran's I) y local (LISA).
   - Implementa una primera versión de Regresión Geográficamente Ponderada (GWR).
   - Exporta los resultados finales (`analisis_final.gpkg` y `.csv`) y las figuras usadas en el informe.

**Importante:** el segundo notebook depende de los archivos generados por el primero. Deben ejecutarse en orden.
