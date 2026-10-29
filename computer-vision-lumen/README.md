# Detección de lúmenes en imágenes histológicas

Demo de procesamiento de imágenes: a partir de una fotografía de tejido teñido, separa el tejido del
fondo, aísla los lúmenes (los huecos del tejido), descarta los artefactos y caracteriza el lumen más
grande con trece medidas geométricas.

El notebook se ejecuta de principio a fin y guarda las imágenes intermedias de cada paso, así que se
puede seguir el pipeline viendo qué produce cada transformación.

## Problema

Una imagen histológica mezcla tres cosas: el fondo, el tejido teñido y los lúmenes. Contarlos o
medirlos a ojo no es viable, y un umbral simple sobre la imagen original confunde fondo con lumen.
La demo resuelve el problema en pasos encadenados, cada uno con una decisión explícita.

## Pipeline

1. **Cargar la imagen** histológica y normalizarla.
2. **Transformar a CMYK**: el canal que separa tejido de fondo es más limpio en ese espacio que en RGB.
3. **Umbralizar** para separar los píxeles del fondo de la región tisular.
4. **Limpiar artefactos**: elimina los objetos blancos pequeños que no son lúmenes.
5. **Rellenar el fondo con ceros** para quedarse únicamente con los lúmenes.
6. **Rellenar los objetos** de los lúmenes para recuperar su interior completo.
7. **Detectar y dibujar los contornos** de los lúmenes sobre la imagen original.
8. **Identificar y recortar el lumen más grande** y extraer sus trece características geométricas.

El proceso se repite con una segunda imagen para comprobar que los parámetros no están ajustados a un
solo caso.

## Resultado

El notebook muestra, paso a paso, la imagen resultante de cada transformación y termina con una tabla
de trece medidas del lumen mayor: área, perímetro, excentricidad, ejes, solidez y el resto de
descriptores que entrega `regionprops`.

## Cómo ejecutar

```bash
uv sync
uv run jupyter lab
```

Abre `lumen-detection.ipynb` y ejecútalo de arriba abajo. Las dos imágenes de ejemplo están en esta
misma carpeta, así que no hay que descargar nada.

## Datos

- `histo_1.jpg` y `histo_2.jpg`: dos cortes de tejido teñido con lúmenes de distinto tamaño.

## Notas

`scikit-image` se fija por debajo de 0.26 porque esa versión renombró varios descriptores de
`regionprops` que el notebook usa (`bbox_area`, `convex_area`, `equivalent_diameter`, etc.).
