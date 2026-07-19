# Salifort Motors: análisis y predicción de rotación de empleados

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/7.%20Google%20Advanced%20Data%20Analytics%20Capstone/salifort_motors_employee_attrition_colab.ipynb)

## Caso de estudio

En este proyecto trabajé como profesional de datos para **Salifort Motors**, una empresa ficticia cuyo departamento de Recursos Humanos busca comprender por qué algunos empleados abandonan la organización.

La empresa había recopilado información sobre satisfacción, carga de trabajo, evaluaciones, antigüedad, salario, promociones y otras características laborales, pero necesitaba transformar esos datos en recomendaciones concretas.

La pregunta principal del caso fue:

> ¿Qué factores hacen más probable que un empleado deje la empresa?

El objetivo fue analizar los datos de Recursos Humanos, identificar los principales factores asociados con la rotación y construir modelos capaces de predecir si un empleado probablemente permanecerá o abandonará la compañía.

## Objetivos del proyecto

El notebook desarrolla las siguientes tareas:

- Comprender el problema de negocio.
- Identificar a los principales stakeholders.
- Analizar consideraciones éticas relacionadas con datos de empleados.
- Cargar y revisar el dataset.
- Limpiar nombres de columnas.
- Detectar valores faltantes y registros duplicados.
- Analizar valores atípicos.
- Crear visualizaciones relacionadas con satisfacción, carga laboral, salario, antigüedad, desempeño y departamento.
- Preparar variables numéricas y categóricas para modelado.
- Construir una regresión logística.
- Evaluar los supuestos de la regresión logística.
- Construir y optimizar un modelo Random Forest.
- Comparar ambos modelos.
- Formular recomendaciones para reducir la rotación.

## Stakeholders

Los principales interesados en el proyecto son:

- **Departamento de Recursos Humanos:** responsable de diseñar estrategias de retención.
- **Alta dirección:** interesada en reducir costos asociados con contratación y capacitación.
- **Jefaturas de departamento:** responsables de la distribución de carga laboral y gestión de equipos.
- **Empleados:** afectados directamente por las decisiones derivadas del análisis.
- **Equipo de datos:** responsable de la calidad técnica, la ética y el seguimiento del modelo.

## Consideraciones éticas

El modelo debe utilizarse como una herramienta de apoyo para mejorar las condiciones laborales, no como un sistema de vigilancia o castigo.

Los principales riesgos considerados son:

- Reidentificación de empleados mediante combinaciones de variables.
- Uso de predicciones para despidos preventivos.
- Reproducción de sesgos presentes en evaluaciones o salarios.
- Etiquetado injusto de empleados como desleales.
- Decisiones automatizadas sin revisión humana.
- Uso del modelo sin informar a las personas afectadas.

Las predicciones deben orientar intervenciones positivas, como redistribución de trabajo, revisiones salariales, oportunidades de desarrollo y conversaciones de apoyo.

## Estructura del proyecto

```text
7. Google Advanced Data Analytics Capstone/
├── HR_capstone_dataset.csv
├── README.md
├── requirements.txt
└── salifort_motors_employee_attrition_colab.ipynb
```

## Dataset

El dataset contiene aproximadamente **15.000 registros** y 10 variables originales:

| Variable original | Descripción |
|---|---|
| `satisfaction_level` | Nivel de satisfacción laboral entre 0 y 1 |
| `last_evaluation` | Resultado de la última evaluación |
| `number_project` | Cantidad de proyectos asignados |
| `average_montly_hours` | Promedio de horas trabajadas al mes |
| `time_spend_company` | Años en la empresa |
| `Work_accident` | Indica si el empleado tuvo un accidente laboral |
| `left` | Indica si el empleado dejó la empresa |
| `promotion_last_5years` | Indica si fue promovido en los últimos cinco años |
| `Department` | Departamento del empleado |
| `salary` | Nivel salarial: bajo, medio o alto |

Durante la limpieza se corrigen los nombres:

```python
rename_dict = {
    "number_project": "number_projects",
    "average_montly_hours": "average_monthly_hours",
    "time_spend_company": "tenure",
    "Work_accident": "work_accident",
    "Department": "department"
}
```

## Carga compatible con Google Colab

El notebook busca primero una copia local de:

```text
HR_capstone_dataset.csv
```

También revisa las carpetas habituales de Google Colab:

```text
/content
/mnt/data
```

Si no encuentra el archivo, intenta descargarlo desde GitHub. Como respaldo, puede utilizar el mismo dataset bajo el nombre:

```text
HR_comma_sep.csv
```

Esto permite ejecutar el notebook sin cargar manualmente el archivo cuando existe una fuente pública disponible.

## Qué se hace en el código

### 1. Importación de bibliotecas

Se utilizan:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

Para modelado y evaluación se utilizan:

```python
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
```

También se utiliza `statsmodels` para diagnósticos estadísticos.

## 2. Exploración inicial

El notebook revisa:

```python
df0.info()
df0.shape
df0.describe()
df0.describe(include="object")
```

Esto permite conocer:

- Cantidad de filas y columnas.
- Tipos de datos.
- Distribución de variables numéricas.
- Categorías más frecuentes.
- Posibles problemas de calidad.

## 3. Limpieza de datos

### Valores faltantes

Se verifica:

```python
df0.isna().sum()
```

El dataset analizado no presenta valores faltantes.

### Registros duplicados

Se identifican:

```python
df0.duplicated().sum()
```

Se encontraron **3.008 filas duplicadas**.

Después de eliminarlas:

```text
Filas originales: 14.999
Filas limpias: 11.991
```

El código utiliza:

```python
df_clean = (
    df0.drop_duplicates(keep="first")
    .reset_index(drop=True)
)
```

## 4. Valores atípicos en antigüedad

Se crea un boxplot de `tenure` y se aplica la regla:

```text
Q1 - 1,5 × IQR
Q3 + 1,5 × IQR
```

El resultado identifica aproximadamente:

```text
824 registros atípicos
6,87 % del dataset limpio
```

Estos registros no se eliminan automáticamente porque pueden corresponder a empleados reales con mayor antigüedad.

## 5. Balance de la variable objetivo

La variable `left` representa:

| Valor | Significado |
|---:|---|
| 0 | El empleado permaneció |
| 1 | El empleado dejó la empresa |

Después de limpiar duplicados:

```text
83,4 % permaneció
16,6 % dejó la empresa
```

Existe un desbalance de clases, por lo que no es suficiente evaluar los modelos solo con accuracy.

También se utilizan:

- Precision.
- Recall.
- F1-score.
- ROC-AUC.

## 6. Satisfacción laboral

Se compara `satisfaction_level` entre quienes permanecieron y quienes dejaron la empresa.

El análisis muestra que:

- Los empleados que permanecen presentan una satisfacción más alta y estable.
- Quienes se van presentan niveles más bajos y una distribución más variable.
- La satisfacción es uno de los principales indicadores de rotación.

## 7. Carga de trabajo y burnout

Se analiza la relación entre:

- `number_projects`
- `average_monthly_hours`
- `left`

Los resultados muestran tres zonas.

### Sobrecarga crítica

Los empleados con 6 o 7 proyectos trabajan frecuentemente entre 250 y 300 horas mensuales y presentan una elevada rotación.

### Subutilización

Los empleados con solo 2 proyectos también muestran una rotación relevante, posiblemente asociada con desmotivación o falta de desarrollo.

### Rango de equilibrio

Los empleados con 3 a 5 proyectos y jornadas moderadas presentan mayor estabilidad.

## 8. Antigüedad y ventana crítica

El análisis muestra una concentración de salidas entre los años 3 y 5.

El cuarto año aparece como un período especialmente sensible.

Esto sugiere la necesidad de:

- Revisiones de carrera.
- Conversaciones de desarrollo.
- Evaluaciones salariales.
- Planes de promoción.

## 9. Salario y rotación

La mayoría de los empleados que se van pertenece a los niveles salariales:

- Bajo.
- Medio.

Los empleados con salario alto presentan mayor retención.

El salario no debe interpretarse de forma aislada, ya que puede interactuar con carga laboral, promoción y satisfacción.

## 10. Evaluación de desempeño

La distribución de `last_evaluation` muestra dos grupos entre quienes se van:

- Empleados con evaluaciones bajas.
- Empleados con evaluaciones muy altas.

El segundo grupo puede representar una pérdida de talento asociada con sobrecarga y falta de reconocimiento.

## 11. Rotación por departamento

El código calcula la tasa promedio de rotación por departamento:

```python
dept_order = (
    df_clean.groupby("department")["left"]
    .mean()
    .sort_values(ascending=False)
    .index
)
```

Esto permite identificar departamentos que necesitan intervenciones específicas.

## 12. Promociones y accidentes laborales

Se analiza la rotación según:

- Promoción durante los últimos cinco años.
- Accidentes laborales.

Los empleados sin promoción presentan mayor rotación.

Los accidentes laborales no aparecen como el principal factor de salida voluntaria en este dataset.

## 13. Correlaciones

Se crea un mapa de calor con variables numéricas.

La satisfacción presenta la asociación negativa más importante con la salida.

Las variables relacionadas con carga laboral también muestran relaciones relevantes.

La correlación no demuestra causalidad, pero ayuda a identificar patrones que deben investigarse.

# Modelado predictivo

## 14. Preparación de variables

Las variables categóricas `salary` y `department` se convierten mediante variables dummy:

```python
df_model = pd.get_dummies(
    df_clean,
    columns=["salary", "department"],
    drop_first=True,
    dtype=int
)
```

La variable objetivo es:

```python
y = df_model["left"]
```

Los predictores son:

```python
X = df_model.drop("left", axis=1)
```

## 15. División en entrenamiento y prueba

Se utiliza una división estratificada:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.3,
    random_state=42,
    stratify=y
)
```

- 70 % para entrenamiento.
- 30 % para prueba.
- Estratificación para conservar la proporción de empleados que se van.

## 16. Estandarización

Para la regresión logística se utiliza:

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

El escalador se ajusta solo con entrenamiento para evitar fuga de información.

# Regresión logística

## 17. Construcción del modelo

```python
log_reg = LogisticRegression(
    max_iter=1000,
    random_state=42
)

log_reg.fit(
    X_train_scaled,
    y_train
)
```

La regresión logística se utiliza como modelo base por su facilidad de interpretación.

## 18. Diagnósticos

El notebook revisa:

- Independencia de observaciones.
- Multicolinealidad mediante VIF.
- Observaciones influyentes mediante distancia de Cook.
- Relación de variables continuas con el logit.

### VIF

Los valores reportados son bajos, aproximadamente entre 1,05 y 1,22, por lo que no existe multicolinealidad severa.

### Distancia de Cook

Se identifican observaciones influyentes para su inspección, pero no se eliminan automáticamente.

### Linealidad

Algunas variables muestran relaciones no lineales o efectos de umbral, lo que limita el desempeño de la regresión logística.

## 19. Resultados de regresión logística

Los resultados aproximados reportados son:

| Métrica | Resultado |
|---|---:|
| Accuracy | 0,83 |
| Precision para empleados que se van | 0,50 |
| Recall para empleados que se van | 0,21 |
| F1-score | 0,30 |
| ROC-AUC | 0,825 |

La principal debilidad es el recall.

El modelo no identifica a una gran parte de los empleados que finalmente se van.

# Random Forest

## 20. Construcción y optimización

Se utiliza:

```python
rf = RandomForestClassifier(
    random_state=42
)
```

Después se aplica `GridSearchCV` para ajustar:

- Profundidad máxima.
- Número de árboles.
- Número mínimo de muestras por hoja.
- Número de variables consideradas por división.
- Proporción de muestras por árbol.

El mejor modelo se selecciona según el F1-score.

## 21. Resultados de Random Forest

Los resultados aproximados reportados son:

| Métrica | Resultado |
|---|---:|
| Accuracy | 98,36 % |
| Precision | 98,24 % |
| Recall | 91,75 % |
| F1-score | 0,9488 |
| ROC-AUC | 0,9769 |

El modelo Random Forest supera ampliamente a la regresión logística.

La mejora más importante está en el recall, porque detecta a la mayoría de los empleados que realmente dejan la empresa.

## Resultado principal

El análisis indica que los principales factores asociados con la rotación son:

- Baja satisfacción.
- Exceso de horas mensuales.
- Demasiados proyectos.
- Falta de promoción.
- Salario bajo o medio.
- Antigüedad entre 3 y 5 años.
- Sobrecarga de empleados con evaluaciones altas.

Random Forest fue seleccionado como el modelo preferido porque captura mejor las relaciones no lineales y los efectos combinados.

## Recomendaciones para Salifort Motors

### 1. Redistribuir la carga laboral

Evitar que empleados acumulen 6 o 7 proyectos simultáneamente sin apoyo adicional.

### 2. Controlar jornadas excesivas

Establecer alertas cuando las horas mensuales superen niveles sostenibles.

### 3. Crear una revisión estratégica en el cuarto año

Implementar conversaciones formales sobre carrera, salario, promoción y desarrollo entre los años 3 y 5.

### 4. Proteger a los empleados de alto rendimiento

Evitar que una buena evaluación se traduzca automáticamente en más trabajo.

### 5. Mejorar las oportunidades de promoción

Revisar por qué una proporción tan pequeña de empleados ha recibido promociones.

### 6. Aplicar intervenciones por departamento

Los departamentos con mayor rotación necesitan medidas específicas.

### 7. Utilizar el modelo responsablemente

El modelo debe activar apoyo y conversaciones, no castigos ni despidos automáticos.

## Limitaciones

- El dataset es transversal y no muestra cambios individuales a lo largo del tiempo.
- No incluye edad, modalidad de trabajo, beneficios ni distancia de traslado.
- Las evaluaciones pueden contener sesgos.
- Las asociaciones no prueban causalidad.
- La rotación futura puede cambiar si cambian las condiciones de la empresa.
- Los datos duplicados originales pueden indicar problemas en el proceso de recolección.
- El modelo debe monitorearse para detectar degradación y diferencias injustas entre grupos.

## Cómo ejecutar el proyecto

### Google Colab

Haz clic en el botón **Abrir en Google Colab** al comienzo del README.

Luego selecciona:

```text
Entorno de ejecución → Ejecutar todas
```

El notebook intentará descargar automáticamente el dataset.

### Ejecución local

Clona el repositorio:

```bash
git clone https://github.com/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate.git
```

Entra en la carpeta:

```bash
cd "Google-Advanced-Data-Analytics-Professional-Certificate/7. Google Advanced Data Analytics Capstone"
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
jupyter notebook salifort_motors_employee_attrition_colab.ipynb
```

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Statsmodels
- Jupyter Notebook
- Google Colab
- GitHub

## Curso

**Google Advanced Data Analytics Professional Certificate**

Curso 7: **Google Advanced Data Analytics Capstone**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
