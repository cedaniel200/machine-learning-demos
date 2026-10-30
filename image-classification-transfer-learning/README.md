# Clasificación de imágenes: desde cero, MobileNetV2 y VGG16

Comparación de tres estrategias para clasificar gestos de **piedra, papel o tijera** a partir de
imágenes:

1. Una CNN entrenada **desde cero**.
2. **MobileNetV2** con transferencia de aprendizaje y ajuste fino.
3. **VGG16** con transferencia de aprendizaje y ajuste fino.

El objetivo es ver cómo cambia el rendimiento y el coste de entrenamiento cuando se parte de una red
entrenada con ImageNet en lugar de aprender desde cero con un dataset pequeño.

## Datos

- **Rock-Paper-Scissors**: 2.188 imágenes de manos en tres clases (`rock`, `paper`, `scissors`).
- Fuente: [drgfreeman/rockpaperscissors](https://www.kaggle.com/datasets/drgfreeman/rockpaperscissors).
  Licencia **CC BY-SA 4.0**.
- El dataset **no se versiona**: el notebook lo descarga con `kagglehub`.

## Protocolo

- **Split estratificado 70/15/15** por clase, con semilla fija: 1.531 imágenes de entrenamiento, 328
  de validación y 329 de test.
- La validación se usa para `EarlyStopping`, `ReduceLROnPlateau` y `ModelCheckpoint`.
- El **test solo se usa al final**, para medir el resultado. No participa en la selección del modelo.
- Aumento de datos, regularización L2 y dropout en la estrategia desde cero.

## Resultados en test

| Estrategia | Pérdida | Accuracy |
|---|---|---|
| Desde cero | 0.0521 | **99.70%** |
| MobileNetV2 | 0.0976 | 96.96% |
| VGG16 | 0.0168 | 99.39% |

La red desde cero queda por delante en accuracy, pero VGG16 consigue la pérdida más baja. MobileNetV2
es la más frágil en el ajuste fino: necesita descongelar solo las últimas capas para no perder lo
aprendido en ImageNet.

## Qué muestra el notebook

- Descarga y preparación del dataset con `kagglehub`.
- Split reproducible en train/validación/test.
- Tres arquitecturas y dos fases de entrenamiento en las redes preentrenadas.
- Callbacks y curvas de aprendizaje.
- Evaluación por clase, matriz de confusión y comparación de estrategias.

## Cómo ejecutar

### En Colab (recomendado)

1. Sube `image-classification-transfer-learning.ipynb` a [Colab](https://colab.research.google.com).
2. En **Secrets**, añade `userName` y `apiKey` con tus credenciales de Kaggle y activa el acceso.
3. **Runtime → Change runtime type → T4 GPU**.
4. **Runtime → Run all**.

Si `kagglehub` no está instalado, ejecuta antes `!pip install -q kagglehub`.

### En local

```bash
uv sync --extra deep-learning
uv run jupyter lab
```

Las credenciales de Kaggle se leen de `~/.kaggle/kaggle.json` o de `KAGGLE_USERNAME` y `KAGGLE_KEY`.
TensorFlow está disponible para macOS ARM, pero el entrenamiento de los tres modelos es pesado; para
probarlo, Colab con GPU es la opción práctica.

## Créditos

Proyecto original desarrollado en equipo con Daniel Alfonso Correa Angarita, America Garrido Sánchez y
Javier García Masegosa. Esta versión corrige el protocolo de evaluación, reentrena los tres modelos y
documenta los resultados.

## Notas

Keras muestra un `UserWarning` de estilo al construir las capas de aumento de datos. No afecta al
entrenamiento ni a los resultados.
