*Leer este documento en otros idiomas: [English](README.md)*

# Proyecto de Deep Learning — Pronóstico Multivariable de Series Temporales para la Temperatura del Aceite de Transformadores Eléctricos

Este proyecto desarrolla un benchmark integral de Deep Learning para el pronóstico de series temporales multivariables utilizando el Electricity Transformer Dataset (ETDataset).

El objetivo es predecir la Temperatura del Aceite (OT) futura de transformadores eléctricos a partir de mediciones históricas de temperatura, variables de carga eléctrica y características temporales. Un pronóstico preciso de la temperatura del aceite es fundamental para prevenir el sobrecalentamiento, mejorar la confiabilidad de los activos y respaldar estrategias de mantenimiento predictivo en sistemas de distribución eléctrica.

---

## Acerca del Dataset

**Dominio:** Analítica Energética y Monitoreo Industrial
**Variable Objetivo:** `OT` (Temperatura del Aceite)

El proyecto utiliza el conjunto de datos ETTh1 de la colección [ETDataset](https://github.com/zhouhaoyi/ETDataset), introducido originalmente en el artículo de investigación:

> Zhou, H. et al. (2021). *Informer: Beyond Efficient Transformer for Long Sequence Time-Series Forecasting.* AAAI 2021.

Una copia del repositorio oficial se incluye en `data/ETDataset-main.zip`.

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
LSTM bidireccional con una capa de atención aditiva que pondera los 48 estados ocultos antes de la cabeza de regresión.

### Transformer Vanilla
Proyección lineal de la entrada + codificación posicional sinusoidal + 2 capas encoder de Transformer, con pooling promedio en el tiempo.

### PatchTST
La ventana de 48 horas se divide en 4 parches de 12 horas; cada parche es un token para un encoder Transformer de 2 capas.

### DLinear
Descompone la ventana en tendencia (media móvil) y componente estacional y aplica una capa lineal a cada una.

### ConvTransformer
Convolución 1D para extraer patrones locales, seguida de un encoder Transformer de 2 capas.

### Línea Base Ingenua (Persistencia)
`OT(t) = OT(t-1)`. En un pronóstico a un paso de una serie muy autocorrelacionada, es la referencia que cualquier modelo debe superar.

---

## Pipeline de Deep Learning

El proyecto incluye un flujo de trabajo completo para pronóstico:

* Ingesta y preprocesamiento de datos
* Análisis exploratorio de datos (EDA)
* Ingeniería de características temporales
* División cronológica train / validación / test (70% / 10% / 20%)
* Normalización de datos (scalers ajustados solo con train)
* Generación de secuencias mediante ventanas deslizantes
* Creación de DataLoaders en PyTorch
* Implementación de modelos
* Optimización de hiperparámetros con Optuna
* Entrenamiento con Early Stopping (se restauran los mejores pesos)
* Evaluación comparativa (benchmark)
* Visualización de pronósticos

---

## Optimización de Hiperparámetros

### Optuna

Se utiliza optimización bayesiana (sampler TPE, 20 trials sobre el modelo DLinear) para identificar la tasa de aprendizaje óptima durante el entrenamiento.

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

## Resultados

Conjunto de test (último 20% de la serie, 3.484 horas). Métricas en la escala original (°C). Entrenado en una NVIDIA GTX 1650.

| Modelo | MAE (°C) | MSE | R² | Tiempo de entrenamiento (s) |
|---|---|---|---|---|
| **DLinear** | **0.436** | **0.399** | **0.966** | 6.3 |
| Ingenuo (persistencia) | 0.448 | 0.428 | 0.964 | - |
| Vanilla Transformer | 0.604 | 0.601 | 0.949 | 16.3 |
| LSTM-Attention | 0.649 | 0.733 | 0.938 | 9.0 |
| ConvTransformer | 0.747 | 0.946 | 0.920 | 10.5 |
| PatchTST | 0.753 | 0.918 | 0.923 | 13.2 |

**Hallazgos principales**

* DLinear es el modelo más preciso y el más rápido.
* La línea base de persistencia es muy fuerte en el pronóstico a un paso: solo DLinear la supera. Un R² alto por sí solo no demuestra que un modelo aporte valor.
* Próximos pasos: ajuste de hiperparámetros por modelo, horizontes más largos (24h / 48h), variables temporales cíclicas y promedio sobre varias semillas.

---

## Estructura del Proyecto

```
├── data/
│   └── ETDataset-main.zip          # Repositorio oficial de ETDataset (ETTh1 se lee desde aquí)
├── notebooks/
│   └── Forecasting Oil Temperature.ipynb
├── requirements.txt
├── README.md
└── README_es.md
```

---

## Cómo Ejecutar

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows  (Linux/Mac: source .venv/bin/activate)
# Opcional, para GPU NVIDIA:
pip install torch --index-url https://download.pytorch.org/whl/cu126
pip install -r requirements.txt
jupyter notebook "notebooks/Forecasting Oil Temperature.ipynb"
```

El notebook usa la GPU automáticamente si está disponible (también funciona en CPU, pero el entrenamiento es mucho más lento).

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
