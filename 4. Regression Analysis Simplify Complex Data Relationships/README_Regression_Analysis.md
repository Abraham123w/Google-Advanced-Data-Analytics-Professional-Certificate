# Automatidata: predicción de tarifas de taxi con regresión lineal múltiple

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/4.%20Regression%20Analysis%20Simplify%20Complex%20Data%20Relationships/automatidata_multiple_linear_regression.ipynb)

## Caso de negocio

En este proyecto trabajé como profesional de datos para **Automatidata**, una empresa consultora ficticia que desarrolla una solución para la **New York City Taxi and Limousine Commission (NYC TLC)**.

Después de completar la planificación inicial, el análisis exploratorio y las pruebas estadísticas, el siguiente objetivo fue construir un modelo capaz de **predecir el monto de la tarifa de un viaje en taxi antes de que este finalice**.

La pregunta central fue:

> ¿Qué características conocidas al inicio de un viaje permiten estimar su tarifa y con qué precisión puede realizarse esa predicción?

Para responderla se desarrolló un modelo de **regresión lineal múltiple** utilizando información histórica de viajes de taxis amarillos de Nueva York.

## Objetivo del proyecto

El notebook busca:

- Explorar y limpiar los datos.
- Identificar valores inválidos y atípicos.
- Crear variables predictoras a partir de la ruta y del momento del viaje.
- Preparar variables numéricas y categóricas.
- Entrenar un modelo de regresión lineal múltiple.
- Evaluar su capacidad predictiva.
- Analizar los residuos y los coeficientes.
- Traducir los resultados en conclusiones útiles para NYC TLC.

## Estructura del proyecto

```text
4. Regression Analysis Simplify Complex Data Relationships/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── automatidata_multiple_linear_regression.ipynb
```

## Carga del dataset

El notebook descarga el CSV directamente desde GitHub, por lo que puede ejecutarse en Google Colab sin subir el archivo manualmente:

```python
DATA_URL = (
    "https://raw.githubusercontent.com/"
    "Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/"
    "main/4.%20Regression%20Analysis%20Simplify%20Complex%20Data%20Relationships/"
    "2017_Yellow_Taxi_Trip_Data.csv"
)

df0 = pd.read_csv(DATA_URL)
```

El dataset contiene:

- **22.699 viajes**
- **18 variables originales**

Entre las columnas principales se encuentran:

- `tpep_pickup_datetime`
- `tpep_dropoff_datetime`
- `passenger_count`
- `trip_distance`
- `PULocationID`
- `DOLocationID`
- `fare_amount`
- `tip_amount`
- `tolls_amount`
- `total_amount`

## Qué se hace en el código

### 1. Importación de bibliotecas

Se utilizan herramientas para manipulación de datos, visualización, preprocesamiento y modelado:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import (
    mean_absolute_error,
    mean_squared_error,
    r2_score
)
```

### 2. Exploración inicial

El notebook revisa:

```python
df0.shape
df0.info()
df0.isna().sum()
df0.duplicated().sum()
df0.describe()
```

Esta etapa permite comprobar:

- Dimensiones del dataset.
- Tipos de datos.
- Valores faltantes.
- Filas duplicadas.
- Estadísticas descriptivas.
- Posibles problemas de calidad.

En la base analizada no se identificaron valores nulos ni filas duplicadas.

### 3. Conversión de fechas

Las columnas de recogida y llegada se convierten a `datetime`:

```python
df0["tpep_pickup_datetime"] = pd.to_datetime(
    df0["tpep_pickup_datetime"]
)

df0["tpep_dropoff_datetime"] = pd.to_datetime(
    df0["tpep_dropoff_datetime"]
)
```

Esto permite calcular la duración y extraer información temporal.

### 4. Creación de la duración del viaje

Se calcula la duración en minutos:

```python
df0["duration"] = (
    df0["tpep_dropoff_datetime"]
    - df0["tpep_pickup_datetime"]
).dt.total_seconds() / 60
```

La duración real no se utiliza directamente como predictor final porque no se conoce antes de terminar el viaje. En su lugar, posteriormente se crea una duración promedio histórica por ruta.

### 5. Detección de valores atípicos

Se crean boxplots para:

- `trip_distance`
- `fare_amount`
- `duration`

```python
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

sns.boxplot(data=df0, x="trip_distance", ax=axes[0])
sns.boxplot(data=df0, x="fare_amount", ax=axes[1])
sns.boxplot(data=df0, x="duration", ax=axes[2])
```

El análisis muestra valores extremos en las tres variables.

También se detectan:

- Tarifas negativas.
- Duraciones negativas.
- Viajes con distancia igual a cero.
- Montos y duraciones excesivamente altos.

### 6. Tratamiento de valores inválidos y extremos

Los valores negativos se reemplazan por cero:

```python
df0.loc[
    df0["fare_amount"] < 0,
    "fare_amount"
] = 0

df0.loc[
    df0["duration"] < 0,
    "duration"
] = 0
```

Para los extremos superiores se utiliza:

```text
Q3 + 6 × IQR
```

La función creada en el notebook limita los valores que superan ese umbral.

Los límites obtenidos son aproximadamente:

- `fare_amount`: **62,50 USD**
- `duration`: **88,78 minutos**

Este tratamiento evita que observaciones extremas dominen los coeficientes del modelo.

### 7. Identificación de la tarifa fija de JFK

Al analizar las tarifas superiores a 50 USD se observa una concentración en:

```text
52 USD
```

Esta línea corresponde a la tarifa fija histórica de ciertos viajes relacionados con el aeropuerto JFK y no debe tratarse automáticamente como un error.

### 8. Creación de la ruta de cada viaje

Se combinan las ubicaciones de origen y destino:

```python
df0["pickup_dropoff"] = (
    df0["PULocationID"].astype(str)
    + " "
    + df0["DOLocationID"].astype(str)
)
```

La dirección importa: una ruta de A hacia B se considera distinta de una ruta de B hacia A.

### 9. Creación de `mean_distance`

El código calcula la distancia promedio histórica para cada combinación de origen y destino:

```python
grouped = (
    df0.groupby("pickup_dropoff")[["trip_distance"]]
    .mean()
)
```

Luego asigna esa media a cada viaje:

```python
df0["mean_distance"] = (
    df0["pickup_dropoff"]
    .map(grouped.to_dict()["trip_distance"])
)
```

Esta variable representa la distancia esperada de una ruta conocida.

### 10. Creación de `mean_duration`

Se repite el mismo procedimiento para obtener la duración promedio histórica por ruta:

```python
grouped_duration = (
    df0.groupby("pickup_dropoff")[["duration"]]
    .mean()
)

df0["mean_duration"] = (
    df0["pickup_dropoff"]
    .map(grouped_duration.to_dict()["duration"])
)
```

Esta variable permite incorporar información de duración sin utilizar la duración real del viaje que se quiere predecir.

### 11. Variables de día y mes

Se crean las columnas:

```python
df0["day"] = (
    df0["tpep_pickup_datetime"]
    .dt.day_name()
    .str.lower()
)

df0["month"] = (
    df0["tpep_pickup_datetime"]
    .dt.month_name()
    .str.lower()
)
```

Estas variables permiten estudiar efectos temporales sobre la tarifa.

### 12. Variable `rush_hour`

Se identifica si un viaje ocurrió en horario punta durante un día laboral.

Los intervalos definidos son:

- 06:00 a 10:00
- 16:00 a 20:00
- Solo de lunes a viernes

La columna toma:

```text
1 = horario punta
0 = fuera del horario punta
```

### 13. Relación entre duración promedio y tarifa

Se crea un gráfico de dispersión:

```python
sns.scatterplot(
    x="mean_duration",
    y="fare_amount",
    data=df0,
    alpha=0.5
)
```

La visualización muestra una relación positiva general: las rutas con mayor duración promedio tienden a tener tarifas más altas.

### 14. Selección de variables para el modelo

Se eliminan columnas que:

- Son identificadores.
- Repiten información.
- No estarán disponibles antes de terminar el viaje.
- Forman parte directa del monto total.
- No son necesarias en el modelo final.

El dataset de modelado conserva:

- `VendorID`
- `passenger_count`
- `mean_distance`
- `mean_duration`
- `day`
- `month`
- `rush_hour`
- `fare_amount` como variable objetivo

### 15. Correlaciones

El notebook utiliza un pairplot y un mapa de calor para examinar las relaciones entre las variables:

```python
sns.heatmap(
    df2.corr(numeric_only=True),
    annot=True,
    cmap="coolwarm"
)
```

Los predictores con mayor relación con la tarifa son:

- `mean_distance`
- `mean_duration`

Estas dos variables también están relacionadas entre sí, lo que debe considerarse al interpretar los coeficientes.

### 16. Separación entre predictores y objetivo

La variable objetivo es:

```python
y = df2["fare_amount"]
```

Las demás variables conforman:

```python
X = df2.drop(columns=["fare_amount"])
```

### 17. Codificación de variables categóricas

`VendorID`, `day` y `month` se convierten mediante variables dummy:

```python
X = pd.get_dummies(
    X,
    drop_first=True,
    dtype=int
)
```

Esto permite utilizarlas en un modelo matemático.

### 18. División en entrenamiento y prueba

Se utiliza:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=0
)
```

- 80 % para entrenamiento.
- 20 % para evaluación.

### 19. Estandarización

Los predictores se estandarizan con:

```python
scaler = StandardScaler()
scaler.fit(X_train)

X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

El escalador se ajusta únicamente con los datos de entrenamiento para evitar utilizar información del conjunto de prueba.

### 20. Entrenamiento del modelo

Se utiliza una regresión lineal múltiple:

```python
model = LinearRegression()
model.fit(X_train_scaled, y_train)
```

### 21. Evaluación del modelo

El notebook calcula:

- Coeficiente de determinación `R²`.
- Error absoluto medio `MAE`.
- Error cuadrático medio `MSE`.
- Raíz del error cuadrático medio `RMSE`.
- Suma de cuadrados de los residuos `RSS`.

Resultados aproximados obtenidos con el código:

| Conjunto | R² | MAE | RMSE |
|---|---:|---:|---:|
| Entrenamiento | 0,8405 | 2,18 USD | 4,22 USD |
| Prueba | 0,8686 | 2,13 USD | 3,78 USD |

El modelo explica aproximadamente el **86,9 % de la variación de las tarifas en el conjunto de prueba**.

El error absoluto promedio es cercano a **2,13 USD por viaje**.

### 22. Análisis de residuos

El notebook crea:

- Gráfico de valores reales frente a predicciones.
- Histograma de residuos.
- Gráfico de residuos frente a valores predichos.
- Cálculo de la media residual.

La media de los residuos es cercana a cero:

```text
-0,02 USD
```

Esto indica que el modelo no presenta un sesgo promedio importante hacia la sobreestimación o subestimación.

### 23. Interpretación de coeficientes

Los coeficientes se ordenan según su magnitud absoluta.

Los predictores más influyentes son:

1. `mean_distance`
2. `mean_duration`

Las variables de día, mes, proveedor y horario punta presentan efectos comparativamente menores.

Debido a que los predictores fueron estandarizados, sus coeficientes representan el cambio esperado en la tarifa cuando una variable aumenta una desviación estándar, manteniendo las demás constantes.

## Resultado principal

El modelo de regresión lineal múltiple logra predecir las tarifas con un desempeño sólido:

- `R²` de prueba cercano a 0,87.
- Error absoluto promedio cercano a 2,13 USD.
- Resultados similares entre entrenamiento y prueba.
- Residuos centrados aproximadamente en cero.

La distancia promedio histórica de la ruta es el predictor más importante, seguida por la duración promedio de esa misma ruta.

## Interpretación para NYC TLC

El modelo podría utilizarse como base para:

- Estimar tarifas antes de iniciar un viaje.
- Informar al pasajero un rango esperado.
- Detectar cobros atípicos.
- Comparar tarifas reales con valores estimados.
- Apoyar la planificación y supervisión del servicio.
- Identificar rutas cuyo comportamiento difiere del patrón histórico.

## Limitaciones

El modelo presenta algunas limitaciones importantes:

- La relación entre variables puede no ser completamente lineal.
- `mean_distance` y `mean_duration` están correlacionadas.
- Las tarifas especiales pueden seguir reglas distintas.
- Los resultados dependen de la calidad de los datos históricos.
- El modelo no incorpora tráfico en tiempo real, clima ni eventos.
- Los promedios de ruta deben calcularse únicamente con datos históricos disponibles al momento de predecir.

### Riesgo de fuga de información

En un flujo de producción, `mean_distance` y `mean_duration` deberían calcularse usando solamente el conjunto de entrenamiento y después aplicarse al conjunto de prueba.

Calcular estos promedios con todo el dataset antes de dividirlo puede introducir información del conjunto de prueba y producir una estimación ligeramente optimista del rendimiento.

## Cómo ejecutar el proyecto

### Google Colab

Haz clic en el botón **Abrir en Google Colab** al inicio del README.

Después selecciona:

```text
Entorno de ejecución → Ejecutar todas
```

### Ejecución local

Clona el repositorio:

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

Entra en la carpeta:

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/4. Regression Analysis Simplify Complex Data Relationships"
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
jupyter notebook automatidata_multiple_linear_regression.ipynb
```

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook
- Google Colab
- GitHub

## Curso

**Google Advanced Data Analytics Professional Certificate**

Curso 4: **Regression Analysis: Simplify Complex Data Relationships**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
