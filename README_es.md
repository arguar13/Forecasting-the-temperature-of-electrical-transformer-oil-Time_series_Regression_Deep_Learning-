*Leer este documento en otros idiomas: [English](README.md)*

# Proyecto de Deep Learning — Pronóstico Multivariable de Series Temporales para la Temperatura del Aceite de Transformadores Eléctricos

Este proyecto desarrolla un benchmark integral de Deep Learning para el pronóstico de series temporales multivariables utilizando el Electricity Transformer Dataset (ETDataset).

El objetivo es predecir la Temperatura del Aceite (OT) futura de transformadores eléctricos a partir de mediciones históricas de temperatura, variables de carga eléctrica y características temporales. Un pronóstico preciso de la temperatura del aceite es fundamental para prevenir el sobrecalentamiento, mejorar la confiabilidad de los activos y respaldar estrategias de mantenimiento predictivo en sistemas de distribución eléctrica.

---

## Acerca del Dataset

**Dominio:** Analítica Energética y Monitoreo Industrial
**Variable Objetivo:** `OT` (Temperatura del Aceite)

El proyecto utiliza el conjunto de datos ETTh1 de la colección ETDataset, introducido originalmente en un artículo de investigación.

El dataset contiene aproximadamente dos años de mediciones horarias recopiladas en estaciones de transformadores eléctricos.

### Variables Disponibles

* HUFL — Carga Útil Alta
* HULL — Carga No Útil Alta
* MUFL — Carga Útil Media
* MULL — Carga No Útil Media
* LUFL — Carga Útil Baja
* LULL — Carga No Útil Baja
* OT — Temperatura del Aceite (Objetivo)

Además, se extraen características temporales a partir de las marcas de tiempo:

* Mes
* Día
* Hora

---

## Objetivo del Proyecto

Construir y comparar arquitecturas modernas de Deep Learning capaces de:

* Aprender dependencias temporales de corto y largo plazo
* Pronosticar la temperatura del aceite de transformadores a partir de secuencias multivariables
* Evaluar la efectividad de modelos basados en atención y en descomposición de series temporales
* Medir tanto el rendimiento predictivo como la eficiencia computacional

---

## Estrategia de Pronóstico

El problema se formula como una tarea supervisada de pronóstico de series temporales multivariables.

### Enfoque de Ventana Deslizante

Las observaciones históricas se transforman en secuencias de longitud fija mediante ventanas móviles.

* Ventana de Entrada: 48 horas
* Horizonte de Pronóstico: 1 paso hacia adelante

Esto permite que las redes neuronales aprendan patrones temporales directamente del comportamiento secuencial de los transformadores.

---

## Arquitecturas de Deep Learning

Se implementan y comparan cinco arquitecturas modernas.

### LSTM-Attention

### Transformer Vanilla

### PatchTST

### DLinear

### ConvTransformer

---

## Pipeline de Deep Learning

El proyecto incluye un flujo de trabajo completo para pronóstico:

* Ingesta y preprocesamiento de datos
* Análisis exploratorio de datos (EDA)
* Ingeniería de características temporales
* Normalización de datos
* Generación de secuencias mediante ventanas deslizantes
* Creación de DataLoaders en PyTorch
* Implementación de modelos
* Optimización de hiperparámetros con Optuna
* Entrenamiento con Early Stopping
* Evaluación comparativa (benchmark)
* Visualización de pronósticos

---

## Optimización de Hiperparámetros

### Optuna

Se utiliza optimización bayesiana para identificar la tasa de aprendizaje óptima durante el entrenamiento.

**Parámetro Optimizado:**

* Learning Rate (Tasa de Aprendizaje)

La mejor configuración obtenida se aplica posteriormente a todos los experimentos del benchmark.

---

## Métricas de Evaluación

El rendimiento de los modelos se evalúa mediante:

* MAE (Error Absoluto Medio)
* MSE (Error Cuadrático Medio)
* R² Score
* Tiempo de Entrenamiento

Esto permite comparar la precisión de los pronósticos y la eficiencia computacional de cada arquitectura.

---

## Visualizaciones

El proyecto genera diversas visualizaciones analíticas:

### Análisis Exploratorio

* Serie Temporal de la Temperatura del Aceite
* Mapa de Calor de Correlaciones

### Análisis Comparativo

* Comparación de MAE
* Comparación de MSE
* Comparación de Tiempo de Ejecución

### Análisis de Pronóstico

* Curvas de Temperatura Real vs. Predicha
* Visualización del Rendimiento de Pronóstico

---

## Aplicaciones Industriales

* Mantenimiento predictivo
* Monitoreo de la salud de transformadores
* Confiabilidad de redes eléctricas
* Sistemas de soporte a decisiones SCADA
* Gestión de carga
* Detección de riesgos térmicos
* Optimización del ciclo de vida de activos
* Analítica de infraestructura energética

---

## Licencia

Proyecto de Deep Learning con fines educativos y de investigación.

---

## Autor

**Armando Guarnera**
Data Scientist
Argentina
