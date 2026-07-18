# Automatidata Exploratory Data Analysis

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/2.%20Go%20Beyond%20the%20Numbers%20Translate%20Data%20into%20Insights/trabajo_final_automatidata_eda.ipynb)

Proyecto desarrollado como parte del curso **Go Beyond the Numbers: Translate Data into Insights**, correspondiente al **Google Advanced Data Analytics Professional Certificate**.

## Descripción

En este proyecto continué el análisis del caso **Automatidata** para la **New York City Taxi and Limousine Commission (NYC TLC)**.

El objetivo fue realizar un análisis exploratorio de datos para comprender el comportamiento de los viajes en taxis amarillos de Nueva York, detectar problemas de calidad, identificar valores atípicos y crear visualizaciones que apoyen la toma de decisiones.

El trabajo se desarrolló en Python utilizando **Pandas**, **NumPy**, **Matplotlib** y **Seaborn**, siguiendo las cuatro etapas del marco de trabajo **PACE**:

- Plan
- Analyze
- Construct
- Execute

## Objetivos del proyecto

- Cargar el dataset directamente desde GitHub o desde el entorno local.
- Revisar la estructura, dimensiones y tipos de datos.
- Identificar valores nulos, duplicados y posibles anomalías.
- Convertir las fechas de recogida y llegada a formato `datetime`.
- Calcular la duración de cada viaje.
- Detectar valores atípicos mediante boxplots, histogramas y el método IQR.
- Analizar la distancia, los montos totales, las propinas y la cantidad de pasajeros.
- Examinar la cantidad de viajes e ingresos por mes y día de la semana.
- Comunicar los principales hallazgos al equipo de Automatidata y NYC TLC.

## Estructura del proyecto

```text
2. Go Beyond the Numbers Translate Data into Insights/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── trabajo_final_automatidata_eda.ipynb
```

## Archivos

- `trabajo_final_automatidata_eda.ipynb`: notebook principal con el análisis exploratorio, la limpieza y las visualizaciones.
- `2017_Yellow_Taxi_Trip_Data.csv`: dataset utilizado en el proyecto.
- `requirements.txt`: dependencias necesarias para ejecutar el notebook.
- `README.md`: documentación general del proyecto.

## Dataset

El dataset contiene:

- **22.699 viajes**
- **18 variables**
- Información sobre fechas, distancia, cantidad de pasajeros, tarifas, propinas, peajes y montos totales.

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

## Carga automática del dataset

El notebook está configurado para buscar primero el archivo:

```text
2017_Yellow_Taxi_Trip_Data.csv
```

en el entorno local o en Google Colab.

Si el archivo no está disponible localmente, se descarga automáticamente desde esta ruta del repositorio:

```text
2. Go Beyond the Numbers Translate Data into Insights/2017_Yellow_Taxi_Trip_Data.csv
```

La carga también comprueba:

- Que el archivo no esté vacío.
- Que el dataset contenga filas y columnas.
- Que incluya las variables principales esperadas.
- Que el archivo corresponda al dataset correcto.

## Metodología PACE

### Plan

Se revisó el contexto del proyecto, las necesidades de NYC TLC y las variables más útiles para comprender el comportamiento de los viajes.

### Analyze

Se inspeccionaron:

- Dimensiones del dataset.
- Tipos de datos.
- Valores nulos.
- Registros duplicados.
- Estadísticas descriptivas.
- Distribuciones.
- Valores extremos y posibles anomalías.

### Construct

Se prepararon nuevas variables y visualizaciones para analizar:

- Duración de los viajes.
- Distancia recorrida.
- Monto total.
- Propinas.
- Número de pasajeros.
- Viajes por mes.
- Viajes por día de la semana.
- Ingresos por mes y por día.
- Relación entre distancia y monto total.

### Execute

Se resumieron los hallazgos y se formularon recomendaciones para las siguientes etapas de análisis, visualización y modelado predictivo.

## Principales hallazgos

- Las variables de fecha estaban almacenadas inicialmente como texto y debieron convertirse a formato `datetime`.
- El dataset no presenta valores nulos en los registros analizados.
- Las distribuciones de `trip_distance`, `total_amount` y `tip_amount` presentan asimetría positiva.
- Se identificaron valores atípicos en la distancia, los montos totales y las propinas.
- Algunos registros presentan distancia igual a cero.
- Se observaron tarifas y montos totales negativos.
- Existen montos extremadamente altos que requieren una investigación adicional.
- La mayoría de los viajes corresponde a distancias relativamente cortas.
- Las propinas registradas electrónicamente pueden analizarse principalmente en los pagos con tarjeta.
- La cantidad de viajes y los ingresos presentan variaciones según el mes y el día de la semana.
- Los valores extremos deben revisarse antes de entrenar modelos predictivos.

## Visualizaciones incluidas

El notebook contiene:

- Boxplot de distancia de viaje.
- Histograma de distancia de viaje.
- Boxplot de monto total.
- Histograma de monto total.
- Boxplot de propinas para pagos con tarjeta.
- Histograma de propinas.
- Comparación de propinas por proveedor.
- Análisis de propinas superiores a 10 USD.
- Propina promedio por cantidad de pasajeros.
- Cantidad de viajes por mes.
- Cantidad de viajes por día de la semana.
- Ingresos por mes.
- Ingresos por día de la semana.
- Análisis de duración de viajes.
- Relación entre distancia y monto total.

## Tableau

La actividad también propone desarrollar un dashboard opcional en Tableau Public que muestre un mapa de Nueva York con los viajes por mes.

Para mejorar la accesibilidad del dashboard se recomienda:

- Usar títulos claros y descriptivos.
- Mantener un contraste visual alto.
- No depender únicamente del color.
- Incorporar etiquetas y descripciones.
- Evitar una cantidad excesiva de elementos.
- Diseñar la visualización para usuarios sin experiencia técnica.
- Considerar a personas con discapacidad visual.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio de este README.

También puedes utilizar este enlace:

```text
https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/2.%20Go%20Beyond%20the%20Numbers%20Translate%20Data%20into%20Insights/trabajo_final_automatidata_eda.ipynb
```

Dentro de Colab:

1. Selecciona **Entorno de ejecución**.
2. Presiona **Ejecutar todas**.
3. Espera a que el dataset se cargue desde GitHub.
4. Revisa las tablas, visualizaciones y conclusiones.

No es necesario subir manualmente el CSV si el archivo está disponible públicamente en GitHub.

### Opción 2: ejecución local

#### 1. Clonar el repositorio

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

#### 2. Entrar en la carpeta del proyecto

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/2. Go Beyond the Numbers Translate Data into Insights"
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

#### 4. Instalar las dependencias

```bash
pip install -r requirements.txt
```

#### 5. Abrir el notebook

```bash
jupyter notebook trabajo_final_automatidata_eda.ipynb
```

#### 6. Ejecutar todas las celdas

Dentro de Jupyter Notebook:

```text
Cell → Run All
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

Curso 2: **Go Beyond the Numbers: Translate Data into Insights**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
