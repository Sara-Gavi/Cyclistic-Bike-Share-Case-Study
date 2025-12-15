# Phase 3 – Process

**Project:** Cyclistic Bike-Share Case Study  
**Author:** Sara Gavilán - Full Stack Developer · Data Analytics · Creativity  
**Date:** November 2025

---

## Overview

During the Process phase, the data was prepared to be ready for analysis. The main goal of this step was to clean and transform the original files into consistent and usable datasets.

Spreadsheet tools were used as the main working environment. Google Sheets was initially used for quick exploration of smaller files, but due to the large size of several monthly datasets, Microsoft Excel became the main tool. Excel allowed working with large volumes of data without performance issues and made it easier to manage formulas and formats.

---

## Data Collection and Organization

Twelve months of trip data were downloaded from the official Divvy Tripdata source in ZIP format. All files were unzipped into CSV format and organized into two main folders:

- `01_Raw_CSV`: original raw data files
- `02_Clean_Sheets`: cleaned and transformed datasets

A clear and consistent file-naming system was used for all monthly files.

---

## Data Cleaning and Transformation

Each monthly file was reviewed individually. Two new key variables were created in every dataset:

- **ride_length**: calculated as the difference between the trip end time and the trip start time.
- **day_of_week**: calculated from the trip start date to identify the day of the week for each ride.

During the process, different date and time formats were detected across files. Some datasets included milliseconds, which caused errors in calculations. To solve this issue, helper columns were created to convert all date values into a valid numerical Excel format. This ensured that all months followed the same structure and allowed correct calculations.

---

## Data Integrity and Validation

Several rows from each monthly file were checked manually to verify that:

- Ride durations were positive and correctly calculated.
- Days of the week matched the corresponding dates.

These checks confirmed that the transformations had been applied correctly and that the datasets were ready for analysis.

---

## Final Output

After validation, all cleaned datasets were saved in Excel format (`.xlsx`). These files are now consistent, structured, and fully prepared for the Analysis phase.

Due to GitHub file size limitations, only sample raw and cleaned datasets are stored in the repository, while the full datasets are kept locally.

---

---

# Fase 3 – Process

**Proyecto:** Cyclistic Bike-Share Case Study  
**Autora:** Sara Gavilán - Full Stack Developer · Data Analytics · Creativity
**Fecha:** Noviembre 2025

---

## Visión general

En la fase Process se prepararon los datos para que estuvieran listos para el análisis. El objetivo principal de esta etapa fue limpiar y transformar los archivos originales en conjuntos de datos consistentes y utilizables.

Se utilizaron principalmente herramientas de hojas de cálculo. Google Sheets se empleó de forma inicial para la exploración rápida de archivos pequeños, pero debido al gran tamaño de varios datasets mensuales, Microsoft Excel fue la herramienta principal. Excel permitió trabajar con grandes volúmenes de datos sin problemas de rendimiento y facilitó la gestión de fórmulas y formatos.

---

## Recopilación y organización de los datos

Se descargaron doce meses de datos de viajes desde la fuente oficial Divvy Tripdata en formato ZIP. Todos los archivos se descomprimieron a formato CSV y se organizaron en dos carpetas principales:

- `01_Raw_CSV`: archivos de datos originales
- `02_Clean_Sheets`: conjuntos de datos ya limpiados y transformados

Se siguió una convención clara y coherente de nombres para todos los archivos mensuales.

---

## Limpieza y transformación de datos

Cada archivo mensual fue revisado de forma individual. En todos los datasets se crearon dos nuevas variables clave:

- **ride_length**: calculada como la diferencia entre la hora de finalización y la hora de inicio de cada viaje.
- **day_of_week**: calculada a partir de la fecha de inicio del viaje para identificar el día de la semana.

Durante el proceso se detectaron distintos formatos de fecha y hora entre los archivos. Algunos incluían milisegundos, lo que generaba errores en los cálculos. Para solucionarlo, se utilizaron columnas auxiliares para convertir todas las fechas a un formato numérico válido de Excel. Esto permitió unificar la estructura de todos los meses y garantizar la correcta aplicación de las fórmulas.

---

## Integridad y validación de los datos

Se revisaron manualmente varias filas de cada archivo mensual para comprobar que:

- Las duraciones de los viajes fueran positivas y estuvieran correctamente calculadas.
- Los días de la semana coincidieran con las fechas correspondientes.

Estas verificaciones confirmaron que las transformaciones se habían aplicado correctamente y que los datos estaban listos para el análisis.

---

## Resultado final

Una vez validados, todos los datasets limpios se guardaron en formato Excel (`.xlsx`). Estos archivos quedaron estructurados, consistentes y preparados para la fase de análisis.

Debido a las limitaciones de tamaño de GitHub, únicamente se almacenan en el repositorio archivos de ejemplo (sample), mientras que los datasets completos se conservan de forma local.
