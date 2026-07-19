# Salifort Motors: análisis y predicción de rotación de empleados

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/blob/main/6.%20Google%20Advanced%20Data%20Analytics%20Capstone/salifort_motors_employee_attrition_colab.ipynb)

## Caso de estudio

En este proyecto asumí el rol de profesional de datos para **Salifort Motors**, una empresa ficticia cuyo departamento de Recursos Humanos busca comprender por qué algunos empleados abandonan la organización.

La empresa había recopilado información sobre satisfacción laboral, carga de trabajo, evaluaciones de desempeño, antigüedad, salario, promociones y departamento. Sin embargo, necesitaba transformar estos datos en conclusiones útiles para reducir la rotación.

La pregunta principal fue:

> ¿Qué factores hacen más probable que un empleado deje la empresa?

El objetivo fue analizar los datos de Recursos Humanos, identificar los principales factores asociados con la salida de empleados y construir modelos capaces de predecir si una persona probablemente permanecerá o abandonará la compañía.

## Objetivos del proyecto

El notebook desarrolla las siguientes tareas:

- Comprender el problema de negocio.
- Identificar a los stakeholders.
- Analizar consideraciones éticas.
- Cargar y explorar el dataset.
- Corregir nombres de columnas.
- Detectar valores faltantes, duplicados y valores atípicos.
- Analizar satisfacción, carga laboral, salario, antigüedad y desempeño.
- Preparar variables para modelado.
- Construir una regresión logística.
- Evaluar sus supuestos.
- Construir y optimizar un modelo Random Forest.
- Comparar el desempeño de ambos modelos.
- Formular recomendaciones para reducir la rotación.

## Stakeholders

Los principales interesados en el proyecto son:

- **Recursos Humanos:** responsable de diseñar estrategias de retención.
- **Alta dirección:** interesada en reducir costos de contratación y capacitación.
- **Jefaturas de departamento:** responsables de la carga de trabajo y del desarrollo de sus equipos.
- **Empleados:** directamente afectados por las decisiones basadas en el análisis.
- **Equipo de datos:** responsable de la calidad, la ética y el seguimiento del modelo.

## Consideraciones éticas

El modelo debe utilizarse como una herramienta de apoyo para mejorar las condiciones laborales, no como un sistema de vigilancia o castigo.

Los principales riesgos considerados son:

- Reidentificación de empleados.
- Uso de predicciones para despidos preventivos.
- Reproducción de sesgos presentes en evaluaciones o salarios.
- Etiquetado injusto de empleados como desleales.
- Decisiones automatizadas sin revisión humana.
- Uso del modelo sin informar a las personas afectadas.

Las predicciones deben orientar intervenciones positivas, como redistribución de trabajo, revisiones salariales, oportunidades de desarrollo y conversaciones de apoyo.

## Estructura del proyecto

```text
6. Google Advanced Data Analytics Capstone/
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

Durante la limpieza se corrigen varios nombres:

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

El notebook carga el archivo desde la siguiente ruta:

```python
DATA_URL = (
    "https://raw.githubusercontent.com/"
    "Abraham123w/Google-Advanced-Data-Analytics-Professional-Certificate/"
    "main/6.%20Google%20Advanced%20Data%20Analytics%20Capstone/"
    "HR_capstone_dataset.csv"
)

df0 = pd.read_csv(DATA_URL)
```

También busca una copia local en:

```text
/content
/mnt/data
```

Esto permite ejecutar el proyecto tanto en Google Colab como en un entorno local.

## Qué se hace en el código

### 1. Exploración inicial

El notebook utiliza:

```python
df0.info()
df0.shape
df0.describe()
df0.describe(include="object")
```

Esto permite revisar:

- Cantidad de filas y columnas.
- Tipos de datos.
- Valores mínimos y máximos.
- Promedios y cuartiles.
- Categorías más frecuentes.
- Posibles problemas de calidad.

### 2. Limpieza de datos

Se comprueban valores faltantes:

```python
df0.isna().sum()
```

El dataset no presenta valores nulos.

También se identifican duplicados:

```python
df0.duplicated().sum()
```

Se encontraron **3.008 filas duplicadas**.

Después de eliminarlas:

```text
Filas originales: 14.999
Filas limpias: 11.991
```

### 3. Valores atípicos

Se analiza la variable `tenure` mediante un boxplot y la regla del rango intercuartílico.

Se identifican aproximadamente:

```text
824 registros atípicos
6,87 % del dataset limpio
```

Estos registros no se eliminan automáticamente porque pueden representar empleados reales con mayor antigüedad.

### 4. Balance de la variable objetivo

La variable `left` indica:

| Valor | Significado |
|---:|---|
| 0 | El empleado permaneció |
| 1 | El empleado dejó la empresa |

Después de limpiar duplicados:

```text
83,4 % permaneció
16,6 % dejó la empresa
```

Existe un desbalance de clases, por lo que se evalúan los modelos con:

- Accuracy.
- Precision.
- Recall.
- F1-score.
- ROC-AUC.

## Principales hallazgos del análisis exploratorio

### Satisfacción laboral

Los empleados que permanecen presentan una satisfacción más alta y estable.

Quienes se van muestran niveles de satisfacción más bajos y una distribución más variable.

### Carga laboral y burnout

Los empleados con 6 o 7 proyectos trabajan frecuentemente entre 250 y 300 horas mensuales y presentan una elevada rotación.

También existe rotación entre empleados con solo 2 proyectos, posiblemente asociada con desmotivación o falta de desarrollo.

El rango más estable se encuentra entre:

```text
3 a 5 proyectos
150 a 250 horas mensuales
```

### Antigüedad

La mayor concentración de salidas ocurre entre los años 3 y 5.

El cuarto año aparece como un período especialmente crítico.

### Salario

La mayoría de los empleados que se van pertenece a los niveles salariales bajo y medio.

Los empleados con salario alto presentan una mayor retención.

### Evaluación de desempeño

Entre quienes se van aparecen dos grupos:

- Empleados con evaluaciones bajas.
- Empleados con evaluaciones muy altas.

El segundo grupo puede representar una pérdida de talento asociada con sobrecarga o falta de reconocimiento.

### Departamento

La rotación no está distribuida de forma uniforme.

Algunos departamentos requieren intervenciones específicas en lugar de una política general para toda la empresa.

### Promociones

Los empleados que no han recibido promociones durante los últimos cinco años presentan una mayor rotación.

## Preparación para el modelado

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

## División de entrenamiento y prueba

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
- Estratificación para mantener la proporción de empleados que se van.

## Regresión logística

La regresión logística se utiliza como modelo base por su facilidad de interpretación.

```python
log_reg = LogisticRegression(
    max_iter=1000,
    random_state=42
)
```

Antes del entrenamiento, las variables se estandarizan:

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

También se revisan:

- Independencia de observaciones.
- Multicolinealidad mediante VIF.
- Observaciones influyentes mediante distancia de Cook.
- Relación entre variables continuas y el logit.

### Resultados aproximados

| Métrica | Resultado |
|---|---:|
| Accuracy | 0,83 |
| Precision | 0,50 |
| Recall | 0,21 |
| F1-score | 0,30 |
| ROC-AUC | 0,825 |

La principal debilidad del modelo es el bajo recall, porque no identifica a una gran parte de los empleados que finalmente se van.

## Random Forest

Se construye un modelo:

```python
rf = RandomForestClassifier(
    random_state=42
)
```

Después se utiliza `GridSearchCV` para ajustar:

- Profundidad máxima.
- Número de árboles.
- Número mínimo de muestras por hoja.
- Número de variables consideradas en cada división.
- Proporción de observaciones por árbol.

El mejor modelo se selecciona según el F1-score.

### Resultados aproximados

| Métrica | Resultado |
|---|---:|
| Accuracy | 98,36 % |
| Precision | 98,24 % |
| Recall | 91,75 % |
| F1-score | 0,9488 |
| ROC-AUC | 0,9769 |

Random Forest supera ampliamente a la regresión logística, especialmente en recall.

## Resultado principal

Los principales factores asociados con la rotación son:

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

Implementar conversaciones formales sobre carrera, salario y promoción entre los años 3 y 5.

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
- No incluye modalidad de trabajo, beneficios ni distancia de traslado.
- Las evaluaciones pueden contener sesgos.
- Las asociaciones no prueban causalidad.
- La rotación futura puede cambiar si cambian las condiciones de la empresa.
- Los datos duplicados originales sugieren problemas de recolección.
- El modelo debe monitorearse para detectar degradación y diferencias injustas.

## Cómo ejecutar el proyecto

### Google Colab

Haz clic en el botón **Abrir en Google Colab** ubicado al inicio del README.

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
cd "Google-Advanced-Data-Analytics-Professional-Certificate/6. Google Advanced Data Analytics Capstone"
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

Curso 6: **Google Advanced Data Analytics Capstone**

## Autor

**Abraham Andrés Castro Copa**

Ingeniero Civil Industrial orientado al análisis de datos, visualización, automatización e inteligencia artificial.
