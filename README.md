# Prompt Engineering Lab

**Track:** Module 2 — Prompt engineering · **When:** Week 2, Day 5 · **Status:** Required

This repository contains everything you need for this lab.

## Files

- [`instructions.md`](./instructions.md) — the lab instructions
- [`rubric.md`](./rubric.md) — how your submission is graded; this is what the AI reviewer checks your PR against

## How to complete this lab

1. **Fork** this repository.
2. Do the work described in `instructions.md`, committing to your fork.
3. Open a **pull request** back into this repository.
4. You'll receive **AI feedback** on your PR based on `rubric.md`. Address any blocking feedback and push updates to the same PR.

## Submission hygiene

- Keep this repository scoped to this lab only — no unrelated projects or personal files.
- Use clear, descriptive filenames.
- Remove secrets, API keys, and tokens before committing.

---

## Mi trabajo en este lab

Se usa **Gemini** en lugar de OpenAI, a través de su endpoint compatible con la librería `openai`, con el modelo `gemini-3.1-flash-lite-preview` (modelo en versión preliminar). Las llamadas se lanzan en paralelo (5 hilos) con reintentos para errores 503 y 429. La clave se guarda en un archivo `.env` local (variable `GEMINI_API_KEY`), que no se sube al repositorio.

### Mapa de archivos

| Archivo o carpeta | Qué contiene |
|---|---|
| `prompt_engineering_lab.ipynb` | Cuaderno con configuración, prompts v1, v2 y v3, ejecuciones y análisis |
| `paso12_13_variaciones_y_evaluacion.ipynb` | Cuaderno de los pasos 12 y 13: variaciones de las tareas y tabla final v1, v2 y v3 |
| `lab_summary.md` | Resumen en un solo párrafo |
| `analisis_fallos_v1.md` | Análisis de fallos de los prompts v1 (5, 10 y 15 ejecuciones) |
| `informe_comparativo_v1_v2_v3.md` | Tabla de consistencia v1, v2 y v3, criterios de respuesta válida y limitaciones |
| `resultados/` | Respuestas guardadas de cada serie en JSON (`sentimiento`, `producto`, `extraccion` en v1, v2 y v3) y `lectura_v1.txt` |
| `instructions.md`, `rubric.md` | Enunciado y rúbrica originales del lab |

### Cómo ejecutarlo

1. Crear un entorno con Python 3.11 e instalar `openai` y `python-dotenv`.
2. Crear un archivo `.env` en la raíz con `GEMINI_API_KEY=` seguido de tu clave (no se incluye en el repositorio).
3. Abrir el cuaderno, ejecutar la celda de configuración y después las celdas de cada versión, de una en una. La tarea de 15 ejecuciones consume cuota de la API.

### Notas

- Los errores del servicio (503 y 429) no se cuentan como respuestas del modelo.
- En producto y extracción, los resultados de 5 y 10 ejecuciones se calcularon sobre las primeras respuestas de la serie de 15, no con tandas nuevas.
- Las primeras celdas del cuaderno (hasta la que prueba modelos candidatos) son la configuración inicial y la selección del modelo. La serie definitiva empieza en la celda de configuración consolidada. No hace falta volver a ejecutar las celdas anteriores a ella.
- Las variaciones del paso 12 usan 5 ejecuciones por entrada, con 2 llamadas en paralelo, para cuidar la cuota gratuita de la API.

