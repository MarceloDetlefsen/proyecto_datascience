# 🎵 Hábitos Musicales & Salud Mental — Guía Fase 2

> **Equipo:** Marcelo Detlefsen · 24554 | Julián Divas · 24687 | Luis Angel Girón · 24753

---

## 📌 Contexto rápido

El objetivo de la Fase 2 es predecir el nivel de **ansiedad** (escala 0–10) a partir de hábitos de consumo musical. Se formuló como un problema de **regresión**. El dataset quedó en **715 filas y 61 columnas** tras limpieza y filtro de outliers.

Pueden asumir que el lector ya conoce la Fase 1. Enfóquense en explicar el salto: *¿por qué regresión?* y *¿qué modelos elegimos y por qué?*

---

## 🧹 Preprocesamiento (está correcto, no hay que cambiar nada)

| Paso | Decisión |
|---|---|
| Outliers | Se eliminaron registros con `Hours per day > 10` (735 → 715 filas) |
| Nulos numéricos | Imputación con **mediana** |
| Nulos categóricos | Rellenados con `"Unknown"` |
| Encoding | One-Hot Encoding en variables nominales |
| Frecuencias | Never=0, Rarely=1, Sometimes=2, Very frequently=3 |
| Features nuevos | `total_music_freq`, `genre_diversity`, `avg_music_freq`, `high_listener` |

---

## 📊 Resultados reales de los modelos

> ⚠️ **IMPORTANTE:** Estos son los números que salieron al ejecutar el notebook con el dataset real. Úsenlos en el documento y la presentación, no los inventen.

| Modelo | RMSE ↓ | MAE ↓ | R² ↑ | Veredicto |
|---|---|---|---|---|
| Linear Regression | 2.406 | 1.900 | 0.256 | Baseline |
| **Random Forest** ⭐ | **2.252** | **1.815** | **0.348** | **Mejor modelo** |
| XGBoost | 2.419 | 1.880 | 0.248 | Peor modelo |

---

## ⚠️ Overfitting en Random Forest

Aunque Random Forest es el mejor modelo, tiene un problema de overfitting que **deben mencionar** en el documento. No lo escondan, al contrario, muestra que hicieron un análisis crítico.

| Conjunto | R² |
|---|---|
| Entrenamiento | 0.9035 |
| Prueba | 0.3482 |

**Cómo explicarlo:** El modelo aprende casi perfectamente los datos de entrenamiento pero generaliza mal a datos nuevos. Causas posibles: dataset pequeño (715 registros) y ruido inherente de datos auto-reportados. Soluciones que pueden mencionar: más datos, reducir `max_depth`, regularización.

---

## 🏆 Importancia de variables (Random Forest)

Este es uno de los hallazgos más interesantes del proyecto. **La música importa menos de lo esperado.**

| # | Variable | Importancia | Tipo |
|---|---|---|---|
| 1 | Depression | 0.298 | Salud mental |
| 2 | OCD | 0.091 | Salud mental |
| 3 | Age | 0.064 | Demográfico |
| 4 | Insomnia | 0.064 | Salud mental |
| 5 | Hours per day | 0.031 | 🎵 Musical |
| 6 | genre_diversity | 0.027 | 🎵 Musical |
| 7 | Frequency [Lofi] | 0.026 | 🎵 Musical |

**Interpretación clave para destacar:** El mayor predictor de ansiedad es la depresión (0.298), no la música. Esto conecta directamente con la Fase 1 donde se vio una correlación ansiedad–depresión de 0.52. Las variables musicales aparecen recién desde la posición #5. La música influye, pero no es el factor dominante.

---

## 🖥️ Guía para la presentación

Estructura sugerida slide por slide:

| Slide | Contenido | Nota |
|---|---|---|
| 01 | Portada y contexto | Pregunta de investigación + objetivo Fase 2 |
| 02 | Preprocesamiento | Tabla o diagrama de flujo con los pasos |
| 03 | Los 3 modelos — ¿Por qué cada uno? | Justificar elección, sin resultados todavía |
| 04 | **Tabla comparativa de métricas** ⭐ | RF ganador, XGBoost peor. Slide crítica. |
| 05 | **Overfitting en Random Forest** ⚠️ | Train R²=0.90 vs Test R²=0.35. No omitir. |
| 06 | **Importancia de variables** ⭐ | Depression (0.298) domina. Conectar con Fase 1. |
| 07 | Conclusiones | Mejor modelo: RF. Limitaciones. R² máx 0.35. |

---

## ✅ Checklist antes de entregar

- [ ] El documento dice **Random Forest** como mejor modelo (no XGBoost)
- [ ] Incluyen los números reales: RMSE 2.252, MAE 1.815, R² 0.348 para RF
- [ ] Mencionan el overfitting con valores Train R²=0.90 / Test R²=0.35
- [ ] Explican que Depression es la variable más predictiva (0.298)
- [ ] Conectan conclusiones con los hallazgos de la Fase 1 (correlación ansiedad–depresión 0.52)
- [ ] El documento tiene una sección de **limitaciones** (datos auto-reportados, sesgo de muestra, tamaño del dataset)
- [ ] Las gráficas del notebook se pueden insertar directamente como imágenes en el doc