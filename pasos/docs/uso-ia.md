# Declaración de uso de inteligencia artificial

> Sección a completar por el grupo al finalizar el trabajo.

| Campo | Detalle |
|---|---|
| Herramienta | ChatGPT, Claude y asistente del entorno de desarrollo |
| Fecha de uso | 7 y 8 de noviembre de 2026 |
| Finalidad | Consultas puntuales en la etapa de pruebas, CI y code review (Natalia). |
| Grado de intervención | Medio/bajo. Medio en consultas de configuración (CI y comandos de Git/GitHub); bajo en el resto. La implementación y verificación las hizo el equipo. |
| Verificación humana | Ejecución de pytest, revisión de cobertura, CI en GitHub Actions y control de la documentación antes de subir cambios. |

## Registro de prompts

| # | Fecha y hora | Prompt (textual) | Resultado obtenido | Uso en el trabajo |
|---|---|---|---|---|
| 1 | 2026-11-07 | ¿Cómo armar fixtures en pytest para probar un servicio sin tocar el disco? | Ejemplos de fake de repositorio y mock de notificador. | Se adaptó a `conftest.py` y pruebas de servicio/CLI. |
| 2 | 2026-11-07 | Ejemplo de GitHub Actions con ruff y pytest, cobertura de ramas mínima 80 %. | Esquema de workflow. | Se ajustó al repo y se validó en Actions. |
| 3 | 2026-11-07 | Me trabé con Git: ¿cómo hago add, commit, tag y push a main sin pisar el trabajo del grupo? | Secuencia de comandos y cuidados con `pull` si el push es rechazado. | Se usó como guía al subir commits y el tag `etapa-pruebas`. |
| 4 | 2026-11-08 | Riesgo de guardar un JSON sobrescribiendo el archivo directo y cómo hacerlo de forma más segura. | Explicación de escritura atómica (temporal + replace). | Se usó para corregir OBS-04 y sumar pruebas. |
| 5 | 2026-11-08 | Diferencia entre prueba de caracterización (golden master) y pruebas unitarias en un legacy. | Criterio de cuándo usar cada una. | Apoyó la explicación de por qué se mantuvo el golden master y se ampliaron unitarias. |
| 6 | 2026-11-08 | Qué conviene revisar en un checklist de code review para un proyecto Python pequeño. | Lista de ítems (diseño, errores, pruebas, estilo). | Referencia al completar `docs/code-review.md`. |