# Backlog de Historias de Usuario — Portal de Cotizaciones

> Fuente original: [`Historias-de-Usuario.xlsx`](../Historias-de-Usuario.xlsx) (hoja "Historias de Usuario", v0.3 revisión Compras).
> Este documento es una versión legible en Markdown del mismo contenido, para que quede disponible como contexto de trabajo. **El Excel es la fuente autoritativa** (tiene además columnas de Prioridad MoSCoW, Estimación y Notas que no se repiten acá) — si hay una edición futura del backlog, actualizar el Excel y regenerar este archivo.

**Totales:** 70 historias · 42 Mínimo (MVP) · 11 Versión 2 · 17 Fuera de la plataforma.

Cada ítem: **ID** (rol) — historia, con su criterio de aceptación y dependencias (IDs de otras historias que deben resolverse antes).

---

## Mínimo indispensable (MVP)

### 1 · Alta y gestión del caso

- **US-01** (Comprador) — quiero crear un caso de cotización cargando nº identificador, unidad (opcional), comprador, código y descripción (Excel adjunto) y moneda, para iniciar formalmente un pedido de cotización.
  - *Criterio de aceptación:* El Excel trae código, descripción y cantidad del listado de necesidad, más las especificaciones que aplican.
  - *Dependencias:* US-50
- **US-02** (Comprador) — quiero que el sistema asigne un número único cuando el caso nace a mano, para identificar cada cotización sin ambigüedad.
  - *Criterio de aceptación:* Todo caso manual recibe un identificador único no repetible.
  - *Dependencias:* US-01
- **US-03** (Comprador) — quiero adjuntar la planilla Excel de ítems y los planos/especificaciones, para que los proveedores tengan todo lo necesario para cotizar.
  - *Criterio de aceptación:* Los planos/especificaciones se obtienen como hoy, al inicio del proceso.
  - *Dependencias:* US-01, US-56
- **US-04** (Comprador) — quiero ver y seguir el estado del caso (creación → … → adjudicada), para saber en qué etapa está cada cotización.
  - *Criterio de aceptación:* El estado se muestra y cambia según los hitos de la licitación.
  - *Dependencias:* US-01
- **US-63** (Comprador) — quiero tener un listado (bandeja) de las necesidades pendientes de cotizar, para gestionar mi trabajo pendiente.
  - *Criterio de aceptación:* El comprador ve su pool de necesidades sin cotizar todavía.
  - *Dependencias:* US-01

### 2 · Proveedores e invitación

- **US-07** (Comprador) — quiero tener disponible la lista de proveedores de SAP (export/sync) para elegir al invitar (desplegable/buscador), para invitar sin recargar datos.
  - *Criterio de aceptación:* El MVP trae los proveedores desde SAP (solo lectura).
  - *Dependencias:* US-50
- **US-09** (Comprador) — quiero invitar a varios proveedores (del listado de SAP) a un caso, para recibir ofertas de distintas fuentes.
  - *Criterio de aceptación:* Al invitar se crea un vínculo caso-proveedor por cada uno.
  - *Dependencias:* US-01, US-07

### 3 · Interacción con el proveedor

- **US-12** (Proveedor) — quiero recibir la invitación por mail, acceder al portal y aceptar o rechazar la invitación, viendo solo lo mío, para participar sin ver información de otros.
  - *Criterio de aceptación:* Aislamiento total: cada proveedor ve solo su invitación.
  - *Dependencias:* US-09, US-48
- **US-13** (Proveedor) — quiero descargar la planilla, completarla y re-subir mi oferta con adjuntos, para enviar mi cotización de forma trazable.
  - *Criterio de aceptación:* La oferta queda registrada, fechada y asociada a mi invitación.
  - *Dependencias:* US-12, US-03, US-56
- **US-14** (Proveedor) — quiero usar el portal en español o inglés, para entender todo en mi idioma.
  - *Criterio de aceptación:* El proveedor elige idioma ES/EN.
  - *Dependencias:* US-12
- **US-15** (Proveedor) — quiero declinar/rechazar la cotización dejando constancia, para avisar que no participo sin usar mail.
  - *Criterio de aceptación:* Se registra el rechazo y el caso puede avanzar igual.
  - *Dependencias:* US-12
- **US-16** (Comprador) — quiero reconsultar (dentro de la misma ronda, antes del cierre) a todos o a algunos proveedores, para pedir mejora o aclaración de la oferta.
  - *Criterio de aceptación:* Puedo elegir a quiénes reconsultar; NO fuerza a recotizar a quien no fue reconsultado (distinto de la 2ª ronda, que sí fuerza a todos).
  - *Dependencias:* US-13
- **US-17** (Proveedor / Comprador) — quiero hacer y responder preguntas dentro del portal, para registrar las aclaraciones formalmente.
  - *Criterio de aceptación:* Toda interacción formal comprador↔proveedor queda en el portal, NO en el mail.
  - *Dependencias:* US-12
- **US-61** (Comprador) — quiero antes del Cierre (sin haber visto precios), poder sumar un proveedor nuevo — extensión, para ampliar la competencia mientras se mantenga la equidad.
  - *Criterio de aceptación:* Extensión = solo agrega gente; los que ya cotizaron NO tienen que volver a cotizar. Solo antes del Cierre y sin haber abierto ofertas.
  - *Dependencias:* US-09
- **US-62** (Comprador) — quiero ver el estado de cada cotización sin ver el detalle ni los precios, para hacer seguimiento sin romper el sellado.
  - *Criterio de aceptación:* Muestra estados tipo 'Cotizado / Sin respuesta / Re-consulta', sin montos.
  - *Dependencias:* US-13, US-52
- **US-21** (Proveedor) — quiero recibir un recordatorio antes del vencimiento si todavía no cargué mi oferta, para no perder la oportunidad de cotizar.
  - *Criterio de aceptación:* El comprador define los tiempos del aviso; solo aplica a quien no subió oferta aún.
  - *Dependencias:* US-46

### 4 · Cierre, comparativa y adjudicación

- **US-22** (Comprador) — quiero cerrar la recepción de ofertas, para pasar a la etapa de apertura/comparación.
  - *Criterio de aceptación:* Al cerrar, se bloquean cotizaciones y nuevos proveedores.
  - *Dependencias:* US-13, US-04
- **US-29** (Comprador) — quiero que el caso se cierre automáticamente al vencer el plazo, para evitar ofertas fuera de término.
  - *Criterio de aceptación:* La fecha límite es SIEMPRE obligatoria al crear el caso; al vencer, se bloquean las subidas (auto-close).
  - *Dependencias:* US-22
- **US-69** (Comprador) — quiero reabrir el caso entre el Cierre y la Apertura, para que un proveedor rezagado cargue su oferta, para no perder una oferta por un cierre automático.
  - *Criterio de aceptación:* Solo permitido si la licitación TODAVÍA NO fue abierta (mantiene la transparencia).
  - *Dependencias:* US-22
- **US-70** (Comprador) — quiero abrir la licitación (acción manual) para ver las ofertas de todos los proveedores, para pasar a la etapa de comparación.
  - *Criterio de aceptación:* A partir de la Apertura ya NO se puede extender ni reabrir el caso (transparencia).
  - *Dependencias:* US-22, US-52
- **US-23** (Comprador) — quiero lanzar una 2ª ronda tras la Apertura, donde TODOS los proveedores que participaron deben volver a cotizar, para negociar mejores condiciones sin perder trazabilidad.
  - *Criterio de aceptación:* La oferta anterior queda versionada (ya no vigente). Distinto de la extensión (US-61), que solo agrega gente.
  - *Dependencias:* US-70
- **US-24** (Jefe de Compras / Comprador) — quiero cancelar/anular un caso con registro (archivar o declarar desierta), para descartar cotizaciones sin perder la traza.
  - *Criterio de aceptación:* Requiere justificación; queda el histórico.
  - *Dependencias:* US-04, US-50
- **US-27** (Comprador) — quiero registrar la adjudicación (elegir proveedor, puede ser parcial), para dejar constancia de la decisión.
  - *Criterio de aceptación:* Cambia el estado a 'Adjudicada'. La OC se hace en SAP.
  - *Dependencias:* US-70
- **US-64** (Jefe de Compras) — quiero tener una bandeja con TODOS los casos de mi área (no solo los propios), para actuar como coordinador de mi equipo.
  - *Criterio de aceptación:* Es un rol de visión global: revisar, intervenir o reasignar casos si es necesario (no aprueba comparativas en el MVP).
  - *Dependencias:* US-04, US-50
- **US-65** (Jefe de Compras) — quiero reasignar un caso a otro comprador de mi área, para redistribuir carga de trabajo o cubrir una ausencia.
  - *Criterio de aceptación:* El caso pasa a otro comprador de la misma área, con su historial intacto.
  - *Dependencias:* US-64
- **US-66** (Jefe de Compras) — quiero intervenir/actuar sobre un caso de mi área con las mismas capacidades que el comprador titular, para resolver algo sin esperar al comprador titular.
  - *Criterio de aceptación:* Scope acotado a los casos de mi área.
  - *Dependencias:* US-64

### 5 · Repositorio e histórico

- **US-38** (Comprador) — quiero que todo el caso quede guardado y consultable, para tener un repositorio único.
  - *Criterio de aceptación:* Casos, ofertas, archivos y mensajes quedan almacenados.
  - *Dependencias:* US-56, US-01
- **US-39** (Comprador) — quiero buscar en el histórico por varios campos, para encontrar cotizaciones pasadas.
  - *Criterio de aceptación:* Búsqueda por QR, proveedor, fecha, unidad, etc.
  - *Dependencias:* US-38
- **US-40** (Comprador) — quiero consultar al instante lo de los últimos ~6 meses, para trabajar con lo reciente sin demora.
  - *Criterio de aceptación:* Limitar la base inmediata ayuda al desarrollo; si no, toda la base disponible.
  - *Dependencias:* US-38
- **US-41** (Comprador) — quiero que nunca se pise una oferta (versionado), para no perder versiones anteriores.
  - *Criterio de aceptación:* Cada re-subida crea una versión; se marca la vigente.
  - *Dependencias:* US-13, US-56
- **US-42** (Responsable de datos) — quiero que se capturen datos crudos (fechas, invitados vs respondieron, precios), para poder medir indicadores.
  - *Criterio de aceptación:* Se guardan los datos base para calcular KPIs.
  - *Dependencias:* US-04, US-13

### 6 · Notificaciones

- **US-46** (Comprador / Proveedor) — quiero recibir mail de invitación (y aviso de oferta recibida), para enterarme sin revisar el portal.
  - *Criterio de aceptación:* Se envían los mails transaccionales mínimos.
  - *Dependencias:* US-09, US-13
- **US-67** (Proveedor) — quiero recibir un aviso cuando se cierra la recepción de mi caso, para saber que ya no puedo cargar cambios.
  - *Criterio de aceptación:* Mail automático al momento del Cierre (manual o auto-close).
  - *Dependencias:* US-22
- **US-68** (Proveedor) — quiero recibir un aviso cuando se lanza una 2ª ronda, para saber que debo volver a cotizar.
  - *Criterio de aceptación:* Mail automático al lanzar la 2ª ronda (US-23).
  - *Dependencias:* US-23

### 7 · Seguridad y trazabilidad

- **US-48** (Responsable de seguridad) — quiero garantizar el aislamiento total entre proveedores, para que nadie vea ofertas ajenas.
  - *Criterio de aceptación:* Validado en el backend, no solo en la pantalla.
  - *Dependencias:* US-50
- **US-49** (Responsable de seguridad) — quiero asegurar confidencialidad entre áreas/unidades, para que no se cruce info entre compradores.
  - *Criterio de aceptación:* Cada área ve solo lo suyo; el Jefe General ve todo.
  - *Dependencias:* US-50
- **US-50** (Admin) — quiero definir roles con alcance por área/región, para que cada uno vea/haga solo lo suyo.
  - *Criterio de aceptación:* Permisos por rol + capacidad + área.
- **US-51** (Responsable de datos) — quiero tener auditoría completa de ediciones, para saber quién hizo qué y cuándo.
  - *Criterio de aceptación:* Registro de todas las acciones con usuario y fecha.
  - *Dependencias:* US-50
- **US-52** (Responsable de seguridad) — quiero que las ofertas queden selladas hasta la apertura, para garantizar la transparencia (nadie ve precios antes del cierre).
  - *Criterio de aceptación:* Parte del flujo: el comprador ve las ofertas recién en la Apertura.
  - *Dependencias:* US-13, US-22
- **US-53** (Responsable de seguridad) — quiero tener un hash de integridad por cada Excel subido, para detectar archivos alterados.
  - *Criterio de aceptación:* El desarrollador define la implementación.
  - *Dependencias:* US-13

### 8 · Base técnica

- **US-56** (Responsable de datos) — quiero guardar los archivos pesados fuera de la base (Drive/GCS), para soportar el volumen de archivos.
  - *Criterio de aceptación:* La base guarda metadatos y link; lo define el desarrollador.
- **US-57** (Responsable de datos) — quiero tener disponibilidad 24/7, para que proveedores de otros husos puedan operar.
  - *Criterio de aceptación:* El portal está disponible de forma continua.

## Versión 2 (F2)

### 1 · Alta y gestión del caso

- **US-05** (Comprador) — quiero cargar varias cotizaciones juntas desde un Excel, para ahorrar tiempo en cargas masivas.
  - *Criterio de aceptación:* No se trabaja a nivel 'líneas de ítem'; es carga de casos.
  - *Dependencias:* US-01, US-03

### 4 · Cierre, comparativa y adjudicación

- **US-25** (Comprador) — quiero armar/subir la comparativa dentro del portal y retocarla, para dejar el análisis formal en el portal.
  - *Criterio de aceptación:* Fase 2: en el MVP la comparativa se hace fuera (SAP); el portal la incorpora después.
  - *Dependencias:* US-27
- **US-26** (Aprobador) — quiero aprobar o rechazar la comparativa en el portal, para dar el visto bueno antes de adjudicar.
  - *Criterio de aceptación:* Fase 2 — rol de aprobación a definir en esa etapa (no es el Jefe de Compras coordinador del MVP).
  - *Dependencias:* US-25
- **US-28** (Comprador) — quiero exportar la comparativa a Excel, para compartirla o adjuntarla a la OC.
  - *Criterio de aceptación:* Fase 2 (cuando la comparativa viva en el portal).
  - *Dependencias:* US-25
- **US-31** (Comprador) — quiero que el portal arme la comparativa automáticamente desde las ofertas, para evitar el armado manual.
  - *Criterio de aceptación:* Fase 2.
  - *Dependencias:* US-13
- **US-32** (Comprador) — quiero que las ofertas en distinta moneda se normalicen (USD/ARS, según área), para comparar en una base común.
  - *Criterio de aceptación:* Fase 2 (con la comparativa en el portal).
  - *Dependencias:* US-31
- **US-33** (Comprador) — quiero generar el memo de adjudicación (GEM) desde el portal, para completar el circuito.
  - *Criterio de aceptación:* Fase 2 (y luego conectar con SAP).
  - *Dependencias:* US-27

### 5 · Repositorio e histórico

- **US-43** (Comprador) — quiero buscar dentro del contenido de los archivos, para encontrar un part number dentro de un PDF.
  - *Criterio de aceptación:* La búsqueda alcanza el contenido de los adjuntos.
  - *Dependencias:* US-38
- **US-44** (IT) — quiero hacer bajadas masivas del histórico, para respaldos y migraciones.
  - *Criterio de aceptación:* Se puede exportar el histórico en lote.
  - *Dependencias:* US-38

### 7 · Seguridad y trazabilidad

- **US-55** (Admin) — quiero que el ingreso interno sea con cuenta de Google (y 2FA a futuro), para reforzar la seguridad de cuentas internas.
  - *Criterio de aceptación:* Login interno = cuenta Google Workspace; 2FA en Fase 2.
  - *Dependencias:* US-50

### 8 · Base técnica

- **US-58** (Responsable de datos) — quiero dejar previsto el 'tipo de evento' en el modelo (RFI/RFP a futuro), para no rehacer el modelo después.
  - *Criterio de aceptación:* Etapa posterior.

## Fuera de la plataforma

### 1 · Alta y gestión del caso

- **US-06** (Comprador) — quiero que el caso traiga datos automáticamente desde DEYEL, para no recargar información que ya existe.
  - *Criterio de aceptación:* Fuera: se trabaja con múltiples fuentes, no solo DEYEL.

### 2 · Proveedores e invitación

- **US-08** (Jefe de Desarrollo de Proveedores) — quiero dar de alta / editar / activar proveedores en el portal, para mantener un catálogo propio.
  - *Criterio de aceptación:* Fuera: los proveedores vienen de SAP, no se mantienen en el portal.
- **US-10** (Proveedor) — quiero registrarme por mi cuenta en el portal, para autogestionar mi alta.
  - *Criterio de aceptación:* Fuera: los proveedores se toman de SAP.
- **US-11** (Jefe de Desarrollo de Proveedores) — quiero gestionar el onboarding completo (datos bancarios, calificación, performance), para tener un maestro completo.
  - *Criterio de aceptación:* Fuera: lo maneja SAP.

### 3 · Interacción con el proveedor

- **US-18** (Comprador) — quiero que las preguntas se anonimicen y la respuesta llegue a todos los invitados, para mantener la equidad entre oferentes.
  - *Criterio de aceptación:* Fuera de esta versión.
- **US-19** (Proveedor) — quiero cargar mi oferta ítem por ítem en el portal, para evitar el intercambio por Excel.
  - *Criterio de aceptación:* Fuera: no se trabaja a nivel líneas de ítem.
- **US-20** (Proveedor) — quiero ver si mi oferta fue elegida, para conocer el resultado.
  - *Criterio de aceptación:* Fuera: la adjudicación se formaliza en SAP, no en el portal.

### 4 · Cierre, comparativa y adjudicación

- **US-30** (Comprador) — quiero adjudicar por línea a distintos proveedores, para maximizar el ahorro.
  - *Criterio de aceptación:* Fuera de esta versión (la adjudicación parcial se maneja por estado).
- **US-34** (Comprador) — quiero evaluar con scoring ponderado técnico + comercial, para decidir por más que el precio.
  - *Criterio de aceptación:* Fuera de alcance.
- **US-35** (Comprador) — quiero ver el costo total (TCO / landed cost por Incoterm), para comparar costo real.
  - *Criterio de aceptación:* Fuera de alcance.
- **US-36** (Comprador) — quiero correr subastas inversas, para que los proveedores compitan bajando el precio.
  - *Criterio de aceptación:* Fuera de alcance.
- **US-37** (Comprador) — quiero usar sobre técnico/comercial separado (envelope bidding), para separar la evaluación técnica del precio.
  - *Criterio de aceptación:* Fuera de alcance.

### 5 · Repositorio e histórico

- **US-45** (Gerencia) — quiero ver un dashboard de indicadores (ahorro, tiempos, tasa de respuesta), para medir el desempeño.
  - *Criterio de aceptación:* Fuera de esta versión.

### 6 · Notificaciones

- **US-47** (Comprador) — quiero enviar recordatorios y resúmenes automáticos (digests), para dar seguimiento sin esfuerzo manual.
  - *Criterio de aceptación:* Fuera de esta versión.

### 7 · Seguridad y trazabilidad

- **US-54** (Admin) — quiero editar las reglas de negocio sin reprogramar, para adaptar el proceso sin depender de IT.
  - *Criterio de aceptación:* Fuera de esta versión.

### 8 · Base técnica

- **US-59** (Comprador) — quiero volcar la adjudicación a SAP automáticamente, para evitar recargar la OC.
  - *Criterio de aceptación:* Fuera: no se crea la OC en el portal.
- **US-60** (Proveedor) — quiero usar una app mobile nativa, para operar desde el celular.
  - *Criterio de aceptación:* Fuera de esta versión (el web es responsive).
