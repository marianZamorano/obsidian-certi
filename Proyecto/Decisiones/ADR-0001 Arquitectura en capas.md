---
tipo: adr
estado: aceptada
creado: 2026-10-27
tags: [proyecto, arquitectura, adr]
---

# ADR-0001 Arquitectura en capas

## Contexto

El sistema implementa reglas de contabilidad de costos ([[Contabilidad de costos]]) que son estables: vienen del [[Libro de Horngren]] y cambian poco. En cambio, la interfaz, la base de datos y las integraciones sí pueden cambiar. Además, las reglas deben poder verificarse contra los ejemplos numéricos del libro.

## Decisión

Usar una **arquitectura en capas** (Presentación → Aplicación → Dominio, con Infraestructura implementando interfaces) desplegada como **monolito modular**, con los módulos de [[Modelo de dominio]].

Reglas obligatorias (ver también `CLAUDE.md`):

1. Las dependencias de código apuntan hacia el dominio.
2. Las fórmulas contables viven solo en el dominio.
3. Dinero con decimales, nunca con punto flotante.
4. Cada regla del dominio tiene una prueba basada en un ejemplo del libro.

## Alternativas consideradas

| Alternativa | Por qué no (por ahora) |
|---|---|
| Arquitectura sin capas (MVC con lógica en controladores) | Mezcla las fórmulas con la UI y la BD; difícil de probar contra el libro. |
| Microservicios por módulo | Complejidad operativa innecesaria al inicio; los módulos comparten el núcleo de clasificación de costos. |

## Consecuencias

- ✅ Las reglas contables se prueban sin base de datos ni UI.
- ✅ Se puede cambiar el framework o la BD sin tocar el dominio.
- ✅ El monolito modular permite separar un módulo más adelante si hace falta.
- ⚠️ Más archivos y mapeos (entidades ↔ DTOs ↔ tablas) que un CRUD simple.
- ⚠️ Hace falta disciplina para no saltarse capas; conviene una prueba automática de dependencias entre capas.

Relacionado: [[Arquitectura del sistema]].
