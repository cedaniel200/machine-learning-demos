# machine-learning-demos

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat-square&logo=python&logoColor=white)
![uv](https://img.shields.io/badge/uv-managed-261230?style=flat-square)
![Jupyter](https://img.shields.io/badge/Jupyter-notebooks-F37626?style=flat-square&logo=jupyter&logoColor=white)
![Demos](https://img.shields.io/badge/demos-5-blueviolet?style=flat-square)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

Colección de demos de IA y análisis de datos en Python: aprendizaje automático, visión por computador,
aprendizaje profundo, lógica difusa y estadística aplicada. Cada carpeta es un caso independiente, con
su propio README, sus datos y un notebook que se ejecuta de principio a fin.

## Table of Contents

- [Demos](#demos)
- [Requisitos](#requisitos)
- [Cómo ejecutar](#cómo-ejecutar)
- [Estructura](#estructura)
- [Licencia](#licencia)

## Demos

| Demo | Qué muestra | Stack |
|---|---|---|
| [computer-vision-lumen](computer-vision-lumen/) | Detección y caracterización de lúmenes en imágenes histológicas | OpenCV, scikit-image, NumPy |
| [model-selection](model-selection/) | Comparación de modelos de regresión y clasificación con búsqueda de hiperparámetros | scikit-learn, pandas |
| [fuzzy-famacha](fuzzy-famacha/) | Recomendación de desparasitación en ovinos con lógica difusa (Mamdani) | scikit-fuzzy, NumPy |
| [eda-anova-regression](eda-anova-regression/) | Análisis exploratorio, ANOVA y regresión sobre sesiones de lectura | pandas, statsmodels, SciPy |
| [image-classification-transfer-learning](image-classification-transfer-learning/) | Clasificación de imágenes desde cero, con MobileNetV2 y con VGG16 | TensorFlow, Keras |

Cada demo explica en su README el problema, los datos, las decisiones y los resultados.

## Requisitos

- [uv](https://docs.astral.sh/uv/) — gestiona el entorno y las dependencias.
- Python 3.12 (uv lo instala si hace falta).

La demo de clasificación de imágenes necesita además:

```bash
uv sync --extra deep-learning
```

Ese extra instala TensorFlow, `kagglehub` y `seaborn`. También requiere credenciales de Kaggle y, para
entrenar con comodidad, una GPU; el README de la demo explica cómo ejecutarla en Colab.

## Cómo ejecutar

### Entorno base

```bash
uv sync
uv run jupyter lab
```

Abre el notebook de la demo que quieras probar y ejecútalo de arriba abajo.

### Demo de deep learning

```bash
uv sync --extra deep-learning
uv run jupyter lab
```

## Estructura

```
machine-learning-demos/
├── computer-vision-lumen/   # detección de lúmenes en imágenes histológicas
├── model-selection/         # comparación de modelos con GridSearchCV
├── fuzzy-famacha/           # sistema difuso de desparasitación en ovinos
├── eda-anova-regression/    # EDA, ANOVA y regresión sobre sesiones de lectura
├── image-classification-transfer-learning/  # desde cero, MobileNetV2 y VGG16
├── pyproject.toml           # dependencias compartidas
└── uv.lock                  # entorno reproducible
```

## Licencia

[MIT](LICENSE)
