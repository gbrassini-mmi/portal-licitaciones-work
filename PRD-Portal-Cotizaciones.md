# **PRD — Portal de Cotizaciones** 

Documento de Requisitos de Producto  ·  Batfer  ·  alternativa interna a SAP Ariba 

_Versión 0.4 (borrador) — roles (Jefe de Compras coordinador), extensión vs 2ª ronda, notificaciones, fecha límite obligatoria_ 

|**Campo**|**Detalle**|
|---|---|
|**Producto**|Portal de Cotizaciones (gestión de la interacción con proveedores en cotizaciones<br>de compra)|
|**Encuadre**|Producto independiente,bajogobierno PrMO/Stage-Gate del TMO|
|**Sponsor/ aprobador final**|Leo Spina|
|**Dueño deproducto(PrMO)**|Mathias/Pablo de Luca/Fernanda Abdala —[Confirmar con Mathi]|
|**Desarrollo**|Chivilo (contacto: Vázquez) — externo. El stack lo decide Chivilo (sugerencia:<br>herramientas Google).|
|**Piloto**|Unidad Batfer|
|**Criterio de éxito**|Correr una cotización depunta apunta sin usar mail,con transparencia total.|



_Cómo leer este documento: el alcance obligatorio es el MVP; lo marcado [F2] queda fuera de la primera entrega; lo marcado [DEF] está "por definir" (posible MVP, se decide con Compras/gerencia). Los puntos abiertos están marcados [SUPUESTO] (decisión por defecto, revisable) o [Confirmar con Mathi]. El Anexo B lista las mejoras surgidas de la investigación de la industria._ 

## **1 · Resumen ejecutivo** 

Batfer gestiona cotizaciones a proveedores (principalmente del exterior/China) mediante intercambios informales por mail, lo que dificulta la trazabilidad, la transparencia y la consulta del histórico. En vez de contratar SAP Ariba (costo elevado), se construye un portal interno liviano que traslada ese intercambio a una plataforma. 

El portal cubre SOLO la interacción con el proveedor y correr el proceso de licitación lo más regulado posible: crear la licitación → lanzarla a proveedores → recibir ofertas → cerrar → abrir → registrar la adjudicación. No arma el pedido ni gestiona la orden de compra/pago. 

**Alcance del MVP (definido por Compras):** la comparativa, la aprobación y el memo NO van en el MVP — hoy se hacen en SAP (desconectado del portal). El portal las incorpora en Fase 2 y, más adelante, se conecta con SAP. Los proveedores se toman de SAP (export/lectura). No se trabaja a nivel "líneas de ítem". 

Filosofía: MVP mínimo funcional primero; los "chiches" (comparativa automática, integraciones, rankings) son fases posteriores. 

## **2 · Glosario** 

|**Término**|**Significado**|
|---|---|
|**Cotización / Caso / QR**|El pedido de cotización, identificado por un número QR. Contenedor de ítems,<br>proveedores invitadosyofertas.|
|**QR-Proveedor**|Unidad central del sistema: la combinación de una QR con un proveedor invitado.<br>UnaQR se envía a variosproveedores.|
|**Ítem**|Línea a cotizar. En el MVP vive dentro de un Excel adjunto, no como campo del<br>portal.|
|**Oferta**|La respuesta de un proveedor a una QR-Proveedor (Excel completado + adjuntos).<br>Puede tener varias rondas.|
|**Reconsulta**|Nueva ronda depedido al mismoproveedor sobre la mismaQR.|
|**Comparativa**|Cuadro de análisis de ofertas. En el MVP se arma fuera delportalyse sube.|
|**Adjudicación**|Elección del/losproveedor(es) ganador(es).|
|**Antecedentes deprecio**|Últimosprecios/compras(referencia SAP ME2L)usados al comparar.|
|**Unidad de negocio**|MMA,MAV,WEB,CIPO,BPE,CDS,etc.|
|**Área/ Región**|Ámbito(UY/Batfer,Argentina…) que delimita accesosyaprobaciones.|



## **3 · Actores y roles** 

Internos (autenticación Google Workspace @batfer.net): 

|**Rol**|**Qué ve**|**Qué hace**|
|---|---|---|
|**Comprador**|sus casos|crear, invitar, gestionar el ciclo<br>(reconsulta, extiende, cierra, abre,<br>adjudica).|
|**Jefe de Compras (coordinador)**|TODOS los casos de su área|reabrir, cancelar, reasignar a otro<br>comprador, intervenir (mismas<br>capacidades que el comprador<br>titular).|
|**Jefe General de Compras**|todo|cruza unidades|



|**Admin**|config/usuarios|administración|
|---|---|---|
|**Proveedor (externo)**|solo sus QR-Proveedor|aceptar/rechazar invitación,<br>cargar oferta, preguntar,rechazar|



Cambio v0.4: el rol "Aprobador" se descarta para el MVP — no hay acción de aprobar dentro del portal (la comparativa es Fase 2). Lo que se llamaba "Aprobador" es en realidad el Jefe de Compras, coordinador de área. El rol "Jefe de Desarrollo de Proveedores" tampoco aplica en el MVP: los proveedores vienen de SAP. 

Modelo de permisos: rol (persona) + capacidades (reabrir, reasignar, intervenir, cancelar, ver_todo) + scope (área/región + unidad de negocio). 

#### **Regla de aislamiento (NO negociable): un proveedor nunca ve otros invitados, sus precios, la comparativa ni la adjudicación. Se aplica en el backend por fila (no solo ocultar en el front). Confidencialidad también entre unidades internas.** 

## **4 · Hitos de la licitación** 

1. 1. Creación — se arma la licitación (borrador). Proveedores desde SAP. Fecha límite SIEMPRE obligatoria. 

2. 2. Lanzamiento — se envía la licitación a los proveedores → cotizan (aceptan/rechazan la invitación). 

3. 3. Cierre — bloqueo por plataforma, manual o automático al vencer la fecha límite: no se cotiza más ni entran proveedores nuevos. 

4. 4. Apertura — acción MANUAL del comprador (no ocurre sola): recién acá ve las ofertas (antes están selladas). Por transparencia, a partir de la apertura ya no se puede extender ni reabrir. 

5. 5. Comparativa — en el MVP se hace fuera (SAP); el portal la incorpora en Fase 2. 

6. 6. Adjudicación — se registra el proveedor elegido (puede ser parcial) → estado "Adjudicada". La OC se hace en SAP. 

**Dos mecanismos distintos, no confundir:** Extensión (antes del Cierre) = sumar un proveedor nuevo sin ver precios, sin forzar a recotizar a los que ya cotizaron. 2ª ronda (opcional, tras la Apertura) = TODOS los proveedores que participaron deben volver a cotizar (BAFO); la oferta anterior queda versionada, ya no vigente; vuelve a Lanzamiento. 

**Reapertura (ventana):** si falta un proveedor y todavía NO se hizo la Apertura, el comprador puede reabrir momentáneamente el caso para que cargue su oferta. Una vez abierta la licitación, esta ventana ya no existe. 

El comprador ve el estado de cada cotización (Cotizado / Sin respuesta / Re-consulta) SIN ver montos hasta la Apertura. Casos borde: el proveedor puede rechazar o no cotizar → se registra y el caso avanza. Un caso puede cancelarse / declararse desierto con justificación, o reasignarse a otro comprador (Jefe de Compras). 

## **5 · Modelo de datos** 

Entidades: CASO_QR (contenedor) → QR_PROVEEDOR (llave) → OFERTA → ARCHIVO; más PROVEEDOR (catálogo), COMPARATIVA, MENSAJE_QA, USUARIO_INTERNO, AUDITORIA. 

- Llave del sistema: QR-Proveedor. Ofertas, archivos, preguntas y estados cuelgan de ahí. 

- Precios NO son campos en el MVP: viven dentro del Excel de la oferta. [SUPUESTO confirmado por Renzo] 

- Archivos pesados (Excel, PDF, fotos, planos — ~4/oferta) → almacenamiento de objetos (GCS o Drive); la base guarda metadatos + links. 

- Moneda: se guarda y muestra la original; la conversión sugerida es informativa. 

- Auditoría: log de todas las ediciones (quién / qué / cuándo). 

- CASO_QR incluye un campo tipo de evento (event_type = RFQ hoy; deja lugar a RFI/RFP a futuro). 

- Versionado: las ofertas nunca se sobrescriben; cada re-subida es una versión y se marca la vigente. 

## **6 · Requisitos funcionales** 

_[MVP] = primera entrega (mínimo) · [F2] = Fase 2 · [Fuera] = no va en la plataforma (o lo maneja SAP)._ 

_La lista completa y vigente, historia por historia y con dependencias, está en el backlog "Historias-de-Usuario.xlsx" y en el doc 09. Abajo, el resumen del MVP y qué queda para Fase 2 / fuera._ 

|**ID**|**Requisito**|**Fase**|
|---|---|---|
|**RF-01**|Crear la licitación (nº identificador, unidad opc., comprador, código+descripción vía Excel,<br>moneda)+ fecha límite SIEMPRE obligatoria.|MVP|
|**RF-02**|Adjuntar laplanilla de ítems +planos/especificaciones.|MVP|
|**RF-03**|Número único cuando el caso nace a mano.|MVP|
|**RF-04**|Veryseguir el estado según los hitos(creación → … → adjudicada).|MVP|
|**RF-05**|Bandeja: necesidades pendientes (comprador) + bandeja de área con TODOS los casos (Jefe<br>de Compras).|MVP|
|**RF-06**|Tomar los proveedores desde SAP (export/lectura) y elegir al invitar<br>(desplegable/buscador).|MVP|
|**RF-07**|Invitar Nproveedores(crea un vínculo caso-proveedorpor cada uno).|MVP|
|**RF-08**|El proveedor recibe la invitación por mail, entra al portal, la acepta/rechaza y ve solo lo<br>suyo.|MVP|
|**RF-09**|Elproveedor descarga laplanilla,la completayre-sube su oferta con adjuntos.|MVP|
|**RF-10**|Portal bilingüe ES/ENpara elproveedor.|MVP|
|**RF-11**|Reconsultar (misma ronda, antes del cierre) a todos o algunos — no fuerza a recotizar a<br>quien no fue reconsultado.|MVP|
|**RF-12**|Q&A dentro delportal(toda interacción formal en elportal,no en el mail).|MVP|
|**RF-13**|Extensión: antes del cierre y sin ver precios, sumar un proveedor nuevo sin forzar<br>recotizado a los demás.|MVP|
|**RF-14**|El comprador ve el estado (Cotizado/Sin respuesta/Re-consulta) SIN ver montos hasta la<br>Apertura.|MVP|
|**RF-15**|Cerrar la recepción(manual) ycierre automático al vencer la fecha límite(auto-close).|MVP|
|**RF-16**|Ofertas selladas hasta la Apertura(transparencia).|MVP|
|**RF-17**|Ventana de reapertura entre Cierre y Apertura, para que un proveedor rezagado cargue su<br>oferta — solo si aún no se abrió.|MVP|
|**RF-18**|Cancelar/declarar desierta conjustificación(queda la traza).|MVP|



|**RF-19**|Apertura: acción manual del comprador para ver las ofertas. Desde aquí ya no se puede<br>extender ni reabrir.|MVP|
|---|---|---|
|**RF-20**|2ª ronda (opcional, tras la Apertura): TODOS los proveedores deben recotizar; oferta<br>anterior versionada, ya no vigente.|MVP|
|**RF-21**|Jefe de Compras: reasignar un caso a otro comprador de su misma área.|MVP|
|**RF-22**|Jefe de Compras: intervenir — actuar sobre un caso de su área con las mismas capacidades<br>del comprador titular.|MVP|
|**RF-23**|Registrar la adjudicación(puede serparcial)→ estado "Adjudicada". La OC se hace en SAP.|MVP|
|**RF-24**|Repositorio consultable + búsquedapor múltiples campos + consulta instantánea ~6 meses.|MVP|
|**RF-25**|Versionado: nuncapisar una oferta(cada re-subida es versión;se marca la vigente).|MVP|
|**RF-26**|Capturar datos crudos(fechas,invitados vs respondieron, precios) para KPIs.|MVP|
|**RF-27**|Hash de integridadpor cada Excel subido.|MVP|
|**RF-28**|Mail de invitación + aviso de oferta recibida(al comprador).|MVP|
|**RF-29**|Notificar alproveedor cuando se cierra la recepción de su caso.|MVP|
|**RF-30**|Notificar alproveedor cuando se lanza una 2ª ronda(debe volver a cotizar).|MVP|
|**RF-31**|Recordatorio de vencimiento alproveedorque todavía no cargó su oferta.|MVP|
|**RF-32**|Aislamiento entre proveedores + confidencialidad entre áreas + roles por área + auditoría<br>de ediciones.|MVP|
|**RF-33**|Almacenamiento de archivospesados fuera de la base(Drive/GCS)+ disponibilidad 24/7.|MVP|
|**RF-34**|Login interno con cuenta Google(Workspace@batfer.net).|MVP|
|**RF-35**|Comparativa dentro del portal (armar/subir/retocar, automática), aprobación, export y<br>memo(GEM);normalización de moneda.|F2|
|**RF-36**|Conectar la comparativa/adjudicación/memo con SAP.|F2|
|**RF-37**|Buscar dentro de archivos · bajadas masivas · 2FA · tipo de evento (RFI/RFP) · carga masiva<br>de casos.|F2|
|**RF-38**|DEYEL automático · oferta ítem-por-ítem · proveedor ve resultado · adjudicación por línea ·<br>scoring/TCO/subastas/envelope · dashboard KPIs · digests · reglas editables ·<br>autoregistro/onboarding de proveedores (gestión en SAP) · volcar OC a SAP · app mobile<br>nativa.|Fuera|



## **7 · Requisitos no funcionales** 

- Seguridad / aislamiento: autorización a nivel de fila; un proveedor solo accede a sus QRProveedor; confidencialidad entre unidades. Auditar con revisor de seguridad antes de prod. 

- Auditoría: traza completa de ediciones (quién, qué, cuándo). 

- Disponibilidad: 24/7 (proveedores en husos horarios distintos). 

- Volumen: ~150 ofertas/mes (Batfer); ×10 si escala al grupo; crece en el tiempo. ~4 archivos/oferta. Dimensionar para el histórico acumulado, no para alta concurrencia. 

- Almacenamiento: archivos pesados en objeto (GCS/Drive); base solo metadatos. Definir política de retención (online ~6 meses, luego frío). [SUPUESTO] 

- Idioma: portal bilingüe EN/ES; cara del proveedor sobre todo EN. 

- Multi-sociedad / multi-área: reglas y accesos diferenciados por sociedad y área. 

- Parametrización: las reglas de negocio duras deben ser editables sin redeploy. [Mathi define las reglas] 

- Navegadores modernos de escritorio; deseable responsive básico / mobile-first en la cara del proveedor. 

## **8 · Autenticación y autorización** 

- Internos: Google Workspace @batfer.net (SSO). Rol + capacidades + scope. 

- Proveedores (externos): cuenta con contraseña (hash seguro, verificación de mail; 2FA opcional a futuro). Link de invitación como puerta de entrada/activación inicial. [SUPUESTO — ver Anexo B, D-09: la industria favorece magic link]. 

- Principio: el método de login NO otorga permisos. Todo se decide por rol/capacidad/scope en el backend. Un proveedor nunca puede aprobar ni ver datos de terceros. 

## **9 · Integraciones** 

- Proveedores [MVP]: se toman desde SAP (export/lectura) — ya existe el pipeline LFA1-Sync en el ecosistema Batfer, reusable. [Falta definir: mecanismo/frecuencia de refresco] 

- Antecedentes de precio ME2L [F2]: solo lectura, para armar la comparativa cuando entre al portal. 

- Orden de Compra en SAP [F2]: el MVP registra "Adjudicada"; la OC se carga a mano en SAP. La conexión automática (con confirmación humana) es Fase 2. [Falta definir qué exporta el portal para esa carga manual] 

- Origen de la licitación [Falta definir]: hoy el alta es manual; a confirmar con Mathi si se importa también desde SOLPED/DEYEL o queda 100% manual. 

- Almacenamiento/Drive: preferencia por Drive; Chivilo define el store según velocidad y búsqueda por tamaño. Debe poder buscarse desde el portal. 

## **10 · UI / Look & Feel** 

Referencia obligatoria: el template de ejemplo (pendiente, lo pasa Leo) + el estándar de apps internas Batfer (skill Design-Btf-Apps / DepositoViewer): tema oscuro, header compacto, tablas estilo Excel con orden/filtro/export, badges de estado, paneles laterales. 

Ver también el documento 06-ux-lookandfeel y el prototipo mockups/index.html (5 pantallas). 

Pantallas núcleo del MVP: (1) Lista de casos (home = cola de tareas), (2) Detalle de la QR, (3) Vista del proveedor (simple, bilingüe, mobile-first), (4) Comparativa/aprobación, (5) Repositorio/búsqueda. 

## **11 · Notificaciones** 

- MVP: invitación al proveedor + aviso al comprador al recibir una oferta + aviso al proveedor cuando se cierra su caso + aviso al proveedor cuando se lanza una 2ª ronda + recordatorio de vencimiento al proveedor que no cargó oferta. 

- Fase 2: digests, notificaciones in-app. 

- El mail sigue existiendo en paralelo pero deja de ser el canal formal. 

## **12 · Fuera de alcance (Fase 2/3)** 

Armar el pedido/QR desde cero · memo GEM · comparativa automática · captura ítem-a-ítem · integración SAP de escritura · DEYEL por API (salvo se decida F1) · rankings/ahorro automático · SLP/onboarding-performance · recordatorios · reverse auctions · envelope bidding · TCO/landed cost. 

## **13 · Criterios de aceptación del MVP** 

7. CA-01: Un comprador crea una licitación (con fecha límite), adjunta el Excel, invita a 3 proveedores y estos reciben acceso — sin enviar un mail manual. 

8. CA-02: Los 3 proveedores entran, aceptan la invitación, descargan el Excel, suben su oferta con adjuntos, hacen una pregunta y uno rechaza — cada uno ve solo lo suyo (ofertas selladas). 

9. CA-03: El comprador cierra la recepción (manual o auto-close); como falta un proveedor y todavía no se abrió, reabre momentáneamente el caso para que cargue su oferta. 

10. CA-04: El comprador abre la licitación (acción manual) y ve las ofertas; a partir de ahí ya no puede extender ni reabrir. Se registra la adjudicación (puede ser parcial) → estado "Adjudicada". 

11. CA-05: El caso queda en el repositorio y se encuentra por QR, proveedor y fecha. 

12. CA-06: La auditoría muestra la traza de ediciones del caso. 

13. CA-07 (seguridad): Un proveedor no puede acceder por URL directa a la QR-Proveedor de otro (verificado por revisor de seguridad). 

14. CA-08: Un Jefe de Compras ve todos los casos de su área, reasigna uno a otro comprador, y este puede seguir gestionándolo con el historial intacto. 

15. CA-09: Al cerrarse un caso, el/los proveedor(es) reciben un mail; al lanzarse una 2ª ronda, reciben aviso y deben volver a cotizar (oferta anterior versionada). 

## **14 · Entregables y hitos (sprints)** 

_De menor a mayor riesgo; cada sprint entrega algo funcionando. Estimaciones a cargo de Chivilo._ 

|**Sprint**|**Entrega**|
|---|---|
|**0 · Fundaciones**|Auth interna + externa, modelo de datos, sync de proveedores desde SAP<br>(reusar/extender LFA1-Sync),almacenamiento de archivos.|
|**1 · Alta e invitación**|Crear caso (manual), invitar proveedores, bandejas (comprador + Jefe de Compras),<br>estados. Demo con un caso real.|
|**2 · Interacción del proveedor**|Vista del proveedor, subir oferta + adjuntos, Q&A, rechazar, reconsultas, extensión<br>antes del cierre. El corazón;validar aislamientoysellado.|
|**3 · Cierre, apertura y**<br>**adjudicación**|Cerrar/auto-close, ventana de reapertura, apertura manual, cancelar/desierta,<br>reasignar/intervenir(Jefe de Compras),registrar adjudicación.|
|**4 · 2ª ronda, repositorio y**<br>**notificaciones**|2ª ronda con versionado, búsqueda histórica, mails (invitación, oferta, cierre, 2ª<br>ronda,recordatorio),auditoría.|
|**Gate MVP**|Cumplir CA-01..09 + revisión de seguridad → aprobación de Stage-Gate (sponsor<br>Leo).|



## **15 · Restricciones técnicas y de gobierno** 

- Stack: lo decide Chivilo; sugerencia de herramientas Google. Debe cumplir los RNF (24/7, volumen, aislamiento, búsqueda). 

- Propiedad: el código y los datos viven en infraestructura de Batfer (GCP), no del proveedor. Handoff de credenciales por canal seguro; sin secretos en repos. 

- Datos sensibles: precios de proveedores = confidenciales; el aislamiento es requisito legal/reputacional. 

- Gobierno: producto bajo PrMO/Stage-Gate del TMO. Sponsor Leo aprueba pasajes de compuerta. 

## **Anexo A · Supuestos y pendientes** 

### **Resueltas en la ronda v0.4 (dudas cerradas con Renzo)** 

- [Resuelto] El "Aprobador" es el Jefe de Compras, coordinador de área 

   - (revisa/interviene/reasigna) — no aprueba comparativas en el MVP. 

- [Resuelto] Extensión (agregar proveedores) distinta de 2ª ronda (todos recotizan) — dos mecanismos distintos. 

- [Resuelto] Notificaciones al proveedor: se agregan avisos de cierre, 2ª ronda y recordatorio de vencimiento (antes F2, ahora MVP). 

- [Resuelto] El rol "Jefe de Desarrollo de Proveedores" no aplica en el MVP (gestión en SAP). 

- [Resuelto] La fecha límite es siempre obligatoria; existe una ventana de reapertura entre el Cierre y la Apertura (nunca después). 

### **Supuestos por defecto (revisables)** 

- Precios dentro del Excel, no como campos (MVP). 

- Login proveedor = cuenta con contraseña + link de invitación (ver D-09). 

- Adjudicación split simple (no por ítem estructurado). 

- Retención online ~6 meses. 

- Apertura es una acción manual explícita del comprador (nunca ocurre sola). 

- Reasignación de casos queda acotada a la misma área del Jefe de Compras. 

### **Pendientes de conseguir** 

- Template de look & feel (Leo). 

- Template actual de comparativa en Excel (Renzo). 

### **Pendientes [Confirmar con Mathi]** 

- ¿Ítems visibles en el portal (aunque sea de solo lectura) o solo dentro del Excel? 

- ¿Se importa la licitación también desde SOLPED/DEYEL, o queda 100% manual? (RF-01) 

- Reglas de negocio duras (parametrizables). 

- Dueño de producto definitivo. 

- Métricas de éxito en detalle. 

## **Anexo B · Consideraciones de la industria (e-sourcing) — deltas** 

## **sugeridos** 

_Surgidas de investigar Ariba, Oracle Sourcing, Coupa, Jaggaer, Ivalua + reviews reales. Recomendaciones a incorporar respecto de la v0.1. Detalle y fuentes en el documento 05-consideraciones-industria._ 

|**#**|**Cambio recomendado**|**Fase**|
|---|---|---|
|**D-01**|Sealed/blind bidding: el proveedor nunca ve ofertas ajenas (blind); ofertas no visibles hasta el<br>cierre(sealed). Diferencial central vs mail.|MVP|
|**D-02**|Audit trail append-only + hash del Excel en cada submission. Compliance y defensa ante<br>disputas.|MVP|
|**D-03**|Auto-closepor deadline + soporte de ≥2 rondas con archivo de la rondaprevia.|MVP|



|**D-04**|Q&A anonimizadoyvisible a todos los invitados. Equidad.|MVP|
|---|---|---|
|**D-05**|Adjudicaciónpor línea conjustificación(+3–7% ahorro).|MVP|
|**D-06**|Versionado de archivos: nunca sobrescribir(re-subida = versión nueva).|MVP|
|**D-07**|Campo event_type en el modelo (RFQ hoy; RFI/RFP a futuro sin repintar).|MVP<br>(schema)|
|**D-08**|Normalización de moneda con FX a fecha (hoy en el Excel externo; interno cuando la<br>comparativa entre alportal).|MVP nota<br>/F2|
|**D-09**|Reconsiderar login del proveedor → magic link: la industria lo favorece para terceros no<br>técnicos;seguro(single-use,expira) ysin fricción.|Decisión<br>abierta|
|**D-10**|Instrumentar KPIs (fechas, invitados vs respondieron, precios) → savings, cycle time, tasa de<br>respuesta.|MVP|
|**D-11**|Reverse auctions/envelope bidding /TCO/SLP completo → fuera de alcance,documentados.|F2+|



