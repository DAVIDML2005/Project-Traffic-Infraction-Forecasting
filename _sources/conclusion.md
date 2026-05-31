# Conclusión

## Resumen de Hallazgos Fundamentales

El presente estudio desarrolló un análisis integral y un modelado predictivo de los comparendos electrónicos en la ciudad de Barranquilla para el período 2018-2025, abarcando cinco infracciones de mayor relevancia: C29 (exceso de velocidad), C02 (estacionamiento en sitios prohibidos), C03 (bloquear calzada o intersección), D04 (no detenerse ante una luz roja o amarilla de semáforo) y C32 (no respetar el paso de peatones). Los resultados obtenidos permiten extraer conclusiones significativas tanto sobre la naturaleza del fenómeno como sobre la aplicabilidad de diferentes enfoques de modelado predictivo en el contexto de la movilidad urbana.

## Caracterización del Fenómeno y Hallazgos del Análisis Exploratorio

### Distribución y Frecuencia de Infracciones

El análisis univariado y bivariado permitió identificar que la infracción C29 (exceso de velocidad) constituye la más frecuente del dataset, concentrando aproximadamente el 45% del total de comparendos electrónicos, lo que la convierte en el objeto de estudio predominante y en el principal desafío para la gestión de la seguridad vial en la ciudad. En el extremo opuesto, la infracción C14, asociada exclusivamente a cámaras de carril bus, presentó los menores volúmenes de registro debido a su reciente incorporación al sistema integrado de fotodetección, con apenas tres meses de datos disponibles.

### Comportamientos Temporales Heterogéneos

Un hallazgo central del estudio es que no todas las infracciones siguen patrones temporales homogéneos. La descomposición STL aplicada a cada serie reveló comportamientos sustancialmente diferentes:

- **C29 (exceso de velocidad)**: Presentó una tendencia decreciente significativa (-18.9 comparendos/mes, R²=0.58) con estacionalidad muy débil (5.66% de la varianza), indicando una reducción sostenida en el tiempo posiblemente asociada a un efecto disuasivo de las cámaras o a cambios en los patrones de movilidad.

- **C02 (estacionamiento prohibido)**: Serie estacionaria sin tendencia significativa (p=0.28) y estacionalidad débil (5.2%), lo que sugiere un comportamiento estable alrededor de una media constante.

- **C03 (bloqueo de calzada)**: Única infracción con tendencia creciente significativa (+10.3 comparendos/mes, R²=0.515), lo que la convierte en un foco de atención prioritario para las autoridades, dado su crecimiento sistemático a lo largo del período estudiado.

- **D04 (paso de semáforo en rojo)**: Tendencia negativa débil (pendiente -2.10) pero con una estacionalidad marcada (25.9% de la varianza), reflejando patrones cíclicos predecibles.

- **C32 (peatones)**: Serie estacionaria sin tendencia (p=0.27), pero con alta variabilidad y valores atípicos que dificultan su predicción.

Esta heterogeneidad de comportamientos justifica metodológicamente la decisión de modelar cada infracción de manera independiente, en lugar de aplicar un enfoque único para todas.

## Evaluación Comparativa de Modelos Predictivos

### Inexistencia de un Modelo Universal

El resultado más relevante del estudio es que no existe un único modelo capaz de sobresalir en la predicción de todas las infracciones. La efectividad de cada enfoque depende críticamente de las características específicas de cada serie temporal:

| Infracción | Mejor modelo | Característica clave explotada |
|------------|--------------|-------------------------------|
| C29 | Holt-Winters (tendencia amortiguada) | Adaptación a aceleración de la caída en 2025 |
| C02 | Holt-Winters (SES) | Estacionariedad y robustez ante outliers |
| C03 | Ridge (regresión regularizada) | Captura de tendencia creciente lineal |
| D04 | SARIMA(1,0,0)(0,0,0)[12] (AR(1) puro) | Adaptación a cambio de régimen por ausencia de estacionalidad forzada |
| C32 | SARIMA(1,0,1)(0,0,0)[12] (ARMA(1,1)) | Componente MA para absorber shocks rápidamente |

### Modelos Estadísticos Clásicos vs. Aprendizaje Automático

Contrario a lo que podría esperarse en la era del "big data" y el aprendizaje automático, los modelos estadísticos clásicos (Holt-Winters, ARIMA, SARIMA, Ridge) superaron consistentemente a los modelos de ensamble basados en árboles (Random Forest, XGBoost, LightGBM) en el conjunto de prueba (2025). Esta observación se explica por varias razones:

1. **Volumen de datos limitado**: Con series temporales de longitud moderada (7 años), los modelos complejos tienden a sobreajustar patrones específicos del pasado que no se generalizan al futuro.

2. **Ausencia de relaciones no lineales significativas**: La estructura dominante en las series analizadas fue lineal (tendencias) o de dependencia temporal simple (autorregresiva), para lo cual los modelos lineales son óptimos.

3. **Shocks estructurales**: Los cambios abruptos observados en 2025 penalizaron desproporcionadamente a los modelos de machine learning, que extrapolaron patrones históricos sin capacidad de adaptación rápida.

### El problema de la Validación Cruzada como Indicador de Desempeño Futuro

Un hallazgo metodológicamente relevante es que el mejor modelo en validación cruzada no siempre fue el mejor en el conjunto de prueba. Esto ocurrió de manera especialmente notoria en:

- **C29**: SARIMA fue superior en CV (RMSE 729) pero Holt-Winters lo superó ampliamente en test (RMSE 479 vs 740).
- **D04**: ARIMA fue mejor en CV (RMSE 232) pero SARIMA(1,0,0)(0,0,0)[12] fue abrumadoramente superior en test (RMSE 98 vs 296).
- **C32**: Lasso fue mejor en CV (RMSE 15.9) pero SARIMA(1,0,1) fue muy superior en test (RMSE 7.9 vs 18.8).

Esta discrepancia se explica por la presencia de cambios estructurales no anticipados en 2025 (aceleración de la caída en C29, desplome en D04 y C32), que invalidaron los patrones aprendidos durante el período de validación cruzada (2018-2024). La lección es crucial para aplicaciones prácticas: el desempeño histórico no garantiza el desempeño futuro, y los modelos más simples y adaptables pueden ser preferibles ante entornos no estacionarios.

### El papel de la Tendencia en el Desempeño Predictivo

La presencia o ausencia de tendencia determinó de manera fundamental qué modelo resultó más adecuado:

- **Con tendencia creciente (C03)**: Ridge (regresión con tendencia explícita) superó a modelos estacionarios que subestimaron sistemáticamente.
- **Con tendencia decreciente acelerada (C29)**: Holt-Winters con tendencia amortiguada permitió una adaptación gradual que otros modelos no lograron.
- **Sin tendencia (C02, C32)**: Modelos estacionarios (SES, ARMA) fueron suficientes y superiores a enfoques que introducían tendencias adulteradas.

## Pronósticos para 2026 e Implicaciones para la Gestión Pública

### Tendencias Proyectadas

Los pronósticos generados con los mejores modelos para cada infracción permiten anticipar el siguiente panorama para 2026:

| Infracción | Pronóstico 2026 (promedio mensual) | Tendencia proyectada |
|------------|-------------------------------------|----------------------|
| C29 (exceso velocidad) | 803 - 989 | Decreciente (de 989 a 803) |
| C02 (estacionamiento) | 1940 constante | Estable |
| C03 (bloqueo calzada) | 809 - 847 | Creciente (especialmente oct-dic) |
| D04 (semáforo rojo) | 141 - 262 | Decreciente acelerada |
| C32 (peatones) | 3 - 5 | Muy baja, estable |

### Implicaciones para las Autoridades de Tránsito

Con base en estos pronósticos, se derivan las siguientes recomendaciones para la gestión de la seguridad vial en Barranquilla:

1. **Priorizar la infracción C03 (bloqueo de calzada)** : Es la única con tendencia creciente confirmada y proyectada. Se recomienda intensificar los controles en zonas y horarios críticos, así como campañas educativas dirigidas a conductores de vehículos particulares y de carga.

2. **Mantener los esfuerzos en C29 (exceso de velocidad)** : La tendencia decreciente proyectada sugiere que las cámaras de fotodetección están cumpliendo su función persuasiva. Sin embargo, la caída acelerada en 2025 debe ser monitoreada para confirmar si responde a una mejora sostenida o a factores coyunturales.

3. **Evaluar las causas del desplome en D04 (paso de semáforo en rojo)** : La disminución proyectada de 262 a 141 comparendos mensuales a lo largo de 2026 representa una reducción significativa. Debe investigarse si esto responde a mejoras en la infraestructura semafórica, cambios en el comportamiento de los conductores, o simplemente a una reducción en la operatividad de las cámaras.

4. **Monitorear la evolución de C02 (estacionamiento prohibido)** : Su estabilidad proyectada alrededor de 1940 comparendos mensuales indica que es un problema persistente que requiere intervenciones estructurales (mayor disponibilidad de parqueaderos, mejor señalización, etc.).

5. **Interpretar con cautela los pronósticos de C32 (peatones)** : Dado el bajo volumen proyectado (3-5 comparendos mensuales), la relevancia práctica de estas estimaciones es limitada. Sin embargo, cualquier incremento en esta infracción debe ser atendido con prioridad por su impacto directo en la seguridad de los peatones.

## Limitaciones del Estudio y Líneas de Trabajo Futuro

### Limitaciones Identificadas

El presente estudio reconoce las siguientes limitaciones que deben ser consideradas al interpretar los resultados:

#### Limitaciones en los Datos Disponibles
- **Ausencia de hora real**: Todos los registros tienen hora normalizada a las 12:00 AM, impidiendo análisis de franjas horarias dentro del día y la identificación de patrones de infracción por hora pico o nocturnas.
- **Sin identificación geográfica precisa**: La dirección de la cámara está en formato texto, no en coordenadas (latitud/longitud), limitando análisis espaciales avanzados como clusters de infracciones, mapas de calor o modelos espaciotemporales.
- **Inconsistencias en la nomenclatura de cámaras**: Cambios en las denominaciones a lo largo del tiempo y casos de duplicados dificultaron el seguimiento individualizado por punto de control.
- **Fechas de operación desconocidas**: No se dispone de la fecha exacta en que cada cámara inició o cesó operaciones, solo los registros generados.
- **Sin información del infractor**: No se incluyen variables demográficas (edad, género, tipo de licencia, reincidencia individual), lo que impide análisis de perfiles de riesgo y segmentación de conductores.

#### Limitaciones Metodológicas
- **Modelos univariados**: Los modelos se construyeron únicamente a partir de la información histórica de comparendos, sin incorporar variables externas como condiciones climáticas, calendarios de eventos masivos, intervenciones en vías, cambios normativos, o fluctuaciones en el flujo vehicular.
- **Cambio estructural en 2025**: La disminución repentina de comparendos en 2025 no pudo ser anticipada por ningún modelo, lo que sugiere la necesidad de integrar información contextual para explicar estos quiebres.
- **Inexistencia de patrones consistentes**: La alta variabilidad año a año y la falta de recurrencia en los patrones de infracción limitaron la capacidad predictiva incluso de modelos complejos.

### Líneas de Trabajo Futuro

Con base en las limitaciones identificadas, se proponen las siguientes líneas de investigación futura:

1. **Modelos multivariados**: Incorporar variables explicativas adicionales como flujo vehicular (sensores de tráfico), condiciones meteorológicas (precipitación, visibilidad), calendario de festividades y eventos especiales, y cambios en la normativa de tránsito.

2. **Análisis espacial avanzado**: Georreferenciar con precisión las cámaras (coordenadas) para implementar modelos espaciotemporales que identifiquen clusters de infracciones y patrones de contagio entre zonas adyacentes.

3. **Evaluación de impacto**: Diseñar un estudio cuasiexperimental que evalúe el efecto causal de la instalación de nuevas cámaras sobre la reducción de infracciones y siniestros, utilizando metodologías de diferencias en diferencias o variables instrumentales.

4. **Integración con datos de siniestralidad**: Vincular la base de comparendos con registros de accidentes de tránsito para evaluar si las reducciones en infracciones se traducen efectivamente en menor siniestralidad y fatalidades.

5. **Verificación del pronóstico 2026**: Una vez que la Alcaldía de Barranquilla publique los datos de comparendos para 2026, se recomienda comparar los pronósticos generados con los valores reales para validar (o refutar) la bondad de los modelos seleccionados.

## Contribución Original y Aporte al Conocimiento

El presente estudio aborda una brecha significativa en la literatura especializada. Si bien existen antecedentes internacionales de aplicación de modelos predictivos a series temporales de infracciones de tránsito, esta investigación constituye la primera comparativa sistemática y exhaustiva entre múltiples enfoques de modelado (estadísticos clásicos, regularizados, ensambles de árboles y basados en similitud) aplicada específicamente al análisis de comparendos electrónicos en Colombia y, más particularmente, en la ciudad de Barranquilla.

La principal contribución del trabajo es doble: por un lado, aporta evidencia empírica local sobre el comportamiento de las infracciones y la efectividad de diferentes técnicas predictivas; por otro lado, ofrece recomendaciones prácticas concretas para las autoridades de tránsito sobre qué modelo utilizar para cada tipo de infracción y cómo interpretar los pronósticos generados.

## Reflexión Final

En un contexto de transformación digital de la gestión pública, donde los datos abundan pero la capacidad de convertirlos en inteligencia accionable es aún limitada, esta investigación demuestra que la sofisticación del modelo no garantiza su utilidad práctica. Los modelos estadísticos más simples, cuando están bien adaptados a las características específicas de cada fenómeno, pueden superar a enfoques computacionalmente más complejos, especialmente en escenarios de cambios estructurales no anticipados.

La lección más valiosa del estudio quizás no sea cuál modelo ganó, sino la importancia de comprender la naturaleza del fenómeno antes de seleccionar la herramienta predictiva. La heterogeneidad de comportamientos entre infracciones, la presencia o ausencia de tendencia, la fuerza de la estacionalidad, y la ocurrencia de quiebres estructurales son factores que deben guiar la selección metodológica, en lugar de aplicar mecánicamente el algoritmo de moda.

Finalmente, este trabajo aspira a contribuir, aunque sea modestamente, a la construcción de ciudades más seguras, ordenadas y humanas, donde la tecnología al servicio de la movilidad se traduzca efectivamente en la salvaguarda de la vida y el bienestar de todos los actores viales de Barranquilla. Los pronósticos aquí presentados para 2026 no son un veredicto inmutable, sino una herramienta para la acción informada: su verdadero valor se materializará en la medida en que las autoridades los utilicen para anticipar problemas, asignar recursos de manera eficiente y diseñar intervenciones basadas en evidencia.