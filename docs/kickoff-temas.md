# Kickoff — temas a cerrar

Reunión de inicio formal del proyecto (Fernanda, Leonardo, Pablo, Mathi + Digito).

> **La lista completa y actualizada de preguntas, riesgos y decisiones técnicas, con las respuestas de la reunión, está en el artefacto de seguimiento:** https://claude.ai/artifact/9MTH9ydmHrM6kPP6JeFeex. Este archivo queda como resumen inicial. Base: PRD v0.4 + RF-39..43 y backlog v1 (75 historias) en [`v1/`](v1/).

## 1 · Qué cambió en v1

Solo se **agregaron** 5 historias; ninguna existente cambió de texto ni categoría.

| Nuevo | RF | Qué pide | Fase |
|---|---|---|---|
| US-72 | RF-39 | 2ª ronda **parcial**: el comprador elige un sub-conjunto de los que cotizaron; los demás mantienen su oferta vigente. No reemplaza la 2ª ronda total. | MVP |
| US-73 | RF-40 | Checkbox del proveedor al enviar la oferta → mail de confirmación con fecha/hora, detalle y adjuntos. | MVP |
| US-74 | RF-41 | El proveedor puede responder con **PDF, Excel o texto libre** en el historial del caso. | MVP |
| US-71 | RF-42 | Licitaciones que llegan automáticamente desde SAP y no ameritan proceso formal → se cierran solas o no se generan. Criterios sin definir. | V2 |
| US-75 | RF-43 | Registrar **todos** los precios cotizados (no solo el adjudicado) → base unificada de precios de mercado. | V2 |

MVP: 42 → 45 historias. V2: 11 → 13.

## 2 · Preguntas sobre los requisitos nuevos

**2ª ronda parcial (US-72)**
- Ahora hay tres mecanismos para "pedir de nuevo": Reconsulta (antes del Cierre, no obliga), 2ª ronda parcial (después de la Apertura, obliga a los elegidos) y 2ª ronda total (obliga a todos). ¿Confirmamos esos tres y esos nombres?
- Las nuevas ofertas de la ronda parcial, ¿vuelven a quedar selladas hasta una nueva Apertura, o el comprador las ve a medida que llegan? (El comprador ya vio los precios de la ronda 1.)
- ¿Lleva fecha límite propia y auto-close propio? ¿Puede haber 3ª ronda parcial?
- Los no elegidos: ¿se enteran de algo? (Propuesta: no; aislamiento.)
- ¿Hace falta una justificación de por qué se eligió a esos proveedores (equidad/auditoría)?

**Mail de confirmación (US-73)**
- El criterio dice "ítems cargados", pero los ítems viven dentro del Excel. Propuesta: el mail lleva nº QR, versión, fecha/hora, lista de adjuntos con su hash y el texto libre si lo hubo, sin interpretar el Excel. ¿OK?
- Idioma del mail: ¿el que eligió el proveedor en el portal (ES/EN)?

**Oferta en PDF / Excel / texto libre (US-74)**
- "Historial del caso" suena a chat/Q&A. La oferta en texto libre **tiene que quedar sellada** hasta la Apertura, igual que un archivo; no puede quedar visible como un mensaje del Q&A. Propuesta: es una oferta (versionada, sellada) con tres formatos posibles, separada del Q&A. ¿Confirmamos?
- ¿Se puede ofertar solo con PDF o solo con texto, sin el Excel de la planilla? Si sí, se pierde la uniformidad para comparar y para capturar precios (RF-26 / US-75).
- El hash de integridad (RF-27) hoy dice "por cada Excel": propuesta de extenderlo a todo archivo y al texto de la oferta.

**Origen automático desde SAP (US-71)**
- Da por hecho que las licitaciones pueden llegar solas desde SAP, pero en el PRD el origen sigue "a definir, hoy manual". ¿Hay import desde SOLPED/SAP previsto? ¿En qué fase?
- El Excel cita `docs/10-arquitectura-sistema-gaps.md` y el PRD un "doc 09": no los tenemos. Pedirlos.

**Base de precios de mercado (US-75)**
- Choca con el supuesto de que "los precios no son campos, viven en el Excel" y con que la carga ítem por ítem (US-19) está fuera. Para que sea viable a futuro, el MVP debería imponer una **planilla estándar** (columnas fijas: código, descripción, cantidad, unidad, precio unitario, moneda, plazo) que después se pueda leer automáticamente. ¿Quién la define? ¿Renzo, a partir del template de comparativa?

## 3 · Temas que siguen abiertos desde v0.4

- **Quién desarrolla.** El PRD sigue nombrando a Chivilo. Las decisiones técnicas las toma Digito (German); a Batfer solo se le confirma la restricción de que código y datos vivan en su GCP.
- **Login del proveedor:** usuario + contraseña o magic link (D-09).
- **Área/Región vs Unidad de negocio:** cómo se asigna cada comprador y qué ve cada Jefe. Si existe el Jefe General en el piloto.
- **Reabrir:** según el PRD lo hace el Jefe de Compras, según US-69 el Comprador. ¿Los dos?
- **Anexo B vs backlog:** D-04 (Q&A anonimizado para todos) y D-05 (adjudicación por línea) figuran como MVP en el Anexo B, pero US-18 y US-30 están Fuera. Confirmar que quedan fuera.
- **Sync de proveedores desde SAP (LFA1-Sync):** cómo y cada cuánto.
- **Ítems visibles en el portal o solo en el Excel.**
- **Retención:** ~6 meses online y después archivo frío.
- **Dueño de producto definitivo** (hoy figuran Mathias / Pablo / Fernanda) y **revisor de seguridad** para CA-07.
- **Materiales pendientes:** template de look & feel (Leo), template de comparativa (Renzo), documentos 05, 06, 09 y 10.

## 4 · Próximos pasos propuestos

1. Cerrar las respuestas de este documento y dejarlas en `decisiones-pendientes.md`.
2. Definir arquitectura (app + automatizaciones + base + storage) y modelo de datos, ya con rondas parciales, oferta en tres formatos y hash por archivo.
3. Estimar el backlog (la columna "Estimación" del Excel está vacía) y armar el plan de sprints.
4. Opcional: sumar al mock de Lovable la 2ª ronda parcial, el checkbox de confirmación y la oferta en texto/PDF para validarlos visualmente con Compras.
