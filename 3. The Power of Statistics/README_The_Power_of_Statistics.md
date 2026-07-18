# Automatidata Statistical Analysis

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/3.%20The%20Power%20of%20Statistics/automatidata_statistical_analysis_colab.ipynb)

Proyecto desarrollado como parte del curso **The Power of Statistics**, correspondiente al **Google Advanced Data Analytics Professional Certificate**.

## Descripción

En este proyecto continué el caso de estudio de **Automatidata**, una empresa de consultoría ficticia que trabaja para la **New York City Taxi and Limousine Commission (NYC TLC)**.

El objetivo fue analizar si existe una diferencia estadísticamente significativa entre el monto promedio de las tarifas pagadas con tarjeta de crédito y las pagadas en efectivo.

Para ello se aplicaron:

- Estadísticas descriptivas.
- Segmentación de datos por tipo de pago.
- Formulación de hipótesis.
- Prueba t de Welch para dos muestras independientes.
- Interpretación del valor p.
- Comunicación de resultados y limitaciones del análisis.

## Pregunta de investigación

> ¿Existe una diferencia estadísticamente significativa en el monto promedio de la tarifa entre los clientes que pagan con tarjeta de crédito y quienes pagan en efectivo?

## Hipótesis

### Hipótesis nula

**H₀:** No existe una diferencia en el monto promedio de la tarifa entre los clientes que pagan con tarjeta de crédito y los clientes que pagan en efectivo.

### Hipótesis alternativa

**Hₐ:** Existe una diferencia en el monto promedio de la tarifa entre los clientes que pagan con tarjeta de crédito y los clientes que pagan en efectivo.

Se utilizó un nivel de significancia de:

```text
α = 0.05
```

## Estructura del proyecto

```text
3. The Power of Statistics/
├── 2017_Yellow_Taxi_Trip_Data.csv
├── README.md
├── requirements.txt
└── automatidata_statistical_analysis_colab.ipynb
```

## Archivos

- `automatidata_statistical_analysis_colab.ipynb`: notebook principal con el análisis estadístico y la prueba de hipótesis.
- `2017_Yellow_Taxi_Trip_Data.csv`: dataset de viajes de taxis amarillos de Nueva York.
- `requirements.txt`: dependencias necesarias para ejecutar el proyecto.
- `README.md`: documentación general del proyecto.

## Dataset

El dataset contiene información sobre viajes de taxis amarillos de Nueva York, incluyendo:

- Tipo de proveedor.
- Fecha y hora de recogida.
- Fecha y hora de llegada.
- Cantidad de pasajeros.
- Distancia recorrida.
- Tipo de pago.
- Monto de la tarifa.
- Propinas.
- Peajes.
- Monto total.

La variable `payment_type` está codificada de la siguiente forma:

| Código | Tipo de pago |
|---:|---|
| 1 | Tarjeta de crédito |
| 2 | Efectivo |
| 3 | Sin cargo |
| 4 | Disputa |
| 5 | Desconocido |

## Carga automática de datos

El notebook está configurado para descargar el dataset directamente desde GitHub:

```python
DATA_URL = (
    "https://raw.githubusercontent.com/"
    "Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/"
    "main/3.%20The%20Power%20of%20Statistics/"
    "2017_Yellow_Taxi_Trip_Data.csv"
)

taxi_data = pd.read_csv(DATA_URL, index_col=0)
```

Esto permite ejecutar el proyecto en Google Colab sin subir manualmente el archivo CSV.

## Metodología PACE

El proyecto se organizó mediante el marco de trabajo **PACE**.

### Plan

- Definición de la pregunta de investigación.
- Identificación de los grupos de comparación.
- Formulación de la hipótesis nula y alternativa.
- Selección del nivel de significancia.

### Analyze

- Revisión de la distribución de los tipos de pago.
- Cálculo del promedio de la tarifa por tipo de pago.
- Comparación entre pagos con tarjeta y efectivo.

### Construct

- Separación de los datos en dos grupos.
- Aplicación de una prueba t de Welch.
- Evaluación del estadístico t y del valor p.
- Repetición del análisis con muestras aleatorias pequeñas para observar el efecto del tamaño muestral.

### Execute

- Interpretación de los resultados.
- Comunicación de conclusiones a las partes interesadas.
- Identificación de limitaciones y posibles variables de confusión.

## Código principal de la prueba

```python
credit_card = taxi_data[
    taxi_data["payment_type"] == 1
]["fare_amount"]

cash = taxi_data[
    taxi_data["payment_type"] == 2
]["fare_amount"]

test_result = stats.ttest_ind(
    a=credit_card,
    b=cash,
    equal_var=False
)

test_result
```

Se utilizó `equal_var=False` para aplicar la **prueba t de Welch**, que no supone igualdad de varianzas entre los grupos.

## Principales resultados

El análisis del conjunto completo mostró:

- Una tarifa promedio mayor para los pagos con tarjeta de crédito.
- Una tarifa promedio menor para los pagos en efectivo.
- Un valor p considerablemente inferior a `0.05`.
- Evidencia suficiente para rechazar la hipótesis nula con el conjunto completo.

El resultado indica que existe una diferencia estadísticamente significativa entre los montos promedio de las tarifas de ambos grupos.

También se realizó una prueba con una muestra aleatoria de 30 observaciones por grupo. En esa prueba, el valor p fue superior a `0.05`, por lo que no se rechazó la hipótesis nula.

Esto demuestra que el tamaño de la muestra influye en:

- La precisión de las estimaciones.
- La variabilidad de los resultados.
- La potencia estadística de una prueba.
- La capacidad para detectar diferencias reales.

## Interpretación para el negocio

Los clientes que pagan con tarjeta de crédito presentan, en promedio, tarifas más altas que los clientes que pagan en efectivo.

Este resultado podría sugerir oportunidades como:

- Facilitar el pago con tarjeta.
- Mejorar la experiencia de pago digital.
- Analizar incentivos para pagos electrónicos.
- Estudiar el comportamiento de los viajes de mayor valor.

Sin embargo, no se debe concluir automáticamente que el uso de tarjeta provoca tarifas más altas.

## Limitaciones

El dataset corresponde a datos observacionales y no a un experimento controlado real.

Los clientes seleccionaron su propio método de pago, por lo que pueden existir variables de confusión, como:

- Distancia del viaje.
- Duración.
- Zona de origen o destino.
- Hora del viaje.
- Tipo de tarifa.
- Preferencias del cliente.
- Diferencias en el registro de propinas.

Por esta razón, el análisis permite identificar una asociación estadística, pero no demostrar causalidad sin un experimento aleatorizado.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio de este README.

Dentro de Colab:

1. Abre el menú **Entorno de ejecución**.
2. Selecciona **Ejecutar todas**.
3. Espera a que el dataset se descargue desde GitHub.
4. Revisa los resultados de las pruebas estadísticas.

No es necesario instalar Python ni descargar el CSV manualmente.

### Opción 2: ejecución local

#### 1. Clonar el repositorio

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

#### 2. Entrar en la carpeta del proyecto

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/3. The Power of Statistics"
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

## Curso y certificación

**Google Advanced Data Analytics Professional Certificate**

Curso 3: **The Power of Statistics**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
