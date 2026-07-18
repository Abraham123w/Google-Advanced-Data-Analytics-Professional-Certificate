# Automatidata: análisis estadístico de tarifas según el tipo de pago

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/3.%20The%20Power%20of%20Statistics/automatidata_statistical_analysis_colab.ipynb)

## Caso de negocio

En este proyecto trabajé como profesional de datos para **Automatidata**, una empresa consultora ficticia contratada por la **New York City Taxi and Limousine Commission (NYC TLC)**.

El equipo recibió una nueva solicitud: analizar la relación entre el **tipo de pago** y el **monto de la tarifa** de los viajes en taxi.

La pregunta principal fue:

> ¿Existe una diferencia estadísticamente significativa entre la tarifa promedio de los viajes pagados con tarjeta de crédito y la tarifa promedio de los viajes pagados en efectivo?

El propósito del análisis fue determinar si la diferencia observada entre ambos grupos podía explicarse por variación aleatoria o si existía evidencia estadística suficiente para concluir que las medias eran distintas.

## Qué se hace en el código

### 1. Importación de bibliotecas

El notebook utiliza:

```python
import pandas as pd
import numpy as np
from scipy import stats
```

- `pandas` para cargar, filtrar y agrupar los datos.
- `numpy` para apoyar operaciones numéricas.
- `scipy.stats` para ejecutar la prueba de hipótesis.

### 2. Carga del dataset desde GitHub

El archivo se carga directamente desde el repositorio para que el notebook pueda ejecutarse en Google Colab:

```python
DATA_URL = (
    "https://raw.githubusercontent.com/"
    "Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/"
    "main/3.%20The%20Power%20of%20Statistics/"
    "2017_Yellow_Taxi_Trip_Data.csv"
)

taxi_data = pd.read_csv(DATA_URL, index_col=0)
```

El dataset contiene información sobre viajes de taxis amarillos de Nueva York, como:

- Distancia del viaje.
- Cantidad de pasajeros.
- Tipo de pago.
- Tarifa.
- Propina.
- Peajes.
- Monto total.

## Exploración inicial

El código revisa la cantidad de viajes según el tipo de pago:

```python
taxi_data["payment_type"].value_counts()
```

La variable `payment_type` está codificada así:

| Código | Tipo de pago |
|---:|---|
| 1 | Tarjeta de crédito |
| 2 | Efectivo |
| 3 | Sin cargo |
| 4 | Disputa |
| 5 | Desconocido |

Después se calcula la tarifa promedio para cada tipo de pago:

```python
taxi_data.groupby("payment_type")["fare_amount"].mean()
```

Esta comparación inicial muestra que los viajes pagados con tarjeta de crédito presentan una tarifa promedio mayor que los viajes pagados en efectivo.

Sin embargo, una diferencia entre promedios no es suficiente para concluir que existe una diferencia real en la población. Por eso se realiza una prueba de hipótesis.

## Hipótesis del análisis

### Hipótesis nula

**H₀:** No existe diferencia en la tarifa promedio entre los clientes que pagan con tarjeta de crédito y los clientes que pagan en efectivo.

### Hipótesis alternativa

**Hₐ:** Existe una diferencia en la tarifa promedio entre los clientes que pagan con tarjeta de crédito y los clientes que pagan en efectivo.

Se utiliza un nivel de significancia de:

```text
α = 0.05
```

## Separación de los grupos

El código crea dos grupos independientes:

```python
credit_card = taxi_data[
    taxi_data["payment_type"] == 1
]["fare_amount"]

cash = taxi_data[
    taxi_data["payment_type"] == 2
]["fare_amount"]
```

- `credit_card`: tarifas de los viajes pagados con tarjeta.
- `cash`: tarifas de los viajes pagados en efectivo.

## Prueba t de Welch

Para comparar las medias se aplica una prueba t de Welch:

```python
test_result = stats.ttest_ind(
    a=credit_card,
    b=cash,
    equal_var=False
)
```

Se usa `equal_var=False` porque la prueba de Welch no supone que ambos grupos tengan la misma varianza.

El resultado con el conjunto completo entrega un valor p muy inferior a `0.05`.

Por lo tanto:

```text
Se rechaza la hipótesis nula.
```

Existe evidencia estadística suficiente para afirmar que las tarifas promedio de ambos grupos son diferentes.

## Comparación con una muestra pequeña

El notebook también toma una muestra aleatoria de 30 observaciones por grupo:

```python
credit_card_sample = credit_card_all.sample(
    n=30,
    random_state=42
)

cash_sample = cash_all.sample(
    n=30,
    random_state=42
)
```

Luego se repite la prueba t:

```python
sample_test = stats.ttest_ind(
    a=credit_card_sample,
    b=cash_sample,
    equal_var=False
)
```

En esta muestra pequeña, el valor p es superior a `0.05`, por lo que no se rechaza la hipótesis nula.

Esto permite observar cómo el tamaño de la muestra influye en:

- La precisión de las estimaciones.
- La variabilidad de los resultados.
- La potencia estadística.
- La capacidad de detectar diferencias reales.

## Resultado principal

Con el conjunto completo de datos, los viajes pagados con tarjeta de crédito presentan una tarifa promedio mayor que los viajes pagados en efectivo, y la diferencia es estadísticamente significativa.

Esto significa que existe una asociación entre el tipo de pago y el monto de la tarifa.

Sin embargo, el análisis no demuestra que pagar con tarjeta cause una tarifa más alta.

## Interpretación para NYC TLC

El resultado puede ayudar a NYC TLC a comprender mejor el comportamiento de pago de los pasajeros y a estudiar posibles oportunidades relacionadas con:

- Pagos digitales.
- Experiencia de pago.
- Viajes de mayor valor.
- Segmentación de usuarios.
- Incentivos para medios de pago electrónicos.

Antes de tomar decisiones, sería necesario investigar si los pagos con tarjeta se relacionan con otros factores, como:

- Mayor distancia recorrida.
- Viajes a aeropuertos.
- Peajes.
- Horarios específicos.
- Zonas de origen o destino.
- Duración del viaje.

## Limitaciones del análisis

El dataset contiene datos históricos observacionales y no proviene de un experimento controlado real.

Los pasajeros eligieron su propio medio de pago, por lo que pueden existir variables de confusión.

Además:

- Los grupos no fueron asignados aleatoriamente.
- Los viajes más largos pueden pagarse con tarjeta con mayor frecuencia.
- La distancia y la duración influyen directamente en la tarifa.
- Las propinas en efectivo no siempre quedan registradas.
- Una diferencia estadísticamente significativa no implica causalidad.
- Un tamaño muestral grande puede detectar diferencias pequeñas.

Para demostrar causalidad sería necesario diseñar un experimento con asignación aleatoria y controlar otras variables relevantes.

## Estructura del proyecto

```text
3. The Power of Statistics/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── automatidata_statistical_analysis_colab.ipynb
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
cd "Google-Advanced-Data-Analytics-Professional-Certificate/3. The Power of Statistics"
```

Instala las dependencias:

```bash
pip install -r requirements.txt
```

Abre el notebook:

```bash
jupyter notebook automatidata_statistical_analysis_colab.ipynb
```

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- SciPy
- Jupyter Notebook
- Google Colab
- GitHub

## Curso

**Google Advanced Data Analytics Professional Certificate**

Curso 3: **The Power of Statistics**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
