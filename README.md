## 📖 Descripción del Proyecto
Este proyecto desarrolla un sistema de **alerta temprana** para identificar estudiantes en riesgo de abandono en instituciones de educación superior. Utilizando un enfoque de **Data Science**, el modelo clasifica a los estudiantes según su probabilidad de deserción, permitiendo a la institución priorizar intervenciones académicas y financieras.

El valor diferencial de este repositorio radica en la transición de un análisis estático a un **pipeline auditable**, aplicando rigor estadístico y métricas (KS, Gini) para validar la efectividad de las predicciones en un entorno de toma de decisiones real.

---


## 🔍 1. Descripción de Variables Principales

El dataset cuenta con 35 variables. Para efectos de este modelo, las agrupamos en tres dimensiones críticas:

* **Variables Académicas (Predictor Clave):** Unidades curriculares aprobadas, matriculadas y notas del 1er y 2do semestre.
* **Variables Socioeconómicas:** Tasa de desempleo, inflación, PIB, becado, deudor y ocupación de los padres.
* **Variables Demográficas:** Edad al matricular, género, estado civil y nacionalidad.

## 🛠️ 2. Auditoría de Datos (Data Quality)

Antes del modelado, se realizó un proceso de saneamiento para asegurar la integridad de los resultados:

| Proceso | Resultado | Acción Realizada |
| :--- | :--- | :--- |
| **Traducción** | 35 columnas | Renombramiento de variables de Inglés a Español para legibilidad. |
| **Nulos** | 0 nulos | El dataset se encuentra completo, no se requirió imputación. |
| **Duplicados** | 0 registros | No se detectaron filas repetidas en la muestra. |
| **Tipos de Datos** | Correctos | Conversión de variables categóricas codificadas a tipos `int/float` para compatibilidad con XGBoost. |
| **Target** | Binario | Exclusión de la clase 'Enrolled' para focalizar el análisis en **Deserción (1)** vs **Graduado (0)**. |

---

## 📈 3. Análisis Exploratorio de Datos (EDA)

### Estrategia de Correlación
Se identificó una **fuerte correlación positiva ($>0.90$)** entre las unidades aprobadas del 1er y 2do semestre. Esto indica que el éxito inicial es el mejor predictor del éxito final. Sin embargo, para evitar la **multicolinealidad**, el modelo XGBoost fue configurado para manejar la redundancia de estas variables.

<img width="847" height="741" alt="image" src="https://github.com/user-attachments/assets/a3b2205f-65f7-405d-a3f9-47c697cd0ab7" />


### Distribución del Rendimiento Académico
El análisis de densidad de las notas del 2do semestre revela una separación clara:
* **Graduados:** Presentan una distribución normal centrada en 13.0.
* **Desertores:** Presentan una distribución bimodal con un pico masivo en **0.0**, lo que sugiere abandonos tempranos antes de completar evaluaciones.

<img width="850" height="470" alt="image" src="https://github.com/user-attachments/assets/1587fa50-4098-4c70-8607-0ae9e6e454ba" />


### Análisis de Importancia SHAP
A diferencia de la importancia de variables tradicional, SHAP revela la **direccionalidad** del impacto:
* **Menos unidades aprobadas** desplazan la predicción significativamente hacia la derecha (Abandono).
* **Tener deudas** actúa como un empuje adicional hacia el riesgo, validando la hipótesis de que la deserción tiene un componente económico innegable.

<img width="795" height="940" alt="image" src="https://github.com/user-attachments/assets/20064979-c36b-495f-86b0-c772961efaa2" />

## 📝 Resumen Ejecutivo y Resultados

El proyecto se centró en la transición de un análisis descriptivo a uno predictivo, optimizando el modelo no solo por precisión estadística, sino por **utilidad operativa**.

### 📈 Comparativa de Escenarios de Decisión

| Métrica | Enfoque Equilibrado (Matemático) | Enfoque Proactivo (Recomendado) |
| :--- | :--- | :--- |
| **Umbral de Decisión** | 0.59 | **0.35** |
| **Recall (Sensibilidad)** | 87.32% | **93.66%** |
| **Falsos Negativos (FN)** | 36 estudiantes | **18 estudiantes** |
| **Falsos Positivos (FP)** | 12 alarmas | 41 alarmas |
| **Objetivo de Negocio** | Eficiencia de recursos. | **Prevención máxima de deserción.** |

---

<img width="1349" height="590" alt="image" src="https://github.com/user-attachments/assets/206aea75-8b59-4237-8fbb-0eb2da56b652" />

### 🧮 Métricas de Poder Predictivo

Basado en el análisis de la curva ROC y la distribución de poblaciones, el modelo demuestra una capacidad de separación superior:

* **AUC-ROC:** $0.92$ (Capacidad del modelo para clasificar correctamente).
* **Gini:** $0.84$ (Poder de discriminación del modelo).
* **Estadística KS:** $0.85$ (Máxima separación entre capturas y falsas alarmas).

---

<img width="857" height="572" alt="image" src="https://github.com/user-attachments/assets/6b26efc6-de2c-4016-8308-a725c0abb7ac" />

### 🔍 Interpretación del Modelo (SHAP)

Utilizamos **SHAP Values** para garantizar una auditoría clara del riesgo estudiantil. Los hallazgos principales son:

1.  **Rendimiento Académico:** Las unidades aprobadas en el 2do semestre son el predictor #1. Una caída en la aprobación genera un aumento exponencial en el log-odds de deserción.
2.  **Solvencia Financiera:** El estatus de "Matrícula al día" y ser "Deudor" son los siguientes factores de peso, indicando que el riesgo no es solo académico, sino socioeconómico.
3.  **Factor Demográfico:** La edad al matricular presenta una correlación positiva con el riesgo, sugiriendo que estudiantes de mayor edad requieren programas de apoyo diferenciados.

---

### 💡 Conclusión Estratégica
> "Al desplazar el umbral de decisión a **0.35**, logramos capturar al **93.6%** de los desertores potenciales. Aunque esto incrementa las falsas alarmas, desde una óptica de **Gestión de Riesgo Estudiantil**, el costo de una tutoría preventiva es despreciable frente al costo de pérdida de matrícula y el impacto social de la deserción."




 





