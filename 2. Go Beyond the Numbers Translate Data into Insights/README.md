# Automatidata: Exploratory Data Analysis of NYC Taxi Trips

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/2.%20Go%20Beyond%20the%20Numbers%20Translate%20Data%20into%20Insights/trabajo_final_automatidata_eda.ipynb)

## Caso de negocio

En este proyecto trabajé como profesional de datos para **Automatidata**, una empresa consultora ficticia contratada por la **New York City Taxi and Limousine Commission (NYC TLC)**.

El equipo necesitaba comprender mejor el comportamiento de los viajes en taxis amarillos de Nueva York antes de avanzar hacia pruebas estadísticas y modelos predictivos.

La solicitud principal fue realizar un **análisis exploratorio de datos (EDA)** que permitiera:

- Conocer la estructura del dataset.
- Detectar problemas de calidad.
- Identificar valores atípicos.
- Analizar la distribución de las principales variables.
- Estudiar cambios en la cantidad de viajes e ingresos a lo largo del tiempo.
- Crear visualizaciones comprensibles para personas sin experiencia técnica.
- Preparar los datos para futuras etapas del proyecto.

Además, el caso planteó la creación de visualizaciones en Tableau con criterios de accesibilidad, considerando que parte de la audiencia podía presentar dificultades visuales.

## Objetivo del análisis

El objetivo fue limpiar, estructurar y explorar los datos de viajes para responder preguntas como:

- ¿Cómo se distribuye la distancia de los viajes?
- ¿Qué valores atípicos aparecen en las tarifas, propinas y montos totales?
- ¿Cómo cambia la cantidad de viajes según el mes y el día de la semana?
- ¿Qué días y meses generan mayores ingresos?
- ¿Existen diferencias en las propinas según el proveedor?
- ¿Cómo se relacionan la distancia, la duración y el monto total del viaje?
- ¿Qué ubicaciones de destino presentan mayores distancias promedio o mayor cantidad de viajes?

## Qué se hace en el código

### 1. Importación de bibliotecas

El notebook utiliza:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

- `pandas` para cargar, transformar, agrupar y resumir los datos.
- `numpy` para cálculos numéricos.
- `matplotlib` para crear gráficos.
- `seaborn` para generar visualizaciones estadísticas.

## 2. Carga del dataset desde GitHub

El archivo se carga directamente desde el repositorio para que el notebook pueda ejecutarse en Google Colab:

```python
url = (
    "https://raw.githubusercontent.com/"
    "Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/"
    "main/2.%20Go%20Beyond%20the%20Numbers%20Translate%20Data%20into%20Insights/"
    "2017_Yellow_Taxi_Trip_Data.csv"
)

df = pd.read_csv(url)
```

Esto permite abrir el notebook y ejecutar todas las celdas sin subir manualmente el CSV.

## 3. Exploración inicial del dataset

El código comienza revisando los primeros registros:

```python
print(df.head(10))
```

También examina el tamaño y la forma de la base:

```python
print(f"El tamaño del dataset es: {df.size}")
print(f"La forma del dataset es: {df.shape}")
```

El dataset contiene:

- **22.699 viajes**
- **18 variables**

Después se utilizan:

```python
df.describe()
df.info()
```

Estas funciones permiten revisar:

- Tipos de datos.
- Valores mínimos y máximos.
- Promedios.
- Medianas y cuartiles.
- Valores nulos.
- Posibles anomalías.

## 4. Preparación de las fechas

Las variables de recogida y llegada se convierten a formato `datetime`:

```python
df["tpep_pickup_datetime"] = pd.to_datetime(
    df["tpep_pickup_datetime"]
)

df["tpep_dropoff_datetime"] = pd.to_datetime(
    df["tpep_dropoff_datetime"]
)
```

Esta transformación permite:

- Calcular la duración de los viajes.
- Obtener el mes.
- Obtener el día de la semana.
- Realizar análisis temporales.

## 5. Análisis de la distancia de los viajes

El notebook crea un boxplot para detectar valores atípicos:

```python
sns.boxplot(
    x=df["trip_distance"],
    fliersize=1
)
```

También genera un histograma:

```python
sns.histplot(
    df["trip_distance"],
    bins=range(0, 26, 1)
)
```

Estas visualizaciones muestran que:

- La mayoría de los viajes tiene una distancia relativamente corta.
- La distribución está sesgada hacia la derecha.
- Existen viajes considerablemente más largos que el resto.

## 6. Análisis del monto total

Se crea un boxplot de `total_amount`:

```python
sns.boxplot(
    x=df["total_amount"],
    fliersize=1
)
```

Después se genera un histograma para observar la distribución de los montos:

```python
sns.histplot(
    df["total_amount"],
    bins=range(-10, 101, 5)
)
```

El análisis permite detectar:

- Montos negativos.
- Valores extremadamente altos.
- Concentración de viajes en montos relativamente bajos.
- Asimetría positiva en la distribución.

Estos valores deben investigarse antes de utilizar la información en un modelo predictivo.

## 7. Análisis de las propinas

El notebook filtra los pagos realizados con tarjeta de crédito:

```python
credit_card_trips = df[
    df["payment_type"] == 1
]
```

Este filtro es importante porque las propinas en efectivo normalmente no quedan registradas electrónicamente.

Después se crean:

- Un boxplot de propinas.
- Un histograma de propinas.
- Una comparación de propinas por proveedor.
- Un análisis específico de propinas superiores a 10 USD.

Ejemplo:

```python
sns.histplot(
    data=df_cc,
    x="tip_amount",
    bins=range(0, 21, 1),
    hue="VendorID",
    multiple="dodge",
    shrink=0.8,
    palette="pastel"
)
```

Este análisis permite observar si alguno de los proveedores concentra una mayor cantidad de propinas altas.

## 8. Propina promedio por cantidad de pasajeros

El código calcula la propina promedio para cada cantidad de pasajeros:

```python
mean_tips_by_passenger_count = (
    df.groupby("passenger_count")[["tip_amount"]]
    .mean()
)
```

Después crea un gráfico de barras y agrega una línea de promedio global:

```python
ax.axhline(
    df["tip_amount"].mean(),
    ls="--",
    color="red",
    label="Global mean"
)
```

Esto permite comparar cada grupo de pasajeros con el promedio general de propinas.

## 9. Creación de variables de mes y día

El código crea dos nuevas columnas:

```python
df["month"] = (
    df["tpep_pickup_datetime"]
    .dt.month_name()
)

df["day"] = (
    df["tpep_pickup_datetime"]
    .dt.day_name()
)
```

Estas variables se utilizan para analizar el comportamiento temporal de los viajes.

## 10. Cantidad de viajes por mes

El notebook cuenta los viajes de cada mes:

```python
monthly_rides = df["month"].value_counts()
```

Luego ordena los meses en orden calendario:

```python
month_order = [
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December"
]
```

Finalmente se crea un gráfico de barras que permite comparar la cantidad de viajes durante el año.

## 11. Cantidad de viajes por día de la semana

El código repite el procedimiento para los días:

```python
daily_rides = df["day"].value_counts()
```

Los días se organizan desde lunes hasta domingo y se visualizan mediante un gráfico de barras.

Esto permite identificar:

- Días con mayor demanda.
- Días con menor cantidad de viajes.
- Posibles patrones laborales o de fin de semana.

## 12. Ingresos por día y mes

El notebook calcula los ingresos totales por día:

```python
total_amount_day = (
    df.groupby("day")[["total_amount"]]
    .sum()
)
```

También calcula los ingresos por mes:

```python
total_amount_month = (
    df.groupby("month")[["total_amount"]]
    .sum()
)
```

Las visualizaciones permiten comparar:

- Volumen de viajes.
- Ingresos generados.
- Diferencias entre meses y días.

Esto es útil porque un período con más viajes no siempre tiene que ser el período con mayor ingreso promedio.

## 13. Distancia promedio por ubicación de destino

El código calcula la distancia promedio para cada `DOLocationID`:

```python
distance_by_dropoff = (
    df.groupby("DOLocationID")["trip_distance"]
    .mean()
)
```

Después ordena los resultados y crea un gráfico de barras.

Este análisis ayuda a identificar:

- Destinos asociados a viajes más largos.
- Posibles viajes hacia aeropuertos o zonas alejadas.
- Diferencias geográficas en el comportamiento de los viajes.

## 14. Cantidad de viajes por ubicación de destino

El notebook cuenta los viajes por `DOLocationID`:

```python
distance_by_dropoff = (
    df["DOLocationID"]
    .value_counts()
)
```

Después ordena las ubicaciones desde las menos visitadas hasta las más populares.

Esto permite identificar:

- Zonas con mayor volumen de llegadas.
- Zonas poco frecuentes.
- Posibles concentraciones geográficas de demanda.

## 15. Cálculo de la duración de los viajes

La duración se calcula como la diferencia entre la hora de llegada y la hora de recogida:

```python
df["trip_duration"] = (
    df["tpep_dropoff_datetime"]
    - df["tpep_pickup_datetime"]
).dt.total_seconds() / 60
```

Luego se filtran viajes con duraciones poco razonables:

```python
df_duration_clean = df[
    (df["trip_duration"] > 1)
    & (df["trip_duration"] < 120)
]
```

El objetivo es analizar la distribución típica sin que los errores o valores extremos dominen la visualización.

## 16. Histograma accesible de duración

El código crea un histograma de duración con una línea roja que representa la media:

```python
sns.histplot(
    data=df_duration_clean,
    x="trip_duration",
    bins=30,
    kde=True,
    color="#2c3e50"
)

plt.axvline(
    df_duration_clean["trip_duration"].mean(),
    color="red",
    linestyle="--",
    label=(
        "Mean: "
        f"{df_duration_clean['trip_duration'].mean():.2f} min"
    )
)
```

La visualización utiliza alto contraste y una referencia explícita para facilitar su interpretación.

## Principales hallazgos

El análisis exploratorio muestra que:

- La mayoría de los viajes tiene una distancia corta.
- `trip_distance`, `tip_amount` y `total_amount` presentan asimetría positiva.
- Existen valores atípicos en distancia, propinas y montos.
- Algunos viajes registran distancia igual a cero.
- Existen tarifas y montos negativos.
- Las propinas pueden analizarse con mayor precisión en pagos con tarjeta.
- La cantidad de viajes varía según el mes y el día.
- Los ingresos también presentan variaciones temporales.
- Algunas ubicaciones de destino concentran más viajes.
- Ciertos destinos presentan distancias promedio más altas.
- La duración típica puede analizarse después de filtrar valores poco razonables.

## Interpretación para NYC TLC

Los resultados permiten comprender mejor:

- La distribución de la demanda.
- Los períodos de mayor actividad.
- La generación de ingresos.
- El comportamiento de las propinas.
- La relación entre distancia, duración y monto.
- La importancia de revisar los valores atípicos antes de modelar.

Esta información puede apoyar decisiones relacionadas con:

- Distribución de vehículos.
- Planificación por horarios.
- Priorización de zonas.
- Análisis de viajes de alto valor.
- Preparación de modelos predictivos.

## Limitaciones

El análisis utiliza una muestra de viajes y no necesariamente representa todos los viajes realizados en Nueva York.

Además:

- Los valores extremos pueden ser errores o eventos reales.
- Las propinas en efectivo no quedan completamente registradas.
- Los identificadores de ubicación no incluyen por sí solos el nombre de cada zona.
- Se requiere información geográfica adicional para interpretar completamente los mapas.
- La asociación entre variables no implica causalidad.
- Los datos deben limpiarse y validarse antes del modelado.

## Tableau y accesibilidad

El caso también propone crear un dashboard en Tableau Public con un mapa de viajes por mes.

Para que la visualización sea accesible se recomienda:

- Utilizar títulos claros.
- Mantener un alto contraste.
- Evitar depender únicamente del color.
- Incorporar etiquetas y descripciones.
- Evitar exceso de elementos.
- Diseñar para personas sin experiencia técnica.
- Considerar a usuarios con dificultades visuales.

## Estructura del proyecto

```text
2. Go Beyond the Numbers Translate Data into Insights/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── trabajo_final_automatidata_eda.ipynb
```

## Cómo ejecutar el notebook

### Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio del README.

Luego selecciona:

```text
Entorno de ejecución → Ejecutar todas
```

El dataset se cargará automáticamente desde GitHub.

### Ejecución local

Clona el repositorio:

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

Entra en la carpeta:

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/2. Go Beyond the Numbers Translate Data into Insights"
```

Instala las dependencias:

```bash
pip install -r requirements.txt
```

Abre el notebook:

```bash
jupyter notebook trabajo_final_automatidata_eda.ipynb
```

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook
- Google Colab
- GitHub
- Tableau Public

## Curso

**Google Advanced Data Analytics Professional Certificate**

Curso 2: **Go Beyond the Numbers: Translate Data into Insights**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
