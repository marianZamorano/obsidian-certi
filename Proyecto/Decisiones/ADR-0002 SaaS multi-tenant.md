---
tipo: adr
estado: aceptada
creado: 2026-10-27
tags: [proyecto, arquitectura, adr]
---

# ADR-0002 SaaS multi-tenant

## Contexto

Según el [[Modelo de negocio]], Crecost se ofrece como suscripción a startups de Bolivia y lo opera una sola persona. El costo variable por cliente debe ser muy bajo para llegar al punto de equilibrio (~58 clientes) con costos fijos de Bs 11,333/mes. Las startups pagan planes con distintos módulos habilitados.

## Decisión

1. **Multi-tenant con base de datos compartida:** cada registro de negocio lleva un `empresaId` (tenant). Una sola instancia de la aplicación atiende a todas las startups.
2. **El tenant se resuelve en la capa de Presentación** (a partir del usuario autenticado) y viaja como parte del contexto del caso de uso. **El dominio no sabe de tenants**: sus reglas contables son las mismas para todas las empresas.
3. **Infraestructura filtra siempre por tenant:** los repositorios aplican el filtro `empresaId` de forma obligatoria (y, si el motor lo permite, con seguridad a nivel de fila).
4. **Planes como permisos por módulo:** la capa de Aplicación verifica antes de cada caso de uso si el plan de la empresa incluye el módulo ([[Modelo de dominio]]).
5. **Impuestos, moneda y facturación** (Bolivia) se configuran por empresa y se implementan en Infraestructura, aislados del dominio.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Una base de datos por startup | Multiplica el costo de infraestructura y el mantenimiento (migraciones × N). |
| Una instalación por cliente (on-premise) | Imposible de mantener con un equipo pequeño; las mejoras no llegan a todos. |

## Consecuencias

- ✅ Costo de infraestructura casi fijo hasta cientos de clientes.
- ✅ Una sola versión que mantener; las mejoras llegan a todos a la vez.
- ⚠️ El aislamiento de datos entre empresas es **crítico**: hace falta una prueba automática en cada repositorio que verifique que una empresa nunca ve datos de otra.
- ⚠️ Un cliente muy pesado afecta a los demás: monitorear el uso por tenant (también sirve para la [[Rentabilidad del cliente]]).

Relacionado: [[ADR-0001 Arquitectura en capas]], [[Arquitectura del sistema]].
