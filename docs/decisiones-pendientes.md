# Decisiones pendientes, supuestos y bloqueos

Extraído del Anexo A del PRD (`docs/prd.md`) y de las notas dispersas en los documentos fuente. Mantener esta lista actualizada a medida que se cierren temas — es la que hay que revisar antes de tomar decisiones de diseño/implementación que dependan de algo "por confirmar".

## Ya resueltas (ronda v0.4, con Renzo)

- El "Aprobador" es el Jefe de Compras, coordinador de área (revisa/interviene/reasigna) — no aprueba comparativas en el MVP.
- Extensión (agregar proveedores) es distinta de 2ª ronda (todos recotizan) — dos mecanismos separados, no confundir.
- Notificaciones al proveedor de cierre, 2ª ronda y recordatorio de vencimiento: pasaron de F2 a MVP.
- El rol "Jefe de Desarrollo de Proveedores" no aplica en el MVP (gestión de proveedores queda en SAP).
- La fecha límite es siempre obligatoria; existe una ventana de reapertura entre el Cierre y la Apertura (nunca después de la Apertura).

## Supuestos por defecto (revisables — hoy se está diseñando/construyendo asumiendo esto)

- Los precios viven dentro del Excel de la oferta, no como campos estructurados del portal (MVP).
- Login del proveedor = cuenta con contraseña + link de invitación como activación inicial (alternativa evaluada: magic link, ver D-09 en el PRD — la industria lo favorece por ser más simple para terceros no técnicos).
- Adjudicación es un split simple entre proveedores, no estructurada por ítem/línea.
- Retención "online" del histórico: ~6 meses (después, archivo frío).
- La Apertura es siempre una acción manual explícita del comprador — nunca ocurre sola/automática.
- La reasignación de casos por parte del Jefe de Compras queda acotada a comprador de su misma área.

## Pendientes de conseguir (materiales)

- Template de look & feel — lo tiene que pasar Leo.
- Template actual de comparativa en Excel — lo tiene que pasar Renzo.
- Documento "06-ux-lookandfeel" y prototipo `mockups/index.html` (5 pantallas), mencionados en el PRD §10 pero no presentes todavía en esta carpeta.
- Documento "05-consideraciones-industria" (fuente del Anexo B), tampoco presente todavía.

## Pendientes [Confirmar con Mathi]

- ¿Los ítems son visibles en el portal (aunque sea de solo lectura), o quedan solo dentro del Excel adjunto?
- ¿Se importa la licitación también desde SOLPED/DEYEL, o el alta queda 100% manual? (afecta RF-01)
- Reglas de negocio "duras" que deben quedar parametrizables sin redeploy — cuáles son, específicamente.
- Dueño de producto (PrMO) definitivo — hoy son tres nombres candidatos: Mathias / Pablo de Luca / Fernanda Abdala.
- Métricas de éxito en detalle (más allá del criterio general "correr una cotización de punta a punta sin mail, con transparencia total").

## Otros puntos abiertos que aparecen en el cuerpo del PRD

- **Sync de proveedores desde SAP:** existe el pipeline LFA1-Sync reusable, pero falta definir mecanismo y frecuencia de refresco (§9).
- **Carga manual de OC en SAP (F2):** falta definir qué exporta el portal para esa carga manual (§9).
- **Stack técnico:** lo decide Chivilo (desarrollo externo); sugerencia de usar herramientas Google. Debe cumplir los requisitos no funcionales (24/7, volumen, aislamiento por fila, búsqueda) — ver PRD §15.
- **Revisor de seguridad:** el aislamiento entre proveedores (CA-07) y en general el modelo de permisos deben auditarse con un revisor de seguridad antes de producción — todavía no asignado/agendado.

## Nuevos pendientes por los requisitos v1 (US-71 a US-75)

Detalle y propuestas en [`kickoff-temas.md`](kickoff-temas.md).

- **2ª ronda parcial (US-72):** ¿las nuevas ofertas vuelven a quedar selladas hasta una nueva Apertura? ¿Tiene fecha límite y auto-close propios? ¿Hace falta justificar la selección?
- **Oferta en texto libre (US-74):** tiene que tratarse como oferta (sellada y versionada), no como mensaje del Q&A. ¿Se permite ofertar sin la planilla Excel?
- **Hash de integridad:** extenderlo de "cada Excel" (RF-27) a todo archivo y al texto de la oferta.
- **Mail de confirmación (US-73):** contenido (sin interpretar el Excel) e idioma.
- **Origen automático desde SAP (US-71):** supone un import desde SAP/SOLPED que el PRD todavía no define.
- **Base de precios (US-75):** en tensión con "los precios viven en el Excel". Supuesto propuesto: planilla estándar con columnas fijas desde el MVP, para poder leerla después.
- **Documentos referenciados que faltan:** `docs/10-arquitectura-sistema-gaps.md` y "doc 09".

## Cómo tratar esto de acá en adelante

Cuando una de estas decisiones se cierre, actualizar este archivo (mover el ítem a "resueltas" con la decisión tomada) y reflejar el cambio en `docs/prd.md` si corresponde. No asumir un valor no confirmado como definitivo en el diseño de datos o permisos sin dejarlo marcado como supuesto.
