# Hábitos Musicales y Salud Mental

Proyecto 2 de la asignatura *Introducción a la Ciencia de Datos Aplicada al Diseño*.

## Integrantes

- Marcelo Detlefsen · 24554
- Julián Divas · 24687
- Luis Angel Girón · 24753

## Descripción

Este proyecto analiza la relación entre los hábitos de consumo musical y la salud mental, con un enfoque de regresión para predecir el nivel de ansiedad en una escala de 0 a 10.

El trabajo parte del dataset `mxmh_survey_results.csv`, que incluye variables demográficas, hábitos musicales y autoinformes sobre salud mental. Después de la limpieza y el preprocesamiento, se evaluaron tres modelos:

- Regresión Lineal
- Random Forest
- XGBoost

## Objetivo

Construir un modelo que permita estimar el nivel de ansiedad a partir de variables relacionadas con el consumo musical y otros factores asociados, y comparar el desempeño de distintos enfoques de regresión.

## Metodología

- Limpieza de datos y tratamiento de nulos
- Eliminación de outliers extremos
- Codificación de variables categóricas
- Escalado de variables numéricas cuando fue necesario
- Feature engineering para crear variables agregadas
- División de datos en entrenamiento y prueba con proporción 80/20
- Ajuste de hiperparámetros con `GridSearchCV`
- Evaluación con RMSE, MAE, R² y MAPE

## Resultados

| Modelo | RMSE ↓ | MAE ↓ | R² ↑ | MAPE ↓ |
|---|---:|---:|---:|---:|
| Regresión Lineal | 2.338 | 1.863 | 0.297 | 44.10% |
| Random Forest | 2.263 | 1.819 | 0.342 | 42.61% |
| XGBoost | 2.312 | 1.822 | 0.313 | 44.54% |

El mejor desempeño en prueba lo obtuvo **Random Forest**, aunque presentó una diferencia importante entre entrenamiento y prueba, lo que indica sobreajuste.

## Archivos principales

- `Proyecto1_Fase2.ipynb`: notebook con el análisis, el entrenamiento de modelos y las gráficas
- `mxmh_survey_results.csv`: dataset utilizado en el proyecto

## Reproducibilidad

1. Abrir `Proyecto1_Fase2.ipynb` en Jupyter Notebook, JupyterLab o VS Code.
2. Ejecutar las celdas de arriba hacia abajo.
3. Verificar que el dataset `mxmh_survey_results.csv` esté en el mismo directorio del notebook.

## Conclusión

La variable de salud mental con mayor peso en el modelo fue `Depression`, seguida por `OCD` e `Insomnia`. En conjunto, los resultados sugieren que los factores de salud mental explican una parte importante de la ansiedad reportada, mientras que las variables musicales aportan información adicional pero menor.

## Nota

Este repositorio contiene el notebook final y el dataset usado para el análisis. El documento de entrega desarrolla la interpretación completa, la comparación de modelos y la discusión de resultados.
