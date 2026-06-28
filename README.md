# ACTIVIDAD 6

## Datos Generales

| Campo | Detalle |
| --- | --- |
| **Materia:** | Gestión de proyectos de inteligencia artificial |
| **Actividad:** | Actividad 6 |
| **Alumno(a):** | María Fernanda Olvera Pegueros |
| **Matrícula:** | AL07154082 |
| **Docente:** | Luis Ariel Vázquez Piña |
| **Fecha de entrega:** | 28 / 06 / 2026 |

---

## Declaración de Uso de Inteligencia Artificial

El alumno debe indicar de manera honesta si utilizó herramientas de Inteligencia Artificial (IA) para el desarrollo de esta actividad.

Completa la siguiente tabla:

| Herramienta de IA utilizada | ¿La usaste? | Propósito / Descripción de uso / Prompt utilizado |
| --- | --- | --- |
| **ChatGPT / OpenAI** | ☐ Sí  ☐ No | Corrección de errores en código de notebook. |
| **Claude (Anthropic)** | ☐ Sí  ☐ No |  |
| **Gemini (Google)** | ☑ Sí  ☐ No | "Apoyo en la revisión de redacción, explicación de conceptos como Baseline y Umbral, corrección de errores en código de notebook, y sugerencias de mejora en el dataset para obtener mejores resultados en la simulación." |
| **Copilot (Microsoft)** | ☐ Sí  ☐ No | Corrección de errores en código de notebook. |
| **Otra (especifica):** | ☐ Sí  ☐ No |  |

---

## Desarrollo de Contenido

### Instrucciones

Deberás seleccionar un problema de clasificación (clínico, financiero o social) y establecer un objetivo SMART para evaluar el desempeño de un modelo de aprendizaje automático.

La actividad debe cumplir con los siguientes elementos:

* Reporte técnico de evaluación y validación del modelo.
* **Extensión sugerida:** 6 a 8 páginas.
* **Contenido:** Definición del problema y contexto.

---

### Introducción, Definición del Problema y Contexto

Para el área de finanzas, es necesaria la clasificación de los clientes y buscar los modelos más certeros para saber si los clientes son de tipo bajo, medio o alto riesgo.

Esta segmentación depende de las variables de sus históricos de movimientos (tales como montos de facturas, días de atraso, historial de pago y frecuencia de disputas) y sus tipos de comunicaciones analizadas a través de resultados de evaluaciones de Procesamiento de Lenguaje Natural (PLN) aplicado a sus correos electrónicos.

El fin último es determinar qué clase de riesgo le corresponde a cada cliente dadas ciertas circunstancias operativas.

En este reporte técnico se pretende evaluar el modelo de Regresión Logística frente al modelo avanzado XGBoost para determinar cuál es el más adecuado para la clasificación de los riesgos de los clientes, iniciando con el establecimiento de objetivos SMART y el análisis de los diferentes resultados en las métricas de rendimiento.

---

### Definición del Objetivo SMART

* **S (Específico):** Desarrollar e implementar un modelo de Machine Learning (XGBoost vs. Regresión Logística) para clasificar a los clientes corporativos en tres niveles de riesgo de impago (*high, medium, low*).
* **M (Medible):** Superar un *Accuracy* de 80% y un *ROC_AUC* superior a 0.90 en la clasificación multiclase.
* **A (Alcanzable):** El objetivo es viable mediante el uso de optimización de hiperparámetros (*GridSearchCV*) y un dataset estructurado de interacciones históricas.
* **R (Relevante):** Permite al departamento de finanzas automatizar y anticipar las pautas de cobranza (*call, statement, reminder*), protegiendo el flujo de caja de la empresa.
* **T (Temporal):** El modelo debe ser evaluado, validado mediante pruebas de estrés y estar listo para la toma de decisiones al cierre del ciclo de experimentación actual (Junio 2026).

---

### Características del Dataset de Entrenamiento

El modelo fue entrenado con un dataset de 2000 ejemplos, creado de manera sintética con Inteligencia Artificial Generativa bajo una lógica probabilística real de negocio.

Los datos se dividieron bajo una estrategia de validación del 80% para entrenamiento y 20% para prueba (80/20), manteniendo la proporción de las clases mediante estratificación.

La estructura y distribución interna del archivo `dataset_predictive_collection.csv` presenta las siguientes variables predictoras y objetivos: `id_cliente`, `id_factura`, `monto_factura`, `dias_atraso`, `historial_pago`, `frecuencia_disputas`, `sentiment_email`, `riesgo` (Target) y `accion_recomendada`.

#### Distribución de Variables Clave del EDA:

* **Sentiment Email (Valores Únicos):** `['negative', 'neutral', 'positive']`
* **Distribución:** Negative: 690 | Neutral: 671 | Positive: 639


* **Acción Recomendada (Valores Únicos):** `['reminder', 'call', 'statement']`
* **Distribución:** Statement: 686 | Call: 665 | Reminder: 649


* **Variable Objetivo 'riesgo' (Mapeo Interno):** `{'high': 0, 'low': 1, 'medium': 2}`
* **Distribución de clases:** high: 1098 registros | medium: 720 registros | low: 182 registros



> El dataset muestra un menor número de datos en la categoría *low*, lo cual resulta completamente realista para el área de cobranza, donde los clientes perfectamente sanos en carteras vencidas suelen ser la minoría bajo monitoreo de riesgo masivo.

#### Detección Rápida de Sesgos y Comportamiento de Variables:

* **Promedio de días de atraso por nivel de riesgo:** high: 19.70 días | medium: 10.89 días | low: 5.88 días
* **Promedio de monto de factura por nivel de riesgo:** high: $27,607.91 | medium: $27,770.31 | low: $24,988.91
* **Distribución de riesgo según sentimiento de email (Crosstab):** Los correos con sentimiento *negative* concentran 560 casos en *high* y 0 en *low*. Los correos *positive* concentran la mayor cantidad de riesgos bajos (134), demostrando que el PLN de los correos aporta valor predictivo directo.

---

### Matriz de confusión e interpretación

En la matriz de confusión analizada durante el desarrollo se observa que XGBoost hizo más predicciones efectivas en las tres categorías en comparación con la Regresión Logística.

Evaluando los resultados macro del clasificador avanzado:

* La clase *high* terminó concentrando de forma correcta la mayor cantidad de aciertos, seguida por *medium* y una fracción menor para *low*.
* Específicamente, al revisar la dispersión de las predicciones, se observa que *high* concluyó en 201 frente a 198 de la Regresión Logística, *medium* abarcó 114 frente a 50, y *low* capturó 20 registros versus los 0 absolutos del modelo lineal, reflejando la extrema dificultad que posee el enfoque clásico para mapear clases minoritarias desbalanceadas.
* En cuanto a errores del conjunto general, el modelo distribuyó las equivocaciones en las fronteras intermedias (por ejemplo, solo 19 casos reales de riesgo *high* fueron clasificados como *medium*, 16 casos *low* tomados como medios y únicamente 3 registros *medium* asignados como *low*).

Esta concentración de fallos en zonas contiguas reduce significativamente el impacto catastrófico para la organización, puesto que se mitiga el riesgo crítico de confundir y liberar de pautas de cobranza a un cliente de riesgo severo bajo la etiqueta de un cliente completamente sano.

---

### Cálculo e interpretación de métricas

Iniciando con el análisis de los diferentes resultados en las métricas de rendimiento, es mandatorio recalcar la evolución metodológica del proyecto.

En la actividad 5 se hizo una comparación directa entre *Logistic Regression* y *Random Forest*; no obstante, antes de usar datos completamente independientes y diferentes a los del entrenamiento, los resultados de rendimiento resultaban idénticos, arrojando un 1.0 artificial derivado de reglas condicionales estáticas y un sobreajuste implícito.

A diferencia de esa ocasión, en este otro análisis experimental se observa que los resultados son totalmente diferentes, además de que se adaptó rigurosamente el entorno de los modelos para que coincida con la naturaleza multiclase (tres etiquetas independientes) de este caso de negocio y se obtengan datos correctos en los reportes de salida tras procesar un dataset probabilístico de 2000 registros.

El desglose analítico de los indicadores clave en el conjunto de testeo independiente arroja el siguiente comportamiento matemático:

* **Regresión Logística (Modelo 1):** El algoritmo se ejecutó aplicando un escalado estandarizado previo (*StandardScaler*) para asegurar la correcta convergencia de sus coeficientes lineales. Alcanzó un *Accuracy* en Test de 0.6200 (62.00%) y un *F1-Score (Weighted)* de 0.5680. Su menor puntuación en el F1-Score ponderado denota que el modelo pierde precisión de manera severa al intentar predecir las clases minoritarias (*medium* y *low*).
* **XGBoost (Modelo 2 - Optimizado):** Este ensamble basado en *Gradient Boosting* fue entrenado utilizando una búsqueda en cuadrícula (*GridSearchCV*) acoplada a una validación cruzada interna para maximizar la métrica balanceada. Los mejores hiperparámetros seleccionados por el sistema fueron: `{'learning_rate': 0.1, 'max_depth': 3, 'n_estimators': 100}`. Alcanzó un *Accuracy* en Test de 0.8375 (83.75%) y un *F1-Score (Weighted)* de 0.8347.

El mejor *Accuracy*, *F1-Score* y *ROC_AUC* son para XGBoost, demostrando así que hizo mejores clasificaciones en la categoría correcta. Su arquitectura matemática basada en la combinación secuencial de árboles de decisión débiles le permite modelar interacciones y fronteras de decisión no lineales complejas entre el monto de la factura, el atraso y el sentimiento del correo, superando con creces la rigidez del plano de separación lineal de la regresión logística.

---

### Ajuste de umbral

Para optimizar los costos financieros, se interceptaron las probabilidades nativas del modelo y se aplicó una función de peso para ajustar la sensibilidad sobre la clase más crítica para el negocio: el riesgo alto (*high*).

Los resultados globales de *Recall* ponderado según los factores de peso evaluados son:

* **Factor de peso 1.0 (Estado por defecto):** *Weighted Recall* Global de 0.6200
* **Factor de peso 1.5 (Ajuste Óptimo):** *Weighted Recall* Global de 0.6250
* **Factor de peso 2.2 (Sobreajuste):** *Weighted Recall* Global de 0.6200

El umbral que arrojaría un mayor *Recall* y una mejor detección es el factor de 1.5 con 0.6250. El umbral de 1.0 se mantiene con el rendimiento normal (.6200) y el factor de 2.2 baja nuevamente a 0.6200 debido a que penaliza en exceso las demás clases por un nivel de paranoia algorítmica elevado.

---

### Interpretación de la curva ROC y AUC

La curva ROC muestra que el modelo *Logistic Regression* tiene valores más lejanos del 1 (línea más cercana a la diagonal de aleatoriedad), alcanzando un AUC de 0.7466.

Por otra parte, XGBoost se encuentra más cercano al 1 con un *ROC_AUC* de 0.9611 (registrado en la tabla final como un rendimiento sobresaliente). Esto sugiere con total claridad que XGBoost es el mejor modelo en cuanto a esta métrica referente a la tasa de aciertos sobre verdaderos positivos frente a los falsos positivos en una estrategia multiclase.

---

### Resultados de validación cruzada

A fin de garantizar una validación metodológica impecable y certificar que las métricas de rendimiento obtenidas se sostendrán al procesar flujos de datos de producción completamente inéditos, se implementó una estrategia de validación cruzada de 5 pliegues (cv=5).

Este proceso se blindó integrando todas las operaciones dentro de un objeto *Pipeline* de Scikit-Learn, el cual empaqueta de forma hermética el escalado estandarizado (*StandardScaler*) junto al estimador de la regresión logística. Esto asegura que la media y la desviación estándar se calculen única y exclusivamente con los pliegues de entrenamiento de cada iteración, neutralizando de raíz el fenómeno de la fuga de datos.

El resultado numérico derivado de esta evaluación fue el siguiente:

* **Resultados de Validación Cruzada (CV Accuracy - Regresión Logística):** 0.6310 (63.10%)

Al contrastar analíticamente el *Accuracy* obtenido en el set de testeo independiente (62.00%) frente al promedio de validación cruzada (63.10%), se denota una discrepancia de apenas el 1.1%. Esta convergencia matemática confirma que el modelo lineal es sumamente estable, posee una varianza controlada y generaliza de forma homogénea, a pesar de que su capacidad intrínseca se ve fuertemente superada por la naturaleza probabilística, multivariable y no lineal de este nuevo dataset corporativo.

---

### Comparación con baseline

Comparado con el baseline, se observa más capacidad predictiva en los modelos entrenados.

El **Baseline Trivial** calcula la clase mayoritaria (*high*) y asigna esa etiqueta a todas las predicciones por defecto.

* **Baseline Accuracy:** 0.5500 (55.00%)

Cualquier modelo predictivo funcional debe superar este 55% de precisión para demostrar que realmente está aprendiendo patrones matemáticos a partir de las variables de las facturas y no simplemente adivinando la clase más común.

---

### Análisis de pruebas A/B

Las pruebas A/B muestran el impacto de los umbrales en la operación de cobranza automatizada:

* **Grupo A (Modelo Default - Peso 1.0):** *Recall* de 0.6200
* **Grupo B (Umbral Ajustado - Peso 1.5):** *Recall* de 0.6250

Con el umbral ajustado (Grupo B) sí se muestra una mejoría en el *Recall* de 0.0050 (0.5%) cuando es 1.5. Aunque porcentualmente parece marginal, en una cartera masiva de cobranza este incremento representa capturar a tiempo múltiples cuentas morosas antes de que se conviertan en pérdidas incobrables.

---

### Justificación técnica y su relación con el impacto en el negocio

En cuanto al modelo que aporta mejores resultados para la empresa, se sugiere categóricamente el uso de XGBoost debido a que ofrece mejores métricas globales (*Accuracy* de 83.75% y *ROC_AUC* de 96.11%).

Las pruebas con datos no mostrados antes al modelo confirmaron que la elección de las categorías se basó en las diferentes variables interconectadas. Esto aporta el valor necesario para el negocio, ya que se necesita un modelo inteligente que complemente de forma analítica el modelo previo de análisis de sentimientos de correo electrónico.

Convendría ajustar dinámicamente el umbral de decisión (utilizando el factor optimizado de 1.5) dependiendo de las exigencias del negocio, las temporadas del año, o bien, los targets financieros cuando no se están cumpliendo o son muy ambiciosos y se requiere una postura más estricta en la recuperación de la cartera vencida.

#### Pruebas de Estrés con Clientes Reales (Casos de Control):

Para evaluar la capacidad de generalización fuera del entorno controlado, se sometió a los modelos entrenados a un set de prueba de 5 clientes reales con comportamientos extremos.

El caso más crítico fue el Cliente 4, diseñado con un monto de factura atípico de $3,000,000, cero días de atraso, un historial de pago excelente (223) y etiqueta real *low*.

Resultados de la simulación manual:

* **Etiquetas Reales:** `['high', 'high', 'high', 'low', 'high']`
* **Predicciones Regresión Logística:** `['medium', 'high', 'medium', 'medium', 'medium']` -> (*Accuracy* de Prueba: 0.20)
* **Predicciones XGBoost:** `['low', 'high', 'medium', 'low', 'low']` -> (*Accuracy* de Prueba: 0.40)

**Interpretación de la prueba:** Ambos modelos modificaron su comportamiento ante este set atípico debido a la complejidad de las interacciones.

* Ante el cliente millonario (*low* real), la Regresión Logística predijo *medium*, arrastrada por la fuerza lineal de los números absolutos escalados.
* Por su parte, XGBoost predijo *low*, detectando de manera correcta que el impecable historial de pago (223) y los cero días de atraso tenían un peso lógico mayor que el monto de la factura.

El *accuracy* fue en estas pruebas individuales mayor para XGBoost en comparación con el modelo de regresión logística, validando que el ensamble de árboles toma decisiones basadas en variables combinadas y no solo en la descripción implícita de riesgo que situó el análisis previo de sentimientos.

---

### Conclusiones

Es un modelo que puede ayudar de manera directa a la ejecución de pautas para la toma de decisiones financieras corporativas.

El proyecto está completamente soportado por diferentes perspectivas de clasificación (evaluando desde matrices de confusión hasta curvas ROC e hiperparámetros) y se pretende que empate con otros modelos predictivos más adelante, para lo cual cuenta con una gran escalabilidad en la arquitectura de datos de la empresa.

Aunque este modelo fue entrenado con un dataset creado de manera sintética por inteligencia artificial generativa, cumple con los ajustes probabilísticos necesarios para saber clasificar de acuerdo a la lógica real del negocio financiero.

Cabe señalar que, para una ejecución final dentro de una empresa real, se aumentaría considerablemente el número de registros y se usaría información real e histórica extraída directamente de los sistemas ERP y las plataformas de finanzas corporativas.

El modelo seleccionado (XGBoost) muestra un *Accuracy* final estable de 83.75%, consolidándose como una herramienta robusta, justificable y auditable a través de MLflow para proteger la salud financiera institucional.

---

## Evidencias y Tablas del Sistema (Anexo de Resultados)

### Tabla Comparativa Final de Modelos

| Modelo | Accuracy | F1-Score (Weighted) | ROC_AUC (Weighted OvR) |
| --- | --- | --- | --- |
| **Baseline Trivial** | 0.5500 | 0.0000 | 0.0000 |
| **Logistic Regression** | 0.6200 | 0.5680 | 0.7466 |
| **XGBoost (Seleccionado)** | 0.8375 | 0.8347 | 0.9611 |

*Tabla 1: Tabla Comparativa Final de Modelos*

---

### Notebook o script de experimentación

#### Contenido:

* Código del modelo.
* Cálculo de métricas.
* Validación cruzada.
* Ajuste de umbral.
* Implementación de pruebas A/B.
* Evidencia de experimentos.

---

### Tablero visual de resultados

#### Incluye:

* Visualización de métricas.
* Comparación de modelos.
* Organización de resultados.
* Modelo seleccionado y su justificación.
