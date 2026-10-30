# Selección y comparación de modelos

Demo de cómo elegir un modelo con datos y no a ojo. Dos problemas sintéticos, el mismo método: partir
los datos, buscar hiperparámetros con validación cruzada, evaluar en test y comparar los resultados.

El notebook se ejecuta de principio a fin y guarda las tablas y las gráficas de cada comparación.

## Qué demuestra

- Cómo montar un `GridSearchCV` con varias métricas a la vez y refinar por una de ellas.
- Por qué el error de entrenamiento no sirve para decidir: KNN lo lleva a cero y aun así es el peor en
  test.
- Cómo cambia el rendimiento cuando se reduce el número de variables.
- Que un selector estadístico como `SelectKBest` no siempre encuentra las variables que el propio
  generador marcó como informativas.

## Problema 1 — Regresión

Datos generados con `make_regression`: 2000 observaciones, 10 características y ruido 1. Se comparan
OLS, Lasso, Ridge y KNN con validación cruzada de 5 pliegues y cuatro métricas (MAE, MSE, RMSE y R²).

| Modelo | MAE | MSE | RMSE | R² |
|---|---|---|---|---|
| OLS | 0.7731 | 0.9534 | 0.9764 | 1.0000 |
| Lasso | 0.8031 | 1.0349 | 1.0173 | 1.0000 |
| **Ridge** | **0.7727** | **0.9524** | **0.9759** | **1.0000** |
| KNN | 50.0401 | 4579.8916 | 67.6749 | 0.8720 |

Ridge gana por una diferencia mínima sobre OLS, y esa mínima diferencia es la regularización
funcionando. KNN es el caso contrario: error cero en entrenamiento y el peor resultado en test.

## Problema 2 — Clasificación

Datos generados con `make_classification`: 1600 observaciones, 12 características (5 informativas, 3
redundantes y 4 de ruido). Se comparan árbol de decisión, regresión logística y SVM, cada uno con las
12 variables y con las 5 que elige `SelectKBest`.

| Modelo | 5 variables | 12 variables | Diferencia |
|---|---|---|---|
| Árbol de decisión | 0.8219 | 0.8938 | −0.0719 |
| Regresión logística | 0.8344 | 0.8469 | −0.0125 |
| **SVC** | 0.9219 | **0.9344** | −0.0125 |

`SelectKBest` eligió los índices `[1, 4, 5, 6, 7]`: solo 2 de las 5 variables informativas. Las tres
redundantes puntuaron más alto porque tienen una relación lineal más evidente con la clase. Reducir de
12 a 5 variables baja la complejidad, pero no siempre mejora la precisión: el árbol pierde 7.2 puntos.

## Cómo ejecutar

```bash
uv sync
uv run jupyter lab
```

Abre `model-selection.ipynb` y ejecútalo de arriba abajo.

## Datos

No hay ficheros de datos: el notebook genera los dos conjuntos con `make_regression` y
`make_classification`, con semilla fija en 42. El resultado es reproducible.
