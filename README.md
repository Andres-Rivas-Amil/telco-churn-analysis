# 📊 Análisis de Churn - Telecomunicaciones

## 📌 Descripción del Proyecto

Este proyecto consiste en un **análisis completo de abandono de clientes (churn)** para una empresa de telecomunicaciones. El objetivo es identificar los factores que más influyen en la pérdida de clientes y proponer estrategias de retención basadas en datos.

## 📁 Fuente de Datos

- **Origen:** IBM Telco Customer Churn dataset (disponible en Kaggle)
- **Formato:** CSV
- **Contenido:** 7,043 registros de clientes con 21 características (demográficas, servicios contratados, información de cuenta, etc.)

## 🛠️ Tecnologías y Herramientas Utilizadas

| Herramienta | Uso |
|-------------|-----|
| **Python** | Lenguaje principal para análisis y modelado |
| **Pandas / NumPy** | Manipulación y limpieza de datos |
| **MySQL** | Almacenamiento y consultas SQL |
| **SQLAlchemy** | Conexión, ETL y definición de estructura de tabla |
| **Scikit-learn** | Modelo de regresión logística |
| **Statsmodels** | Interpretación de coeficientes (Odds Ratio) |
| **Matplotlib / Seaborn** | Visualización de datos en Python |
| **Power BI** | Dashboard interactivo |
| **Jupyter Notebook** | Análisis exploratorio y documentación |
| **Git / GitHub** | Control de versiones |

## 🔄 Proceso de Análisis

### 1. Extracción y Limpieza de Datos (ETL)
- Carga del CSV original en Python
- Limpieza de valores nulos en `TotalCharges`
- Conversión de tipos de datos
- **Definición de tipos SQL y Primary Key** en la tabla `churn`
- Exportación a base de datos MySQL

### 2. Almacenamiento en Base de Datos
- Creación de la base de datos `TELCO` en MySQL
- Carga de datos desde Python usando `SQLAlchemy`
- **Estructura definida con tipos específicos** (VARCHAR, INT, DECIMAL, etc.)
- **Primary Key** en `customerID` para garantizar integridad
- Verificación de integridad de datos

### 3. Análisis Exploratorio con SQL
- Consultas SQL para entender distribución de datos
- Análisis de churn por: Tipo de contrato, servicio de internet, método de pago, antigüedad y servicios adicionales.

### 4. Modelo de Machine Learning (Regresión Logística)
- Creación de variables dummy (contrato, pago, servicios)
- Entrenamiento del modelo con `scikit-learn` y `statsmodels`
- Identificación de **Odds Ratio** para cada factor e impacto en el riesgo.

### 5. Dashboard en Power BI
- Conexión directa a la base de datos `TELCO` en MySQL
- Creación de medidas DAX (Total Clientes, Tasa Churn, Ingreso Perdido)
- Diseño de dashboard interactivo

---

## 📊 Resumen Ejecutivo y Hallazgos Clave

### 1. Datos Generales

| Métrica | Valor |
| :--- | :--- |
| **Total de clientes analizados** | 7,043 |
| **Clientes que abandonaron (Churn = Yes)** | 1,869 (26.5%) |
| **Clientes activos (Churn = No)** | 5,174 (73.5%) |

### 2. Factores de Riesgo vs. Factores de Protección (Basado en Odds Ratio)

#### 🔴 Factores que MÁS aumentan el riesgo de Churn
| # | Factor | Odds Ratio | Aumento de riesgo |
|---|---|---|---|
| 1 | Internet: Fiber optic | 9.15 | +815% riesgo |
| 2 | Contrato: Month-to-month | 4.30 | +330% riesgo |
| 3 | Internet: DSL | 3.25 | +225% riesgo |
| 4 | Pago: Electronic check | 1.46 | +46% riesgo |
| 5 | Senior Citizen | 1.33 | +33% riesgo |

#### 🟢 Factores que protegen contra el Churn
| # | Factor | Odds Ratio | Reducción de riesgo |
|---|---|---|---|
| 1 | Online Security | 0.69 | -31% riesgo |
| 2 | Tech Support | 0.72 | -28% riesgo |
| 3 | Tenure (cada mes adicional) | 0.97 | -3% riesgo |

---

### 3. Profundización en Hallazgos Clave

#### A. Antigüedad (CustomerType) - IMPACTO MUY ALTO 🔴
| Tipo de cliente | Antigüedad | Total | Tasa Churn |
| :--- | :--- | :--- | :--- |
| **New** | < 12 meses | 2,069 | **48.28%** |
| **Regular** | 12-23 meses | 1,047 | **29.51%** |
| **Loyal** | ≥ 24 meses | 3,927 | **14.29%** |

> 💡 **Conclusión:** Los clientes nuevos tienen **3.4 VECES MÁS** probabilidad de abandonar la compañía que los clientes leales.

#### B. Senior Citizen (Edad) - IMPACTO MUY ALTO 🔴
| Senior Citizen | Total | Tasa Churn | Diferencia |
| :--- | :--- | :--- | :--- |
| **No (menor de 65 años)** | 5,901 | 23.61% | *Base* |
| **Sí (65 años o más)** | 1,142 | **41.68%** | **+18.07%** |

> 💡 **Conclusión:** Los adultos mayores (Seniors) presentan una tasa de abandono **76% MÁS ALTA** que el resto de los segmentos demográficos.

#### C. Servicios de Streaming - HALLAZGO CRÍTICO ⚠️
| CustomerType | Streaming_Both (Tv & Movies) | Tasa Churn |
| :--- | :--- | :--- |
| **New** | Sí | **68.18%** |
| **Regular** | Sí | **46.92%** |
| **Loyal** | Sí | **19.93%** |

> 💡 **Conclusión:** El servicio de streaming no es deficiente en sí mismo; el problema radica en **ofrecerlo de entrada a clientes nuevos sin un compromiso de permanencia** mínimo asociado.

---

### 4. Segmentos Críticos de Alto Riesgo

#### 👥 Segmento General de Alto Riesgo
| Internet | Contrato | Método de pago | Clientes perdidos | Ingreso perdido MENSUAL |
|---|---|---|---|---|
| **Fiber optic** | Month-to-month | Electronic check | **789** | **$68,282** |
| Fiber optic | Month-to-month | Bank transfer | 149 | $13,062 |
| Fiber optic | Month-to-month | Credit card | 122 | $10,731 |
| DSL | Month-to-month | Electronic check | 192 | $8,776 |

> 📊 El segmento crítico **Fiber + Month-to-month + Electronic check** genera pérdidas **5 veces mayores** que el segundo segmento más afectado.

#### 👴 Segmento Senior de Alto Riesgo
| Internet | Contrato | Método de pago | Clientes perdidos | % del total Senior perdido |
|---|---|---|---|---|
| **Fiber optic** | Month-to-month | Electronic check | **260** | **54.6%** |

> 💡 **Conclusión:** El **54.6%** de todos los adultos mayores que abandonan la empresa cumplen exactamente con este perfil crítico.

---

### 5. Impacto Económico Total

| Segmento | Clientes perdidos | Ingreso perdido MENSUAL | Ingreso perdido ANUAL |
| :--- | :--- | :--- | :--- |
| Fiber + Month-to-month + Electronic check (general) | 789 | $68,282 | $819,384 |
| Senior Citizens (todos) | 476 | $35,000 | $420,000 |
| New + Streaming_Both | 180 | $15,000 | $180,000 |
| **TOTAL ESTIMADO** | **1,445** | **$118,282** | **$1,419,384** |

---

## 🚀 Recomendaciones Estratégicas

### 🔴 PRIORIDAD 1 (URGENTE) - Mitigación del Segmento Crítico General
* **Perfil:** Fiber + Month-to-month + Electronic check
* **Indicador de riesgo combinado:** Odds Ratio de **57.4** (¡Riesgo 57 veces mayor!).
* **Impacto financiero:** 789 clientes | $68,282 / mes.
* **Acciones recomendadas:**
  - Ofrecer un descuento del 15-20% por la migración hacia métodos de pago automático.
  - Lanzar campañas de llamadas de retención personalizadas al universo de 789 clientes identificados.
  - Ofrecer un upgrade gratuito de velocidad de internet por 3 meses.
  - Incentivar la migración a contratos anuales otorgando 2 meses de servicio gratuito.
  - Regalar licencias de *Online Security* y *Tech Support* durante los primeros 3 meses.

### 🔴 PRIORIDAD 1B (URGENTE) - Campaña de Retención Senior
* **Impacto financiero:** 260 clientes | $22,766 / mes.
* **Acciones recomendadas:**
  - Implementar una línea telefónica de soporte técnico prioritario sin coste para adultos mayores.
  - Crear un "Descuento por Edad" (15-20%) exclusivo para planes de Fibra Óptica.
  - Diseñar un Plan Senior específico que mantenga la factura física en papel y atención 100% humana.
  - Contacto directo a través de llamadas de fidelización a los 260 seniors mapeados.

### 🔴 PRIORIDAD 1C (URGENTE) - Control de Clientes Nuevos con Streaming
* **Tasa de Churn:** 68.18%
* **Impacto financiero:** 264 clientes | $15,000 / mes (estimado).
* **Acciones recomendadas:**
  - Restringir la contratación de Streaming sin una permanencia mínima estipulada (6 a 12 meses).
  - Vincular los add-ons de streaming a contratos de fidelización anuales.
  - Realizar llamadas de seguimiento antes de que finalicen los periodos promocionales.

### 🟠 PRIORIDAD 2 - Incentivar Servicios Protectores (Cross-selling)
* **Impacto:** Online Security (**-31% riesgo**) | Tech Support (**-28% riesgo**).
* **Acciones recomendadas:**
  - Crear un paquete (Bundle) obligatorio: *Fiber + Online Security + Tech Support*.
  - Aplicar un 30% de descuento directo al contratar ambos servicios adicionales de forma conjunta.
  - Incluir estos servicios de forma gratuita los primeros 3 meses a cualquier cliente nuevo.

---

## 📺 Contenido del Dashboard (Power BI)

| Sección | Descripción | Insights Principales |
|---------|-------------|---------------------|
| **KPIs Superiores** | Total clientes, Clientes perdidos, Tasa churn, Ingreso perdido mensual | 26.5% de churn, $139K ingresos perdidos/mes |
| **Segmentadores** | Rango antigüedad, Género | Filtrar todo el dashboard de forma dinámica |
| **Abandono por Contrato** | Gráfico de barras | Contratos Month-to-month representan 330% más riesgo |
| **Evolución del Churn** | Gráfico de líneas | Clientes nuevos: 49% churn vs. Clientes leales: 20% |
| **Proporción de Abandono** | Gráfico de donut | 73.5% Retención, 26.5% Abandono |
| **Abandono por Internet** | Barras agrupadas | Fibra Óptica: 36.5% churn, DSL: 25.9%, Sin internet: 7.4% |
| **Abandono por Pago** | Gráfico de barras | Electronic check incrementa en un +46% el riesgo |
| **Impacto de la Seguridad** | Barras agrupadas | La combinación de Security + Backup reduce el churn al 15% |

---

## ⚙️ Pipeline Automatizado en Python

Para facilitar la reproducción del análisis y la carga de datos, el proyecto incluye un script automatizado (`pipeline_churn.py`) que ejecuta secuencialmente todo el flujo de trabajo:

1. **Conexión a BD:** Conexión automatizada mediante `SQLAlchemy` a MySQL.
2. **ETL Automatizado:** Lee, limpia el dataset, asigna tipos SQL específicos y define la **Primary Key** en `customerID`.
3. **EDA en SQL:** Ejecuta consultas predefinidas para extraer las métricas de negocio de forma directa.
4. **Visualizaciones:** Genera y exporta automáticamente 7 gráficos profesionales a la carpeta `graficos_informe/`.
5. **Modelado:** Entrena la Regresión Logística y calcula los *Odds Ratio*.
6. **Reporte Automático:** Genera el informe final ejecutivo en formato Markdown (`informe_ejecutivo.md`).

### Cómo ejecutar el pipeline:
```bash
# 1. Clonar el repositorio
git clone [https://github.com/Andres-Rivas-Amil/telco-churn-analysis.git](https://github.com/Andres-Rivas-Amil/telco-churn-analysis.git)
cd telco-churn-analysis

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Ejecutar el pipeline completo
python pipeline_churn.py
