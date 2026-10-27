---
tipo: adr
estado: aceptada
creado: 2026-10-27
tags: [proyecto, arquitectura, adr, stack]
---

# ADR-0003 Stack React, NestJS y PostgreSQL

## Contexto

Crecost lo desarrolla y mantiene **una sola persona con Claude** ([[Modelo de negocio]]). El stack debe:

- permitir respetar las capas de [[ADR-0001 Arquitectura en capas]] y el multi-tenant de [[ADR-0002 SaaS multi-tenant]];
- minimizar la cantidad de tecnologías que una persona tiene que dominar;
- ser muy conocido, para que Claude genere y revise código con confianza.

## Decisión

**TypeScript de punta a punta:** React en el front, NestJS en el back y PostgreSQL como base de datos, en un **monorepo**.

```
crecost/
├── apps/
│   ├── web/                  ← React (capa de Presentación: UI)
│   └── api/                  ← NestJS
│       └── src/modulos/<modulo>/
│           ├── presentacion/     controladores y DTOs de entrada (validación de formato)
│           ├── aplicacion/       casos de uso y puertos (interfaces de repositorios)
│           └── infraestructura/  repositorios con ORM, facturación SIN, cobros QR
└── packages/
    └── dominio/              ← TypeScript puro (capa de Dominio)
        └── src/<modulo>/         entidades, objetos de valor y fórmulas del libro
```

Los `<modulo>` son los del [[Modelo de dominio]]: `clasificacion`, `costeo`, `presupuestos`, `variaciones`, `decisiones`, `aplicacion-costos`.

### Por capa

| Capa | Tecnología | Reglas |
|---|---|---|
| Presentación (web) | React + Vite + TypeScript, TanStack Query (datos del servidor), React Hook Form + Zod (formularios), Tailwind CSS, gráficos para punto de equilibrio y variaciones | Sin fórmulas contables: solo muestra lo que calcula el back |
| Presentación (api) | Controladores NestJS, `class-validator` en los DTOs, guard de autenticación que resuelve el `empresaId` | Un controlador llama a un caso de uso y nada más |
| Aplicación | Servicios NestJS (un caso de uso por clase), guard de plan por módulo | Depende de `@crecost/dominio` y de interfaces (puertos), nunca del ORM |
| Dominio | `packages/dominio`: TypeScript **sin** NestJS, sin ORM, sin decoradores. Única dependencia: una librería de decimales para `Dinero` | Las fórmulas del libro viven solo aquí |
| Infraestructura | ORM con migraciones (propuesta: Prisma), PostgreSQL | Mapea tablas ↔ entidades del dominio; siempre filtra por `empresaId` |

### PostgreSQL

- Dinero en `NUMERIC(18,4)`, nunca `float`/`real`.
- Columna `empresa_id` en todas las tablas de negocio, más **Row-Level Security** como segunda barrera del aislamiento entre startups.
- Un solo servidor gestionado con respaldos automáticos diarios.

### Garantías automáticas (en CI)

1. **Regla de dependencias entre capas** verificada con una herramienta de análisis de imports (p. ej., `dependency-cruiser`): el build falla si `dominio` importa NestJS o el ORM, o si un controlador importa un repositorio.
2. **Pruebas del dominio** con los ejemplos del libro (p. ej., O'Shea en [[Variación en precio y en eficiencia]]).
3. **Prueba de aislamiento multi-tenant** por repositorio, contra un PostgreSQL real de prueba.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Express sin framework | Habría que construir a mano los módulos, la inyección de dependencias y la validación que NestJS ya trae. |
| Next.js full-stack (front y back juntos) | Tienta a poner lógica de negocio en el front o en las rutas, lo que rompe las capas. |
| Back en otro lenguaje (Java, C#, Python) | Dos lenguajes para una sola persona; no se comparten tipos entre front y back. |
| MongoDB | Los datos contables son relacionales y necesitan transacciones y decimales exactos. |

## Consecuencias

- ✅ Un solo lenguaje y tipos compartidos (los DTOs pueden generarse para el front).
- ✅ Los módulos de NestJS encajan con los módulos del dominio.
- ✅ El dominio en un paquete aparte hace **imposible** importar NestJS o el ORM sin que se note.
- ⚠️ Más mapeos (entidad ↔ fila ↔ DTO) que un CRUD directo con el ORM: es el precio de mantener las capas limpias.
- ⏳ Pendiente: autenticación, hosting y CI/CD (próximos ADR).

Relacionado: [[Arquitectura del sistema]].
