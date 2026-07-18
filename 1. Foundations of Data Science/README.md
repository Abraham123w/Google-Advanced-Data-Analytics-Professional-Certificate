# Automatidata Taxi Trip Analysis

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/1.%20Foundations%20of%20Data%20Science/trabajo_final_automatidata_completo.ipynb)

Proyecto desarrollado como parte del curso **Foundations of Data Science**, correspondiente al Certificado Profesional de Análisis de Datos Avanzado de Google.

## Descripción

En este proyecto trabajé como analista de datos para **Automatidata**, una empresa consultora ficticia contratada por la **Comisión de Taxis y Limusinas de Nueva York (NYC TLC)**.

El objetivo fue inspeccionar y comprender una base de datos de viajes en taxis amarillos, dejándola preparada para futuras etapas de análisis exploratorio, visualización, pruebas estadísticas y modelado predictivo.

El trabajo se desarrolló en Python utilizando las bibliotecas **Pandas** y **NumPy**, y siguiendo las cuatro etapas del marco de trabajo **PACE**:

- Plan
- Analyze
- Construct
- Execute

## Archivos del proyecto

- `trabajo_final_automatidata_completo.ipynb`: notebook principal con el análisis desarrollado durante el proyecto.
- `actividad_corregida_referencia.ipynb`: notebook de referencia utilizado para comparar y validar cálculos e interpretaciones.
- `2017_Yellow_Taxi_Trip_Data.csv`: base de datos utilizada, compuesta por 22.699 viajes y 18 variables.
- `requirements.txt`: bibliotecas necesarias para ejecutar el proyecto.

## Principales hallazgos

- La base de datos contiene 22.699 registros y 18 variables.
- No se identificaron valores nulos en los registros analizados.
- Las variables de recogida y llegada estaban almacenadas como texto y requieren su conversión a formato `datetime` para realizar análisis temporales.
- Se detectaron valores que requieren una revisión adicional, como tarifas negativas, viajes con distancia cero y montos considerablemente elevados.
- El monto total máximo observado fue de aproximadamente 1.200,29 USD.
- La propina promedio registrada para los viajes pagados con tarjeta fue de aproximadamente 2,73 USD.
- Para los pagos en efectivo, el campo de propinas presenta un promedio de 0 USD, ya que estas propinas normalmente no quedan registradas automáticamente en el sistema.
- Los dos proveedores presentaron montos totales promedio similares, cercanos a 16,30 USD por viaje.
- `trip_distance` y `total_amount` fueron identificadas como dos variables relevantes para un modelo predictivo inicial.

## Variables para modelado predictivo

Para una primera aproximación al desarrollo de un modelo predictivo:

- **Variable predictora:** `trip_distance`
- **Variable objetivo:** `total_amount`

La distancia recorrida puede utilizarse para explicar parte de la variación del monto total cobrado por un viaje.

Antes de entrenar el modelo, es necesario revisar los valores negativos, las observaciones extremas y los viajes con distancia cero para evitar que distorsionen los resultados.

En etapas posteriores también podrían incorporarse variables como:

- Hora de recogida.
- Hora de llegada.
- Código de tarifa.
- Peajes.
- Tipo de pago.
- Ubicación de origen y destino.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio de este README.

Cuando se abra el notebook:

1. Selecciona **Entorno de ejecución**.
2. Presiona **Ejecutar todas**.
3. Espera a que el dataset sea cargado desde GitHub.
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
jupyter notebook trabajo_final_automatidata_completo.ipynb
```

#### 5. Ejecutar las celdas

Dentro de Jupyter Notebook, selecciona:

```text
Cell → Run All
```

### Opción 3: GitHub Codespaces

1. Entra en el repositorio de GitHub.
2. Selecciona **Code**.
3. Abre la pestaña **Codespaces**.
4. Presiona **Create codespace on main**.
5. En la terminal, entra en la carpeta:

```bash
cd "1. Foundations of Data Science"
```

6. Instala las dependencias:

```bash
pip install -r requirements.txt
```

7. Abre `trabajo_final_automatidata_completo.ipynb` y ejecuta sus celdas.

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Jupyter Notebook
- Google Colab
- GitHub

## Metodología

El análisis se organizó utilizando el marco de trabajo **PACE**:

1. **Plan:** comprensión del contexto, los objetivos y las necesidades del cliente.
2. **Analyze:** inspección de la estructura, los tipos de datos y las estadísticas descriptivas.
3. **Construct:** preparación de los datos para análisis posteriores.
4. **Execute:** comunicación de los hallazgos y recomendaciones al equipo de datos.

## Curso y certificación

**Google Advanced Data Analytics Professional Certificate**

Curso 1: **Foundations of Data Science**
