# landing-experiment-analysis
# Experimento A/B en Página de Inicio: Evaluación y Recomendación de Negocio

El objetivo principal de este proyecto es analizar los resultados de un **experimento A/B** realizado en la página de inicio (*landing page*) de una plataforma digital. La prueba comparó la versión original (**Página A**) con una variante optimizada (**Página B**) a lo largo de 28 días, con el fin de determinar qué versión maximiza la **tasa de conversión** y el **gasto promedio por cliente**, respaldando una decisión estratégica basada en datos rigurosos.

---

## 🧩 Paso 1: Carga, Validación y Análisis Exploratorio de Datos (EDA)

El conjunto de datos `landing_experiment.csv` consta de **40,000 registros únicos**, recolectados entre el **1 de enero de 2026** y el **28 de enero de 2026**. No se identificaron registros duplicados por `user_id` ni valores nulos.

### Estructura de las Variables

| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| `user_id` | Cadena (`object`) | Identificador único universal del usuario. |
| `date` | Fecha (`datetime64`) | Fecha de interacción en el experimento. |
| `landing` | Categórica (`object`) | Versión de la landing page mostrada (**A** o **B**). |
| `region` | Categórica (`object`) | Región geográfica (**Norte**, **Centro**, **Sur**, **Occidente**, **Oriente**). |
| `dispositivo` | Categórica (`object`) | Dispositivo de acceso (**Mobile**, **Desktop**). |
| `traffic_source` | Categórica (`object`) | Canal de adquisición (**Organic**, **Ads**, **Email**, **Referral**). |
| `user_type` | Categórica (`object`) | Perfil de historial del usuario (**Nuevo**, **Recurrente**). |
| `converted` | Binaria (`int64`) | Indicador de conversión (1 = compró, 0 = no compró). |
| `gasto` | Numérica (`float64`) | Monto total gastado en USD ($0.00 si no convirtió). |

### Distribución General del Conjunto de Datos

* **Variante del Experimento (`landing`):** Página A = 19,982 usuarios (49.955%) vs. Página B = 20,018 usuarios (50.045%). Asignación equitativa 50/50.
* **Dispositivo (`dispositivo`):** Móvil = 24,829 usuarios (62.07%) | Escritorio = 15,171 usuarios (37.93%).
* **Tipo de Usuario (`user_type`):** Nuevo = 26,033 usuarios (65.08%) | Recurrente = 13,967 usuarios (34.92%).
* **Fuente de Tráfico (`traffic_source`):** Orgánico = 17,987 (44.97%), Anuncios = 11,935 (29.84%), Email = 6,123 (15.31%), Referidos = 3,955 (9.89%).
* **Conversión General (`converted`):** 5,706 conversiones totales (Tasa global del 14.265%).
* **Monto de Gasto (`gasto`):** 
  * *Muestra total:* Media = $9.33 USD, Desviación estándar = $25.67 USD, Máximo = $303.68 USD.
  * *Clientes convertidos (n = 5,706):* Media = $65.37 USD, Mediana = $59.86 USD, Mínimo = $12.12 USD, Máximo = $303.68 USD.

---

## 💰 Paso 2: Comparar el Gasto Promedio por Cliente (Página A vs. B)

Se evaluó si la versión de la landing page influye en el valor monetario gastado por los usuarios que realizaron una transacción efectiva.

### Métricas Descriptivas de Gasto por Cliente

| Variante | Clientes Convertidos (n) | Gasto Promedio | Desviación Estándar |
| :--- | :---: | :---: | :---: |
| **Página A** | 2,512 | $61.09 USD | $29.41 USD |
| **Página B** | 3,194 | $68.75 USD | $31.54 USD |
| **Diferencia** | **+682 clientes** | **+$7.66 USD (+12.54%)** | — |

### Formulación de Hipótesis
* **Hipótesis Nula ($H_0$):** El gasto promedio de los clientes convertidos es igual entre la Página A y la Página B ($\mu_A = \mu_B$).
* **Hipótesis Alternativa ($H_1$):** El gasto promedio de los clientes convertidos difiere entre la Página A y la Página B ($\mu_A \neq \mu_B$).

### Prueba Estadística (Prueba t de Student para dos Muestras Independientes)
* **Estadístico t:** -9.366
* **Valor p (p-value):** $1.06 \times 10^{-20}$ ($\alpha = 0.01$)

### Conclusión e Interpretación de Negocio
Dado que el p-value $\le 0.01$, **se rechaza la hipótesis nula ($H_0$)** con un nivel de confianza superior al 99%. Existe evidencia estadística suficiente para afirmar que la **Página B genera un gasto promedio por cliente significativamente mayor (+$7.66 USD o +12.54%)** en comparación con la Página A.

---

## 📈 Paso 3: Comparar la Tasa de Conversión (Página A vs. B)

Se analizó la efectividad de cada landing page para convertir visitantes en compradores.

### Métricas de Conversión por Variante

| Variante | Usuarios Totales (N) | Conversiones (k) | Tasa de Conversión (p) |
| :--- | :---: | :---: | :---: |
| **Página A** | 19,982 | 2,512 | **12.57%** |
| **Página B** | 20,018 | 3,194 | **15.96%** |
| **Diferencia Absoluta** | — | **+682** | **+3.38 pp (+26.97% relativo)** |

### Formulación de Hipótesis
* **Hipótesis Nula ($H_0$):** La tasa de conversión es idéntica en ambas versiones ($p_A = p_B$).
* **Hipótesis Alternativa ($H_1$):** La tasa de conversión es diferente entre la Página A y la Página B ($p_A \neq p_B$).

### Prueba Estadística (Prueba z de dos Proporciones)
* **Estadístico z:** -9.677
* **Valor p (p-value):** $3.76 \times 10^{-22}$ ($\alpha = 0.01$)

### Conclusión e Interpretación de Negocio
Con un p-value $\le 0.01$, **se rechaza la hipótesis nula ($H_0$)**. La **Página B es sustancialmente superior en tasa de conversión**, logrando un incremento de **+3.38 puntos porcentuales** respecto a la versión control (Página A), representando un aumento relativo en volumen de compras del **26.97%**.

---

## 🔗 Paso 4: Relación entre la Fuente de Tráfico y la Conversión

Se examinó si la probabilidad de conversión varía en función del canal de adquisición del usuario.

### Distribución de Conversiones por Fuente de Tráfico

| Fuente de Tráfico | Usuarios Totales | Convertidos (1) | No Convertidos (0) | Tasa de Conversión (%) |
| :--- | :---: | :---: | :---: | :---: |
| **Email** | 6,123 | 918 | 5,205 | **14.99%** |
| **Ads** | 11,935 | 1,759 | 10,176 | **14.74%** |
| **Referral** | 3,955 | 549 | 3,406 | **13.88%** |
| **Organic** | 17,987 | 2,480 | 15,507 | **13.79%** |

### Formulación de Hipótesis
* **Hipótesis Nula ($H_0$):** La conversión es independiente de la fuente de tráfico.
* **Hipótesis Alternativa ($H_1$):** La conversión depende de la fuente de tráfico.

### Prueba Estadística (Prueba de $\chi^2$ de Independencia)
* **Estadístico Chi-cuadrado ($\chi^2$):** 8.662
* **Grados de Libertad (df):** 3
* **Valor p (p-value):** 0.034 ($\alpha = 0.05$)

### Conclusión e Interpretación de Negocio
Puesto que el p-value = $0.034 < 0.05$, **se rechaza la hipótesis nula ($H_0$)**. Existe una dependencia estadística entre el canal de origen y la probabilidad de conversión. El tráfico proveniente de **Email (14.99%)** y **Anuncios (14.74%)** genera el rendimiento relativo más alto.

---

## 👤 Paso 5: Relación entre el Tipo de Usuario y la Conversión

Se evalúo si el perfil de visita del usuario (**Nuevo** vs. **Recurrente**) incide en la conversión.

### Distribución de Conversiones por Tipo de Usuario

| Tipo de Usuario | Usuarios Totales | Convertidos (1) | No Convertidos (0) | Tasa de Conversión (%) |
| :--- | :---: | :---: | :---: | :---: |
| **Nuevo** | 26,033 | 3,738 | 22,295 | **14.36%** |
| **Recurrente** | 13,967 | 1,968 | 11,999 | **14.09%** |

### Formulación de Hipótesis
* **Hipótesis Nula ($H_0$):** La conversión es independiente del tipo de usuario.
* **Hipótesis Alternativa ($H_1$):** La conversión depende del tipo de usuario.

### Prueba Estadística (Prueba de $\chi^2$ de Independencia)
* **Estadístico Chi-cuadrado ($\chi^2$):** 0.513
* **Grados de Libertad (df):** 1
* **Valor p (p-value):** 0.474 ($\alpha = 0.05$)

### Conclusión e Interpretación de Negocio
Puesto que el p-value = $0.474 > 0.05$, **NO se rechaza la hipótesis nula ($H_0$)**. No existe evidencia estadística suficiente para sostener que el perfil del usuario (Nuevo vs. Recurrente) afecte la probabilidad de conversión. La propuesta de valor de la plataforma funciona de manera uniforme para ambos segmentos.

---

## 📊 Paso 6: Resumen Sintético de Pruebas Estadísticas

| Comparación / Prueba | Estadístico | Valor p | Significativo ($\alpha=0.05$) | Decisión de Negocio |
| :--- | :---: | :---: | :---: | :--- |
| **Gasto Promedio (A vs B)** | $t = -9.366$ | $1.06 \times 10^{-20}$ | Sí ($p < 0.01$) | Página B genera **+$7.66 USD** por venta. |
| **Tasa Conversión (A vs B)** | $z = -9.677$ | $3.76 \times 10^{-22}$ | Sí ($p < 0.01$) | Página B incrementa conversión en **+3.38 pp**. |
| **Fuente de Tráfico vs Conversión** | $\chi^2 = 8.662$ | 0.034 | Sí ($p < 0.05$) | Email y Ads muestran mayor tasa de conversión. |
| **Tipo Usuario vs Conversión** | $\chi^2 = 0.513$ | 0.474 | No ($p > 0.05$) | Rendimiento equitativo entre nuevos y recurrentes. |

---

## 🚀 Recomendaciones de Negocio

1. **Implementación Inmediata de la Página B:** Se recomienda sustituir la Página A e implementar la **Página B como la nueva landing page oficial al 100% del tráfico**. Esta decisión incrementa simultáneamente el volumen de transacciones en un **26.97%** y el valor promedio del ticket de compra en un **12.54%**.
2. **Reasignación Estratégica del Presupuesto de Marketing:** Incrementar la inversión en campañas de **Email Marketing** y **Anuncios Pagados (Ads)**, al ser los canales con mejor tasa de conversión ($14.99\%$ y $14.74\%$, respectivamente).
3. **Estrategia Uniforme por Perfil de Usuario:** Dado que no existen diferencias de conversión entre usuarios Nuevos y Recurrentes, la Página B demuestra ser igualmente efectiva tanto para la adquisición de prospectos nuevos como para el cierre de usuarios recurrentes.
