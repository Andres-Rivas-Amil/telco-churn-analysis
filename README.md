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
- Análisis de churn por:
  - Tipo de contrato
  - Servicio de internet
  - Método de pago
  - Antigüedad del cliente
  - Servicios adicionales (security, backup, streaming)

### 4. Modelo de Machine Learning (Regresión Logística)
- Creación de variables dummy (contrato, pago, servicios)
- Entrenamiento del modelo con `scikit-learn` y `statsmodels`
- Identificación de **Odds Ratio** para cada factor

**Principales resultados del modelo:**

| Factor | Odds Ratio | Impacto |
|--------|------------|---------|
| Internet: Fiber optic | 9.15 | 🔴 +815% riesgo |
| Contrato: Month-to-month | 4.30 | 🔴 +330% riesgo |
| Internet: DSL | 3.25 | 🔴 +225% riesgo |
| Pago: Electronic check | 1.46 | 🟡 +46% riesgo |
| Senior Citizen | 1.33 | 🟡 +33% riesgo |
| **Online Security** | **0.69** | 🟢 **-31% riesgo** |
| **Tech Support** | **0.72** | 🟢 **-28% riesgo** |

### 5. Dashboard en Power BI
- Conexión directa a la base de datos `TELCO` en MySQL
- Creación de medidas DAX (Total Clientes, Tasa Churn, Ingreso Perdido)
- Diseño de dashboard interactivo

## 📊 Contenido del Dashboard

| Sección | Descripción | Insights Principales |
|---------|-------------|---------------------|
| **KPIs Superiores** | Total clientes, Clientes perdidos, Tasa churn, Ingreso perdido mensual | 26.5% de churn, $139K ingresos perdidos/mes |
| **Segmentadores** | Rango antigüedad, Género | Filtrar todo el dashboard |
| **Abandono por Contrato** | Gráfico de barras | Month-to-month = 330% más riesgo |
| **Evolución del Churn** | Gráfico de líneas | Clientes nuevos: 49% churn, leales: 20% |
| **Proporción de Abandono** | Gráfico de donut | 73.5% se quedan, 26.5% se van |
| **Abandono por Servicio Internet** | Barras agrupadas | Fiber: 36.5% churn, DSL: 25.9%, No: 7.4% |
| **Abandono por Método de Pago** | Barras | Electronic check: +46% riesgo |
| **Impacto de la Seguridad** | Barras agrupadas | Security + Backup reduce churn a 15% |

## ⚙️ Pipeline Automatizado en Python

Para facilitar la reproducción del análisis y la carga de datos, he desarrollado un **pipeline completo en Python** (`pipeline_churn.py`) que ejecuta de forma secuencial y automatizada todo el proceso:

### El pipeline incluye:
1. **Conexión a base de datos:** Establece la conexión con MySQL.
2. **ETL (Extracción, Transformación y Carga):**
   - Lee el archivo `Telco-Customer-Churn.csv`.
   - Limpia los datos (maneja valores nulos, convierte tipos de datos).
   - **Define la estructura de la tabla con tipos SQL específicos** (VARCHAR, INT, DECIMAL, etc.).
   - **Añade Primary Key** a la columna `customerID`.
   - Carga los datos limpios a la base de datos `TELCO` en MySQL.
3. **Análisis Exploratorio (EDA):** Ejecuta un conjunto de consultas SQL predefinidas para obtener un resumen rápido de métricas clave.
4. **Generación de Gráficos:** Crea automáticamente 7 gráficos profesionales en Python y los guarda en la carpeta `graficos_informe/`.
5. **Modelo de Machine Learning:** Entrena un modelo de regresión logística, calcula los Odds Ratio y muestra los resultados más importantes.
6. **Informe Ejecutivo:** Genera un informe en formato Markdown (`informe_ejecutivo.md`) con un resumen de los hallazgos y recomendaciones estratégicas.

### Ventajas del pipeline:
- **Reproducibilidad:** Cualquier persona puede clonar el repositorio y ejecutar `python pipeline_churn.py` para obtener los mismos resultados.
- **Automatización:** Elimina la necesidad de ejecutar celdas manualmente en un notebook.
- **Estandarización:** Asegura que el proceso de ETL, análisis y generación de informes sea consistente.
- **Integridad de datos:** La base de datos se crea con tipos SQL definidos y Primary Key.
- **Facilidad de uso:** Un solo comando ejecuta todo el flujo de trabajo.

### Cómo ejecutar el pipeline:
```bash
# 1. Clonar el repositorio
git clone https://github.com/Andres-Rivas-Amil/telco-churn-analysis.git
cd telco-churn-analysis

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Ejecutar el pipeline
python pipeline_churn.py
