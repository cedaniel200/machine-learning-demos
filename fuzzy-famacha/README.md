# Sistema difuso para recomendar desparasitación en ovinos

Sistema **Mamdani** que recomienda si conviene desparasitar una oveja a partir de dos señales que el
productor ya observa en campo:

- **Mucosa del párpado inferior** (método FAMACHA, 1–5, de rojo a blanco): indica el grado de anemia.
- **Condición corporal** (1–5, de flaca a gorda): indica el estado nutricional.

El problema real no es solo detectar anemia: desparasitar de más acelera la resistencia de los
parásitos a los fármacos, y desparasitar de menos puede matar al animal. El sistema devuelve una de
tres recomendaciones: **no desparasitar**, **a criterio del productor** o **desparasitar**.

## Diseño

| Variable | Etiquetas | Rango |
|---|---|---|
| Mucosa (entrada) | roja, rosada, blanca | 1–5 |
| Condición corporal (entrada) | flaca, normal, gorda | 1–5 |
| Recomendación (salida) | no, a criterio, sí | 1–5 |

Las funciones de pertenencia son trapecios y triángulos, con un diseño **preventivo**: la mucosa
rosada termina en 3.5 en vez de 4.0 y la condición flaca se extiende hasta 3.2, para que el sistema
avise antes de que el animal esté claramente enfermo.

## Reglas

| Regla | Mucosa | Condición corporal | Recomendación |
|---|---|---|---|
| R1 | roja | cualquiera | no desparasitar |
| R2 | rosada | gorda | no desparasitar |
| R3 | rosada | normal | a criterio del productor |
| R4 | rosada | flaca | desparasitar |
| R5 | blanca | cualquiera | desparasitar |

La mucosa manda; la condición corporal solo decide en el caso intermedio, cuando la mucosa está
rosada.

## Resultados

| Oveja | Entrada | Salida | Recomendación |
|---|---|---|---|
| KT001-26 | blanca / normal | 4.407 | sí |
| KT002-26 | rosada / normal | 3.000 | a criterio |
| KT003-26 | roja / gorda | 1.449 | no |
| KT004-26 | 3.0 / 2.1 | 4.407 | sí |
| KT005-26 | 3.0 / 3.8 | 3.000 | a criterio |

## Comparación con la implementación en FuzzyCLIPS

La primera versión de este sistema se implementó en **FuzzyCLIPS**. Los casos numéricos coinciden;
los que usan etiquetas lingüísticas no, porque FuzzyCLIPS compara el conjunto borroso de la etiqueta
con el de cada regla en vez de convertirla en un número. Esa compatibilidad activa débilmente reglas
que un valor numérico no activaría.

| Oveja | FuzzyCLIPS | Python |
|---|---|---|
| KT001-26 | 3.51 | 4.41 |
| KT002-26 | 3.00 | 3.00 |
| KT003-26 | 2.13 | 1.45 |
| KT004-26 | 4.40 | 4.41 |
| KT005-26 | 3.00 | 3.00 |

Mismo diseño y mismas reglas, distinto motor. La diferencia está documentada en el notebook.

## Notebooks

- **`fuzzy-famacha.ipynb`**: el sistema completo. Funciones de pertenencia, reglas, inferencia con
  `scikit-fuzzy`, los cinco casos y el cálculo manual de KT001-26 paso a paso (fuzzificación,
  activación, recorte, agregación y centroide).
- **`modus-ponens-generalizado.ipynb`**: anexo con el otro método de inferencia, el que se usó en la
  implementación en FuzzyCLIPS.
  Una regla, implicación de Kleene-Dienes y composición max-min, que produce el conjunto de salida
  `[0.6, 0.5, 0.4]` para desparasitar, a criterio y no desparasitar.

## Cómo ejecutar

```bash
uv sync
uv run jupyter lab
```

Abre el notebook que quieras y ejecútalo de arriba abajo.

## Nota sobre dependencias

`scikit-fuzzy 0.5.0` no declara sus dependencias en el paquete: instalarlo solo revienta con
`ModuleNotFoundError`. Por eso el `pyproject` del repositorio incluye explícitamente `scipy`,
`networkx` y `packaging` junto a `scikit-fuzzy`.

## Datos

No hay ficheros de datos: los cinco casos están en el notebook y las funciones de pertenencia se
definen en el propio código.
