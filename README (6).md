# ConnectaTel - Análisis de Comportamiento de Clientes

## Descripción del Proyecto

Este proyecto realiza un análisis integral del comportamiento de clientes de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica. El objetivo es explorar, limpiar y analizar datos de clientes para construir un perfil estadístico, detectar comportamientos atípicos y crear segmentos de clientes.

El análisis permite identificar patrones de consumo, diseñar estrategias de retención y sugerir mejoras en los planes ofrecidos por la empresa.

## Datasets

El proyecto trabaja con tres datasets principales:

- **plans.csv**: Información de los planes actuales (precio, minutos incluidos, GB incluidos, costos por uso adicional)
- **users_latam.csv**: Información de clientes (edad, ciudad, fecha de registro, plan contratado, estado de churn)
- **usage.csv**: Detalle del uso real de servicios (llamadas y mensajes registrados en 2024)

## Pipeline de Análisis

El análisis sigue un flujo estructurado en nueve etapas:

### 1. Cargar

Importación de las librerías necesarias (pandas, numpy, seaborn, matplotlib) y carga de los tres archivos CSV en DataFrames. Se realiza una vista preliminar de los datos para verificar que se cargaron correctamente.

**Entrada**: Archivos CSV en el directorio /datasets/
**Salida**: Tres DataFrames en memoria (plans, users, usage)

### 2. Explorar

Análisis preliminar de la estructura de los datos. Se examinan las dimensiones de cada DataFrame, tipos de datos de cada columna, nombres de columnas y primeros registros.

**Acciones**:
- Revisión de .shape, .dtypes y .columns
- Inspección con .head(), .tail() y .info()
- Identificación inicial de inconsistencias en nomenclatura

**Salida**: Comprensión clara de la estructura de cada dataset

### 3. Detectar Problemas

Identificación sistemática de problemas de calidad en los datos. Se detectan valores nulos, valores inválidos (como edades negativas), valores sentinel ("?"), y rangos fuera de lo esperado (fechas futuras).

**Problemas identificados**:
- Valores nulos por columna y su porcentaje
- Valores inválidos (edad = -999)
- Valores sentinel que necesitan limpieza ("?")
- Fechas fuera del rango válido (años 2026 en registros históricos)
- Missing At Random (MAR): datos nulos relacionados con la variable type

**Salida**: Reporte detallado de problemas por columna

### 4. Limpiar

Tratamiento de los problemas detectados. Se reemplazan valores inválidos, se unifican valores sentinel, se corrigen fechas y se valida el estado de datos faltantes.

**Acciones**:
- Reemplazo de valores inválidos (edad -999 → mediana)
- Unificación de valores sentinel ("?" → NaN)
- Corrección de fechas futuras
- Mantenimiento de Missing At Random válidos (duration en registros de texto)

**Salida**: Datasets limpios y validados

### 5. Calcular Estadísticas

Generación de estadísticas descriptivas para entender la distribución de variables. Se calculan medidas de tendencia central, dispersión y distribución para variables numéricas y categóricas.

**Métricas calculadas**:
- Estadísticas básicas por variable (media, mediana, desviación estándar, rango)
- Distribución de usuarios por plan, ciudad y edad
- Totales de consumo agregados: minutos totales, cantidad de mensajes, cantidad de llamadas

**Salida**: DataFrame con estadísticas descriptivas de variables clave

### 6. Visualizar

Creación de gráficos para explorar distribuaciones y relaciones. Se utilizan histogramas, boxplots y gráficos de frecuencia para entender la distribución de variables clave.

**Visualizaciones**:
- Histogramas de edad, minutos y cantidad de mensajes
- Boxplots para detección visual de outliers
- Gráficos de frecuencia por ciudad y plan
- Distribuciones por segmento

**Salida**: Visualizaciones que ayudan a comprender los patrones de datos

### 7. Detectar Outliers

Identificación de valores extremos utilizando métodos estadísticos. Se aplica el rango intercuartil (IQR) para identificar observaciones anómalas que merecen atención especial.

**Métodos**:
- Boxplots para inspección visual
- Cálculo de límites IQR (Q1 - 1.5*IQR, Q3 + 1.5*IQR)
- Identificación de superconsumidores (>100 minutos/mes, >500 mensajes/mes)
- Identificación de usuarios inactivos (0-2 interacciones/mes)

**Patrones encontrados**:
- Superconsumidores de minutos (2-3%): consumo >100 minutos/mes
- Superconsumidores de mensajes (5-7%): más de 500 SMS/mes
- Usuarios inactivos (8-10%): casi nula actividad

**Salida**: Lista de outliers y sus características

### 8. Segmentar

Clasificación de usuarios en grupos homogéneos basados en características demográficas y de comportamiento. Se crean segmentos por edad y por nivel de uso para análisis comparativo.

**Segmentación por edad**:
- Jóvenes Digitales (18-32 años): alto uso de mensajería, bajo uso de minutos
- Adultos Estables (33-48 años): equilibrio entre minutos y mensajes
- Adultos Consolidados (49-63 años): preferencia por llamadas
- Mayores (64-79 años): consumo mínimo general

**Segmentación por nivel de uso**:
- Bajo uso: menos de 10 minutos/mes, menos de 50 mensajes/mes
- Uso medio: 15-40 minutos/mes, 100-200 mensajes/mes
- Alto uso: más de 50 minutos/mes, más de 300 mensajes/mes

**Salida**: DataFrames con columnas grupo_edad y grupo_uso para cada usuario

### 9. Generar Insights

Traducción de los hallazgos del análisis en conclusiones accionables para el negocio. Se identifican segmentos valiosos, se evalúa el potencial de cada grupo y se proponen estrategias.

**Insights principales**:

**Segmentos más valiosos**:
- Power Users de 49-63 años: generan 40% de ingresos con 15% de la base
- Adultos Estables 33-48 años: generan 35% de ingresos, segmento más grande
- Jóvenes Digitales 18-32 años: generan 20% actual pero con alto potencial de crecimiento

**Oportunidades de ingresos**:
- Migración de superconsumidores a planes ilimitados: +48K USD/mes
- Lanzamiento de plan intermedio (PLUS): +120K USD/mes
- Bundles especializados por segmento: +85K USD/mes
- Potencial total: +400-600K USD/mes en 90 días

**Recomendaciones de productos**:
- Plan LITE para mayores ocasionales
- Plan PLUS para adultos estables
- Plan PRO mejorado para power users
- Plan ULTRA ilimitado para superconsumidores
- Bundles: Joven Conectado, Familia Estable, Conversador Premium, Simplemente Conectado

**Salida**: Documento ejecutivo con hallazgos, segmentación y recomendaciones

## Estructura del Repositorio

```
├── README.md
├── Analisis_Ejecutivo_ConnectaTel.md
├── S7_Version-Estudiante-Project-ConnectaTel.ipynb
├── requirements.txt
├── LICENSE
└── datasets/
    ├── plans.csv
    ├── users_latam.csv
    └── usage.csv
```

## Requisitos

Python 3.9 o superior con las siguientes librerías:

```
pandas >= 1.3.0
numpy >= 1.21.0
matplotlib >= 3.4.0
seaborn >= 0.11.0
jupyter >= 1.0.0
```

## Instalación

### Opción 1: Clonar repositorio

```bash
git clone https://github.com/[usuario]/telecom-analysis.git
cd telecom-analysis
pip install -r requirements.txt
```

### Opción 2: Google Colab

Abre directamente en Google Colab:

```
https://colab.research.google.com/github/[usuario]/telecom-analysis/blob/main/S7_Version-Estudiante-Project-ConnectaTel.ipynb
```

### Opción 3: Jupyter Local

```bash
pip install jupyter
jupyter notebook
# Abre S7_Version-Estudiante-Project-ConnectaTel.ipynb
```

## Uso

### Ejecutar el análisis completo

```bash
jupyter notebook S7_Version-Estudiante-Project-ConnectaTel.ipynb
```

El notebook está dividido en celdas que corresponden a cada etapa del pipeline. Ejecuta las celdas secuencialmente para reproducir el análisis completo.

### Leer solo los hallazgos

Si deseas ver los resultados sin ejecutar código, abre:

```
Analisis_Ejecutivo_ConnectaTel.md
```

## Hallazgos Principales

### Problemas de Datos

Los datos presentaban los siguientes problemas de calidad:

| Columna | Problema | Cantidad | Porcentaje | Acción |
|---------|----------|----------|-----------|--------|
| age | Valor inválido (-999) | 1 | 0.025% | Imputado con mediana |
| city | Valores nulos | 469 | 11.7% | Unificado como Unknown |
| reg_date | Fechas futuras | 40 | 1.0% | Marcado como nulo |
| duration | Valores nulos | 22076 | 55.19% | Válido (Missing At Random) |
| length | Valores nulos | 17896 | 44.74% | Válido (Missing At Random) |

### Segmentos Identificados

**Por Edad**:
- Jóvenes Digitales (18-32): 25% de la base, alto SMS bajo minutos
- Adultos Estables (33-48): 30% de la base, consumo equilibrado
- Adultos Consolidados (49-63): 30% de la base, alto consumo de minutos
- Mayores (64-79): 15% de la base, consumo mínimo

**Por Nivel de Uso**:
- Bajo uso: 35-40%, menos de 10 minutos/mes
- Uso medio: 45-50%, 15-40 minutos/mes
- Alto uso: 10-15%, más de 50 minutos/mes

### Segmentos de Mayor Valor

1. Power Users 49-63 años: 40% de ingresos, altamente leales
2. Adultos Estables 33-48 años: 35% de ingresos, base estable
3. Jóvenes Digitales 18-32 años: 20% ingresos, crecimiento potencial

### Patrones Extremos

- Superconsumidores de minutos (2-3%): más de 100 minutos/mes
- Superconsumidores de mensajes (5-7%): más de 500 SMS/mes
- Usuarios inactivos (8-10%): casi nula actividad

## Recomendaciones de Negocio

### Nuevos Planes Propuestos

- LITE (8 USD): 50 min, 200 SMS, 2 GB - para mayores ocasionales
- BASE (12 USD): 100 min, 500 SMS, 5 GB - para jóvenes digitales
- PLUS (18 USD): 200 min, 800 SMS, 10 GB - para adultos estables
- PRO (28 USD): 800 min, 600 SMS, 15 GB - para power users
- ULTRA (40 USD): minutos ilimitados, 1000 SMS, 30 GB - para superconsumidores

### Bundles Especializados

- Joven Conectado: BASE + 15 GB datos = 16 USD
- Familia Estable: PLUS + línea secundaria = 32 USD
- Conversador Premium: PRO + 100 min internacionales = 35 USD
- Simplemente Conectado: LITE + soporte 24/7 = 11 USD

### Impacto Estimado

- Ingresos adicionales: 400-600K USD/mes
- Aumento en ARPU: 15-20%
- Mejora de margen: 25%
- Reducción de churn: 8-10%
- Timeline de implementación: 90 días

## Métodos y Técnicas

El proyecto utiliza técnicas estándar de análisis de datos:

- Análisis exploratorio de datos (EDA)
- Limpieza y validación de datos
- Estadísticas descriptivas
- Detección de outliers mediante IQR
- Segmentación de usuarios
- Análisis comparativo por segmento

## Reproducibilidad

El análisis es completamente reproducible:

- Código documentado paso a paso
- Datos completos incluidos en el repositorio
- Versiones específicas de librerías en requirements.txt
- Semilla aleatoria fijada en operaciones estocásticas

## Stack Técnico

- Lenguaje: Python 3.9+
- Análisis de datos: Pandas, NumPy
- Visualización: Matplotlib, Seaborn
- Entorno: Jupyter Notebook
- Versionado: Git y GitHub

## Licencia

Este proyecto está bajo licencia MIT.

## Contribuciones

Las contribuciones son bienvenidas. Para cambios importantes:

1. Fork el repositorio
2. Crea una rama para tu feature
3. Realiza tus cambios y commit
4. Push a la rama
5. Abre un Pull Request

## Contacto

Para preguntas sobre el análisis, abrir issues en el repositorio.

---

**Estado**: Análisis completo  
**Última actualización**: Enero 2025  
**Versión**: 1.0
