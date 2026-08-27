# Portal de Cotizaciones — Batfer

## Qué es esto

Plataforma interna para que Batfer gestione **cotizaciones a proveedores** (RFQ) sin depender de mail suelto ni de contratar SAP Ariba. Hoy el intercambio con proveedores (sobre todo del exterior/China) es informal por mail: no hay trazabilidad, no hay transparencia real (nadie garantiza que las ofertas quedaron "selladas" hasta el cierre) y consultar el histórico es un lío. El portal traslada ese intercambio comprador↔proveedor a una plataforma propia, regulando el proceso de licitación paso a paso.

**Lo que el portal NO hace:** no arma el pedido de compra, no gestiona la orden de compra ni el pago — eso sigue en SAP. Tampoco arma la comparativa/aprobación/memo en el MVP (eso es Fase 2); en el MVP esas cosas se siguen haciendo fuera, en SAP.

**Piloto:** Unidad Batfer. **Sponsor:** Leo Spina.

## Estado actual del proyecto

Estamos en la **etapa de definición de producto** — todavía no hay código. Esta carpeta contiene los documentos fuente que German (softechv.com) trajo para arrancar: un PRD, un desglose de requisitos por categoría, y el backlog completo de historias de usuario. Antes de escribir una sola línea de código conviene tener claro el alcance del MVP (ver abajo) y las [decisiones que todavía están abiertas](docs/decisiones-pendientes.md).

**Nota sobre desarrollo:** el PRD dice que el desarrollo lo hace "Chivilo" (equipo externo, contacto Vázquez) y que el stack lo decide ese equipo (sugerencia: herramientas Google). Si quien está trabajando en esta carpeta es parte de ese desarrollo, buena señal; si no, vale la pena confirmar con quien corresponda (Mathi / Leo) cómo se coordina este trabajo con lo que arma Chivilo, para no duplicar ni pisar decisiones de stack.

## Qué se va a construir acá

Según lo conversado: una **app** (el portal en sí — la interfaz de comprador y la de proveedor), un **set de automatizaciones** (notificaciones por mail, auto-close por fecha límite, sync de proveedores desde SAP, recordatorios, etc.) y posiblemente una **base de datos particular** para sostener el modelo de datos del §5 del PRD (casos, ofertas, archivos, auditoría). Todavía no se definió la arquitectura concreta — eso es lo próximo a decidir una vez que el alcance esté claro.

## Mapa de documentos

| Archivo | Contenido |
|---|---|
| [`docs/prd.md`](docs/prd.md) | **PRD completo** (v0.4) — la referencia principal: resumen ejecutivo, glosario, roles, hitos de la licitación, modelo de datos, requisitos funcionales y no funcionales, auth, integraciones, UI, notificaciones, criterios de aceptación, sprints propuestos. |
| [`docs/historias-usuario.md`](docs/historias-usuario.md) | Las 70 historias de usuario del backlog (42 MVP / 11 Versión 2 / 17 fuera), con criterio de aceptación y dependencias entre historias. Espejo en Markdown de `Historias-de-Usuario.xlsx` (que sigue siendo la fuente autoritativa por sus columnas de prioridad/estimación). |
| [`docs/decisiones-pendientes.md`](docs/decisiones-pendientes.md) | Qué está resuelto, qué es un supuesto revisable, y qué falta confirmar con Mathi/Leo/Renzo antes de dar algo por sentado. **Revisar esto antes de tomar decisiones de diseño de datos o de permisos.** |
| [`docs/lovable-mockup-prompt.md`](docs/lovable-mockup-prompt.md) | Prompt listo para pegar en Lovable: genera un mockup navegable de presentación (datos mock, Supabase para auth/DB) con la página de bienvenida/flujo/tareas pedida para pitchear el producto antes de construir el sistema real. |
| `PRD-Portal-Cotizaciones.md` / `PRD-Portal-Cotizaciones (1).md` | Fuentes originales (Word→md) del PRD. El segundo es un duplicado con errores de OCR — `docs/prd.md` ya los consolida a ambos; estos dos quedan como respaldo crudo. |
| `Requisitos-por-Categoria.md` / `.xlsx` | Las mismas 70 historias organizadas por categoría (🟢 Mínimo / 🔵 Versión 2 / ⚪ Fuera), en formato para que Compras deje comentarios. Contenido ya reflejado en `docs/historias-usuario.md`. |
| `Historias-de-Usuario.xlsx` | Backlog completo con columnas adicionales (Prioridad MoSCoW, Estimación, Notas) — fuente autoritativa del backlog. |

## Lo esencial del producto (resumen ultra-corto)

**Flujo:** Creación (borrador, fecha límite obligatoria) → Lanzamiento (invita proveedores desde SAP) → proveedores cotizan → Cierre (manual o auto-close) → [ventana de Reapertura si falta alguien] → Apertura (manual, recién ahí se ven precios) → Comparativa (fuera del portal en el MVP) → Adjudicación (puede ser parcial, la OC se hace en SAP).

**Dos mecanismos que no hay que confundir:**
- **Extensión** (antes del Cierre): sumar un proveedor nuevo, sin precios visibles, sin forzar a recotizar a los que ya cotizaron.
- **2ª ronda** (después de la Apertura, opcional): TODOS los proveedores que participaron deben volver a cotizar (BAFO); la oferta anterior queda versionada.

**No negociable:** un proveedor nunca ve otros invitados, sus precios, la comparativa ni la adjudicación — aislamiento a nivel de fila en el backend, no solo ocultar en el frontend. Las ofertas quedan **selladas** hasta la Apertura. Nunca se pisa una oferta: cada re-subida es una versión nueva.

**Roles:** Comprador (sus casos) · Jefe de Compras — coordinador de área (todos los casos de su área, reabre/cancela/reasigna/interviene) · Jefe General de Compras (cruza áreas) · Admin (config/usuarios) · Proveedor externo (solo sus propias QR-Proveedor). Login interno con Google Workspace @batfer.net; proveedores externos con cuenta+contraseña (magic link es una alternativa en evaluación, ver Anexo B del PRD).

## Alcance: qué entra en el MVP y qué no

- **MVP:** todo el ciclo de vida de la licitación (creación → adjudicación), invitación de proveedores desde SAP, Q&A y reconsultas dentro del portal, sellado y aislamiento, versionado de ofertas, notificaciones básicas por mail, repositorio/búsqueda del histórico (~6 meses "caliente"), auditoría completa, hash de integridad por Excel.
- **Fase 2:** comparativa dentro del portal (armar/aprobar/exportar/memo GEM), conexión con SAP para adjudicación/OC, normalización de moneda, búsqueda dentro de archivos, bajadas masivas, 2FA, carga masiva de casos.
- **Fuera de alcance (por ahora):** autogestión/onboarding de proveedores (eso lo maneja SAP), adjudicación ítem-por-ítem, scoring ponderado, TCO/landed cost, subastas inversas, envelope bidding, dashboard de KPIs, digests automáticos, reglas de negocio editables sin IT, integración de escritura con SAP, app mobile nativa.

Ver el detalle completo, requisito por requisito, en [`docs/prd.md` §6](docs/prd.md#6--requisitos-funcionales) y en [`docs/historias-usuario.md`](docs/historias-usuario.md).

## Antes de empezar a construir

1. Revisar [`docs/decisiones-pendientes.md`](docs/decisiones-pendientes.md) — hay varios supuestos (login de proveedor, retención de datos, adjudicación simple vs. por ítem) que convendría confirmar antes de fijarlos en el modelo de datos.
2. Definir arquitectura concreta (app + automatizaciones + base de datos) — el PRD sugiere "herramientas Google" pero la decisión de stack está abierta.
3. Conseguir los materiales pendientes: template de look & feel (Leo), template de comparativa en Excel (Renzo), y los documentos "06-ux-lookandfeel" / "05-consideraciones-industria" / `mockups/index.html` que el PRD referencia pero que todavía no están en esta carpeta.
