# Automatidata: predicción de pasajeros generosos con modelos de clasificación

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/5.%20The%20Nuts%20and%20bolts%20of%20machine%20learning/automatidata_machine_learning_colab.ipynb)

## Caso de estudio

En este proyecto trabajé como profesional de datos para **Automatidata**, una empresa consultora ficticia que colabora con la **New York City Taxi and Limousine Commission (NYC TLC)**.

Después de completar el análisis exploratorio, las pruebas estadísticas y un modelo de regresión para predecir tarifas, el siguiente desafío fue construir un modelo de clasificación que ayudara a comprender el comportamiento de las propinas.

La solicitud inicial del cliente fue predecir qué pasajeros probablemente no dejarían propina para alertar a los conductores. Sin embargo, este uso presenta un riesgo ético importante: una predicción incorrecta podría provocar discriminación, tiempos de espera más largos o un trato desigual hacia determinados pasajeros.

Por esta razón, el objetivo se reformuló.

> El modelo busca identificar patrones asociados con pasajeros generosos, definidos como clientes que dejan una propina igual o superior al 20 %, y debe utilizarse para análisis interno y planificación, no para negar o retrasar el servicio a personas específicas.

## Objetivo del proyecto

El objetivo técnico fue construir, ajustar y comparar modelos de clasificación basados en árboles para predecir si un viaje realizado con tarjeta de crédito corresponde a un pasajero generoso.

Los modelos utilizados fueron:

- **Random Forest**
- **XGBoost**

El desempeño se comparó principalmente mediante el **F1-score**, porque combina precisión y recall y permite evaluar de forma equilibrada los falsos positivos y falsos negativos.

## Archivos del proyecto

```text
5. The Nuts and bolts of machine learning/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── nyc_preds_means.csv
├── README.md
├── requirements.txt
└── automatidata_machine_learning_colab.ipynb
```

### Descripción

- `automatidata_machine_learning_colab.ipynb`: notebook principal con el análisis ético, la ingeniería de variables y los modelos.
- `2017_Yellow_Taxi_Trip_Data.csv`: dataset original de viajes de taxis amarillos de Nueva York.
- `nyc_preds_means.csv`: variables generadas en el proyecto de regresión anterior.
- `requirements.txt`: dependencias necesarias para ejecutar el notebook.
- `README.md`: documentación del caso y del desarrollo técnico.

## Fuentes de datos

El notebook trabaja con dos conjuntos de datos.

### Dataset original

```text
2017_Yellow_Taxi_Trip_Data.csv
```

Incluye información como:

- Fecha y hora de recogida.
- Fecha y hora de llegada.
- Cantidad de pasajeros.
- Distancia del viaje.
- Código de tarifa.
- Ubicación de origen.
- Ubicación de destino.
- Tipo de pago.
- Monto de la tarifa.
- Propina.
- Peajes.
- Monto total.

### Variables del proyecto anterior

```text
nyc_preds_means.csv
```

Incluye:

- `mean_duration`
- `mean_distance`
- `predicted_fare`

Estas variables fueron generadas en el proyecto anterior de regresión lineal múltiple.

Si el archivo `nyc_preds_means.csv` no está disponible, el notebook reconstruye automáticamente estas columnas a partir del dataset original para mantener la compatibilidad con Google Colab.

## Carga compatible con Google Colab

El notebook:

1. Busca los archivos en el entorno local.
2. Busca los archivos en `/content`, que es la carpeta habitual de Colab.
3. Intenta descargarlos desde GitHub.
4. Reconstruye `nyc_preds_means.csv` si no está disponible.

Esto permite ejecutar el proyecto desde Colab sin cargar manualmente todos los archivos.

## Qué se hace en el código

## 1. Consideraciones éticas

Antes de entrenar el modelo se analiza el impacto potencial de sus predicciones.

### Falso positivo

El modelo predice que una persona será generosa, pero finalmente no deja una propina alta.

Posibles consecuencias:

- Frustración del conductor.
- Expectativas incorrectas.
- Pérdida de confianza en el sistema.

### Falso negativo

El modelo predice que una persona no será generosa, pero sí deja una propina alta.

Posibles consecuencias:

- Trato injusto al pasajero.
- Discriminación.
- Mayor tiempo de espera.
- Deterioro de la reputación de la organización.

Por ello, se recomienda que las predicciones se utilicen para análisis agregado y no como alertas individuales en tiempo real.

## 2. Importación de bibliotecas

El proyecto utiliza:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import GridSearchCV, train_test_split
from sklearn.ensemble import RandomForestClassifier
from xgboost import XGBClassifier
```

También se importan métricas de clasificación:

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    confusion_matrix,
    ConfusionMatrixDisplay
)
```

## 3. Unión de los datasets

El dataset original se combina con las variables del proyecto anterior mediante el índice:

```python
df0 = df0.merge(
    nyc_preds_means,
    left_index=True,
    right_index=True
)
```

El resultado incluye las variables originales y:

- `mean_duration`
- `mean_distance`
- `predicted_fare`

## 4. Filtrado de pagos con tarjeta

El análisis utiliza únicamente viajes pagados con tarjeta:

```python
df1 = df0.copy()
df1 = df1[df1["payment_type"] == 1]
```

Este filtro es necesario porque las propinas en efectivo no siempre quedan registradas en el sistema.

## 5. Creación del porcentaje de propina

Se calcula:

```python
df1["tip_percent"] = (
    df1["tip_amount"]
    / (df1["total_amount"] - df1["tip_amount"])
).round(3)
```

El resultado se redondea a tres decimales para evitar errores derivados de la aritmética de punto flotante.

## 6. Creación de la variable objetivo

La variable objetivo es:

```text
generous
```

Se define así:

```python
df1["generous"] = (
    df1["tip_percent"] >= 0.20
).astype(int)
```

Interpretación:

| Valor | Significado |
|---:|---|
| 0 | Propina inferior al 20 % |
| 1 | Propina igual o superior al 20 % |

El problema se transforma así en una tarea de clasificación binaria.

## 7. Variables de fecha y hora

Las fechas se convierten a formato `datetime`:

```python
df1["tpep_pickup_datetime"] = pd.to_datetime(
    df1["tpep_pickup_datetime"]
)
```

Después se crea la variable del día:

```python
df1["day"] = (
    df1["tpep_pickup_datetime"]
    .dt.day_name()
    .str.lower()
)
```

También se crea el mes abreviado:

```python
df1["month"] = (
    df1["tpep_pickup_datetime"]
    .dt.strftime("%b")
    .str.lower()
)
```

## 8. Franjas horarias

El código clasifica los viajes en cuatro períodos:

| Variable | Horario |
|---|---|
| `am_rush` | 06:00–10:00 |
| `daytime` | 10:00–16:00 |
| `pm_rush` | 16:00–20:00 |
| `nighttime` | 20:00–06:00 |

Cada variable es binaria:

```text
1 = el viaje ocurrió en esa franja
0 = el viaje no ocurrió en esa franja
```

Ejemplo:

```python
def am_rush(hour):
    if 6 <= hour < 10:
        return 1
    return 0
```

## 9. Prevención de fuga de datos

Antes de entrenar los modelos se eliminan variables que revelarían directamente la respuesta o que no estarían disponibles en el momento de la predicción.

Entre ellas:

- `tip_amount`
- `tip_percent`
- `total_amount`
- `payment_type`
- `fare_amount`
- `tolls_amount`
- `trip_distance`
- Variables originales de fecha y hora

La variable objetivo `generous` se conserva.

Este paso es fundamental para evitar **data leakage**.

## 10. Codificación de variables categóricas

Los códigos de ubicación y tarifa se convierten a texto:

```python
df1["RatecodeID"] = df1["RatecodeID"].astype(str)
df1["PULocationID"] = df1["PULocationID"].astype(str)
df1["DOLocationID"] = df1["DOLocationID"].astype(str)
```

Luego se convierten a variables dummy:

```python
df2 = pd.get_dummies(
    df1,
    drop_first=True,
    dtype=int
)
```

## 11. Revisión del balance de clases

El código examina:

```python
df2["generous"].value_counts()
df2["generous"].value_counts(normalize=True)
```

Las clases se encuentran relativamente equilibradas, por lo que no fue necesario aplicar técnicas especiales de sobremuestreo o submuestreo.

## 12. División de entrenamiento y prueba

Se separan los datos mediante:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    stratify=y,
    random_state=42
)
```

- 80 % para entrenamiento.
- 20 % para prueba.
- Estratificación para mantener la proporción de clases.

## 13. Modelo Random Forest

Se crea:

```python
rf = RandomForestClassifier(
    random_state=42
)
```

El modelo se ajusta mediante `GridSearchCV` para buscar combinaciones de:

- Profundidad máxima.
- Número de árboles.
- Número mínimo de muestras por hoja.
- Número mínimo de muestras para dividir un nodo.
- Proporción de observaciones usadas por árbol.
- Cantidad de variables consideradas en cada división.

Ejemplo:

```python
rf1 = GridSearchCV(
    estimator=rf,
    param_grid=cv_params,
    scoring=[
        "accuracy",
        "precision",
        "recall",
        "f1"
    ],
    cv=3,
    refit="f1",
    n_jobs=-1
)
```

El mejor modelo se selecciona según el F1-score.

## 14. Modelo XGBoost

También se construye:

```python
xgb = XGBClassifier(
    objective="binary:logistic",
    random_state=42,
    eval_metric="logloss"
)
```

Se ajustan hiperparámetros como:

- `max_depth`
- `min_child_weight`
- `learning_rate`
- `n_estimators`

XGBoost construye árboles secuencialmente, intentando corregir los errores cometidos por los árboles anteriores.

## 15. Métricas de evaluación

Los modelos se comparan mediante:

### Accuracy

Proporción total de predicciones correctas.

### Precision

Entre los pasajeros clasificados como generosos, cuántos realmente lo eran.

### Recall

Entre los pasajeros realmente generosos, cuántos fueron detectados.

### F1-score

Media armónica entre precision y recall.

```python
f1_score(y_test, predictions)
```

El F1-score se utiliza como métrica principal porque equilibra ambos tipos de error.

## 16. Matriz de confusión

Se genera una matriz de confusión para visualizar:

- Verdaderos negativos.
- Falsos positivos.
- Falsos negativos.
- Verdaderos positivos.

```python
cm = confusion_matrix(
    y_test,
    xgb_preds
)
```

Esta visualización permite analizar qué tipo de error comete con mayor frecuencia el modelo.

## 17. Importancia de variables

El modelo XGBoost permite identificar qué características contribuyen más a las predicciones:

```python
plot_importance(
    xgb1.best_estimator_,
    importance_type="gain",
    max_num_features=10
)
```

Esto ayuda a detectar patrones relacionados con:

- Ubicación de origen.
- Ubicación de destino.
- Horario.
- Día.
- Mes.
- Distancia promedio.
- Duración promedio.
- Tarifa predicha.
- Cantidad de pasajeros.

## Resultado del proyecto

El proyecto compara Random Forest y XGBoost usando validación cruzada y datos de prueba.

La selección final debe considerar:

- F1-score.
- Diferencia entre validación y prueba.
- Precision.
- Recall.
- Estabilidad del modelo.
- Facilidad de interpretación.
- Impacto ético de los errores.

XGBoost suele ofrecer un desempeño competitivo porque cada árbol intenta corregir los errores de los anteriores. Sin embargo, el modelo no debe utilizarse para decidir si un pasajero merece o no recibir servicio.

## Interpretación para NYC TLC

El modelo puede utilizarse para:

- Analizar patrones generales de propinas.
- Identificar horarios con mayores tasas de propina.
- Estudiar zonas de origen y destino.
- Diseñar estrategias de servicio.
- Crear programas de capacitación para conductores.
- Comprender factores asociados con viajes de mayor ingreso.
- Mejorar políticas internas sin identificar ni penalizar a pasajeros individuales.

## Recomendación ética

No se recomienda mostrar predicciones individuales a los conductores.

Un uso responsable sería:

- Analizar tendencias agregadas.
- Planificar turnos y cobertura.
- Evaluar zonas y horarios.
- Mejorar la experiencia del pasajero.
- Diseñar incentivos generales.
- Evitar decisiones que afecten negativamente a una persona específica.

## Limitaciones

- Las propinas en efectivo no están completamente registradas.
- El modelo usa datos históricos y puede reproducir sesgos existentes.
- La asociación entre variables no demuestra causalidad.
- Cambios en tarifas o comportamiento pueden reducir el desempeño futuro.
- Los identificadores geográficos pueden actuar como proxies de características sensibles.
- El modelo debe revisarse periódicamente para detectar sesgos y degradación.
- La calidad de `nyc_preds_means.csv` influye en los resultados.
- Las variables deben estar disponibles antes o durante el viaje para poder usarse en producción.

## Cómo ejecutar el proyecto

### Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio del README.

Después selecciona:

```text
Entorno de ejecución → Ejecutar todas
```

El notebook intentará cargar los archivos desde GitHub. Si no encuentra `nyc_preds_means.csv`, reconstruirá automáticamente sus columnas.

### Ejecución local

Clona el repositorio:

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

Entra en la carpeta:

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/5. The Nuts and bolts of machine learning"
```

Crea un entorno virtual:

```bash
python -m venv .venv
```

Actívalo en Windows:

```bash
.venv\Scripts\activate
```

Actívalo en macOS o Linux:

```bash
source .venv/bin/activate
```

Instala las dependencias:

```bash
pip install -r requirements.txt
```

Abre el notebook:

```bash
jupyter notebook automatidata_machine_learning_colab.ipynb
```

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- Jupyter Notebook
- Google Colab
- GitHub

## Curso

**Google Advanced Data Analytics Professional Certificate**

Curso 5: **The Nuts and Bolts of Machine Learning**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
