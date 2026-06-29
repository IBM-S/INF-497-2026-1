# INF-497-2026-1

Ramo Análisis de Datos Espaciales.

# Patrones espaciales y temporales de delitos de alto impacto en la Ciudad de México (2019–2024)

**Autor:** Ignacio Muñoz Sánchez — Rol: 202173620-4

**Entrega:** Proyecto Final — Entrega 3

## Descripción del proyecto

Este repositorio contiene el pipeline completo de procesamiento y análisis espacial para responder la pregunta: _¿Existen patrones espaciales y temporales en la distribución de delitos de alto impacto en la Ciudad de México, y estos varían según la categoría de delito?_

## Datos

Dado el tamaño de los datos utilizados, estos no se incluyen en este repositorio. Se encuentran disponibles en el Drive indicado en el póster.

Los datos provienen de la Fiscalía General de Justicia de la Ciudad de México, el portal de datos abiertos de la CDMX, INAI e INEGI.

El Drive contiene:

- Un archivo `.zip` con las fuentes originales en formato GeoJSON.
- Una carpeta `resultados/` con los archivos ya procesados (`.gpkg`) generados por los notebooks.

### Fuentes de datos

| Conjunto de datos                | Fuente / Dependencia                               | Archivo                        |
| -------------------------------- | -------------------------------------------------- | ------------------------------ |
| Reportes de incidencia delictiva | Fiscalía General de Justicia de la CDMX            | `reportes_incidenciaU.geojson` |
| Geometría de colonias            | Censo 2010 (INEGI) / Portal de Datos Abiertos CDMX | `GeoColonias.geojson`          |
| Datos demográficos por colonia   | Censo de Población y Vivienda 2010 (INEGI)         | `DemograficosD_GJ.geojson`     |
| Cámaras de videovigilancia       | C5 CDMX, solicitado vía INAI                       | `camaraPosU.geojson`           |
| Aglomeraciones económicas        | DENUE (INEGI)                                      | `aglomeracionesU.geojson`      |

## Estructura del repositorio

```
├── H3_Procesamiento_de_Datos.ipynb   # Limpieza, unión y construcción de variables
├── H3_Analisis.ipynb                 # Análisis exploratorio espacial
└── README.md
```

## Cómo ejecutar

1. Descargar el `.zip` con las fuentes originales desde el Drive y descomprimirlo en la misma carpeta donde se ejecutarán los notebooks.

2. Instalar las dependencias:

   ```
   pip install pandas geopandas matplotlib seaborn numpy libpysal esda mapclassify mgwr
   ```

3. Ejecutar **`H3_Procesamiento_de_Datos.ipynb`** de principio a fin. Este notebook:
   - Filtra incidencias por delitos de alto impacto y período 2019–2024 (189,435 registros).
   - Construye el dataset por colonia uniendo incidencias, geometría y datos demográficos.
   - Agrega conteos por categoría de delito con prefijo `n_` mediante pivot.
   - Construye dataset longitudinal `conteo_anual` (colonia × año × categoría).
   - Incorpora cámaras y aglomeraciones mediante spatial join por geometría.
   - Calcula variables derivadas: `dens_delitos_km2`, `log_dens_delitos`,
     `dens_camaras_km2`, `dens_aglom_km2`, `log_dens_pob`.
   - Exporta a `resultados/`: `demografico.gpkg`, `incidencias.gpkg`,
     `colonias.gpkg`, `conteo_anual.gpkg`.

4. Ejecutar **`H3_Analisis.ipynb`** de principio a fin. Este notebook:
   - Justifica la unidad de análisis (colonia vs alcaldía).
   - Mapas coropléticos de densidad total y por top 4 categorías (Fisher-Jenks).
   - Curva de Lorenz y Gini para medir concentración espacial del crimen.
   - Distribución temporal por hora del día y por año (2019–2024).
   - Autocorrelación espacial global (Moran's I) total, por categoría y por año.
   - Mapa LISA total y evolución por año (2019, 2021, 2024).
   - OLS global con diagnóstico espacial (Moran sobre residuos).
   - GWR con kernel gaussiano y bandwidth óptimo por AICc.
   - OLS por año para análisis de dimensión temporal.
   - Exporta resultados y figuras en formato SVG.

**Importante:** el segundo notebook depende de los archivos generados por el primero. Deben ejecutarse en orden.
