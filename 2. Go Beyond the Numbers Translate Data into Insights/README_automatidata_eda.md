# Automatidata Exploratory Data Analysis

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/3.%20Go%20Beyond%20the%20Numbers%20Translate%20Data%20into%20Insights/trabajo_final_automatidata_eda.ipynb)

Proyecto desarrollado como parte del curso **Go Beyond the Numbers: Translate Data into Insights**, correspondiente al **Google Advanced Data Analytics Professional Certificate**.

## Descripción

En este proyecto continué el análisis del caso **Automatidata** para la **New York City Taxi and Limousine Commission (NYC TLC)**.

El objetivo fue realizar un análisis exploratorio de datos, limpiar y estructurar la información, identificar valores atípicos y crear visualizaciones que permitieran comprender mejor el comportamiento de los viajes en taxis amarillos de Nueva York.

El análisis se desarrolló en Python con **Pandas**, **NumPy**, **Matplotlib** y **Seaborn**, siguiendo el marco de trabajo **PACE**: Plan, Analyze, Construct y Execute.

## Objetivos

- Cargar y revisar el dataset de viajes.
- Evaluar la estructura y los tipos de datos.
- Convertir las fechas de recogida y llegada a formato `datetime`.
- Calcular la duración de los viajes.
- Detectar valores atípicos mediante boxplots, histogramas y el método IQR.
- Analizar distancia, monto total, propinas y cantidad de pasajeros.
- Examinar viajes e ingresos por mes y día de la semana.
- Comunicar los hallazgos al equipo de Automatidata y NYC TLC.

## Archivos del proyecto

```text
3. Go Beyond the Numbers Translate Data into Insights/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── trabajo_final_automatidata_eda.ipynb
```

### Descripción de los archivos

- `trabajo_final_automatidata_eda.ipynb`: notebook principal con el análisis exploratorio y las visualizaciones.
- `2017_Yellow_Taxi_Trip_Data.csv`: dataset utilizado en el proyecto.
- `requirements.txt`: dependencias necesarias para ejecutar el notebook.
- `README.md`: documentación general del proyecto.

## Dataset

El dataset contiene:

- **22.699 viajes**
- **18 variables**
- **0 valores nulos** en los registros analizados

Entre las principales variables se encuentran:

- `VendorID`
- `tpep_pickup_datetime`
- `tpep_dropoff_datetime`
- `passenger_count`
- `trip_distance`
- `RatecodeID`
- `PULocationID`
- `DOLocationID`
- `payment_type`
- `fare_amount`
- `tip_amount`
- `tolls_amount`
- `total_amount`

## Metodología PACE

### Plan

Se revisó el contexto del proyecto y se definieron las variables más relevantes para comprender el comportamiento de los viajes.

### Analyze

Se inspeccionaron:

- Dimensiones del dataset.
- Tipos de datos.
- Valores nulos.
- Estadísticas descriptivas.
- Distribuciones.
- Valores extremos y posibles anomalías.

### Construct

Se crearon nuevas variables y visualizaciones para analizar:

- Duración de los viajes.
- Distancia recorrida.
- Monto total.
- Propinas.
- Número de pasajeros.
- Viajes por mes.
- Viajes por día de la semana.
- Ingresos por mes y día.

### Execute

Se resumieron los hallazgos y se formularon recomendaciones para continuar con las futuras etapas de análisis y modelado.

## Principales hallazgos

- Las variables de fecha estaban almacenadas inicialmente como texto y debieron convertirse a formato `datetime`.
- Las distribuciones de `trip_distance`, `total_amount` y `tip_amount` presentan asimetría positiva.
- Se identificaron valores atípicos en distancia, montos y propinas.
- Algunos registros presentan distancia igual a cero, tarifas negativas o montos extremadamente altos.
- La mayoría de los viajes tiene una distancia relativamente corta.
- Los pagos con tarjeta permiten analizar las propinas registradas electrónicamente.
- La cantidad de viajes y los ingresos cambian según el mes y el día de la semana.
- Los valores extremos deben investigarse antes de desarrollar modelos predictivos.

## Visualizaciones incluidas

El notebook contiene:

- Boxplot de distancia de viaje.
- Histograma de distancia de viaje.
- Boxplot de monto total.
- Histograma de monto total.
- Boxplot e histograma de propinas.
- Comparación de propinas por proveedor.
- Propina promedio por cantidad de pasajeros.
- Viajes por mes.
- Viajes por día de la semana.
- Ingresos por mes y día.
- Análisis temporal de los viajes.
- Relación entre distancia y monto total.

## Tableau

La actividad también propone crear un dashboard opcional en Tableau con un mapa de Nueva York y la distribución mensual de los viajes.

Al diseñar el dashboard se recomienda:

- Utilizar títulos claros.
- Evitar combinaciones de colores con poco contraste.
- No depender únicamente del color para comunicar información.
- Incorporar etiquetas y descripciones accesibles.
- Mantener una estructura fácil de interpretar para personas sin experiencia técnica.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al comienzo del README.

Luego selecciona:

```text
Entorno de ejecución → Ejecutar todas
```

El dataset debe llamarse exactamente:

```text
2017_Yellow_Taxi_Trip_Data.csv
```

y estar disponible en la misma carpeta del notebook o cargarse desde la URL configurada en el código.

### Opción 2: ejecución local

#### 1. Clonar el repositorio

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

#### 2. Entrar en la carpeta del proyecto

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/3. Go Beyond the Numbers Translate Data into Insights"
```

#### 3. Crear un entorno virtual

```bash
python -m venv .venv
```

Activarlo en Windows:

```bash
.venv\Scripts\activate
```

Activarlo en macOS o Linux:

```bash
source .venv/bin/activate
```

#### 4. Instalar dependencias

```bash
pip install -r requirements.txt
```

#### 5. Abrir el notebook

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

## Curso y certificación

**Google Advanced Data Analytics Professional Certificate**

Curso: **Go Beyond the Numbers: Translate Data into Insights**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
