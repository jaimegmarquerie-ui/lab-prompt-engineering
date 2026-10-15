# Análisis de fallos de los prompts v1

Modelo: `gemini-3.1-flash-lite-preview` (endpoint compatible con OpenAI), temperature 0.7, max_tokens 500.
Serie de 15 ejecuciones por prompt (15/10/2026, 10:36). Los errores del servidor (503) no se cuentan como respuestas del modelo.

## Resumen

| Prompt v1 | Ejecuciones | Errores 503 | Respuestas reales | Respuestas distintas | Fallo principal |
|---|---|---|---|---|---|
| Sentimiento | 15 | 3 | 12 | 10 | Formato libre: el sentimiento es correcto (Positive en 12 de 12), pero ninguna respuesta es una sola palabra (0 de 12 con el criterio de una palabra) |
| Producto | 15 | 3 | 12 | 12 | Cada respuesta ofrece tres opciones en lugar de una descripción. Longitud 293-325 palabras. Se inventan especificaciones: "2.4" en 11 de 12, "whisper" en 11 de 12, "dpi" en 9 de 12 |
| Extracción | 15 | 0 | 15 | 1 | Ninguno relevante: las 15 respuestas son idénticas y contienen los 4 datos (pedido, fecha, entrega, embalaje) |

Criterio de "válida" en sentimiento: la respuesta, sin espacios ni punto final, es exactamente Positive, Negative o Neutral. Este criterio lo fijamos nosotros: el prompt v1 no pedía ese formato.

## Qué se observa al pasar de 5 a 10 a 15 ejecuciones

Se calcula sobre las primeras 5, 10 y 15 respuestas de la serie de hoy (aproximación: las ejecuciones se lanzaron en paralelo).

| Prompt v1 | 5 ejecuciones (reales / distintas) | 10 ejecuciones (reales / distintas) | 15 ejecuciones (reales / distintas) |
|---|---|---|---|
| Sentimiento | 3 / 3 | 8 / 8 | 12 / 10 |
| Producto | 5 / 5 | 9 / 9 | 12 / 12 |
| Extracción | 5 / 1 | 10 / 1 | 15 / 1 |

## Patrones de fallo por tarea

**Sentimiento (formato inconsistente, contenido consistente).** A veces la respuesta ocupa dos líneas y a veces incluye viñetas con campos como Sentiment, Category e Intent (5 de 12 con viñetas). El prompt no indica opciones de respuesta, ni longitud, ni formato.

**Producto (mucha variación y datos inventados).** El prompt solo da dos datos (ratón inalámbrico, 29,99 $). Cada respuesta añade tres opciones de descripción, cada una con su titular, y rellena los huecos con especificaciones concretas que no se pidieron (2.4 GHz, función "whisper", DPI 800/1200/1600). Los datos vagos (batería "extended", "meses") no se cuentan como especificación.

**Extracción (estable).** Los datos ya están en el texto, así que el modelo los copia. Hipótesis: cuantos más huecos deja el prompt, más rellena el modelo y más varían las respuestas. La estabilidad aquí no es un fallo; es el contraste con las otras dos tareas.

## Limitaciones

- La medida con palabras clave es aproximada y se validó leyendo respuestas a mano.
- Los números de 5 y 10 ejecuciones son un subconjunto de la serie de 15, no tandas nuevas.
- Con temperature 0.7 los resultados pueden variar en otra serie.
