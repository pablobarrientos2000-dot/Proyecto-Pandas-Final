# Proyecto EDA - Campaña de Marketing Bancario

## Descripción
Análisis exploratorio de datos (EDA) sobre una campaña de marketing directo 
de una institución bancaria portuguesa. El objetivo es identificar qué perfiles 
de clientes tienen mayor probabilidad de contratar un depósito bancario.
## Dataset
- **bank-additional.csv**: 43.000 clientes con 24 variables sobre la campaña de marketing
- **customer-details.xlsx**: información adicional de cada cliente como ingresos, 
  número de hijos, adolescentes en casa y visitas web mensuales

## Herramientas utilizadas
- Python 3.11
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Visual Studio Code

## Estructura del proyecto

Proyecto Pandas Final/
├── data/
│ ├── raw/ → datos originales sin modificar
│ └── processed/ → datos limpios y transformados
├── notebooks/ → análisis en Jupyter Notebook
├── visualizations/ → gráficos generados
└── README.md


## Pasos del análisis

### 1. Carga y exploración inicial
Cargamos el dataset con `pd.read_csv()` y exploramos su estructura básica 
con `head()` para ver las primeras filas, `shape` para conocer las dimensiones 
(43.000 filas x 24 columnas), `info()` para ver los tipos de datos de cada 
columna y `describe()` para obtener estadísticas descriptivas de las columnas numéricas.

### 2. Limpieza de datos
Detectamos valores vacíos con `isnull().sum()` y los tratamos según el tipo de columna:
- Columnas numéricas como `age` → rellenadas con la **mediana** porque es más 
  robusta que la media ante valores extremos
- Columnas de texto como `job`, `marital`, `education` → rellenadas con **'unknown'** 
  porque no podemos inventarnos un valor categórico
- Columnas binarias 0/1 como `default`, `housing`, `loan` → rellenadas con **0** 
  asumiendo que si no hay dato, no tienen ese producto
- Columna `cons.price.idx` → eliminada porque quedó completamente vacía 
  tras intentar convertirla a formato numérico
- Columna `Unnamed: 0` → eliminada por ser un índice automático sin valor analítico

### 3. Transformación de datos
Se creó una nueva columna `franja_edad` usando `apply()` con una función 
personalizada que clasifica a cada cliente en tres grupos:
- **Joven**: menos de 30 años
- **Adulto**: entre 30 y 50 años
- **Senior**: más de 50 años

Se combinaron los dos datasets usando `merge()` por el ID de cliente, 
obteniendo un dataset final de 30 columnas con información bancaria 
y de perfil de cliente.

### 4. Análisis descriptivo
Estadísticas principales del dataset:
- Edad media de los clientes: **39,97 años** (rango de 17 a 98 años)
- Duración media de la llamada: **257 segundos** (~4 minutos)
- Media de contactos por cliente durante la campaña: **2,57**
- El **54%** de los clientes tiene hipoteca
- Solo el **16%** tiene un préstamo personal
- Prácticamente ningún cliente tiene historial de impagos

### 5. Principales hallazgos

#### Por profesión
- **Estudiantes (31%)** y **jubilados (25%)** tienen la mayor tasa de contratación
- **Blue-collar (7%)** es el perfil menos receptivo a la campaña
- Los trabajadores administrativos son el grupo más numeroso pero con tasa media

#### Por nivel educativo
- Clientes con estudios universitarios tienen la mayor tasa real: **13,74%**
- A menor nivel educativo, menor tasa de contratación
- Los clientes con educación básica de 9 años tienen la tasa más baja: **7,81%**

#### Por método de contacto
- Contacto por **móvil (14,74%)** es casi 3 veces más efectivo que por teléfono fijo (5,16%)
- El banco debería priorizar el canal móvil en futuras campañas

#### Por franja de edad
- **Jóvenes (16,12%)** y **seniors (14,48%)** contratan más que los adultos de mediana edad (9,64%)
- Los adultos entre 30-50 años, a pesar de ser el grupo más numeroso, son los menos receptivos

#### Por ingresos
- Los ingresos no son un factor determinante para la contratación
- Diferencia mínima: clientes que contrataron tienen ingresos medios de 91.805€ 
  vs 93.132€ los que no contrataron

### 6. Visualizaciones
Se generaron 3 gráficos guardados en la carpeta `visualizations/`:
- **tasa_contratacion_profesion.png**: tasa de contratación por profesión
- **distribucion_edades.png**: distribución de edades según resultado
- **tasa_contacto.png**: tasa de contratación por método de contacto

## Conclusiones
El perfil con mayor probabilidad de contratar el depósito es un cliente 
**joven o jubilado**, contactado por **móvil**, con **estudios universitarios**. 
Ni la edad exacta ni los ingresos son factores determinantes por sí solos — 
lo que más influye es la profesión y el método de contacto utilizado.

El banco debería enfocar futuras campañas en estudiantes y jubilados, 
priorizando el contacto telefónico móvil sobre el fijo.
