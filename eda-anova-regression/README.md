# Análisis exploratorio, ANOVA y regresión

Análisis de 118 sesiones de lectura para responder tres preguntas: qué factores se asocian con la
**concentración**, con la **cantidad de páginas leídas** y con la **duración de la sesión**. El
recorrido va de lo descriptivo a lo inferencial, y cada decisión queda justificada en el notebook.

## Datos

- `data/reading.parquet`: 118 filas × 21 columnas.
- Fuente: [Reading Performance](https://www.kaggle.com/datasets/chibangng/reading-performance), de
  Bancie Ng. Licencia **MIT**.
- Variables: edad, género, rol, minutos de lectura y descanso, concentración, páginas, dificultad del
  contenido, género literario, sonido, dispositivo, lugar, clima, ánimo, hambre, arousal, estado
  mental en los descansos, comprensión, vínculo con conocimiento previo, fatiga e interés.

## Qué hace el notebook

1. **Estadística descriptiva**: media, mediana, moda, mínimo, máximo y cuartiles de las variables
   numéricas.
2. **Detección de outliers** por rango intercuartílico. Aparecen en `MINUTES_READING` (2 registros),
   `MINUTES_BREAK` (7) y `PAGES` (6); se conservan porque son eventos reales, no errores.
3. **Transformaciones**: winsorización de `MINUTES_READING` en 164.75 y `log1p` en `MINUTES_BREAK` y
   `PAGES` para reducir la influencia de los extremos.
4. **ANOVA** de cada variable de interés contra los factores categóricos, con la prueba de Levene
   previa para comprobar homogeneidad de varianzas.
5. **Mínimos cuadrados ordinarios (OLS)** con variables dummy, interpretación de coeficientes y
   predicciones sobre casos concretos.
6. **Contraste** entre la predicción del modelo y la fórmula reconstruida a mano.

## Resultados

**ANOVA.** La única asociación significativa al 5% es concentración (`FOCUS_LEVEL`) con el vínculo al
conocimiento previo (`LINKING_TO_PREVIOUS_KNOWLEDGE`): F = 5.48, p = 0.0005. La dificultad del
contenido frente a los minutos de lectura queda cerca del umbral (p = 0.083).

**Modelos de regresión.**

| Variable | R² | p (F) | Predictores con significancia |
|---|---|---|---|
| Concentración (`FOCUS_LEVEL`) | 0.423 | 7.26e-07 | ánimo `slightly_bad` (−1.33), sin vínculo previo (−0.88), fatiga `fatigued` (−0.74), dificultad (+0.26) |
| Páginas (`PAGES_log`) | 0.150 | 0.00032 | género `self-development` (+1.01), `technical_&_vocational` (+0.45) |
| Minutos de lectura (`MINUTES_READING_winsor`) | 0.152 | 0.00967 | sesión sin descanso (−26.93) |

La concentración es el fenómeno más predecible: casi la mitad de su variación queda explicada por
factores de ánimo, fatiga y dificultad. Páginas y minutos tienen modelos significativos pero con
poco poder explicativo, lo que sugiere que influyen variables que el dataset no recoge.

**Predicciones.** En los tres casos se compara el valor real con el del modelo y con la fórmula
reconstruida. La fórmula acierta más en concentración y páginas; el modelo acierta más en minutos.

## Cómo ejecutar

```bash
uv sync
uv run jupyter lab
```

Abre `eda-anova-regression.ipynb` y ejecútalo de arriba abajo. El dataset ya está en `data/`, así que
no hay que descargar nada.

## Nota

La muestra es de 118 sesiones y el dataset es de origen público. Los resultados se leen como
asociaciones dentro de esta muestra, no como relaciones causales.
