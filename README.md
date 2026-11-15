# Fundación YPF - Data Science: Análisis de Tasas de Promoción Escolar (2012-2023)

**Alumnas:** Florencia Zoni, Virginia Contreras  
**Programa:** Data Science - Fundación YPF

---

## 📌 Descripción del Proyecto

Este repositorio contiene el trabajo final de Data Science, cuyo objetivo es analizar y modelar las **tasas de promoción efectiva** de los estudiantes en las distintas jurisdicciones de Argentina entre 2012 y 2023.

El estudio se divide en dos grandes etapas metodológicas:

1. **Aprendizaje supervisado (regresión y clasificación):** predicción de la tasa de promoción del nivel secundario del año siguiente ($t+1$) a partir de los indicadores del año actual ($t$).
2. **Aprendizaje no supervisado (clustering):** agrupamiento de las provincias según sus patrones históricos de promoción en los distintos años de escolaridad.

---

## 📂 Dataset

* **Nombre:** `Tasa_de_Promocion_Efectiva_2012-2023_unificada.xlsx`
* **Fuente:** registros oficiales del sistema educativo argentino (2012-2023).
* **Unidad de análisis:** jurisdicción-año (324 filas en el archivo original).
* **Limpieza aplicada:** se excluyen los agregados *Total País*, *Conurbano* y *Resto de Bs As* para evitar duplicidad de observaciones, quedando **24 jurisdicciones** (288 observaciones). *Total País* se usa solo como referencia nacional en el análisis exploratorio.
* **Columnas descartadas:** `Primaria - 7° Año` y `Secundaria - 7° Año`, que tienen nulos estructurales (cada año de estudio existe solo en una de las dos estructuras educativas, 7-5 o 6-6).

---

## 🚀 Estructura del Trabajo

### Parte 1: Aprendizaje Supervisado (Regresión y Clasificación)

**Notebook:** `Entrega_Final_YPF_Data_Science.ipynb`

* **Objetivo de regresión:** predecir `Secundaria_Total_Siguiente` ($t+1$).
* **Objetivo de clasificación:** predecir si la tasa de promoción **sube o no sube** en $t+1$.
* **Variables predictoras:** las 6 tasas de secundaria del año $t$ (Total y 8° a 12° año) y la estructura educativa (6-6 / 7-5).
* **Partición temporal:**
  * *Train:* $t \le 2018$ (168 observaciones).
  * *Test:* $t \ge 2019$ (96 observaciones, incluye el impacto de la pandemia).
* **Validación y optimización:** `GridSearchCV` con validación cruzada temporal por años (*expanding window*, validando en 2014-2018).
* **Modelos comparados:**
  * Regresión: baseline de media, baseline de persistencia, Regresión Lineal, Ridge y Random Forest.
  * Clasificación: baseline de clase mayoritaria, Regresión Logística y Random Forest.

**Resultados en test:**

| Tarea | Modelo seleccionado | Resultados |
|---|---|---|
| Regresión | Random Forest (`max_depth=3`, `min_samples_leaf=5`, `n_estimators=300`) | MAE 4,30 p.p. · RMSE 6,07 p.p. · R² −0,28 |
| Regresión (sin el año de 2020) | Random Forest | MAE 2,59 p.p. · R² 0,38 |
| Clasificación | Regresión Logística (`C=0,01`) | Accuracy 0,59 · F1 0,69 (baseline: 0,45 · 0,62) |

> **Nota sobre el R² negativo:** el salto de la tasa nacional en 2020 (de ≈ 82% a 93,7%, por las flexibilizaciones de promoción durante la pandemia) es un cambio de régimen que los modelos, entrenados con 2012-2018, no pudieron anticipar. Con la persistencia como referencia (MAE 5,05), los modelos igualmente mejoran el error absoluto.

### Parte 2: Aprendizaje No Supervisado (Clustering)

**Notebook:** `<nombre-del-notebook-de-clustering>.ipynb`

* **Objetivo:** identificar agrupamientos de provincias con trayectorias escolares similares.
* **Algoritmos comparados:** K-Means (modelo base), Clustering Jerárquico (Ward) y DBSCAN.
* **Modelo final:** K-Means sobre componentes principales (PCA, 4 componentes, $k=3$).
* **Clusters identificados ($k=3$):**
  1. *Cluster 0 - Promoción alta y sostenida en secundaria inicial.*
  2. *Cluster 1 - Promoción media/baja con mayor caída en los últimos años.*
  3. *Cluster 2 - Desempeño crítico con altas tasas de no promoción.*

---

## 🛠️ Tecnologías Utilizadas

* **Lenguaje:** Python 3.x
* **Librerías principales:**
  * `pandas`, `numpy`, `openpyxl` (manipulación y lectura de datos)
  * `scikit-learn` (regresión, clasificación, PCA, K-Means, DBSCAN, jerárquico, `GridSearchCV` y métricas)
  * `matplotlib`, `seaborn` (visualización)

---

## ⚙️ Instrucciones para Ejecutar

### Opción A: Google Colab (recomendada)
1. Abrir el notebook en [Google Colab](https://colab.research.google.com) (Archivo → Subir notebook).
2. Ejecutar todas las celdas (Entorno de ejecución → Ejecutar todo).
3. Cuando se solicite, subir el archivo `Tasa_de_Promocion_Efectiva_2012-2023_unificada.xlsx`.

### Opción B: Entorno local
1. Clonar el repositorio:
   ```bash
   git clone <URL-del-repositorio>
   cd <carpeta-del-repositorio>
   ```
2. Instalar las dependencias:
   ```bash
   pip install pandas numpy openpyxl scikit-learn matplotlib seaborn jupyter
   ```
3. Colocar el archivo `Tasa_de_Promocion_Efectiva_2012-2023_unificada.xlsx` en la misma carpeta que los notebooks.
4. Abrir y ejecutar los notebooks:
   ```bash
   jupyter notebook
