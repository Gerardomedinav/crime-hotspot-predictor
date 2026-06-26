# 🚨 Crime Hotspot Predictor: Modelo de Clasificación Espacio-Temporal para la Predicción de Delitos

<div align="center">
  <img src="https://img.shields.io/badge/Python-3.9+-blue?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="Scikit-Learn">
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
  <img src="https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=google-colab&logoColor=white" alt="Google Colab">
</div>

---

### 🚀 Acceso Directo al Cuaderno de Desarrollo
El pipeline completo de ingeniería de datos, modelado predictivo y evaluación se encuentra disponible de manera interactiva en el siguiente enlace:

<a href="https://drive.google.com/file/d/1VJdideRlOO4uDmpeMPO7S1lyl-xtFByR/view?usp=sharing" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" width="150">
</a>

---

## 📖 1. Resumen Ejecutivo del Proyecto
Este proyecto aborda la problemática de la seguridad pública desde una perspectiva analítica y predictiva utilizando Ciencia de Datos. El objetivo principal es construir y evaluar un modelo de **Clasificación Binaria** capaz de estimar la probabilidad de que un delito ocurra en una **"Zona Caliente" (Hotspot)** en función de su contexto geográfico y variables temporales (mes, día, hora, estacionalidad).

Este tipo de arquitecturas predictivas permite optimizar el despliegue de recursos de seguridad, transitando de un enfoque reactivo tradicional hacia una estrategia de **patrullaje preventivo inteligente**.

---

## ⚙️ 2. Diseño del Target e Ingeniería de Características (Feature Engineering)

Uno de los mayores desafíos técnicos de este proyecto fue la **ausencia de una variable objetivo predefinida** en el conjunto de datos original. Para resolver esto, se diseñó un pipeline avanzado de Feature Engineering:

* **Definición de la Variable Objetivo (`zona_caliente`):** Se agruparon las coordenadas espaciales continuas mediante la discretización y agregación en bloques geográficos (`zona_lat` + `zona_long`). Posteriormente, se calculó la densidad histórica de criminalidad por bloque.
* **Criterio de Clasificación:**
  * **Clase 1 (Zona Caliente):** Sectores cuya densidad delictiva se ubica en el **percentil 75 o superior** (áreas de máximo riesgo acumulado).
  * **Clase 0 (Zona No Caliente):** Sectores con densidad inferior al percentil 75.
* **🔬 Mitigación Crítica de Data Leakage:** Para asegurar la validez estadística del modelo, el cálculo de las densidades y los umbrales de los percentiles se realizó **exclusivamente sobre el conjunto de datos de entrenamiento (Train Set)**. Los parámetros obtenidos se aplicaron de forma estrictamente pasiva sobre el conjunto de prueba (Test Set), garantizando que el modelo no sufriera fugas de información hacia el futuro o datos globales.

---

## 📊 3. Análisis Exploratorio de Datos (EDA) e Ingeniería Espacial

### 📈 Gráfico 1: Análisis de Tendencias Temporales y Estacionalidad
![Tendencias Temporales](img1.png)
* **Qué representa:** Muestra el comportamiento del volumen de delitos a lo largo del tiempo agrupado por periodos mensuales y patrones cíclicos.
* **Resultado / Insight:** Permite identificar picos estacionales de delincuencia ligados a variables de calendario (ej. fin de año, vacaciones o días festivos), lo que valida la inclusión de variables temporales enriquecidas como características predictivas clave para el algoritmo.

### 🗺️ Gráfico 2: Mapeo Base e Identificación del Espacio Geográfico
![Mapeo Base](ML_1.png)
* **Qué representa:** Cartografía e indexación de la región de estudio, delimitando las fronteras operativas y los puntos de registro donde se concentran los datos crudos de latitud y longitud.
* **Resultado / Insight:** Funciona como el plano fundamental para la posterior discretización espacial. Permite corroborar visualmente la cobertura de los datos y asegurar que la distribución de los bloques geográficos cubra de manera uniforme el tejido urbano analizado.

---

## 🛠️ 4. Evaluación, Diagnóstico y Resultados del Modelo

Para garantizar la robustez del predictor, se evaluaron múltiples algoritmos (como Regresión Logística y ensambles basados en árboles) bajo una estricta estrategia de validación.

### 🔄 Gráfico 3: Estabilidad del Modelo mediante Validación Cruzada (Cross-Validation)
![Validación Cruzada](img5.png)
* **Qué representa:** Evaluación del rendimiento métrico (Accuracy/Métricas de puntuación) distribuido a lo largo de los múltiples pliegues (*K-Folds*) durante el entrenamiento.
* **Resultado / Insight:** La baja varianza observable entre los diferentes *folds* demuestra que el modelo cuenta con una excelente capacidad de generalización. Al no sufrir fluctuaciones drásticas, se confirma empíricamente el éxito de las técnicas utilizadas para prevenir el *overfitting* y el *data leakage*.

### 🎯 Gráfico 4: Matriz de Confusión y Análisis de Errores Operativos
![Matriz de Confusión](img6.png)
* **Qué representa:** Desglose matricial de las predicciones del modelo frente a la realidad del terreno, mostrando de forma directa los Verdaderos Positivos (VP), Verdaderos Negativos (VN), Falsos Positivos (FP, falsas alarmas) y Falsos Negativos (FN, zonas de riesgo no detectadas).
* **Resultado / Insight:** Crucial para calibrar el modelo en contextos de seguridad pública. Permite ajustar el umbral de decisión para minimizar los Falsos Negativos, garantizando que el sistema no pase por alto una zona de alta peligrosidad inminente.

### 📉 Gráfico 5: Curva ROC (Receiver Operating Characteristic) General
![Curva ROC General](curva%20ROC.png)
* **Qué representa:** Gráfico del ratio de Verdaderos Positivos frente al ratio de Falsos Positivos a diferentes umbrales de clasificación para el modelo óptimo.
* **Resultado / Insight:** El área bajo la curva (ROC-AUC) sirve como indicador global de la capacidad de separación de clases del modelo. Una curva que empuja fuertemente hacia la esquina superior izquierda ratifica que el algoritmo distingue con alta efectividad una zona segura de una de alto riesgo.

### 📊 Gráfico 6: Comparativa Detallada de Curvas ROC entre Modelos
![Comparativa Curvas ROC](img4.jpg)
* **Qué representa:** Superposición de las curvas ROC de los diferentes modelos entrenados (ej. Regresión Logística frente a modelos complejos) para contrastar su poder predictivo en un solo plano.
* **Resultado / Insight:** Facilita la toma de decisiones arquitectónicas. Permite evaluar si un incremento en la complejidad del modelo (como un modelo no lineal o ensamble) ofrece un beneficio estadísticamente significativo en el AUC respecto a una baseline lineal más simple y 외 interpretable.

### 🥇 Gráfico 7: Comparativa Multimétrica de Modelos Predictivos
![Comparativa de Métricas](img7.png)
* **Qué representa:** Gráfico de barras que compara de forma directa métricas clave de evaluación como *Accuracy*, *Precision*, *Recall* y *F1-Score* entre los algoritmos candidatos.
* **Resultado / Insight:** Permite balancear el *trade-off* entre Precisión y Recall. En el despliegue de patrullas policiacas, un *Recall* elevado suele priorizarse para no omitir zonas calientes, y este gráfico consolida visualmente qué modelo logra el equilibrio operativo ideal.

### 🔍 Gráfico 8: Importancia de Características y Diagnóstico de Residuos
![Importancia y Diagnóstico](img8.png)
* **Qué representa:** Análisis complementario de la relevancia de las variables del modelo (*Feature Importance*) o distribución detallada de los errores de predicción finales.
* **Resultado / Insight:** Proporciona explicabilidad al modelo (*XAI*). Identifica qué factores (si las coordenadas espaciales o ciertas ventanas horarias nocturnas, por ejemplo) tienen mayor peso matemático al dictar si una zona se convertirá en un punto crítico de criminalidad.

---

## 🎛️ 5. Conclusiones Técnicas y Siguientes Pasos
1. **Factibilidad Predictiva:** El comportamiento espacio-temporal del delito es altamente predecible cuando se estructuran correctamente los datos geométricos urbanos y se asocian a ventanas de tiempo relativas.
2. **Robustez Metodológica:** El diseño del target basado en percentiles dinámicos calculados estrictamente en el *Train Set* demostró ser una solución óptima para mitigar el sesgo y la fuga de datos en series espaciales.
3. **Próximas Implementaciones (Roadmap):**
   * Incorporar algoritmos de ensamble basados en Boosting (como *XGBoost* o *LightGBM*) para capturar interacciones no lineales más complejas entre el tiempo y el espacio.
   * Integrar datos exógenos de infraestructura urbana (presencia de luminarias, estaciones de transporte público, centros comerciales) para enriquecer el mapa de características.

---
*Desarrollado con fines analíticos y de optimización de políticas de seguridad pública basados en Datos.*
