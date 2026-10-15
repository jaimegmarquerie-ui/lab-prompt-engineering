# Comparación v1, v2 y v3

Modelo `gemini-3.1-flash-lite-preview` (endpoint compatible con OpenAI), temperature 0.7, max_tokens 500. 15 ejecuciones por prompt y versión. Los errores del servicio no cuentan como respuestas del modelo y se indican aparte.

## Tabla de consistencia

Consistencia = respuestas válidas ÷ respuestas reales × 100. Una respuesta es válida si cumple los criterios definidos para cada tarea (ver más abajo). Los mismos criterios se aplican a las tres versiones.

| Tarea | v1 | v2 | v3 |
|---|---|---|---|
| Sentimiento | 0 de 12 (0 %), 3 errores 503 | 15 de 15 (100 %) | 15 de 15 (100 %) |
| Producto | 0 de 12 (0 %), 3 errores 503 | 15 de 15 (100 %) | 15 de 15 (100 %) |
| Extracción | Los 4 datos aparecen en 15 de 15, pero sin formato JSON (no se pedía) | 15 de 15 (100 %) | 11 de 11 (100 %), 4 errores 429 (cuota excedida) |

## Criterios de respuesta válida

- **Sentimiento:** la respuesta, sin espacios ni punto final, es exactamente Positive, Negative o Neutral.
- **Producto:** entre 40 y 60 palabras, un solo párrafo sin viñetas, y ningún número aparte de 29.99.
- **Extracción:** JSON legible con exactamente las claves order_number, order_date, delivery y packaging, con 12345 en el pedido y March 15 en la fecha. En v3 el JSON va tras la marca `FINAL JSON:`.

## Qué cambió en cada versión

| Tarea | v1 | v2 | v3 |
|---|---|---|---|
| Sentimiento | Instrucción vaga | Tres opciones, una palabra, sin explicación ni punto | Lo mismo más tres ejemplos (positivo, negativo, neutro) |
| Producto | Instrucción vaga | Un párrafo de 40 a 60 palabras, sin título ni viñetas, solo datos dados | Lo mismo más un ejemplo con otro producto |
| Extracción | Instrucción vaga | JSON con cuatro claves, sin explicación | Lo mismo más razonamiento paso a paso y marca `FINAL JSON:` |

## Patrones de fallo observados en v1

- **Sentimiento:** acierta el sentimiento, pero ninguna respuesta es una sola palabra. Formato libre.
- **Producto:** cada respuesta ofrece tres opciones de unas 300 palabras e inventa especificaciones (2.4 en 11 de 12, whisper en 11 de 12, dpi en 9 de 12).
- **Extracción:** estable en contenido (15 de 15 idénticas), con etiquetas libres.

## Lectura de los resultados

- El salto grande de consistencia ocurre de v1 a v2: instrucciones claras, formato y restricciones.
- En v3 la consistencia no sube porque v2 ya estaba en 100 %. No hay margen medible de mejora con estos criterios.
- En producto v3 no aparecen datos inventados (se buscaron 2.4, whisper, dpi, bluetooth, battery, warranty y similares).
- En extracción v3 el razonamiento paso a paso deja ver cómo se llega al resultado. Es una observación cualitativa, no una medida.

## Limitaciones

- Un solo mensaje de prueba por tarea, claro y sencillo. Un 100 % con un caso fácil no garantiza el mismo resultado con casos ambiguos.
- Los criterios de validez miden el cumplimiento de formato y reglas, no la calidad de la redacción.
- Extracción v3 tuvo 4 errores 429 (cuota gratuita excedida). El 100 % se calcula sobre 11 respuestas reales.
- Los números de 5 y 10 ejecuciones de producto y extracción se calcularon sobre las primeras respuestas de la serie de 15, no con tandas nuevas.
- El paso 12 (variaciones de las tareas) queda pendiente.
