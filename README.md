# 📡 TelecomX LATAM — Análisis de Churn

> Análisis exploratorio de datos para identificar los factores que impulsan la evasión de clientes en TelecomX LATAM y proponer estrategias de retención basadas en evidencia.

---

## 📌 Tabla de Contenidos

1. [Propósito del Análisis](#-propósito-del-análisis)
2. [Estructura del Proyecto](#-estructura-del-proyecto)
3. [Requisitos e Instalación](#-requisitos-e-instalación)
4. [Instrucciones para Ejecutar](#-instrucciones-para-ejecutar)
5. [Solución de Problemas](#-solución-de-problemas)
6. [Introducción al Problema](#-introducción-al-problema)
7. [Limpieza y Tratamiento de Datos](#-limpieza-y-tratamiento-de-datos)
8. [Análisis Exploratorio y Gráficos](#-análisis-exploratorio-y-gráficos)
9. [Conclusiones e Insights](#-conclusiones-e-insights)
10. [Recomendaciones](#-recomendaciones)

---

## 🎯 Propósito del Análisis

El objetivo de este proyecto es analizar el comportamiento de los clientes de **TelecomX LATAM** para comprender **qué factores generan mayor riesgo de abandono (Churn)**.

A través de un proceso ETL completo y un análisis exploratorio de datos, se busca:

- Identificar patrones y variables asociadas a la evasión de clientes
- Cuantificar el impacto de cada variable sobre la tasa de churn
- Proveer insights accionables para los equipos de negocio y retención
- Sentar las bases para un futuro modelo predictivo de churn

---

## 🗂️ Estructura del Proyecto

```
TelecomX_LATAM/
│
├── 📓 TelecomX_LATAM.ipynb        # Notebook principal con ETL + Análisis
├── 📄 README.md                   # Este archivo
│
├── 📁 data/
│   └── (los datos se cargan directamente desde GitHub vía URL)
│
└── 📁 outputs/
    └── grafico_porcentajes.jpg    # Gráfico exportado de variables categóricas
```

---

## ⚙️ Requisitos e Instalación

### Python y Versión Recomendada

```
Python >= 3.8
```

### Librerías Necesarias

| Librería | Versión recomendada | Uso |
|---|---|---|
| `pandas` | >= 1.3 | Manipulación de datos |
| `numpy` | >= 1.21 | Operaciones numéricas |
| `matplotlib` | >= 3.4 | Visualizaciones |
| `seaborn` | >= 0.11 | Gráficos estadísticos |

### Instalación

Si ejecutas el notebook **localmente**, instala las dependencias con:

```bash
pip install pandas numpy matplotlib seaborn
```

Si usas **Google Colab**, todas las librerías ya están disponibles por defecto. No se requiere instalación adicional.

---

## ▶️ Instrucciones para Ejecutar

### Opción 1 — Google Colab (Recomendado)

1. Accede al notebook haciendo clic en el badge:

   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/KonungurProFer/Proyecto_telecom_X/blob/main/TelecomX_LATAM.ipynb)

2. En el menú superior selecciona **`Entorno de ejecución > Ejecutar todo`**
3. Los datos se cargan automáticamente desde GitHub — no necesitas descargar nada

### Opción 2 — Ejecución Local

```bash
# 1. Clona el repositorio
git clone https://github.com/KonungurProFer/Proyecto_telecom_X.git
cd Proyecto_telecom_X

# 2. Instala las dependencias
pip install pandas numpy matplotlib seaborn

# 3. Lanza Jupyter Notebook
jupyter notebook TelecomX_LATAM.ipynb
```

### Orden de Ejecución

Ejecuta las celdas **en orden secuencial de arriba hacia abajo**. El notebook está organizado en las siguientes etapas:

```
📌 Extracción  →  🔧 Transformación  →  📊 Análisis  →  📄 Informe
```

---

## 🛠️ Solución de Problemas

### ❌ Error: `ModuleNotFoundError: No module named 'seaborn'`
```bash
pip install seaborn
```

### ❌ Error al cargar los datos (`ConnectionError` o `JSONDecodeError`)
- Verifica tu conexión a internet; los datos se descargan desde GitHub en tiempo real
- Si el problema persiste, descarga el archivo manualmente desde el repositorio de Alura y cárgalo así:
```python
df = pd.read_json("ruta/local/TelecomX_Data.json")
```

### ❌ `KeyError` en columnas después de normalizar
- Asegúrate de ejecutar **todas las celdas en orden**; algunas celdas dependen de transformaciones previas
- Reinicia el kernel y ejecuta todo desde cero: `Entorno de ejecución > Reiniciar y ejecutar todo`

### ❌ Gráficos no se muestran en Jupyter local
Agrega al inicio del notebook:
```python
%matplotlib inline
```

### ❌ `SettingWithCopyWarning` en pandas
No afecta los resultados. Puedes suprimirlo con:
```python
import warnings
warnings.filterwarnings('ignore')
```

---

## 🔎 Introducción al Problema

**TelecomX LATAM** enfrenta una tasa de abandono de clientes del **26.58%**, lo que significa que aproximadamente **1 de cada 4 clientes** deja de usar el servicio.

Esta tasa supera el umbral crítico del 20% y representa un impacto económico directo: pérdida de ingresos recurrentes, aumento en costos de adquisición de nuevos clientes y deterioro del valor de vida del cliente (LTV).

El análisis parte de un dataset de **7.267 registros** con información demográfica, de servicios contratados y de cuenta de cada cliente.

---

## 🧹 Limpieza y Tratamiento de Datos

### Proceso ETL aplicado

**1. Extracción**
- Carga directa desde repositorio GitHub en formato JSON
- Dataset original: 7.267 filas × 6 columnas (con datos anidados)

**2. Transformación**

| Paso | Descripción | Impacto |
|---|---|---|
| Desanidado JSON | `pd.json_normalize` en 4 columnas anidadas | 21 columnas resultantes |
| Eliminación de Churn vacío | Registros con `Churn = ""` | −224 registros |
| Eliminación de Charges.Total inválidos | Valores no numéricos (tenure = 0) | −11 registros |
| Conversión de binarias | `Yes/No` → `1/0` en 12 variables | Tipos corregidos |
| Conversión de tipos | `Charges.Total` a `float64` | Tipo corregido |
| Traducción de valores | Valores categóricos al español | Legibilidad |
| Variable derivada | `Cargo_Diario = Cargo_Mensual / 30` | Nueva variable |
| Reset de índice | Reindexación secuencial | Integridad del DataFrame |

**3. Dataset Final**
```
7.032 registros × 22 columnas — sin nulos, sin duplicados
```

---

## 📊 Análisis Exploratorio y Gráficos

### Tasa de Churn General

```
No Abandonó  ████████████████████████████░░  73.42%
Sí Abandonó  ████████░░░░░░░░░░░░░░░░░░░░░░  26.58%
```

### Churn por Variables Categóricas

| Variable | Categoría de Mayor Riesgo | Churn |
|---|---|---|
| Método de Pago | Cheque Electrónico | **45.3%** |
| Tipo de Contrato | Mensual | **42.7%** |
| Servicio Internet | Fibra Óptica | **41.9%** |
| Cliente Senior | Sí | **41.7%** |
| Facturación Sin Papel | Sí | **33.6%** |
| Tiene Pareja | No | **33.0%** |
| Tiene Dependientes | No | **31.3%** |
| Género | Femenino / Masculino | **~27%** _(sin diferencia significativa)_ |

### Churn por Antigüedad

- **0–18 meses:** mayor concentración de abandono — zona crítica de retención
- **18–36 meses:** churn en descenso sostenido
- **54–72 meses:** tasa de abandono mínima — clientes altamente fidelizados

### Correlaciones Clave con Churn

| Variable | Correlación | Efecto |
|---|---|---|
| Antigüedad | **-0.354** | Mayor antigüedad = menor churn |
| Cargo Total | -0.199 | Mayor gasto acumulado = menor churn |
| Cargo Mensual | +0.193 | Mayor cargo mensual = mayor riesgo |
| Facturación Sin Papel | +0.191 | Asociado a mayor churn |
| Seguridad Online | -0.171 | Protege contra el abandono |
| Soporte Técnico | -0.165 | Reduce el riesgo de churn |
| Cliente Senior | +0.151 | Mayor vulnerabilidad al abandono |

> 📌 La **antigüedad** es el predictor individual más fuerte del churn.

---

## 💡 Conclusiones e Insights

### Perfil de Cliente de Mayor Riesgo
> Contrato mensual + Fibra Óptica + Pago por cheque electrónico + Sin vínculos familiares + Antigüedad menor a 18 meses

### Perfil de Cliente de Menor Riesgo
> Contrato anual o bienal + DSL + Con pareja o dependientes + Alta antigüedad

### Hallazgos Principales

- **Los primeros 18 meses son determinantes.** La mayor fuga ocurre en clientes nuevos antes de consolidar el hábito de uso.
- **El tipo de contrato es el factor más controlable.** Pasar de mensual a anual reduce el churn de 42.7% a 11.3%.
- **Fibra Óptica presenta una paradoja.** Es el servicio más premium pero tiene la mayor tasa de abandono, lo que sugiere problemas de satisfacción o relación precio-valor.
- **Los servicios adicionales fidelizan.** Clientes con Seguridad Online y Soporte Técnico presentan correlaciones negativas con el churn.
- **Los vínculos familiares protegen.** Tener pareja o dependientes reduce el churn de manera consistente.

---

## 🚀 Recomendaciones

**1. Incentivar contratos a largo plazo**
Ofrecer descuentos, beneficios exclusivos o períodos de gracia para migrar clientes de contratos mensuales a anuales o bienales.

**2. Programa de onboarding en los primeros 18 meses**
Implementar seguimiento activo, check-ins y soporte proactivo durante el período crítico inicial de cada cliente.

**3. Revisar la propuesta de valor de Fibra Óptica**
Con un 41.9% de churn, es urgente evaluar si el precio, la calidad de servicio o el soporte están generando insatisfacción en este segmento.

**4. Promover servicios de valor agregado**
Incentivar la adopción de Seguridad Online, Soporte Técnico y protecciones adicionales, ya que están asociados a mayor retención.

**5. Estrategia diferenciada para clientes senior**
El 16.24% de la base tiene 41.7% de churn. Se recomienda atención personalizada, planes simplificados y soporte dedicado.

**6. Migrar métodos de pago hacia automáticos**
El cheque electrónico tiene el mayor churn (45.3%). Incentivar la migración a transferencia bancaria o tarjeta automática puede reducir la fricción en el cobro y mejorar la retención.

---

## 👤 Fernando Abanto

Proyecto desarrollado como parte del **Challenge 2 — Data Science LATAM** de Alura Cursos.

---

*Dataset fuente: [Alura Cursos — Challenge 2 Data Science LATAM](https://github.com/alura-cursos/challenge2-data-science-LATAM)*
