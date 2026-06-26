# Análisis de Comportamiento de Clientes en ConnectaTel

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/imjesusgarcia/Student-Version-Project-ConnectaTel/blob/main/S7_Version-Estudiante-Project-ConnectaTel.ipynb)

## Descripción

Análisis exploratorio del comportamiento de clientes de **ConnectaTel**, empresa de telecomunicaciones en Latinoamérica, con datos registrados hasta el año 2024.

**Objetivo principal:**
> Construir un perfil estadístico de los clientes, detectar comportamientos atípicos y crear segmentos para diseñar estrategias de retención y mejora de planes.

---

## Datasets

| Archivo | Descripción |
|---|---|
| `plans.csv` | Información de los planes: precio, minutos incluidos, GB incluidos, costo por extra |
| `users_latam.csv` | Información de clientes: edad, ciudad, fecha de registro, plan, churn |
| `usage.csv` | Detalle del uso real de servicios: llamadas y mensajes por usuario |

---

## Estructura del análisis

| Paso | Contenido |
|---|---|
| 1 — Carga y exploración | Carga de los 3 datasets, revisión de forma, tipos de datos y primeras filas |
| 2 — Identificación de problemas | Detección de nulos, sentinels y fechas fuera de rango |
| 3 — Limpieza básica | Corrección de sentinels, fechas imposibles y validación de nulos MAR |
| 4 — Summary statistics | Agregación de uso por usuario y resumen estadístico del perfil combinado |
| 5 — Visualización y outliers | Histogramas por plan, boxplots y análisis IQR de valores extremos |
| 6 — Segmentación | Clasificación por nivel de uso y por grupo de edad |
| 7 — Insight ejecutivo | Conclusiones y recomendaciones para stakeholders |

---

## Problemas detectados en los datos

| Columna | Dataset | Problema | Acción tomada |
|---|---|---|---|
| `age` | users | Sentinel `-999` (~25% de registros) | Reemplazado con la mediana |
| `city` | users | Sentinel `?` (96 registros, 2.4%) | Reemplazado con `NaN` |
| `reg_date` | users | 40 registros con año 2026 (futuro) | Marcado como `NaT` |
| `churn_date` | users | 88.35% de nulos | Conservado — nulo indica usuario activo |
| `duration` | usage | 55.19% de nulos | Conservado — aplica solo a llamadas (MAR) |
| `length` | usage | 44.74% de nulos | Conservado — aplica solo a mensajes (MAR) |
| `date` | usage | 0.12% de nulos | Filas eliminadas |

---

## Segmentación de clientes

**Por nivel de uso** (`grupo_uso`):

| Segmento | Criterio |
|---|---|
| Bajo uso | llamadas < 5 **y** mensajes < 5 |
| Uso medio | llamadas < 10 **y** mensajes < 10 |
| Alto uso | resto de casos |

**Por edad** (`grupo_edad`):

| Segmento | Criterio |
|---|---|
| Joven | age < 30 |
| Adulto | 30 ≤ age < 60 |
| Adulto Mayor | age ≥ 60 |

---

## Principales hallazgos

- La distribución de edad es uniforme entre 20 y 80 años; la edad **no determina** la elección de plan ni el nivel de uso.
- La proporción Básico/Premium (~65%/35%) se mantiene constante en todos los grupos de edad y uso.
- Las variables `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada` presentan outliers superiores que son **plausibles** (clientes de alto consumo, no errores de captura).
- El comportamiento de uso es **homogéneo** entre planes, sin un segmento demográfico claramente diferenciado.

---

## Recomendaciones de negocio

- **Crear un plan intermedio** orientado al segmento de Alto uso, que supera consistentemente los límites IQR en mensajes, llamadas y minutos — este grupo podría valorar un plan con mayores beneficios incluidos.
- **Segmentar campañas por nivel de uso** en lugar de por edad, priorizando ofertas de upgrade para clientes de Alto uso (mayor potencial de gasto).

---

## Tecnologías utilizadas

- Python 3
- pandas
- numpy
- matplotlib
- seaborn

---

## Cómo ejecutar el notebook

1. Haz clic en el badge **Abrir en Colab** al inicio del README.
2. Asegúrate de tener acceso a los datasets en la ruta `/datasets/`.
3. Ejecuta las celdas en orden secuencial (Paso 1 → Paso 7).
