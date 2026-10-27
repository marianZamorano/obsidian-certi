---
tipo: negocio
creado: 2026-10-27
actualizado: 2026-10-27
estado: simulacion
tags: [proyecto, negocio, bolivia]
---

# Modelo de negocio

> [!warning] Simulación
> El sistema no saldrá realmente a producción. Se simula un lanzamiento como SaaS para **startups de Bolivia**, con contacto con dueños de startups reales o simulados. Todas las cifras son **supuestos** para el ejercicio y deben validarse.

**Nombre de trabajo:** *Certi Costos*, un SaaS de contabilidad de costos para startups. Arquitectura en [[Arquitectura del sistema]].

## El problema

Las startups bolivianas suelen llevar la contabilidad **solo para cumplir con Impuestos Nacionales** (facturas, IVA, IT), pero no saben:

- cuánto les cuesta realmente cada producto, servicio o cliente ([[Objeto de costo]]);
- cuánto tienen que vender para no perder ([[Punto de equilibrio]]);
- cuántos meses de caja les quedan con el ritmo de gasto actual;
- si están cumpliendo el plan que presentaron a inversionistas o a una incubadora ([[Presupuesto flexible]]).

Las herramientas existentes son caras, están pensadas para empresas grandes o son hojas de Excel que nadie mantiene.

## Business Model Canvas

| Bloque | Contenido |
|---|---|
| **1. Segmentos de clientes** | **Principal:** startups bolivianas en etapa temprana o de crecimiento (1 a 50 empleados) de Santa Cruz, La Paz y Cochabamba: tecnología, alimentos y bebidas, manufactura liviana y servicios. **Secundario (canal B2B2C):** incubadoras, aceleradoras y universidades con programas de emprendimiento. |
| **2. Propuesta de valor** | "Conoce el costo real de lo que vendes y cuánto necesitas vender para sobrevivir, sin ser contador." Costeo de productos y servicios, punto de equilibrio y margen de seguridad, presupuesto contra realidad con variaciones explicadas en lenguaje simple, rentabilidad por cliente. Todo en bolivianos y adaptado a los impuestos de Bolivia (IVA 13 %, IT 3 %). |
| **3. Canales** | Alianzas con incubadoras y aceleradoras (licencias por cohorte), comunidades de emprendedores y eventos de startups, contenido educativo (talleres "Costea tu startup", plantillas, LinkedIn y TikTok), recomendación entre fundadores. |
| **4. Relación con clientes** | Autoservicio con onboarding guiado, prueba gratis de 30 días, comunidad (grupo de WhatsApp y webinars mensuales) y soporte por chat. Acompañamiento personalizado solo en el plan Escala. |
| **5. Fuentes de ingreso** | Suscripción mensual o anual por planes (ver [[#Planes y precios]]), licencias por cohorte para incubadoras y, más adelante, talleres pagados de formación. |
| **6. Recursos clave** | La plataforma (el dominio contable del [[Libro de Horngren]] implementado en [[Modelo de dominio]]), el equipo (desarrollo y contador), las alianzas con incubadoras y el contenido educativo. |
| **7. Actividades clave** | Desarrollo y mantenimiento de la plataforma, soporte y onboarding, creación de contenido, gestión de alianzas, cumplimiento tributario del propio negocio. |
| **8. Socios clave** | Incubadoras y aceleradoras, universidades, contadores independientes (canal de referidos con comisión), proveedor de nube, bancos o pasarelas de cobro por QR. |
| **9. Estructura de costos** | Costos fijos dominados por el equipo; costos variables bajos por cliente (infraestructura, soporte, comisiones de cobro e impuestos). Detalle en [[#Estructura de costos y punto de equilibrio]]. |

## Planes y precios

**Modelo elegido: suscripción (SaaS) por planes, con prueba gratis de 30 días y sin plan gratuito permanente.**

¿Por qué es el más conveniente para mantener el sistema?

- Los ingresos son **recurrentes y predecibles**, así que cubren costos que también son mensuales (nube y equipo).
- Un plan gratis permanente genera usuarios que **consumen infraestructura y soporte sin pagar**. La prueba de 30 días da el mismo beneficio comercial con un costo acotado.
- Los planes escalonados permiten que la startup **suba de plan cuando crece**, sin cambiar de herramienta.
- La licencia única no financia las mejoras continuas.

Precios en bolivianos, **IVA incluido**. Pago anual = 10 meses (2 de regalo).

| Plan | Precio/mes | Para quién | Incluye | Módulos del dominio |
|---|---|---|---|---|
| **Semilla** | Bs 119 | Startup en validación | 1 empresa, 2 usuarios. Clasificación de costos, costeo de productos o servicios, punto de equilibrio y margen de seguridad. | Clasificación, CVU |
| **Crecimiento** | Bs 299 | Startup con ventas recurrentes | 5 usuarios. Todo lo de Semilla, más presupuesto maestro, presupuesto flexible y variaciones, costeo por órdenes o por procesos, decisiones (fabricar o comprar, pedido especial). | + Costeo, Presupuestos, Variaciones, Decisiones |
| **Escala** | Bs 649 | Startup en expansión o con inversionistas | Usuarios ilimitados, varias empresas o sucursales. Todo lo anterior, más ABC, rentabilidad del cliente, aplicación de costos, reportes para inversionistas, API y acompañamiento mensual. | + Aplicación de costos, ABC |
| **Incubadora** | A convenir por cohorte | Incubadoras y universidades | Cuentas Crecimiento para toda una cohorte durante el programa y un panel para mentores. | — |

> [!nota] Precio desde el valor (aplicando [[Costo meta]])
> Primero se fija el **precio meta**, lo que una startup boliviana pagaría; validarlo en entrevistas. De ahí sale el costo máximo permitido: costo meta = precio meta − utilidad meta. Por eso la arquitectura y la operación se diseñan para que el costo variable por cliente sea muy bajo.

## Estructura de costos y punto de equilibrio

Aplicando [[Costo variable y fijo]] y el [[Análisis costo-volumen-utilidad]] al propio negocio. Cifras mensuales simuladas, en bolivianos.

### Costos fijos mensuales

| Partida | Bs/mes |
|---|---|
| Infraestructura base en la nube (servidor de aplicación, base de datos gestionada, respaldos, monitoreo) | 900 |
| Herramientas, dominio y correo | 300 |
| Desarrollador full-stack | 9,000 |
| Contador y soporte (medio tiempo) | 3,500 |
| Marketing y comunidad | 2,000 |
| **Total costos fijos** | **15,700** |

### Margen de contribución por plan

Costos variables por cliente: impuestos sobre la venta (IVA 13 % + IT 3 %), comisión de cobro (~3 %), infraestructura incremental y soporte.

| Plan | Precio | Neto de IVA e IT | − Cobro, infra y soporte | **Margen de contribución** | Mezcla supuesta |
|---|---|---|---|---|---|
| Semilla | 119 | 100.0 | 23.6 | **76.4** | 50 % |
| Crecimiento | 299 | 251.2 | 49.0 | **202.2** | 35 % |
| Escala | 649 | 545.2 | 109.5 | **435.7** | 15 % |

**Margen de contribución ponderado ≈ Bs 174 por cliente al mes** ([[Margen de contribución]]).

### Punto de equilibrio y metas

- **Punto de equilibrio = 15,700 ÷ 174.3 ≈ 91 startups pagando** ([[Punto de equilibrio]]).
- **Meta del año 1: 150 startups** → utilidad en operación ≈ Bs 10,400/mes, con un **margen de seguridad del 40 %** ([[Margen de seguridad]]).
- **Regla de sostenibilidad:** por encima del punto de equilibrio, el **30 % de la utilidad** va a un fondo de mejoras (nuevas funcionalidades, deuda técnica) y el **20 %** a una reserva de caja de 6 meses de costos fijos.
- El negocio es casi todo **costo fijo** (equipo), así que tiene alto [[Apalancamiento operativo]]: pasado el punto de equilibrio, cada cliente nuevo aporta casi todo su margen a la utilidad, pero por debajo se pierde rápido. Por eso la prioridad es llegar pronto a ~91 clientes.

### Rentabilidad por cliente

Siguiendo [[Rentabilidad del cliente]]: medir el **costo de soporte por cliente**. Si un cliente Semilla consume horas de soporte como uno Escala, destruye margen (la "curva de ballena"). El sistema registra el tiempo de soporte por empresa para detectarlo.

## Cómo el diseño mantiene bajos los costos

Decisiones técnicas que salen directamente de este modelo:

| Decisión | Por qué abarata | Dónde |
|---|---|---|
| **Multi-tenant**: una sola instancia y una base de datos para todas las startups | El costo de infraestructura crece poco por cliente | [[ADR-0002 SaaS multi-tenant]] |
| **Monolito modular en capas** | Un solo despliegue: lo mantiene un equipo pequeño | [[ADR-0001 Arquitectura en capas]] |
| **Planes como "feature flags" por módulo** | Subir de plan habilita módulos sin desarrollo extra | Capa de Aplicación |
| **Dominio puro probado con ejemplos del libro** | Las mejoras no rompen las fórmulas; menos errores en soporte | Capa de Dominio |
| **Cobro por QR y transferencia** (además de tarjeta) | Es el medio de pago más usado en Bolivia y tiene comisiones bajas | Capa de Infraestructura |
| **Onboarding y ayuda dentro del sistema** | Menos tickets de soporte (el principal costo variable) | Capa de Presentación |

## Contexto Bolivia

> [!nota] A verificar
> Datos generales que hay que confirmar con un contador boliviano y con fuentes oficiales antes de usarlos. La situación cambiaria y las normas pueden haber cambiado.

- **Moneda:** precios y reportes en bolivianos (Bs). Los proveedores de nube cobran en dólares; la disponibilidad de divisas y el tipo de cambio son un **riesgo de costo** que hay que monitorear.
- **Impuestos sobre la venta del servicio:** IVA 13 % (incluido en el precio) e IT 3 %; IUE 25 % sobre utilidades.
- **Facturación:** el servicio debe emitir factura electrónica según el sistema de facturación del Servicio de Impuestos Nacionales (SIN). Requiere una integración en Infraestructura.
- **Ecosistema:** startups concentradas en el eje Santa Cruz, La Paz y Cochabamba; incubadoras universitarias y privadas como canal de entrada.

## Plan de simulación con dueños de startups

| Fase | Duración | Qué se hace | Criterio de éxito |
|---|---|---|---|
| 1. Descubrimiento | 2-3 semanas | 10-15 entrevistas con fundadores. ¿Cómo calculan hoy sus costos? ¿Cuánto pagarían? | ≥ 60 % reconoce el problema; precio meta validado |
| 2. Prototipo | 3-4 semanas | Maquetas de las pantallas de costeo y punto de equilibrio; pruebas de usabilidad | Un fundador calcula su PDE solo en < 15 min |
| 3. Piloto (beta) | 6-8 semanas | MVP con 5-10 startups usando sus datos reales, gratis a cambio de retroalimentación | ≥ 5 startups activas cada semana |
| 4. Lanzamiento simulado | — | Activar los precios y medir cuántos pilotos "pagarían" | ≥ 40 % de conversión |

Métricas a seguir: clientes que pagan, MRR (ingreso recurrente mensual), cancelación mensual (*churn*) < 5 %, costo de adquisición (CAC), valor de vida del cliente (LTV) con meta LTV/CAC ≥ 3, horas de soporte por cliente.

### Perfiles de usuario (para las entrevistas)

- **Fundadora de una startup de alimentos (Santa Cruz):** produce por lotes, no sabe cuánto le cuesta cada producto ni a qué precio vender. Necesita costeo por procesos y punto de equilibrio.
- **Fundador de una agencia o software factory (La Paz):** vende proyectos y no sabe qué clientes le dejan ganancia. Necesita costeo por órdenes de trabajo y rentabilidad del cliente.
- **Mentor de una incubadora (Cochabamba):** quiere ver si las startups de su cohorte cumplen su presupuesto. Necesita presupuestos, variaciones y un panel de cohorte.

## Riesgos

| Riesgo | Mitigación |
|---|---|
| Las startups no ven valor en "contabilidad de costos" | Hablar de resultados ("cuánto necesitas vender"), no de contabilidad |
| Baja disposición a pagar | Canal incubadoras (paga la institución); validar el precio en la fase 1 |
| Costos de nube en dólares y riesgo cambiario | Arquitectura frugal y multi-tenant; revisar precios cada 6 meses |
| Cambios en la normativa tributaria | Aislar los impuestos en un módulo de Infraestructura configurable |
| Dependencia de un solo desarrollador | Código en capas con pruebas y documentación en esta bóveda |
