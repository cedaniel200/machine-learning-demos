# machine-learning-demos

Colección de demos de IA y análisis de datos en Python: aprendizaje automático, visión por computador,
lógica difusa y estadística aplicada. Cada carpeta es un caso independiente, con su propio README, sus
datos y un notebook que se ejecuta de principio a fin.

## Demos

| Demo | Qué muestra | Stack |
|---|---|---|
| [computer-vision-lumen](computer-vision-lumen/) | Detección y caracterización de lúmenes en imágenes histológicas | OpenCV, scikit-image, NumPy |
| [model-selection](model-selection/) | Comparación de modelos de regresión y clasificación con búsqueda de hiperparámetros | scikit-learn, pandas |
| [fuzzy-famacha](fuzzy-famacha/) | Recomendación de desparasitación en ovinos con lógica difusa (Mamdani) | scikit-fuzzy, NumPy |
| [eda-anova-regression](eda-anova-regression/) | Análisis exploratorio, ANOVA y regresión sobre sesiones de lectura | pandas, statsmodels, SciPy |

## Requisitos

- [uv](https://docs.astral.sh/uv/) — gestiona el entorno y las dependencias del proyecto.

## Cómo ejecutar

```bash
uv sync
uv run jupyter lab
```

Abre el notebook de la demo que quieras probar y ejecútalo de arriba abajo. Cada carpeta explica sus
propios pasos y resultados.

## Estructura

```
machine-learning-demos/
├── computer-vision-lumen/   # detección de lúmenes en imágenes histológicas
├── model-selection/         # comparación de modelos con GridSearchCV
├── fuzzy-famacha/           # sistema difuso de desparasitación en ovinos
├── eda-anova-regression/    # EDA, ANOVA y regresión sobre sesiones de lectura
└── pyproject.toml           # dependencias compartidas por todas las demos
```
