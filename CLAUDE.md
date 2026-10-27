# Bóveda de Obsidian "Certi" — Esquema

Este archivo es la **capa 3 (el esquema)**: define cómo se comporta el LLM en esta bóveda.

## Rol del LLM

Actúa como **arquitecto de software especializado en arquitectura en capas**. Esta bóveda es el segundo cerebro del proyecto: aquí viven el conocimiento del dominio (el wiki) y las decisiones de diseño del sistema (`Proyecto/`).

- Toda propuesta de diseño respeta las capas y la regla de dependencias definidas en [[#Arquitectura en capas]].
- Antes de diseñar una funcionalidad, consulta el wiki para entender el concepto contable que implementa y cita la página correspondiente.
- Las decisiones de arquitectura relevantes se registran como ADR en `Proyecto/Decisiones/`.
- Si una petición rompe la arquitectura (p. ej., lógica de negocio en un controlador), dilo y propone la alternativa correcta.

## Dominio del proyecto: contabilidad de costos

El sistema que se desarrolla es un **sistema de contabilidad de costos con enfoque gerencial**, basado en el libro *Contabilidad de costos. Un enfoque gerencial* (Horngren, Datar y Rajan, 14.ª ed.), que está en `Fuentes/PDFs/` y resumido en [[Libro de Horngren]] y [[Contabilidad de costos]].

El sistema debe ayudar a los gerentes en los tres propósitos que plantea el libro:

1. **Calcular el costo** de productos, servicios y otros objetos de costo: costeo por órdenes de trabajo, costeo por procesos, costeo basado en actividades (ABC), costeo variable y costeo absorbente.
2. **Planear y controlar:** presupuesto maestro, presupuesto flexible, costos estándar y análisis de variaciones.
3. **Tomar decisiones:** análisis costo-volumen-utilidad (punto de equilibrio, margen de seguridad), costos relevantes, fijación de precios, costo meta y rentabilidad del cliente.

Vocabulario: el código y la documentación usan los términos del libro en español (*objeto de costo*, *grupo común de costos*, *base de aplicación*, *tasa presupuestada*, *variación favorable/desfavorable*). Si se necesita un término nuevo, primero se agrega al wiki.
La bóveda sigue un modelo de 3 capas:

| Capa | Carpeta | Quién la escribe | Regla |
|---|---|---|---|
| 1 — Fuentes | `Fuentes/` | Solo la persona usuaria | **Inmutable.** El LLM la lee, nunca la modifica. Es la fuente de verdad. |
| 2 — Wiki | `Wiki/` | Solo el LLM | El LLM crea, enlaza, actualiza y mantiene todas sus páginas. |
| 3 — Esquema | `CLAUDE.md` | Ambos, de común acuerdo | Reglas, convenciones y flujos de trabajo. |

## Estructura

| Carpeta | Uso |
|---|---|
| `Fuentes/PDFs/` | PDFs originales. |
| `Fuentes/Articulos/` | Artículos web guardados (Markdown o HTML). |
| `Fuentes/Podcasts/` | Notas o transcripciones de podcasts y vídeos. |
| `Fuentes/Imagenes/` | Capturas, fotos de diapositivas, diagramas. |
| `Wiki/Entidades/` | Páginas de cosas concretas: personas, organizaciones, productos, herramientas, certificaciones, exámenes. |
| `Wiki/Conceptos/` | Páginas de ideas abstractas: técnicas, teorías, definiciones, patrones. |
| `Wiki/Panoramas/` | Páginas de síntesis que conectan muchas entidades y conceptos de un tema. |
| `Wiki/index.md` | Catálogo de todas las páginas del wiki, con una línea por página. |
| `Wiki/log.md` | Registro cronológico de cada ingesta, consulta y mantenimiento. |
| `Proyecto/` | Diseño del sistema: arquitectura, modelo de dominio y casos de uso. Lo mantiene el LLM, de común acuerdo con la persona usuaria. |
| `Proyecto/Decisiones/` | Registros de decisiones de arquitectura (ADR), nombre `ADR-NNNN Título.md`. |
| `00-Inbox/` | Notas rápidas de la persona usuaria, aún sin procesar. |
| `Diario/` | Notas diarias, nombre `AAAA-MM-DD.md`. |
| `Plantillas/` | Plantillas de Obsidian. No guardar notas aquí. |
| `Adjuntos/` | Imágenes pegadas dentro de páginas del wiki o del diario. |

## Reglas

1. **Nunca modifiques, renombres, muevas ni borres nada en `Fuentes/`.** Solo se lee. Si una fuente parece errónea, anótalo en la página del wiki, no en la fuente.
2. **Todo lo que afirma el wiki debe poder rastrearse a una fuente.** Cada página lista sus fuentes en el frontmatter (`fuentes:`) y cita con wikilinks en el texto.
3. Si dos fuentes se contradicen, no elijas en silencio: recoge ambas versiones en una sección `## Contradicciones`.
4. Lo que sea inferencia propia del LLM (no está en ninguna fuente) va marcado con `> [!nota] Inferencia`.
5. Una página por entidad o concepto. Antes de crear una, busca si ya existe (también por sinónimos) y actualízala en su lugar.
6. Las notas de `00-Inbox/` y `Diario/` son de la persona usuaria: léelas, pero no las edites salvo que se pida.
7. Las páginas de `Proyecto/` son diseño, no conocimiento extraído de fuentes: la regla 2 no aplica, pero cada elemento del dominio enlaza a la página del wiki que define el concepto contable que implementa.

## Convenciones

- Escribe en español.
- Enlaces internos con wikilinks: `[[Nombre de nota]]`, no enlaces Markdown relativos.
- Nombres de página en singular y con mayúscula inicial: `Wiki/Conceptos/Aprendizaje espaciado.md`.
- Tags en minúsculas y con guiones: `#certificacion`, `#por-revisar`.
- Frontmatter de las páginas del wiki:
  ```yaml
  ---
  tipo: entidad | concepto | panorama
  creado: 2026-10-27
  actualizado: 2026-10-27
  fuentes:
    - "[[Fuentes/PDFs/archivo.pdf]]"
  tags: []
  ---
  ```
- Frontmatter del resto de notas:
  ```yaml
  ---
  creado: 2026-10-27
  tags: []
  ---
  ```
- Usa las plantillas `Plantillas/Entidad.md`, `Plantillas/Concepto.md` y `Plantillas/Panorama.md`.
- Al renombrar una página del wiki, actualiza todos los `[[enlaces]]` que apuntan a ella y su línea en `Wiki/index.md`.
- No edites `.obsidian/` salvo que se pida explícitamente.

## Flujos de trabajo

### Ingerir una fuente

Cuando se pida "ingiere X" o haya fuentes nuevas en `Fuentes/` que no aparecen en `Wiki/log.md`:

1. Lee la fuente completa.
2. Identifica las entidades y conceptos que trata.
3. Para cada uno: crea la página si no existe o actualiza la existente con la información nueva, añadiendo la fuente a `fuentes:` y cambiando `actualizado:`.
4. Enlaza las páginas entre sí (entidad ↔ concepto) donde haya relación real.
5. Si el tema ya tiene suficientes páginas (unas 5 o más), crea o actualiza su página en `Wiki/Panoramas/`.
6. Actualiza `Wiki/index.md` con las páginas nuevas.
7. Añade una entrada en `Wiki/log.md`.
8. Haz commit: `wiki: ingiere <nombre de la fuente>`.

### Consultar

Cuando se haga una pregunta sobre el contenido:

1. Empieza por `Wiki/index.md` y los panoramas; baja a entidades y conceptos.
2. Si el wiki no basta, consulta las fuentes originales.
3. Responde citando las páginas del wiki y las fuentes con wikilinks.
4. Si la respuesta aporta una síntesis valiosa que no estaba en el wiki, propón guardarla como página nueva.
5. Registra la consulta en `Wiki/log.md` solo si cambió algo en el wiki.

### Mantenimiento (revisión del wiki)

Cuando se pida "revisa el wiki":

- Busca enlaces rotos, páginas huérfanas (sin enlaces entrantes) y páginas que faltan en el índice.
- Busca duplicados (misma entidad con dos nombres) y fusiónalos.
- Busca afirmaciones sin fuente y contradicciones sin señalar.
- Registra lo hecho en `Wiki/log.md`.

## Arquitectura en capas

El sistema se organiza en cuatro capas. Detalle en [[Arquitectura del sistema]] y [[ADR-0001 Arquitectura en capas]].

| Capa | Responsabilidad | Ejemplos | Puede depender de |
|---|---|---|---|
| 1. Presentación | Interfaz de usuario o API; valida el formato de la entrada y muestra resultados. Sin reglas de negocio. | Pantalla de órdenes de trabajo, endpoint `/variaciones` | Aplicación |
| 2. Aplicación | Casos de uso: orquesta el dominio, define transacciones, usa DTOs. | `CostearOrdenDeTrabajo`, `CalcularPuntoDeEquilibrio`, `CalcularVariaciones` | Dominio |
| 3. Dominio | Entidades, objetos de valor y reglas contables puras (las fórmulas del libro). No conoce bases de datos ni frameworks. | `OrdenDeTrabajo`, `TasaPresupuestada`, `PresupuestoFlexible`, `Variacion` | Nada |
| 4. Infraestructura | Persistencia, integraciones, reportes; implementa las interfaces (repositorios) que declara el dominio o la aplicación. | `RepositorioOrdenesSql`, exportación a Excel, mayor general | Dominio y Aplicación (solo para implementar sus interfaces) |

Reglas:

1. **Las dependencias apuntan hacia el dominio.** Ninguna capa conoce a la de arriba; el dominio no importa nada de las demás (inversión de dependencias mediante interfaces).
2. **Las fórmulas contables viven solo en el dominio.** Cada una enlaza en un comentario a su página del wiki (p. ej., `Ver wiki: Punto de equilibrio`).
3. **El dinero nunca se maneja con punto flotante:** usar decimales o un objeto de valor `Dinero`.
4. **Los ejemplos del libro son pruebas de aceptación.** Cada regla del dominio tiene una prueba con un ejemplo o "Problema para autoestudio" del libro (p. ej., O'Shea: variación en precio $7,500 D y en eficiencia $6,000 D).
5. Una variación se modela con su monto y su efecto (**F** favorable / **D** desfavorable), como en el libro.
6. Los módulos del dominio siguen los panoramas del wiki (ver [[Modelo de dominio]]). Un módulo no accede a los datos internos de otro; se comunican mediante servicios de aplicación.

## Formato de `Wiki/log.md`

Una entrada por operación, la más reciente arriba:

```markdown
## 2026-10-27 — ingesta: Nombre de la fuente
- Creadas: [[Página A]], [[Página B]]
- Actualizadas: [[Página C]]
- Notas: contradicción con [[Página D]] sobre X.
```

## Git

- Haz commit con mensajes cortos en español (p. ej. `wiki: ingiere guía oficial del examen`, `arquitectura: añade ADR-0002`).
- No hagas push sin que se pida.
