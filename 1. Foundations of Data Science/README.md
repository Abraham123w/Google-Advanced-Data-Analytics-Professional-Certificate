# Automatidata Taxi Trip Analysis

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO/automatidata-taxi-analysis/blob/main/trabajo_final_automatidata.ipynb)

Proyecto desarrollado como parte del curso **Foundations of Data Science**, correspondiente al Certificado Avanzado de Análisis de Datos de Google.

## Descripción

En este proyecto trabajé como analista de datos para **Automatidata**, una empresa consultora ficticia contratada por la **Comisión de Taxis y Limusinas de Nueva York (NYC TLC)**. El objetivo fue inspeccionar y comprender una base de datos de viajes en taxis amarillos, dejándola preparada para futuras etapas de análisis exploratorio, visualización, pruebas estadísticas y modelado predictivo.

El trabajo se desarrolló en Python, utilizando las bibliotecas **Pandas** y **NumPy**, y siguiendo las cuatro etapas del marco de trabajo **PACE** (Plan, Analyze, Construct, Execute).

## Archivos

- `trabajo_final_automatidata.ipynb`: análisis desarrollado durante el proyecto.
- `actividad_corregida_referencia.ipynb`: notebook de referencia corregido, usado para validar cálculos e interpretaciones.
- `2017_Yellow_Taxi_Trip_Data.csv`: base de datos utilizada (22.699 viajes, 18 variables).
- `requirements.txt`: bibliotecas necesarias para ejecutar el proyecto.

## Principales hallazgos

- La base no presentó valores nulos, aunque algunas variables de fecha requerían transformación.
- Se detectaron valores atípicos: tarifas negativas, viajes con distancia cero y montos elevados (hasta 1.200,29 USD).
- La propina promedio en pagos con tarjeta fue de ~2,73 USD; en efectivo no queda registrada en el sistema.
- Ambos proveedores del servicio presentaron un monto total promedio similar (~16,30 USD por viaje).
- La distancia del viaje y el monto total se identificaron como variables clave para futuros modelos predictivos.

## Cómo ejecutar el proyecto

### Opción 1: Google Colab (recomendada, sin instalación)

Haz clic en el botón **"Abrir en Colab"** al inicio de este README. El notebook se abrirá directamente en tu navegador, listo para ejecutar celda por celda.

### Opción 2: Ejecución local

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/TU_USUARIO/automatidata-taxi-analysis.git
   ```
2. Entrar a la carpeta:
   ```bash
   cd automatidata-taxi-analysis
   ```
3. Instalar dependencias:
   ```bash
   pip install -r requirements.txt
   ```
4. Abrir Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
5. Ejecutar `trabajo_final_automatidata.ipynb`

### Opción 3: GitHub Codespaces

Desde este repositorio: **Code → Codespaces → Create codespace on main**. Se abrirá un entorno en la nube con JupyterLab ya instalado.

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Jupyter Notebook

## Curso y certificación

Certificado Profesional Avanzado en Análisis de Datos de Google — Curso 1: *Foundations of Data Science*.
