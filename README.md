# machine-learning-demos

Colección de demos de aprendizaje automático y visión por computador en Python. Cada carpeta es un
caso independiente, con su propio README, sus datos y un notebook que se ejecuta de principio a fin.

## Demos

| Demo | Qué muestra | Stack |
|---|---|---|
| [computer-vision-lumen](computer-vision-lumen/) | Detección y caracterización de lúmenes en imágenes histológicas | OpenCV, scikit-image, NumPy |

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
└── pyproject.toml           # dependencias compartidas por todas las demos
```
