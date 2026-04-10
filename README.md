# Proyecto CASPER: Pipeline de Optimización y Predicción de Consumo Energético Industrial
________________________________________
## ¿Qué puedes hacer con este repositorio?
Este repositorio contiene el código necesario para analizar datos de consumo eléctrico industrial y ejecutar un pipeline de Machine Learning compuesto por tres módulos complementarios:
* Regresión, para estimar el consumo energético esperado.
* Clasificación, para identificar el estado de carga de la planta.
* Agrupamiento, para descubrir patrones diarios de operación.

El objetivo no es únicamente generar predicciones, sino ayudarte a detectar anomalías operativas, interpretar el comportamiento energético del sistema y explorar patrones recurrentes en los datos.
Este proyecto fue desarrollado en el marco del programa Samsung Innovation Campus. 
________________________________________
## ¿Qué hace el pipeline?

A partir de registros eléctricos tomados cada 15 minutos, el sistema permite:
* predecir el consumo energético esperado de la planta,
* detectar desviaciones relevantes entre el consumo real y el estimado,
* clasificar el régimen de carga en categorías operativas,
* agrupar jornadas completas para identificar patrones de comportamiento.

En otras palabras, este repositorio te permite trabajar el problema desde una perspectiva predictiva, supervisada y no supervisada, utilizando una misma base de datos y una misma lógica de análisis.
________________________________________
## Conjunto de datos

Los modelos fueron construidos usando el Steel Industry Energy Consumption Dataset de Kaggle.
El dataset contiene registros eléctricos de una planta siderúrgica tomados en intervalos de 15 minutos. Para poder utilizar correctamente los modelos incluidos en este repositorio, los datos deben conservar su organización cronológica, ya que el pipeline incorpora validación temporal, rezagos y análisis por jornadas completas.

________________________________________
## Estructura funcional del proyecto
### 1. Módulo de regresión
Uso principal: estimar el consumo energético esperado y detectar anomalías.
* Modelo: XGBoost con validación temporal mediante TimeSeriesSplit.
* Salida principal: predicción del consumo esperado.
* Aplicación práctica: comparar el valor real contra el valor predicho para identificar desviaciones operativas.
* Detección de anomalías: se consideran anómalas las desviaciones superiores a 15 kWh, y se clasifican por severidad en Baja, Media y Alta.
* Desempeño reportado: R² = 90.96% y MAE = 4.84 kWh.

Este módulo es útil cuando se desea responder preguntas como:
“¿La planta está consumiendo más energía de la que debería en estas condiciones?”
________________________________________
### 2. Módulo de clasificación
Uso principal: identificar el tipo de carga operativa de la planta.
* Modelo: Random Forest con apoyo de BorderlineSMOTE para el manejo de clases desbalanceadas.
* Salida principal: clasificación del estado de carga en:
    - Light_Load
    - Medium_Load
    - Maximum_Load
* Aplicación práctica: etiquetar automáticamente el régimen operativo del sistema a partir de variables eléctricas y temporales.
* Desempeño reportado: Accuracy = 87.57% y ROC-AUC = 97.07%.

Este módulo resulta útil cuando se necesita saber:
“¿En qué tipo de carga estaba operando la planta en este registro?”
________________________________________
### 3. Módulo de agrupamiento

Uso principal: descubrir patrones diarios de operación sin depender de etiquetas previas.
* Modelo: TimeSeriesKMeans, comparando distancia Euclidiana y Dynamic Time Warping (DTW).
* Entrada: series diarias multivariadas de 96 intervalos de 15 minutos.
* Salida principal: agrupación de jornadas completas según similitud de comportamiento.
* Aplicación práctica: identificar perfiles operativos recurrentes, días atípicos o patrones diferenciados entre jornadas laborales y no laborales.
* Desempeño reportado: Silhouette score = 0.33 con DTW.

Este módulo ayuda a responder preguntas como:
“¿Qué tipos de jornadas operativas se repiten en la planta?”
________________________________________
## Ingeniería de características aplicada

El pipeline incluye transformaciones diseñadas para que los modelos interpreten correctamente el comportamiento energético:
* Rezagos temporales (lags): capturan la inercia del sistema y la memoria operativa a corto y largo plazo.
* Codificación cíclica: transforma variables temporales como hora y día usando seno y coseno para preservar su continuidad.
* Factor de Potencia Total (FP_Total): consolida indicadores eléctricos relacionados en una sola variable más interpretable.
* Selección de variables: excluye atributos que podrían introducir fuga de información o redundancia.

Estas transformaciones son importantes porque los modelos no trabajan únicamente con datos crudos, sino con una representación más útil del comportamiento real del sistema.
________________________________________
## ¿Para quién está pensado este repositorio?

Este repositorio puede ser útil para:
* estudiantes que deseen estudiar un caso aplicado de Machine Learning industrial,
* personas que necesiten una base para proyectos de predicción y monitoreo energético,
* equipos que quieran reutilizar o adaptar módulos de regresión, clasificación o agrupamiento sobre datos temporales,
* usuarios que busquen un ejemplo de pipeline reproducible con validación temporal y análisis multietapa.
________________________________________
## Resultado esperado al usar este código

Al ejecutar los módulos del proyecto, podrás obtener:
* métricas de desempeño por modelo
* detección de anomalías energéticas
* clasificación automática de tipos de carga
* agrupación de jornadas por patrones de operación
* visualizaciones para interpretar el comportamiento del sistema.
________________________________________
## Equipo de desarrollo (Samsung–UDEM)
* Castelan Rosas Enrique Andrés
* Clemente Morales Angel Agustín
* Polo Castelan Jonas Alaim
* Rodriguez Hernandez Francisco Abraham 
________________________________________

