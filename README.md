# 🇲🇽 Motor de Riesgo Crediticio y Optimización P&L (México 2026)

📘 Nota de Evolución del Proyecto

⚠️ Este repositorio es un prototipo inicial de investigación.

Aquí se exploraron las primeras correlaciones estadísticas entre informalidad y riesgo para el entorno financiero mexicano de marzo 2026. Este código ha evolucionado hacia un Framework Profesional de Grado Bancario con arquitectura modular (OOP), optimización de utilidades y procesamiento de alto rendimiento con DuckDB.

👉 **Ver la versión final y robusta aquí:** [Macro-Risk-Supervision-Engine-MX](https://github.com/garqjs/Macro-Risk-Supervision-Engine-MX)

---

## 📌 1. Descripción del Proyecto e Hipótesis
El objetivo principal es predecir la **Probabilidad de Default (PD)** en un contexto de alta volatilidad. A diferencia de los modelos de scoring tradicionales, este motor adapta la política de crédito a las condiciones reales de la economía mexicana actual.

### 💡 Hipótesis de Riesgo (Justificación Estratégica)
> "En un entorno de **inflación persistente (4.02% anual)** y una **informalidad laboral del 55.0%**, la estabilidad del ingreso real y el uso preventivo de líneas revolventes son los predictores con mayor peso en la solvencia del cliente."

**Motivos de la Hipótesis:**
1.  **Vulnerabilidad del Sector Informal:** Los trabajadores informales carecen de redes de seguridad social, siendo los primeros en incumplir pagos ante choques de precios en la canasta básica.
2.  **Estrés Financiero (Efecto TDC):** El agotamiento de las tarjetas de crédito funciona como un **Early Warning Signal**; los clientes suelen financiar gastos básicos antes de caer en default técnico.
3.  **Ajuste por UDI e Inflación:** No se puede evaluar el riesgo en 2026 sin considerar que el valor de la **UDI (8.7413)** y el tipo de cambio afectan la capacidad de pago real de los hogares.

---

## 🗂️ 2. Diccionario de Variables (Enfoque en Riesgo)
* **`FLAG_INFORMAL`**: Variable líder. Identifica la estabilidad de la fuente de ingresos del solicitante.
* **`INGRESO_REAL_AJUSTADO_MXN`**: Capacidad de pago neta deflactada por el INPC mensual (0.50%).
* **`RATIO_APALANCAMIENTO`**: Relación deuda total (interna + externa) vs. ingreso real disponible.
* **`DELTA_ATRASO`**: Variación en días de atraso recientes vs. históricos (Indicador de deterioro).
* **`uso_tdc_promedio`**: Nivel de utilización de tarjetas; mide el estrés de liquidez inmediata.

---

## 🛠️ 3. Auditoría de Datos y Saneamiento
Para garantizar la estabilidad del motor en producción, se implementaron procesos de **Data Quality**:
* **Winsorización (99%)**: Saneamiento de ingresos y deudas extremas para evitar sesgos por valores atípicos.
* **DuckDB Feature Store**: Orquestación de 5 fuentes de datos (Buró, Pagos, CC, Previas) mediante SQL *in-memory* para asegurar consistencia en el cálculo de variables.

---

## 🤖 4. El Motor: XGBoost Monotónico
Implementamos un modelo **XGBoost** configurado para cumplimiento regulatorio:
* **Monotonic Constraints**: El modelo respeta reglas de negocio infranqueables (ej. a mayor apalancamiento, el riesgo *siempre* debe subir).
* **Explicabilidad (SHAP)**: Transparencia total en la toma de decisiones, identificando qué factor movió el score de cada cliente.

<img width="785" height="478" alt="image" src="https://github.com/user-attachments/assets/1316cb28-ba48-44cf-9843-7ce99a8b51d3" />

---

## 📈 5. Evaluación Estadística y Financiera

### Desempeño Predictivo
* **Gini Index**: `0.8840` (Excelente capacidad de ordenamiento de prospectos).
* **Estadístico KS**: `0.4420` (Fuerte separación entre clientes "Sanos" y "Morosos").
* **AUC-ROC**: `0.9420`.

### Optimización de P&L (Profit & Loss)
El modelo optimiza la rentabilidad neta bajo supuestos de **Tasa de Interés (30%)** y **LGD (65%)**:
* **Umbral Óptimo de Aprobación**: `85.0%`.
* **Beneficio Neto Maximizado**: `~$60,000,000 MXN`.
* **Interpretación Estratégica**: La política permite una aprobación amplia debido al margen financiero, pero utiliza el KS para "filtrar" quirúrgicamente a los morosos que el ojo humano no detectaría.

<img width="2132" height="590" alt="image" src="https://github.com/user-attachments/assets/e715a189-f227-4379-9997-f4036320d932" />

---

**Contexto**: Estrategia de Riesgos México Q1-2026.
