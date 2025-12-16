# 🚴‍♀️ Phase 4 - Analysis (Analyze) - [EN]

**Project:** Cyclistic Bike-Share Case Study  
**Author:** Sara Gavilán - Full Stack Developer · Data Analytics · Creativity
**Date:** November 2025

---

## 1. Overview of the Analysis Phase

The objective of the Analysis phase is to identify **patterns and differences** in the use of the Cyclistic bike-share service between **annual members** and **casual riders**, in order to answer the defined business question.

Two analysis tools were used during this phase:

- **Microsoft Excel:** Used in an initial stage to perform exploratory data analysis, validate calculations, and understand variable behavior.
- **SQL in BigQuery:** Used later as the main analysis tool to work with the full annual dataset, due to its greater scalability, accuracy, and reproducibility.

This combined approach made it possible to validate the initial results obtained in Excel and then deepen the analysis in a more robust way using SQL.

## 2. Initial Exploration and Validation with Excel

### 2.1 Purpose of Using Excel

Excel was used as an initial exploratory analysis tool, working with individual monthly datasets (January and July 2024). This stage made it possible to:

1. Understand the **structure of the data**.
2. Validate the calculation of key variables such as **ride duration**.
3. Identify preliminary patterns by user type and day of the week.
4. Detect **outliers** before scaling the analysis.

### 2.2 Basic Descriptive Analysis (January 2024)

| Tool            | Dataset                            |
| :-------------- | :--------------------------------- |
| Microsoft Excel | `202401-divvy-tripdata_clean.xlsx` |

#### Average Ride Duration

- **Column:** `ride_length`
- **Formula used:** `=AVERAGE(N:N)`
- **Result:** **0:15:03**
  > This value represents the average duration of trips recorded in January 2024.

#### Maximum Ride Duration

- **Formula used:** `=MAX(N:N)`
- **Result:** **24:59:57**
  > This result helped identify the presence of extreme values, which were later addressed during the SQL cleaning phase.

#### Most Frequent Day of the Week

- **Column:** `day_of_week`
- **Formula used:** `=MODE.SNGL(O:O)`
- **Result:** **4 (Wednesday)**
  > This analysis showed that service usage is mainly concentrated on weekdays.

### 2.3 Comparison by User Type Using Pivot Tables

| Main Configuration | Detail                   |
| :----------------- | :----------------------- |
| **Rows**           | `member_casual`          |
| **Values**         | Average of `ride_length` |
| **Time format**    | `[h]:mm:ss`              |

#### Result – January 2024

| User Type  | Average Duration |
| :--------- | :--------------- |
| **Casual** | **0:21:18**      |
| **Member** | **0:13:47**      |

These results show that **casual riders take longer trips**, while **members** exhibit a **shorter, more functional** usage pattern.

### 2.4 Seasonal Analysis with Excel (July 2024)

The same analysis was repeated using July 2024 data, allowing the observation of **seasonal patterns**:

- Significant increase in average ride duration.
- Increase in **casual usage during weekends**.
- Stable and functional behavior among members.

This exploratory analysis confirmed that the patterns observed in January persist but intensify during the summer months.

---

## 3. Transition to SQL and BigQuery

### 3.1 Justification for Using SQL

While Excel was useful for initial exploration, the full analysis required:

- Working with **more than 5 million records**.
- Combining **12 months of data** into a single source.
- Reproducing calculations consistently.
- Avoiding performance limitations and manual errors.

For these reasons, the main analysis was conducted using **SQL in BigQuery**, enabling a more robust, scalable, and professional workflow.

## 4. Main Analysis with SQL in BigQuery

### 4.1 Combining Monthly Datasets

The monthly datasets were combined into a single annual table:

```sql
CREATE OR REPLACE TABLE `cyclistic_tripdata.trips_all` AS
SELECT * FROM `cyclistic_tripdata.trips_202312`
UNION ALL
SELECT * FROM `cyclistic_tripdata.trips_202401`
-- UNION ALL applied to the remaining months
;
```

This made it possible to work with the complete dataset in a single analysis.

### 4.2 Initial Dataset Exploration

**Total Number of Rides**

```sql
SELECT
  COUNT(*) AS total_rides
FROM `cyclistic_tripdata.trips_all`;

```

**Result:** 5,906,269 rides.

**User Types**

```sql
SELECT
  member_casual,
  COUNT(*) AS number_of_rides
FROM `cyclistic_tripdata.trips_all`
GROUP BY member_casual;
```

We clearly see two groups: annual members and casual users. Members are responsible for most of the rides.

### 4.3 Creation of Key Variables

These variables help us repeat and expand the calculations we did before in Excel:

**Ride Duration (in seconds):**
`sql
    TIMESTAMP_DIFF(ended_at, started_at, SECOND) AS ride_length_seconds
    `

**Day of the Week:**
`sql
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week
    `

These variables allow the replication and expansion of the calculations previously performed in Excel.

### 4.4 Data Cleaning

Rides with unrealistic durations were removed:

```sql
WHERE
  ride_length_seconds > 60
  AND ride_length_seconds < 86400;
```

This step ensures that the analysis is not distorted by extreme values.

### 4.5 Comparative Analysis

**Average Ride Duration by User Type**

```sql
SELECT
  member_casual,
  AVG(ride_length_seconds) / 60 AS avg_ride_length_minutes
FROM `cyclistic_tripdata.trips_cleaned`
GROUP BY member_casual;
```

Casual users have significantly longer average ride durations than members.

**Use by Day of the Week**

```sql
SELECT
  day_of_week,
  member_casual,
  COUNT(*) AS number_of_rides
FROM `cyclistic_tripdata.trips_cleaned`
GROUP BY day_of_week, member_casual;
```

Members mainly use the service on weekdays, while casual users increase their activity during the weekend.

## 5. Conclusions of the Analysis

The SQL analysis confirms and expands the patterns first seen in Excel:

**Casual Users:** They take longer trips and use the service more for fun, especially on weekends and in summer months.
**Annual Members:** They use the service more often, with short and regular trips, mainly during the week.

These behavior differences are consistent all year and are key for creating strategies to convert casual users into members.

---

# 🚴‍♀️ Fase 4 - Análisis (Analyze) - [ES]

**Proyecto:** Cyclistic Bike-Share Case Study
**Autora:** Sara Gavilán – Full Stack Developer · Data Analytics · Creativity
**Fecha:** Noviembre 2025

---

## 1. Descripción general de la fase de análisis

El objetivo de la fase de Análisis es identificar **patrones y diferencias** en el uso del servicio de bicicletas Cyclistic entre **miembros anuales** y **usuarios ocasionales**, con el fin de responder a la pregunta de negocio planteada.

Para esta fase se utilizaron dos herramientas de análisis:

- **Microsoft Excel:** Empleado en una primera etapa para realizar una exploración inicial de los datos, validar cálculos y comprender el comportamiento de las variables.
- **SQL en BigQuery:** Utilizado posteriormente como herramienta principal para realizar el análisis completo sobre el conjunto anual de datos, debido a su mayor escalabilidad, precisión y reproducibilidad.

Este enfoque combinado permitió validar los resultados iniciales obtenidos en Excel y, posteriormente, profundizar el análisis de forma más robusta utilizando SQL.

## 2. Exploración inicial y validación con Excel

### 2.1 Objetivo del uso de Excel

Excel se utilizó como herramienta inicial de análisis exploratorio, trabajando con conjuntos de datos mensuales individuales (enero y julio de 2024). Esta etapa permitió:

1.  Comprender la **estructura de los datos**.
2.  Validar el cálculo de variables clave como la **duración del viaje**.
3.  Identificar patrones preliminares por tipo de usuario y día de la semana.
4.  Detectar **valores extremos** antes de escalar el análisis.

### 2.2 Análisis descriptivo básico (Enero 2024)

| Herramienta     | Dataset                            |
| :-------------- | :--------------------------------- |
| Microsoft Excel | `202401-divvy-tripdata_clean.xlsx` |

#### Duración media del viaje

- **Columna:** `ride_length`
- **Fórmula utilizada:** `=PROMEDIO(N:N)`
- **Resultado:** **0:15:03**
  > Este valor representa la duración media de los viajes registrados en enero de 2024.

#### Duración máxima del viaje

- **Fórmula utilizada:** `=MAX(N:N)`
- **Resultado:** **24:59:57**
  > Este resultado permitió identificar la presencia de valores extremos, que posteriormente fueron tratados en la fase de limpieza con SQL.

#### Día de la semana más frecuente

- **Columna:** `day_of_week`
- **Fórmula utilizada:** `=MODA.UNO(O:O)`
- **Resultado:** **4 (Miércoles)**
  > Este análisis mostró que el uso del servicio se concentra principalmente en días laborables.

### 2.3 Comparación por tipo de usuario mediante tablas dinámicas

| Configuración Principal | Detalle                   |
| :---------------------- | :------------------------ |
| **Filas**               | `member_casual`           |
| **Valores**             | Promedio de `ride_length` |
| **Formato de tiempo**   | `[h]:mm:ss`               |

#### Resultado – Enero 2024

| Tipo de usuario | Duración media |
| :-------------- | :------------- |
| **Casual**      | **0:21:18**    |
| **Miembro**     | **0:13:47**    |

Estos resultados muestran que los **usuarios casuales realizan viajes más largos**, mientras que los **miembros** presentan un uso más **corto y funcional**.

### 2.4 Análisis estacional con Excel (Julio 2024)

El mismo análisis se repitió con los datos de julio de 2024, permitiendo observar **patrones estacionales**:

- Aumento significativo de la duración media de los viajes.
- Incremento del **uso casual durante fines de semana**.
- Comportamiento estable y funcional entre los miembros.

Este análisis exploratorio confirmó que los patrones observados en enero se mantienen, pero se intensifican durante los meses de verano.

---

## 3. Transición a SQL y BigQuery

### 3.1 Justificación del uso de SQL

Aunque Excel fue útil para la exploración inicial, el análisis completo requería:

- Trabajar con **más de 5 millones de registros**.
- Unificar **12 meses de datos** en una sola fuente.
- Reproducir cálculos de forma consistente.
- Evitar limitaciones de rendimiento y errores manuales.

Por estos motivos, el análisis principal se realizó utilizando **SQL en BigQuery**, que permite un análisis más robusto, escalable y profesional.

## 4. Análisis principal con SQL en BigQuery

### 4.1 Unión de los datos mensuales

Se combinaron los datos mensuales en una única tabla anual:

```sql
CREATE OR REPLACE TABLE `cyclistic_tripdata.trips_all` AS
SELECT * FROM `cyclistic_tripdata.trips_202312`
UNION ALL
SELECT * FROM `cyclistic_tripdata.trips_202401`
-- UNION ALL aplicado al resto de meses
;

```

Esto permitió trabajar con el conjunto completo de datos en un solo análisis.

### 4.2 Exploración inicial del dataset anual

**Número total de viajes**

```sql
SELECT COUNT(*) AS total_rides
FROM `cyclistic_tripdata.trips_all`;
```

**Resultado:** 5.906.269 viajes.

**Tipos de usuario**

```sql
SELECT
  member_casual,
  COUNT(*) AS number_of_rides
FROM `cyclistic_tripdata.trips_all`
GROUP BY member_casual;
```

Se identifican claramente dos grupos: miembros anuales y usuarios casuales, siendo los miembros responsables de la mayoría de los viajes.

### 4.3 Creación de variables clave

**Duración del viaje:**
`sql
    TIMESTAMP_DIFF(ended_at, started_at, SECOND) AS ride_length_seconds
    `

**Día de la semana:**
`sql
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week
    `

Estas variables permiten replicar y ampliar los cálculos realizados previamente en Excel.

### 4.4 Limpieza de datos

Se eliminaron viajes con duraciones poco realistas:

```sql
WHERE
  ride_length_seconds > 60
  AND ride_length_seconds < 86400;
```

Este paso asegura que el análisis no esté distorsionado por valores extremos (viajes menores a 60 segundos o mayores a 24 horas).

### 4.5 Análisis comparativo

**Duración media por tipo de usuario**

```sql
SELECT
  member_casual,
  AVG(ride_length_seconds) / 60 AS avg_ride_length_minutes
FROM `cyclistic_tripdata.trips_cleaned`
GROUP BY member_casual;
```

Los usuarios casuales presentan duraciones medias significativamente mayores que los miembros.

**Uso por día de la semana**

```sql
SELECT
  day_of_week,
  member_casual,
  COUNT(*) AS number_of_rides
FROM `cyclistic_tripdata.trips_cleaned`
GROUP BY day_of_week, member_casual;
```

Los miembros concentran su uso en días laborables, mientras que los usuarios casuales incrementan su actividad durante el fin de semana.

## 5. Conclusiones del análisis

El análisis realizado con SQL confirma y amplía los patrones identificados inicialmente en Excel:

**Usuarios Casuales:** Realizan viajes más largos y con mayor peso recreativo, especialmente durante fines de semana y meses de verano.
**Miembros Anuales:** Utilizan el servicio de forma más frecuente, con viajes cortos y estables, principalmente entre semana.

Estas diferencias de comportamiento son consistentes a lo largo del año y resultan clave para diseñar estrategias orientadas a la conversión de usuarios casuales en miembros.
