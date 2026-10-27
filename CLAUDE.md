# Bóveda de Obsidian "Certi"

Este repositorio es una bóveda de Obsidian. Todo el contenido son notas Markdown.

## Estructura (método PARA)

| Carpeta | Uso |
|---|---|
| `00-Inbox/` | Notas rápidas sin clasificar. Las notas nuevas van aquí. |
| `01-Proyectos/` | Trabajo con objetivo y fecha de fin. |
| `02-Areas/` | Responsabilidades continuas (sin fecha de fin). |
| `03-Recursos/` | Material de referencia y temas de interés. |
| `04-Archivo/` | Proyectos terminados o notas inactivas. |
| `Diario/` | Notas diarias, nombre `AAAA-MM-DD.md`. |
| `Plantillas/` | Plantillas de Obsidian. No guardar notas aquí. |
| `Adjuntos/` | Imágenes, PDFs y otros archivos adjuntos. |

## Convenciones

- Escribe en español.
- Enlaces internos con wikilinks: `[[Nombre de nota]]`, no enlaces Markdown relativos.
- Cada nota empieza con frontmatter YAML:
  ```yaml
  ---
  creado: 2026-10-27
  tags: []
  ---
  ```
- Tags en minúsculas y con guiones: `#certificacion`, `#por-revisar`.
- Al mover o renombrar una nota, actualiza los `[[enlaces]]` que apuntan a ella.
- No edites `.obsidian/` salvo que se pida explícitamente.

## Git

- Haz commit con mensajes cortos en español (p. ej. `notas: añade resumen de módulo 3`).
- No hagas push sin que se pida.
