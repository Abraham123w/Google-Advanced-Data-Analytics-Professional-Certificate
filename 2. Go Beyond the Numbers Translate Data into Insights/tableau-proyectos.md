# Proyectos de visualización en Tableau

Esta sección reúne los dashboards interactivos desarrollados en **Tableau Public** como parte del **Certificado Profesional Avanzado de Análisis de Datos de Google (Google Advanced Data Analytics)**. Corresponden a la etapa de análisis exploratorio y comunicación visual del marco de trabajo **PACE**, en la que los datos ya limpios se transforman en visualizaciones que respondan preguntas concretas de negocio.

Los temas abordados incluyen movilidad urbana, transporte, demanda de bicicletas compartidas, eventos meteorológicos y control de calidad de datos.

---

## NYC Taxi Data: Exploratory Analysis & Data Integrity

Este dashboard forma parte del análisis exploratorio del dataset de viajes en taxis amarillos de Nueva York (el mismo utilizado en el proyecto de Automatidata). El objetivo fue evaluar la **integridad de los datos** antes de avanzar a un modelo predictivo, por lo que el gráfico central es un **diagrama de dispersión** que cruza `trip_distance` (distancia recorrida) contra `total_amount` (monto total pagado).

Cada punto representa un viaje individual. La nube de puntos permite ver visualmente la relación esperada —a mayor distancia, mayor tarifa— y, más importante, hace evidentes los casos que **rompen ese patrón**: viajes con distancia cero pero tarifa cobrada, viajes con tarifas negativas y viajes con montos desproporcionadamente altos para la distancia recorrida. Estos puntos "fuera de la nube" son justamente los candidatos a valores atípicos o errores de registro que se identificaron también en el análisis de Python.

[![NYC Taxi Data Tableau Dashboard](https://public.tableau.com/static/images/NY/NYCTaxiDataExploratoryAnalysisDataIntegrity/Distancevs_TotalAmountOutlierAnalysis/1_rss.png)](https://public.tableau.com/views/NYCTaxiDataExploratoryAnalysisDataIntegrity/Distancevs_TotalAmountOutlierAnalysis)

[Ver dashboard interactivo en Tableau Public](https://public.tableau.com/views/NYCTaxiDataExploratoryAnalysisDataIntegrity/Distancevs_TotalAmountOutlierAnalysis)

**Herramientas y competencias:** Tableau, análisis exploratorio, detección de valores atípicos, calidad de datos y visualización de relaciones entre variables continuas.

---

## Seoul Bike Demand: Holiday vs. Non-Holiday Impact Analysis

Dashboard construido para responder una pregunta puntual de negocio: **¿los días festivos alteran significativamente la demanda de bicicletas compartidas en Seúl?** La visualización principal combina **series de tiempo** con una segmentación por tipo de día (feriado / no feriado), lo que permite comparar el volumen de alquileres directamente, en vez de tener que leer tablas de números por separado.

Además de la comparación general, el dashboard incorpora un análisis de los **días adyacentes a los feriados** (el día antes y el día después), ya que en muchos casos el comportamiento de los usuarios cambia no solo el día del feriado, sino también en los días cercanos —algo que solo se detecta comparando visualmente los tres periodos lado a lado.

[![Seoul Bike Demand Tableau Dashboard](https://public.tableau.com/static/images/Se/SeoulBikeDemandHolidayvs_Non-HolidayImpactAnalysis/Dashboard1/1_rss.png)](https://public.tableau.com/views/SeoulBikeDemandHolidayvs_Non-HolidayImpactAnalysis/Dashboard1)

[Ver dashboard interactivo en Tableau Public](https://public.tableau.com/views/SeoulBikeDemandHolidayvs_Non-HolidayImpactAnalysis/Dashboard1)

**Herramientas y competencias:** Tableau, análisis temporal, comparación de grupos, estudio de demanda urbana y comunicación visual orientada a decisiones operativas.

---

## Interactive Geospatial Intelligence: U.S. Lightning Metrics

Este dashboard aplica **visualización geoespacial** para estudiar la frecuencia e intensidad de rayos en distintas regiones de Estados Unidos. El componente principal es un **mapa interactivo** donde la densidad o el color de cada zona representa la cantidad de eventos registrados, lo que facilita identificar de un vistazo las áreas de mayor actividad eléctrica atmosférica.

El dashboard incluye **filtros por año** y **parámetros interactivos**, de modo que el usuario puede explorar cómo cambia el patrón geográfico a lo largo del tiempo sin necesidad de generar un gráfico distinto para cada periodo. Este tipo de diseño —un solo mapa con controles dinámicos— es preferible a múltiples mapas estáticos porque reduce la carga cognitiva y permite comparaciones directas.

[![U.S. Lightning Metrics Tableau Dashboard](https://public.tableau.com/static/images/In/InteractiveGeospatialIntelligenceU_S_LightningMetricsDashboard/Dashboard1/1_rss.png)](https://public.tableau.com/views/InteractiveGeospatialIntelligenceU_S_LightningMetricsDashboard/Dashboard1)

[Ver dashboard interactivo en Tableau Public](https://public.tableau.com/views/InteractiveGeospatialIntelligenceU_S_LightningMetricsDashboard/Dashboard1)

**Herramientas y competencias:** Tableau, análisis geoespacial, mapas interactivos, uso de filtros y parámetros, interpretación de indicadores territoriales.

---

## Seoul Bike Rental Analysis: Urban Mobility Dashboard

Dashboard de movilidad urbana centrado en el comportamiento del alquiler de bicicletas en Seúl durante 2018. A diferencia del análisis anterior de feriados, aquí el foco está en la **estacionalidad y los ciclos de uso**: la demanda se descompone por **hora del día**, **día de la semana** y **estación del año**, usando gráficos de barras y líneas combinados para que cada nivel de granularidad se pueda leer por separado o en conjunto.

Este tipo de desagregación es clave para un caso de negocio real, ya que responde preguntas operativas distintas: la vista por hora ayuda a planificar la disponibilidad de bicicletas en horas punta, la vista por día de la semana identifica diferencias entre uso laboral y recreativo, y la vista estacional permite anticipar la necesidad de mantenimiento o redistribución de flota según la época del año.

[![Seoul Bike Rental Tableau Dashboard](https://public.tableau.com/static/images/Se/SeoulBikeRentalAnalysisInteractiveTableauDashboardforUrbanMobility/Hoja2/1_rss.png)](https://public.tableau.com/views/SeoulBikeRentalAnalysisInteractiveTableauDashboardforUrbanMobility/Hoja2)

[Ver dashboard interactivo en Tableau Public](https://public.tableau.com/views/SeoulBikeRentalAnalysisInteractiveTableauDashboardforUrbanMobility/Hoja2)

**Herramientas y competencias:** Tableau, movilidad urbana, análisis horario, estacionalidad, comparación de días y diseño de dashboards multinivel.
