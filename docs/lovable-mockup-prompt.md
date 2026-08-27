# Prompt para Lovable — Mockup de presentación del Portal de Cotizaciones

> Este prompt está pensado para pegarse directo en Lovable. Genera un **mockup navegable de presentación** (no el producto final) basado en `docs/prd.md` y `docs/historias-usuario.md`. Si Lovable ajusta detalles de layout/copy al construir, no pasa nada — lo importante es que respete el flujo, los roles y las reglas de negocio "no negociables" del PRD.

---

## Prompt

```
Quiero construir un MOCKUP navegable (no el producto final, es para presentar internamente y conseguir aprobación) de un "Portal de Cotizaciones" — una plataforma interna B2B para que el área de Compras de una empresa (Batfer) gestione licitaciones/cotizaciones a proveedores sin usar mail suelto. Reemplaza intercambios informales por mail por un flujo regulado, transparente y trazable. NO es un e-commerce ni un dashboard genérico: es una herramienta operativa tipo "e-sourcing" liviano (alternativa interna a SAP Ariba).

Usá Supabase para autenticación y base de datos (lo necesito conectado, aunque todos los datos van a ser mock/ficticios — esto es una demo, no un sistema en producción real). Poblá la base con datos semilla (seed data) realistas para que la demo se vea viva, no vacía.

## 0. Cómo debe sentirse este mockup

Es para una presentación a stakeholders (sponsor y gerencia de Compras) antes de construir el sistema real. Tiene que verse pulido y profesional, simular una herramienta enterprise real, pero:
- Todos los datos son mock (proveedores, montos, fechas, nombres de usuarios).
- No hace falta integrar mail real, ni SAP real, ni storage real de archivos — se puede simular ("adjunto: cotizacion_proveedor.xlsx" como referencia visual, sin que el archivo exista de verdad).
- Priorizá que el flujo completo (desde crear una licitación hasta adjudicarla) sea clickeable y demostrable en vivo.

## 1. Página de bienvenida / overview (SOLO para este mockup, no es parte del producto final)

Necesito una página de inicio especial que sirva como "portada de presentación". Dejá claro en el propio diseño (por ejemplo con un badge o nota sutil tipo "Vista de presentación") que esta pantalla es a modo de resumen para la demo y no formará parte de la versión final del producto.

Esta página debe explicar, para alguien que nunca vio la herramienta:
- **Qué es y para qué sirve**: un portal para correr cotizaciones a proveedores de punta a punta sin mail, con transparencia total (nadie ve ofertas ajenas hasta que se abren oficialmente).
- **El flujo de trabajo**, mostrado como un stepper/timeline horizontal con 6 etapas: Creación → Lanzamiento → Cierre → Apertura → Comparativa (fuera del portal por ahora) → Adjudicación. Cada etapa con un ícono y una frase corta de qué pasa ahí.
- **Una vista tipo dashboard de gestión de tareas/eventos** (mock, con datos ficticios) que muestre cosas como: cuántas licitaciones están en curso, cuántas vencen en los próximos días, cuántas ofertas llegaron hoy, cuántas están esperando apertura. Usá cards o un pequeño panel de "Próximos vencimientos" y "Actividad reciente" con 4-6 ítems ficticios cada uno.
- Botones grandes y claros para entrar a la demo como distintos roles: "Entrar como Comprador", "Entrar como Jefe de Compras", "Entrar como Proveedor". Cada botón lleva directo al home de ese rol ya autenticado (usá usuarios demo preseedeados en Supabase, no hace falta que la persona escriba credenciales — un selector de rol tipo "elegí con qué usuario querés entrar" alcanza y es mejor para la demo).

## 2. Roles y autenticación (mock)

Roles a soportar, cada uno con su propio home/vista:
- **Comprador**: ve y gestiona sus propios casos.
- **Jefe de Compras (coordinador)**: ve TODOS los casos de su área (no solo los propios), puede reabrir, cancelar, reasignar a otro comprador, o intervenir directamente en un caso con las mismas capacidades que el comprador titular.
- **Proveedor (externo)**: ve solamente sus propias invitaciones (nunca otros proveedores, ni sus precios).
- (Opcional si da tiempo) **Admin**: pantalla simple de usuarios/roles, no es el foco de la demo.

Para simplificar la demo, sembrá 3-4 usuarios internos demo (2 compradores de distinta área, 1 Jefe de Compras) y 6-8 proveedores demo con nombres de empresas ficticias (mezclá algunos "del exterior/China" para que se sienta real, ej. "Ningbo Hardware Co." o similar, y algunos locales).

Regla de aislamiento clave a respetar en el diseño (aunque sea un mock, mostralo bien): un proveedor jamás debería poder ver, ni por casualidad navegando, la info de otro proveedor invitado al mismo caso, ni precios, ni la comparativa, ni quién ganó. Como es mock, al menos que la UI nunca muestre esa info cuando estás "logueado" como Proveedor.

## 3. Modelo de datos (mock, vía Supabase)

Entidades principales:
- **caso_qr**: id, número identificador (formato tipo "QR-2026-0134"), unidad de negocio (opcional: MMA/MAV/WEB/CIPO/BPE/CDS), comprador asignado, código+descripción (simulá que viene de un Excel adjunto), moneda, fecha límite (obligatoria), estado (Borrador/Lanzada/Cerrada/Abierta/Adjudicada/Cancelada/Desierta), fecha de creación.
- **qr_proveedor**: vínculo entre un caso y un proveedor invitado. Estado individual (Invitado/Cotizó/Rechazó/Sin respuesta/Reconsultado).
- **proveedor**: catálogo simulado "importado de SAP" (nombre, país, contacto, rubro).
- **oferta**: respuesta de un proveedor a su qr_proveedor. Incluye número de versión (nunca se pisa, cada re-subida es versión nueva y se marca cuál es la vigente), fecha de subida, adjuntos simulados (nombre de archivo tipo planilla.xlsx + 2-3 archivos más), monto total (oculto hasta la Apertura del caso).
- **mensaje_qa**: preguntas y respuestas dentro de un caso, con quién la hizo y cuándo.
- **auditoria**: log de acciones (usuario, acción, fecha) por caso, para mostrar trazabilidad.

Sembrá al menos 10-12 casos QR distribuidos en distintos estados del flujo (algunos en Lanzamiento con ofertas pendientes, algunos Cerrados esperando Apertura, alguno con una oferta rezagada en ventana de Reapertura, algunos ya Abiertos con ofertas visibles, uno o dos Adjudicados, uno Cancelado/Desierto) — así la demo puede mostrar cada pantalla con datos reales sin tener que crear casos en vivo.

## 4. Pantallas núcleo a construir

### a) Lista de casos (home del Comprador)
Tabla estilo planilla Excel: filtros y orden por columna, búsqueda, exportar (puede ser mock/deshabilitado con tooltip "próximamente"). Columnas: número QR, descripción, unidad, estado (badge de color), fecha límite, cantidad de proveedores invitados / cuántos cotizaron. Bandeja separada de "pendientes de cotizar" (necesidades sin caso creado todavía — mock, un par de ítems).

### b) Bandeja del Jefe de Compras
Igual a la lista de casos pero mostrando TODOS los casos del área, con acciones extra visibles: reasignar, intervenir, reabrir, cancelar.

### c) Detalle de un caso (vista Comprador / Jefe de Compras)
Vista con tabs o secciones:
- Datos generales del caso (editable en estado Borrador).
- Proveedores invitados, con su estado individual (Cotizado / Sin respuesta / Re-consulta) — SIN mostrar montos hasta que el caso esté Abierto.
- Botón de acción según el hito actual: Lanzar, Reconsultar, Extender (sumar proveedor antes del Cierre), Cerrar recepción, Reabrir (solo si aún no se abrió), Abrir (acción manual, a partir de acá ya no se puede extender ni reabrir), Lanzar 2ª ronda (después de Abrir, fuerza a TODOS a recotizar y versiona la oferta anterior), Registrar adjudicación (puede ser parcial), Cancelar/Declarar desierta (con justificación).
- Una vez Abierto: tabla comparativa simple mostrando las ofertas lado a lado (esto en el producto real es Fase 2 y hoy se hace en SAP — para la demo mostralo igual como "vista previa de lo que viene", aclarado con una etiqueta tipo "Fase 2 · vista preliminar").
- Q&A del caso (preguntas/respuestas).
- Archivos adjuntos (planilla + specs).
- Historial/auditoría (quién hizo qué y cuándo).

### d) Vista del Proveedor
Simple, mobile-first, con selector de idioma ES/EN. Muestra únicamente sus propias invitaciones. Por cada una: aceptar/rechazar invitación, descargar planilla (mock), subir su oferta con adjuntos (mock upload), ver el estado de su propio caso, hacer preguntas, ver avisos (cierre próximo, 2ª ronda lanzada). Nunca debe verse nada de otros proveedores.

### e) Repositorio / búsqueda histórica
Tabla de todos los casos (incluyendo cerrados/adjudicados/viejos), con búsqueda por múltiples campos (número QR, proveedor, fecha, unidad).

## 5. Reglas de negocio a respetar en el diseño (aunque sea mock, que se note)

- **Fecha límite siempre obligatoria** al crear un caso.
- **Ofertas selladas**: nadie (ni el comprador) ve montos antes de la Apertura manual.
- **Extensión ≠ 2ª ronda**: Extensión suma proveedores nuevos antes del Cierre sin forzar a recotizar a nadie más. 2ª ronda ocurre después de la Apertura y fuerza a TODOS los que participaron a recotizar, versionando la oferta anterior.
- **Ventana de reapertura**: solo existe entre el Cierre y la Apertura; una vez Abierto el caso, ya no se puede ni extender ni reabrir.
- **Versionado**: una oferta nunca se sobreescribe, cada re-subida es una versión nueva marcada como vigente.
- **Aislamiento entre proveedores** y **confidencialidad entre áreas internas** (un comprador de un área no debería ver casos de otra área, salvo el Jefe General).

## 6. Estilo visual

Tema oscuro, header compacto, tablas estilo Excel (encabezados fijos, orden/filtro por columna, hover de fila), badges de estado con colores distintivos por etapa (ej. Borrador=gris, Lanzada=azul, Cerrada=naranja, Abierta=violeta, Adjudicada=verde, Cancelada/Desierta=rojo), paneles laterales para detalle/filtros. Estética "herramienta interna enterprise seria", no un producto de consumo. Tipografía clara, buena densidad de información (esto lo van a usar compradores todo el día, no es una landing de marketing) excepto la página de bienvenida del punto 1, que sí puede ser más visual/explicativa.

## 7. Qué NO construir en este mockup (fuera de alcance)

No hace falta simular: integración real con SAP, envío real de mails, autoregistro de proveedores, adjudicación ítem por ítem, scoring ponderado técnico/comercial, TCO o landed cost, subastas inversas, envelope bidding, dashboard de KPIs con gráficos reales, app mobile nativa. Si querés dejar algún botón visible para mostrar "hacia dónde va el producto", que quede deshabilitado o con tooltip "Fase 2", no funcional.
```

---

### Notas antes de pegarlo en Lovable

- El prompt es largo a propósito — Lovable rinde mejor con contexto completo de una sola vez que con iteraciones cortas ambiguas. Si Lovable trunca o pide dividirlo, el orden natural de corte es: (1) auth + modelo de datos + seed, (2) pantallas núcleo, (3) página de bienvenida al final (es la más "cosmética" y depende de que el resto ya exista).
- Los nombres de estados/badges los dejé sugeridos pero no cerrados — si en la reunión de presentación preferís otra nomenclatura, es un ajuste menor de copy después.
- Recordá que esto es *solo para presentar*: no reemplaza ninguna decisión pendiente en [`docs/decisiones-pendientes.md`](decisiones-pendientes.md) (login real de proveedor, retención de datos, etc.) — esas siguen abiertas para cuando se construya el sistema real.
