# Automatidata Taxi Trip Analysis

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/1.%20Foundations%20of%20Data%20Science/trabajo_final_automatidata.ipynb)

Proyecto desarrollado como parte del curso **Foundations of Data Science**, correspondiente al **Google Advanced Data Analytics Professional Certificate**.

## Descripción

En este proyecto asumí el rol de analista de datos para **Automatidata**, una empresa de consultoría ficticia contratada por la **New York City Taxi and Limousine Commission (NYC TLC)**.

El objetivo fue inspeccionar y comprender una base de datos de viajes realizados en taxis amarillos de Nueva York, con el propósito de prepararla para futuras etapas de:

- Análisis exploratorio de datos.
- Visualización.
- Pruebas estadísticas.
- Modelado predictivo.
- Comunicación de hallazgos.

El análisis fue desarrollado en Python mediante las bibliotecas **Pandas** y **NumPy**, siguiendo el marco de trabajo **PACE**: Plan, Analyze, Construct y Execute.

## Objetivos del proyecto

- Comprender el contexto y las necesidades del cliente.
- Cargar y examinar la base de datos.
- Identificar los tipos de variables.
- Revisar valores nulos y posibles inconsistencias.
- Calcular estadísticas descriptivas.
- Investigar valores extremos y atípicos.
- Identificar variables relevantes para futuros modelos predictivos.
- Comunicar los principales hallazgos al equipo de datos.

## Archivos del proyecto

```text
1. Foundations of Data Science/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── trabajo_final_automatidata.ipynb
```

### Descripción de los archivos

- `trabajo_final_automatidata.ipynb`: notebook principal con el análisis completo.
- `2017_Yellow_Taxi_Trip_Data.csv`: base de datos de viajes de taxis amarillos de Nueva York.
- `requirements.txt`: bibliotecas necesarias para ejecutar el proyecto.
- `README.md`: documentación general del proyecto.

## Dataset

La base de datos contiene:

- **22.699 viajes**
- **18 variables**
- Información sobre distancia, pasajeros, fechas, tarifas, propinas, peajes, tipos de pago y montos totales.

Algunas de las principales variables son:

- `VendorID`
- `tpep_pickup_datetime`
- `tpep_dropoff_datetime`
- `passenger_count`
- `trip_distance`
- `RatecodeID`
- `payment_type`
- `fare_amount`
- `tip_amount`
- `tolls_amount`
- `total_amount`

## Metodología PACE

El proyecto fue organizado utilizando el marco de trabajo **PACE**.

### Plan

Se analizó el contexto del proyecto, las necesidades de NYC TLC y la documentación disponible sobre el dataset.

### Analyze

Se inspeccionaron:

- Dimensiones de la base.
- Tipos de datos.
- Valores nulos.
- Estadísticas descriptivas.
- Distribución de variables.
- Registros potencialmente anómalos.

### Construct

Se prepararon los datos y se identificaron las variables que podrían utilizarse en futuros análisis estadísticos y modelos predictivos.

### Execute

Se resumieron los principales hallazgos y se formularon recomendaciones para el equipo de datos.

## Principales hallazgos

- El dataset contiene **22.699 registros y 18 variables**.
- No se encontraron valores nulos en los registros analizados.
- Las variables de recogida y llegada están inicialmente almacenadas como texto y deberían convertirse al tipo `datetime`.
- Se identificaron viajes con distancia igual a cero.
- Se encontraron tarifas y montos totales negativos.
- Se observaron montos extremadamente elevados que requieren investigación.
- El monto total máximo registrado fue de aproximadamente **1.200,29 USD**.
- La propina promedio para viajes pagados con tarjeta fue de aproximadamente **2,73 USD**.
- Las propinas en efectivo presentan un promedio de cero porque normalmente no quedan registradas electrónicamente en el sistema.
- Los dos proveedores presentaron montos totales promedio similares, cercanos a **16,30 USD por viaje**.

## Variables para modelado predictivo

Las dos variables inicialmente identificadas como más útiles para un modelo predictivo son:

### Variable predictora

```text
trip_distance
```

La distancia recorrida puede ayudar a explicar el precio de un viaje.

### Variable objetivo

```text
total_amount
```

Representa el monto total que el modelo podría intentar predecir.

Una primera formulación del modelo sería:

```text
trip_distance → total_amount
```

Antes de desarrollar el modelo, se deben revisar los valores negativos, los registros extremos y los viajes con distancia cero para evitar que distorsionen los resultados.

En etapas posteriores también podrían utilizarse variables como:

- Hora de recogida.
- Hora de llegada.
- Duración del viaje.
- Código de tarifa.
- Peajes.
- Tipo de pago.
- Ubicación de origen.
- Ubicación de destino.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio de este README.

Dentro de Colab:

1. Abre el menú **Entorno de ejecución**.
2. Selecciona **Ejecutar todas**.
3. Espera a que se cargue el dataset desde GitHub.
4. Revisa los resultados generados por cada celda.

No es necesario instalar Python ni descargar manualmente el dataset.

### Opción 2: ejecución local

#### 1. Clonar el repositorio

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

#### 2. Entrar en la carpeta del proyecto

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/1. Foundations of Data Science"
```

#### 3. Instalar las dependencias

```bash
pip install -r requirements.txt
```

#### 4. Abrir el notebook

```bash
jupyter notebook trabajo_final_automatidata.ipynb
```

#### 5. Ejecutar todas las celdas

Dentro de Jupyter Notebook:

```text
Cell → Run All
```

## Carga del dataset

El notebook carga el CSV directamente desde GitHub mediante la siguiente URL:

```python
DATA_URL = (
    "https://raw.githubusercontent.com/"
    "Abraham123w/"
    "Google-Advanced-Data-Analytics-Professional-Certificate/"
    "main/"
    "1.%20Foundations%20of%20Data%20Science/"
    "2017_Yellow_Taxi_Trip_Data.csv"
)
```

Esto permite ejecutar el análisis en Google Colab sin subir manualmente el archivo.

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Jupyter Notebook
- Google Colab
- GitHub

## Curso y certificación

**Google Advanced Data Analytics Professional Certificate**

Curso 1: **Foundations of Data Science**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
