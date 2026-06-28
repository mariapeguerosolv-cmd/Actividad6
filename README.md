# Actividad 6

**Materia:** Gestión de proyectos de inteligencia artificial\
**Actividad:** Actividad 6\
**Alumno(a):** María Fernanda Olvera Pegueros\
**Matrícula:** AL07154082\
**Docente:** Luis Ariel Vázquez Piña\
**Fecha de entrega:** 28/06/2026

# Declaración de Uso de Inteligencia Artificial

  -----------------------------------------------------------------------
  Herramienta de IA       ¿La usaste?             Propósito / Descripción
  utilizada                                       de uso
  ----------------------- ----------------------- -----------------------
  ChatGPT / OpenAI        Sí                      Corrección de errores
                                                  en código de notebook.

  Claude (Anthropic)      No                      ---

  Gemini (Google)         Sí                      Apoyo en revisión de
                                                  redacción, explicación
                                                  de conceptos como
                                                  Baseline y Umbral,
                                                  corrección de errores
                                                  en código y sugerencias
                                                  de mejora del dataset.

  Copilot (Microsoft)     Sí                      Corrección de errores
                                                  en código de notebook.
  
  -----------------------------------------------------------------------

# Desarrollo de Contenido

## Introducción, Definición del Problema y Contexto

Para el área de finanzas es necesaria la clasificación de clientes y la
búsqueda de modelos certeros para identificar si pertenecen a un nivel
de riesgo bajo, medio o alto. Esta segmentación depende de variables
históricas como montos de facturas, días de atraso, historial de pago y
frecuencia de disputas, además del análisis de comunicaciones mediante
Procesamiento de Lenguaje Natural (PLN).

El objetivo del reporte es evaluar Regresión Logística frente a XGBoost
para determinar cuál modelo es más adecuado para clasificar riesgos de
clientes.

# Objetivo SMART

-   **S (Específico):** Desarrollar un modelo Machine Learning (XGBoost
    vs Regresión Logística) para clasificar clientes en niveles de
    riesgo high, medium y low.
-   **M (Medible):** Superar Accuracy de 80% y ROC_AUC superior a 0.90.
-   **A (Alcanzable):** Mediante optimización de hiperparámetros con
    GridSearchCV y un dataset estructurado.
-   **R (Relevante):** Permite automatizar decisiones de cobranza y
    proteger flujo de caja.
-   **T (Temporal):** Evaluación y validación durante junio 2026.

# Características del Dataset

El modelo fue entrenado con 2000 registros sintéticos generados con
Inteligencia Artificial Generativa. La división fue 80% entrenamiento y
20% prueba.

Variables:

-   id_cliente
-   id_factura
-   monto_factura
-   dias_atraso
-   historial_pago
-   frecuencia_disputas
-   sentiment_email
-   riesgo (Target)
-   accion_recomendada

Distribución de riesgo:

-   High: 1098 registros
-   Medium: 720 registros
-   Low: 182 registros

# Matriz de Confusión e Interpretación

XGBoost obtuvo mejores resultados en comparación con Regresión
Logística, especialmente en clases minoritarias. El modelo avanzado
logró capturar correctamente patrones complejos entre variables
financieras y sentimiento del correo.

# Métricas de Evaluación

## Regresión Logística

-   Accuracy: 0.6200 (62%)
-   F1 Score Weighted: 0.5680

## XGBoost

-   Accuracy: 0.8375 (83.75%)
-   F1 Score Weighted: 0.8347
-   ROC_AUC: 0.9611

XGBoost mostró mejor desempeño debido a su capacidad para modelar
relaciones no lineales.

# Ajuste de Umbral

Resultados:

  Factor   Weighted Recall
  -------- -----------------
  1.0      0.6200
  1.5      0.6250
  2.2      0.6200

El factor 1.5 presentó el mejor resultado.

# Curva ROC y AUC

-   Regresión Logística: AUC 0.7466
-   XGBoost: AUC 0.9611

XGBoost presenta mejor capacidad de separación entre clases.

# Validación Cruzada

Se aplicó validación cruzada de 5 pliegues utilizando Pipeline de
Scikit-Learn para evitar fuga de información.

Resultado:

-   Logistic Regression CV Accuracy: 0.6310

# Comparación con Baseline

Baseline:

-   Accuracy: 0.5500

Los modelos entrenados superan el rendimiento del modelo trivial.

# Pruebas A/B

Grupo A:

-   Modelo Default
-   Recall: 0.6200

Grupo B:

-   Umbral ajustado 1.5
-   Recall: 0.6250

El ajuste mejora la detección de clientes de riesgo.

# Justificación Técnica y de Negocio

Se recomienda utilizar XGBoost debido a sus mejores métricas:

-   Accuracy: 83.75%
-   ROC_AUC: 96.11%

Permite una mejor toma de decisiones financieras y estrategias de
cobranza.

# Pruebas de Estrés

Se evaluaron clientes con comportamientos extremos.

Resultados:

-   Regresión Logística Accuracy: 0.20
-   XGBoost Accuracy: 0.40

XGBoost mostró mejor capacidad de generalización.

# Conclusiones

El modelo XGBoost resulta ser la mejor alternativa para clasificar
riesgos financieros. Aunque el dataset fue generado sintéticamente,
mantiene una lógica de negocio adecuada. Para producción sería necesario
incorporar datos históricos reales.

El modelo seleccionado presenta un Accuracy estable de 83.75%, siendo
una solución robusta y escalable.

# Tabla Comparativa Final

  Modelo                  Accuracy   F1 Score   ROC_AUC
  --------------------- ---------- ---------- ---------
  Baseline Trivial          0.5500     0.0000    0.0000
  Logistic Regression       0.6200     0.5680    0.7466
  XGBoost                   0.8375     0.8347    0.9611
