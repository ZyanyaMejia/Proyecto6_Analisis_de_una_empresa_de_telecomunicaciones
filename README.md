# Análisis de Comportamiento de Clientes - ConnectaTel

## Descripción del Proyecto

Este proyecto realiza un **análisis exhaustivo del comportamiento de clientes** de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica. El análisis se enfoca en **identificar patrones de uso, segmentar clientes y detectar oportunidades comerciales** basadas en datos registrados hasta el año 2024.

---

## Objetivo Principal

Evaluar el comportamiento de los clientes de ConnectaTel mediante:
- **Exploración y limpieza de datos** de múltiples fuentes
- **Análisis de distribuciones** de uso y características demográficas
- **Segmentación de clientes** por edad y nivel de uso
- **Generación de insights ejecutivos** para la toma de decisiones estratégicas

---

## Datasets Utilizados

El análisis utiliza **3 archivos CSV** principales:

### 1. **plans.csv**
- Información de los planes actuales de ConnectaTel
- Incluye: precio, minutos incluidos, GB incluidos, costo por extras
- Tamaño: 2 registros

### 2. **users.csv (users_latam.csv)**
- Datos demográficos de los clientes
- Columnas: `user_id`, `first_name`, `last_name`, `age`, `city`, `reg_date`, `plan`, `churn_date`
- Registros: ~3,999 usuarios
- **Valores faltantes detectados:** `city` (11%), `churn_date` (88%)

### 3. **usage.csv**
- Registro de actividad de uso de servicios (mensajes y llamadas)
- Columnas: `id`, `user_id`, `date`, `type`, `duration`, `length`
- Registros: ~40,000 registros de uso
- **Valores faltantes detectados:** `date` (0.1%), `duration` (55%), `length` (44%)

---

## Etapas del Análisis Realizadas

### **Paso 1: Cargar y Explorar Datos**
- Carga de los 3 datasets en memoria
- Revisión de estructura (filas, columnas, tipos de datos)
- Identificación preliminar de inconsistencias

### **Paso 2: Identificación de Problemas de Calidad**
- Detección de valores nulos y su proporción
- Búsqueda de valores inválidos y sentinels
- Revisión y validación de fechas

### **Paso 3: Limpieza Básica de Datos**
- Corrección de sentinels: reemplazo de `-999` en `age` con la mediana
- Tratamiento de valores inválidos en `city` (reemplazo de `"?"` con `pd.NA`)
- Marcado de fechas fuera de rango como `NaT`
- Validación de valores MAR (Missing At Random)

### **Paso 4: Summary Statistics por Usuario**
- Agregación de datos de uso por usuario
- Cálculo de métricas: total de mensajes, total de llamadas, total de minutos
- Combinación de datos de uso con datos demográficos

### **Paso 5: Visualización de Distribuciones y Outliers**
- **Histogramas** para: edad, cantidad de mensajes, cantidad de llamadas, minutos de llamada
- **Boxplots** para identificar outliers en variables numéricas
- Análisis de patrones de sesgo en distribuciones
- Identificación de valores extremos usando método IQR

### **Paso 6: Segmentación de Clientes**
- **Segmentación por edad:**
  - Joven: < 30 años
  - Adulto: 30-60 años
  - Adulto Mayor: ≥ 60 años
  
- **Segmentación por nivel de uso:**
  - Bajo uso: llamadas < 5 y mensajes < 5
  - Uso medio: llamadas < 10 y mensajes < 10
  - Alto uso: el resto de casos

- **Análisis adicional:** método preferente de comunicación (mensajes vs llamadas)

### **Paso 7: Insights Ejecutivos**
- Análisis de problemas detectados en los datos originales
- Identificación de segmentos de clientes más valiosos
- Detección de patrones de uso extremo (outliers)
- Recomendaciones para mejora de planes y estrategias comerciales

---

#### Requisitos y Dependencias

```python
pandas >= 1.3.0
numpy >= 1.20.0
matplotlib >= 3.4.0
seaborn >= 0.11.0
jupyter >= 1.0.0
```
---

## Cómo Ejecutar el Notebook

### **Opción 1: Google Colab (Recomendado)**

1. Abre [Google Colab](https://colab.research.google.com/)
2. Selecciona **File → Open Notebook**
3. Ingresa la URL del repositorio en GitHub o carga el archivo `.ipynb` directamente
4. Sube los archivos CSV a Colab o monta Google Drive:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
5. Actualiza las rutas de carga de datos en las celdas de código
6. Ejecuta las celdas en orden (Shift + Enter)

### **Opción 2: Localmente con Jupyter**

1. **Clona el repositorio:**
   ```bash
   git clone https://github.com/tuusuario/connectatel-analysis.git
   cd connectatel-analysis
   ```

2. **Instala dependencias:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Inicia Jupyter:**
   ```bash
   jupyter notebook
   ```

4. **Abre el archivo:**
   - Busca `S7_Version-Estudiante-Project-ConnectaTel.ipynb`
   - Haz clic para abrir

5. **Asegúrate de que los CSV estén en la carpeta correcta:**
   ```
   proyecto/
   ├── notebooks/
   │   └── S7_Version-Estudiante-Project-ConnectaTel.ipynb
   ├── datasets/
   │   ├── plans.csv
   │   ├── users_latam.csv
   │   └── usage.csv
   └── README.md
   ```

6. **Ejecuta las celdas:**
   - Usa **Shift + Enter** para ejecutar celda a celda
   - O **Ctrl + Shift + Enter** para ejecutar todas (Python 3.8+)

---

##  Guía de Reproducción

### Estructura del Notebook:

```
1 Cargar librerías (pandas, numpy, matplotlib, seaborn)
2 Cargar los 3 datasets (plans, users, usage)
3 Exploración inicial (.shape, .info(), .describe())
4 Revisión de valores nulos (cantidad y proporción)
5 Detección de sentinels y valores inválidos
6 Validación y limpieza de fechas
7 Corrección de sentinels (-999 → mediana)
8 Verificación de MAR en columnas faltantes
9 Agregación de uso por usuario
10 Resumen estadístico y distribuciones
11 Visualización de histogramas y boxplots
12 Identificación de outliers (método IQR)
13 Segmentación por edad y nivel de uso
14 Análisis cruzado (crosstab)
15 Generación de insights ejecutivos
```

### Puntos Clave a Verificar:

- Los CSV están en la ruta correcta
- Todas las dependencias están instaladas
- Se ejecutan las celdas en orden (sin saltar)
- Los gráficos se generan sin errores
- Los valores agregados coinciden con el análisis esperado

---

## Hallazgos Principales

### **Problemas de Datos Originales:**
- `users.city`: 11% de valores faltantes
- `users.churn_date`: 88% de valores faltantes
- `usage.duration`: 55% de valores faltantes (expected: llamadas)
- `usage.length`: 44% de valores faltantes (expected: mensajes)

### **Segmentos Identificados:**

| Segmento | Cantidad | Características |
|----------|----------|-----------------|
| Adultos (30-60 años) | 2,017 | Mayor concentración de clientes |
| Adultos Mayores (>60 años) | 1,222 | 56% prefieren mensajes |
| Jóvenes (<30 años) | 760 | 61% prefieren mensajes |
| Uso Medio | 3,721 | 93% de la cartera |
| Uso Alto | 278 | 7% de la cartera (power users) |

### **Recomendaciones Estratégicas:**
1. Crear paquetes atractivos de **mensajería** (57% del total usa mensajes)
2. Investigar por qué el 93% está en uso medio (fallas de servicio vs. falta de interés)
3. Segmentar marketing por edad (jóvenes, adultos, adultos mayores)
4. Desarrollar planes diferenciados para power users vs. usuarios ocasionales

---

## Notas Importantes

- **Fechas:** El análisis incluye datos hasta 2024. Se detectó un registro con fecha 2026 que fue marcado como nulo y el ultimo data set creado **user_profile** incluye las fechas del data set de **usage** (2022, 2023, y 2024).  
- **Valores MAR:** Los nulos en `duration` y `length` son estructuralmente esperados (tipo "texto" no tiene duración, tipo "llamada" no tiene longitud).
- **Outliers:** Se mantuvieron en el análisis porque representan comportamientos reales (aunque poco frecuentes).

---

## Contribuciones

Este proyecto fue realizado como parte de un proyecto de evaluación del modulo "Análisis estadístico para
detectar patrones y outliers". Se realizó sobre el análisis de datos para telecom. Si deseas sugerir mejoras:

1. Abre un **Issue**
2. Propón cambios en un **Pull Request**
3. Contacta al autor

---

## Contacto

**Analista:** Zyanya ( Data analyst, UNAM Renewable Energy Engineer )

---

**Última actualización:** Septiembre 2026
