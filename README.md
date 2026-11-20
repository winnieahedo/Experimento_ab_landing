# Experimento A/B de landing page: conversión y gasto

Análisis de un experimento A/B realizado en una página de inicio con dos versiones (A y B). Se compara la conversión y el gasto promedio entre versiones, y se revisa si la fuente de tráfico y el tipo de usuario influyen en la conversión, para recomendar qué versión implementar.

**Herramientas:** Python (pandas, NumPy, SciPy, statsmodels, seaborn, matplotlib)

📓 **Notebook del análisis:** [Experimento_AB_Landing.ipynb](Experimento_AB_Landing.ipynb)

---

## 1. Contexto y problema de negocio

Se probaron dos versiones de una landing page (A y B) y se necesita decidir con datos cuál implementar. Las preguntas que responde el análisis:

- ¿Qué versión de la página genera mayor gasto promedio por usuario que convierte?
- ¿Qué versión tiene mayor tasa de conversión?
- ¿La fuente de tráfico está asociada a la conversión?
- ¿El tipo de usuario (nuevo o recurrente) está asociado a la conversión?

## 2. Datos

Archivo `landing_experiment.csv`: **40,000 usuarios** expuestos al experimento entre el 1 y el 28 de enero de 2026 (19,982 en la página A y 20,018 en la B).

| Columna | Descripción |
|---------|-------------|
| `user_id` | Identificador único del usuario |
| `date` | Fecha en que el usuario fue expuesto a la página |
| `landing` | Versión de la página mostrada (A o B) |
| `region` | Región geográfica del usuario |
| `dispositivo` | Tipo de dispositivo |
| `traffic_source` | Canal por el que llegó (Ads, Email, Organic, Referral) |
| `user_type` | Tipo de usuario según historial previo (Nuevo o Recurrente) |
| `converted` | Si el usuario realizó una conversión |
| `gasto` | Monto gastado (0 si no convirtió) |

**Origen de los datos:** Proporcionados por TripleTen

## 3. Metodología

1. **Validación de datos:** revisión de tipos, nulos, usuarios únicos, rango de fechas y categorías esperadas. No se identificaron valores nulos ni errores en las variables categóricas.
2. **Gasto promedio (usuarios que convirtieron):** prueba de Levene para comparar varianzas y prueba t de Welch para comparar medias.
3. **Tasa de conversión:** prueba z de dos proporciones.
4. **Fuente de tráfico y conversión:** prueba chi-cuadrada de independencia y V de Cramér para medir la fuerza de la asociación.
5. **Tipo de usuario y conversión:** prueba chi-cuadrada de independencia.
6. **Visualización** de los resultados por categoría, con conversiones absolutas y tasas de conversión.

Nivel de significancia en todas las pruebas: α = 0.05.

## 4. Resultados

| Pregunta | Prueba | Resultado |
|----------|--------|-----------|
| Gasto promedio por usuario convertido | Levene + t de Welch | B gasta más: $68.75 contra $61.09 (≈ +12.5%). Diferencia significativa (p ≈ 3.6e-21) |
| Tasa de conversión | z de dos proporciones | B convierte más: 15.96% contra 12.57% (+3.39 puntos porcentuales). Diferencia significativa (p ≈ 3.8e-22) |
| Fuente de tráfico y conversión | Chi-cuadrada + V de Cramér | Asociación significativa (p = 0.034), pero con fuerza prácticamente nula (V de Cramér = 0.015) |
| Tipo de usuario y conversión | Chi-cuadrada | Sin diferencia significativa (p = 0.474): nuevos 14.4% y recurrentes 14.1% |

**Conversión por canal:** Email 15.0%, Ads 14.7%, Referral 13.9%, Organic 13.8%. Organic aporta más conversiones en volumen (2,480) porque es el canal que más usuarios trae, no porque sea más efectivo.

## 5. Recomendaciones

1. **Implementar la versión B de la página.** Convierte más y sus clientes gastan más, con diferencias estadísticamente significativas. Es el hallazgo más sólido del análisis.
2. **No reasignar presupuesto ni segmentar estrategias solo por fuente de tráfico o tipo de usuario.** Las diferencias son mínimas o no significativas.
3. **Concentrar la optimización en la experiencia de la página** y evaluar otras variables, como el costo por canal, antes de mover inversión.

## 6. Limitaciones

- El gasto promedio se compara únicamente entre los usuarios que convirtieron.
- El experimento cubre 28 días (enero de 2026).
- No hay información de costos por canal, así que no se puede evaluar el retorno de cada fuente de tráfico.

## 7. Cómo reproducirlo

```bash
pip install pandas numpy scipy statsmodels seaborn matplotlib jupyter
jupyter notebook Experimento_AB_Landing.ipynb
```

Coloca el archivo `landing_experiment.csv` en la carpeta del proyecto y ajusta la ruta en la celda de carga de datos.

## 8. Autora

**Winnie Ahedo** · [LinkedIn](https://www.linkedin.com/in/winnie-ahedo/) · winnieahedo@gmail.com
