# 🎵 Hábitos Musicales & Salud Mental — Guía Fase 2

> **Equipo:** Marcelo Detlefsen · 24554 | Julián Divas · 24687 | Luis Angel Girón · 24753

---

## 📌 Contexto rápido

El objetivo de la Fase 2 es predecir el nivel de **ansiedad** (escala 0–10) a partir de hábitos de consumo musical. Se formuló como un problema de **regresión**. El dataset quedó en **715 filas y 54 columnas** tras limpieza, filtro de outliers y feature engineering.

Pueden asumir que el lector ya conoce la Fase 1. Enfóquense en explicar el salto: *¿por qué regresión?* y *¿qué modelos elegimos y por qué?*

---

## 📦 Entregables (lo que hay que hacer)

Son cuatro productos paralelos. Todos deben estar listos:

### 1. Documento (PDF o Word)
Secciones obligatorias en orden:

| # | Sección | Qué incluir |
|---|---|---|
| 1 | Recordatorio del problema | Problemática y dataset de Fase 1 resumidos en un párrafo |
| 2 | Preprocesamiento | Nulos, outliers, encoding, escalado — **cada decisión justificada** |
| 3 | División de datos | Proporción 80/20, justificación, `random_state=42` |
| 4 | Descripción de modelos | Explicación conceptual **en sus propias palabras** + por qué lo eligieron |
| 5 | Hiperparámetros | Qué exploraron, método (GridSearchCV), validación cruzada, mejor config final |
| 6 | Tabla comparativa de métricas | RMSE, MAE, R², MAPE — cuál priorizan y por qué |
| 7 | Overfitting/underfitting | Train vs test, curva de aprendizaje |
| 8 | Importancia de variables | Top variables del modelo ganador + interpretación de dominio |
| 9 | Conclusión | Modelo recomendado, limitaciones, qué mejorarían |
| 10 | Puntos extra (XGBoost) | Justificación, explicación conceptual, fuente académica citada |

### 2. Notebook (.ipynb)
El notebook ya está completo y ejecutado. Solo necesitan:
- Tomar capturas de las gráficas para insertarlas en el documento

### 3. Análisis e interpretación (va dentro del documento)
- Explicar **por qué** se eligieron los modelos, no solo reportar el resultado
- Leer las métricas en contexto: "el modelo se equivoca en promedio 1.82 puntos en una escala de 0–10"
- Discutir trade-offs: velocidad, interpretabilidad, overfitting
- Conclusión argumentada sobre el modelo recomendado

### 4. Presentación (PPT u otra herramienta)
Estructura slide por slide:

| Slide | Contenido | Nota |
|---|---|---|
| 01 | Portada y contexto | Pregunta de investigación + objetivo Fase 2 |
| 02 | Preprocesamiento | Tabla o diagrama de flujo con los pasos clave |
| 03 | Los 3 modelos — ¿Por qué cada uno? | Justificar elección **antes** de mostrar resultados |
| 04 | **Tabla comparativa de métricas** ⭐ | RF ganador, XGBoost peor. Slide crítica. |
| 05 | **Overfitting en Random Forest** ⚠️ | Train R²=0.87 vs Test R²=0.34. No omitir. |
| 06 | **Importancia de variables** ⭐ | Depression (0.311) domina. Conectar con Fase 1. |
| 07 | Conclusiones | Mejor modelo: RF. Limitaciones. R² máx 0.34. |

---

## 🧹 Preprocesamiento (referencia rápida)

| Paso | Decisión | Justificación para el doc |
|---|---|---|
| BPM | Eliminada | 107 nulos (~15%) + outlier extremo (máx 1×10⁹); no confiable |
| Outliers Hours per day | Se eliminaron filas con > 10 h/día (736 → 715 filas) | Valores imposibles de sostener diariamente |
| Nulos numéricos | Imputación con **mediana** | Distribuciones sesgadas; la mediana es más robusta que la media |
| Nulos categóricos | Rellenados con `"Unknown"` | Preserva que el dato no fue reportado |
| Frecuencias musicales | Ordinal Encoding: Never=0, Rarely=1, Sometimes=2, Very frequently=3 | Tienen orden intrínseco; OHE perdería esa información |
| Variables Yes/No | Binario (Yes=1, No=0) | Codificación directa |
| Nominales (Fav genre, Streaming, Music effects) | One-Hot Encoding | Sin orden natural |
| Escalado | StandardScaler **solo para Regresión Lineal** | RF y XGBoost son invariantes a la escala |
| Feature engineering | `total_music_freq`, `genre_diversity`, `avg_music_freq` (creados **antes** del split) | Variables derivadas que capturan patrones agregados |
| Split | 80% train / 20% test, `random_state=42` | ~572 train + 143 test; suficiente para GridSearchCV con cv=5 |

---

## 📊 Resultados reales de los modelos

> ⚠️ **IMPORTANTE:** Estos son los números exactos del notebook ejecutado. Úsenlos en el documento y la presentación.

| Modelo | RMSE ↓ | MAE ↓ | R² ↑ | MAPE ↓ | R²_train | Veredicto |
|---|---|---|---|---|---|---|
| Regresión Lineal | 2.338 | 1.863 | 0.297 | 44.10% | 0.414 | Baseline — underfitting leve |
| **Random Forest** ⭐ | **2.263** | **1.819** | **0.342** | **42.61%** | **0.872** | **Mejor modelo** |
| XGBoost | 2.312 | 1.822 | 0.313 | 44.54% | 0.592 | Overfitting moderado |

**Hiperparámetros ganadores:**
- Random Forest: `max_depth=20`, `min_samples_split=5`, `n_estimators=100`
- XGBoost: `learning_rate=0.05`, `max_depth=3`, `n_estimators=100`

**CV R² (cross-validation sobre train):**
- Regresión Lineal: 0.2533 ± 0.1183
- Random Forest: 0.3199 ± 0.0434
- XGBoost: 0.3374 ± 0.0546

---

## ⚠️ Overfitting — lo que tienen que explicar

| Modelo | R² Train | R² Test | Gap | Diagnóstico |
|---|---|---|---|---|
| Regresión Lineal | 0.414 | 0.297 | 0.117 | Underfitting moderado |
| **Random Forest** | **0.872** | **0.342** | **0.530** | **Overfitting severo** |
| XGBoost | 0.592 | 0.313 | 0.279 | Overfitting moderado |

**Cómo explicarlo en el documento:** Random Forest aprende casi perfectamente los datos de entrenamiento (R²=0.87) pero generaliza mal a datos nuevos (R²=0.34). El gap de 0.53 es consecuencia directa del tamaño del dataset (~572 muestras de entrenamiento): con árboles de hasta `max_depth=20`, el modelo se especializa en ruido. Soluciones a mencionar: más datos, reducir `max_depth`, aumentar `min_samples_leaf`.

---

## 🏆 Importancia de variables (Random Forest)

| # | Variable | Importancia | Tipo |
|---|---|---|---|
| 1 | Depression | 0.311 | Salud mental |
| 2 | OCD | 0.095 | Salud mental |
| 3 | Insomnia | 0.068 | Salud mental |
| 4 | Age | 0.065 | Demográfico |
| 5 | Hours_per_day | 0.033 | 🎵 Musical |
| 6 | genre_diversity | 0.026 | 🎵 Musical (feature engineered) |
| 7 | Frequency_Lofi | 0.025 | 🎵 Musical |
| 8 | Frequency_Metal | 0.020 | 🎵 Musical |
| 9 | Frequency_Folk | 0.020 | 🎵 Musical |
| 10 | avg_music_freq | 0.019 | 🎵 Musical (feature engineered) |

**Interpretación clave para el documento:**
- Depression (0.311) domina — conecta directamente con la correlación ansiedad–depresión de 0.52 vista en la Fase 1.
- Las variables de salud mental (Depression + OCD + Insomnia) acumulan ~47% de la importancia total.
- `genre_diversity` (feature engineered) aparece antes que cualquier frecuencia individual, validando la decisión de crearlo.
- Frequency_Lofi aparece en #7: tiene sentido en el dominio — Lofi se asocia con escucha bajo estrés o ansiedad.
- Las variables musicales suman ~12% de importancia total: influyen, pero no son el factor dominante.

---

## ✅ Checklist antes de entregar

**Documento:**
- [ ] Menciona que es un problema de **regresión** y explica por qué
- [ ] Cada decisión de preprocesamiento tiene justificación (no solo "se hizo")
- [ ] Describe los 3 modelos conceptualmente en sus propias palabras
- [ ] Incluye la tabla de métricas con los números reales de arriba
- [ ] Menciona qué métrica priorizan y por qué (RMSE por penalizar errores grandes)
- [ ] Analiza el overfitting de RF con valores Train R²=0.87 / Test R²=0.34
- [ ] Tabla de importancia de variables con interpretación de dominio
- [ ] Conecta Depression (0.311) con la correlación 0.52 de la Fase 1
- [ ] Sección de limitaciones (datos auto-reportados, dataset pequeño, variables externas ausentes)
- [ ] Cita fuente de XGBoost: Chen & Guestrin (2016), KDD. https://doi.org/10.1145/2939672.2939785

**Notebook:**
- [ ] Corre completo de arriba a abajo sin errores
- [ ] `random_state=42` en todos los modelos y el split
- [ ] GridSearchCV + cross_val_score en los 3 modelos
- [ ] Gráfica pred vs real para los 3 modelos
- [ ] Curva de aprendizaje (learning curve) de Random Forest
- [ ] Gráfica comparativa de métricas

**Presentación:**
- [ ] 7 slides siguiendo la estructura de arriba
- [ ] No mezclar justificación de modelos con resultados (slides separados)
- [ ] Slide de overfitting no omitida