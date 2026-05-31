# Análisis y Modelado Predictivo de Comparendos Electrónicos en la Ciudad de Barranquilla

## Descripción

Este proyecto desarrolla un análisis integral y un sistema de modelado predictivo para el estudio de los comparendos electrónicos registrados en la ciudad de Barranquilla, Colombia, utilizando técnicas de análisis exploratorio de datos, georreferenciación, series temporales y aprendizaje automático.

La investigación aborda el fenómeno de las infracciones de tránsito captadas por sistemas de fotodetección, con el objetivo de identificar patrones espaciales y temporales, comparar modelos predictivos y generar pronósticos para el año 2026.

El proyecto fue desarrollado durante el curso de **Seminario Investigativo** de la **Universidad del Norte**.


## Colaboradores

* David Marquez
* Cristian Linero

## Objetivos

### Objetivo General

Desarrollar un análisis integral y modelos predictivos para anticipar el comportamiento de las infracciones de tránsito registradas mediante sistemas de fotodetección en Barranquilla.

### Objetivos Específicos

* Caracterizar la distribución espacial y temporal de las infracciones
* Analizar tendencias y patrones estacionales en las series temporales
* Implementar modelos estadísticos clásicos y modelos de Machine Learning
* Comparar sistemáticamente el desempeño predictivo de los modelos
* Seleccionar el modelo óptimo para cada tipo de infracción
* Generar pronósticos para el año 2026

## Dataset

### Fuente de Datos

Los datos fueron obtenidos desde el portal de **Datos Abiertos del Gobierno de Colombia**, correspondientes a los registros de comparendos electrónicos emitidos en Barranquilla entre 2018 y 2025.

* **Dataset Principal** : https://www.datos.gov.co/Transporte/Comparendos-Electr-nicos-Control-y-Regulaci-n-Barr/ps7z-yks5/about_data

* **Dataset Secundario** : https://www.datos.gov.co/Transporte/Fotodetecci-n-en-Barranquilla/cpp6-je64/about_data

### Características del Dataset

* Más de 340,000 registros
* Aproximadamente 50 cámaras de fotodetección
* Información temporal y geográfica detallada
* Variables relacionadas con:

  * Fecha
  * Ubicación geográfica
  * Tipo de infracción
  * Clase de vehículo
  * Servicio de vehículo

## Infracciones Analizadas

El estudio se centra en las cinco infracciones más representativas:

| Código | Descripción                                   |
| ------ | --------------------------------------------- |
| C29    | Exceso de velocidad                           |
| C02    | Estacionamiento en sitios prohibidos          |
| C03    | Bloquear calzada o intersección               |
| D04    | No detenerse ante semáforo en rojo o amarillo |
| C32    | No respetar el paso de peatones               |

## Metodología

El proyecto se desarrolla en múltiples fases metodológicas:

### Análisis Exploratorio de Datos (EDA)

* Análisis univariado
* Análisis bivariado
* Estadísticos descriptivos
* Distribuciones y frecuencias
* Identificación de patrones

### Georreferenciación y Análisis Espacial

Se utilizaron archivos Shapefile de Barranquilla para:

* Ubicar geográficamente las cámaras
* Visualizar la distribución espacial
* Detectar zonas críticas
* Analizar cobertura territorial

### Descomposición Temporal

Se aplicaron técnicas como:

* STL (Seasonal-Trend decomposition using Loess)
* MSTL (Multiple Seasonal-Trend decomposition)

## Modelos Implementados

### Modelos Estadísticos Clásicos

* Holt-Winters
* ARIMA
* SARIMA
* Dynamic Optimized Theta (DOT)

### Modelos de Regresión Regularizada

* Ridge Regression
* Lasso Regression

### Modelos de Machine Learning

* Random Forest
* XGBoost
* LightGBM
* K-Nearest Neighbors (KNN)

## Transformación de Series Temporales

Los modelos de Machine Learning fueron adaptados al problema de series temporales mediante:

* Ventanas deslizantes
* Variables rezagadas (lags)
* Variables de tendencia
* Variables estacionales

Esto permitió convertir la serie temporal en un problema de aprendizaje supervisado.

## Métricas de Evaluación

Los modelos fueron evaluados utilizando:

* MSE (Mean Squared Error)
* RMSE (Root Mean Squared Error)
* MAE (Mean Absolute Error)
* MAPE (Mean Absolute Percentage Error)
* SMAPE (Symmetric Mean Absolute Percentage Error)

## Pronósticos para 2026

Una vez seleccionados los mejores modelos para cada infracción, se generaron pronósticos para el año 2026 con el objetivo de:

* Anticipar tendencias futuras
* Identificar períodos críticos
* Apoyar decisiones en movilidad urbana
* Optimizar estrategias de control vial

## Impacto del Proyecto

Este proyecto busca contribuir a:

* Mejorar la seguridad vial
* Optimizar recursos públicos
* Fortalecer políticas de movilidad urbana
* Apoyar decisiones basadas en evidencia
* Aplicar ciencia de datos al contexto urbano colombiano

## Contribución Académica

La investigación aborda una brecha existente en la literatura nacional relacionada con:

* Predicción de comparendos electrónicos en Colombia
* Comparación sistemática de múltiples modelos predictivos
* Aplicación de Machine Learning a movilidad urbana en Barranquilla
