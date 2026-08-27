# **PRD — Portal de Cotizaciones** 

Documento de Requisitos de Producto  ·  Batfer  ·  alternativa interna a SAP Ariba 

_Versión 0.4 (borrador) — roles (Jefe de Compras coordinador), extensión vs 2ª ronda, notificaciones, fecha límite obligatoria_ 

|**Campo**|**Detalle**|
|---|---|
|**Producto**|Portal de Cotzaciones (gestón de la interacción con<br>proveedores en cotzaciones de compra)|
|**Encuadre**|Producto independiente, bajo gobierno PrMO / Stage-<br>Gate del TMO|
|**Sponsor / aprobador fnal**|Leo Spina|
|**Dueño de producto (PrMO)**|Mathias / Pablo de Luca / Fernanda Abdala —<br>[Confrmar con Mathi]|
|**Desarrollo**|Chivilo (contacto: Vázquez) — externo. El stack lo<br>decide Chivilo(sugerencia: herramientas Google).|
|**Piloto**|Unidad Bater|
|**Criterio de éxito**|Correr una cotzación de punta a punta sin usar mail,<br>con transparencia total.|



_Cómo leer este documento: el alcance obligatorio es el MVP; lo marcado [F2] queda fuera de la primera entrega; lo marcado [DEF] está "por definir" (posible MVP, se decide con Compras/gerencia). Los puntos abiertos están marcados [SUPUESTO] (decisión por defecto, revisable) o [Confirmar con Mathi]. El Anexo B lista las mejoras surgidas de la investigación de la industria._ 

## **1 · Resumen ejecutivo** 

Batfer gestiona cotizaciones a proveedores (principalmente del exterior/China) mediante intercambios informales por mail, lo que dificulta la trazabilidad, la transparencia y la consulta del histórico. En vez de contratar SAP Ariba (costo elevado), se construye un portal interno liviano que traslada ese intercambio a una plataforma. 

El portal cubre SOLO la interacción con el proveedor y correr el proceso de licitación lo más regulado posible: crear la licitación → lanzarla a proveedores → recibir ofertas → cerrar → abrir → registrar la adjudicación. No arma el pedido ni gestiona la orden de compra/pago. 

**Alcance del MVP (definido por Compras):** la comparativa, la aprobación y el memo NO van en el MVP — hoy se hacen en SAP (desconectado del portal). El portal las incorpora en Fase 2 y, más adelante, se conecta con SAP. Los proveedores se toman de SAP (export/lectura). No se trabaja a nivel "líneas de ítem". 

Filosofía: MVP mínimo funcional primero; los "chiches" (comparativa automática, integraciones, rankings) son fases posteriores. 

## **2 · Glosario** 

|**Término**|**Signifcado**|
|---|---|
|**Cotzación / Caso / QR**|El pedido de cotzación, identfcado por un número QR.<br>Contenedor de ítems, proveedores invitadosyofertas.|
|**QR-Proveedor**|Unidad central del sistema: la combinación de una QR<br>con un proveedor invitado. Una QR se envía a varios<br>proveedores.|
|**Ítem**|Línea a cotzar. En el MVP vive dentro de un Excel<br>adjunto,no como campo delportal.|
|**Oferta**|La respuesta de un proveedor a una QR-Proveedor<br>(Excel completado + adjuntos). Puede tener varias<br>rondas.|
|**Reconsulta**|Nueva ronda de pedido al mismo proveedor sobre la<br>mismaQR.|
|**Comparatva**|Cuadro de análisis de ofertas. En el MVP se arma fuera<br>delportalyse sube.|
|**Adjudicación**|Elección del/losproveedor(es) ganador(es).|
|**Antecedentes de precio**|Últmos precios/compras (referencia SAP ME2L) usados<br>al comparar.|
|**Unidad de negocio**|MMA,MAV,WEB,CIPO,BPE,CDS,etc.|
|**Área / Región**|Ámbito (UY/Bater, Argentna…) que delimita accesos y<br>aprobaciones.|



## **3 · Actores y roles** 

Internos (autenticación Google Workspace @batfer.net): 

|**Rol**|**Qué ve**|**Qué hace**|
|---|---|---|
|**Comprador**|sus casos|crear, invitar, gestonar el ciclo<br>(reconsulta,extende,cierra,abre,|



|||adjudica).|
|---|---|---|
|**Jefe de Compras (coordinador)**|TODOS los casos de su área|reabrir, cancelar, reasignar a otro<br>comprador, intervenir (mismas<br>capacidades que el comprador<br>ttular).|
|**Jefe General de Compras**|todo|cruza unidades|
|**Admin**|confg/usuarios|administración|
|**Proveedor (externo)**|solo sus QR-Proveedor|aceptar/rechazar invitación, cargar<br>oferta, preguntar,rechazar|



Cambio v0.4: el rol "Aprobador" se descarta para el MVP — no hay acción de aprobar dentro del portal (la comparativa es Fase 2). Lo que se llamaba "Aprobador" es en realidad el Jefe de Compras, coordinador de área. El rol "Jefe de Desarrollo de Proveedores" tampoco aplica en el MVP: los proveedores vienen de SAP. 

Modelo de permisos: rol (persona) + capacidades (reabrir, reasignar, intervenir, cancelar, ver_todo) + scope (área/región + unidad de negocio). 

**Regla de aislamiento (NO negociable): un proveedor nunca ve otros invitados, sus precios, la comparativa ni la adjudicación. Se aplica en el backend por fila (no solo ocultar en el front). Confidencialidad también entre unidades internas.** 

## **4 · Hitos de la licitación** 

1. 1. Creación — se arma la licitación (borrador). Proveedores desde SAP. Fecha límite SIEMPRE obligatoria. 

2. 2. Lanzamiento — se envía la licitación a los proveedores → cotizan (aceptan/rechazan la invitación). 

3. 3. Cierre — bloqueo por plataforma, manual o automático al vencer la fecha límite: no se cotiza más ni entran proveedores nuevos. 

4. 4. Apertura — acción MANUAL del comprador (no ocurre sola): recién acá ve las ofertas (antes están selladas). Por transparencia, a partir de la apertura ya no se puede extender ni reabrir. 

5. 5. Comparativa — en el MVP se hace fuera (SAP); el portal la incorpora en Fase 2. 

6. 6. Adjudicación — se registra el proveedor elegido (puede ser parcial) → estado "Adjudicada". La OC se hace en SAP. 

**Dos mecanismos distintos, no confundir:** Extensión (antes del Cierre) = sumar un proveedor nuevo sin ver precios, sin forzar a recotizar a los que ya cotizaron. 2ª ronda (opcional, tras la Apertura) = TODOS los proveedores que participaron deben volver a cotizar (BAFO); la oferta anterior queda versionada, ya no vigente; vuelve a Lanzamiento. 

**Reapertura (ventana):** si falta un proveedor y todavía NO se hizo la Apertura, el comprador puede reabrir momentáneamente el caso para que cargue su oferta. Una vez abierta la licitación, esta ventana ya no existe. 

El comprador ve el estado de cada cotización (Cotizado / Sin respuesta / Re-consulta) SIN ver montos hasta la Apertura. Casos borde: el proveedor puede rechazar o no cotizar → se registra y el caso 

avanza. Un caso puede cancelarse / declararse desierto con justificación, o reasignarse a otro comprador (Jefe de Compras). 

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
|**RF-01**|Crear la licitación (nº identfcador,<br>unidad opc., comprador,<br>código+descripción vía Excel,<br>moneda) + fecha límite SIEMPRE<br>obligatoria.|MVP|
|**RF-02**|Adjuntar la planilla de ítems +<br>planos/especifcaciones.|MVP|
|**RF-03**|Número único cuando el caso nace<br>a mano.|MVP|
|**RF-04**|Ver y seguir el estado según los<br>hitos(creación → … → adjudicada).|MVP|
|**RF-05**|Bandeja: necesidades pendientes<br>(comprador) + bandeja de área con<br>TODOS los casos(Jefe de Compras).|MVP|
|**RF-06**|Tomar los proveedores desde SAP<br>(export/lectura) y elegir al invitar<br>(desplegable/buscador).|MVP|
|**RF-07**|Invitar N proveedores (crea un<br>vínculo caso-proveedor por cada<br>uno).|MVP|
|**RF-08**|El proveedor recibe la invitación por<br>mail,entra alportal,la|MVP|



|**RF-09**|acepta/rechazayve solo lo suyo.<br>El proveedor descarga la planilla, la<br>completa y re-sube su oferta con<br>adjuntos.|MVP|
|---|---|---|
|**RF-10**|Portal bilingüe ES/EN para el<br>proveedor.|MVP|
|**RF-11**|Reconsultar (misma ronda, antes<br>del cierre) a todos o algunos — no<br>fuerza a recotzar a quien no fue<br>reconsultado.|MVP|
|**RF-12**|Q&A dentro del portal (toda<br>interacción formal en el portal, no<br>en el mail).|MVP|
|**RF-13**|Extensión: antes del cierre y sin ver<br>precios, sumar un proveedor nuevo<br>sin forzar recotzado a los demás.|MVP|
|**RF-14**|<br>El comprador ve el estado<br>(Cotzado/Sin respuesta/Re-<br>consulta) SIN ver montos hasta la<br>Apertura.|MVP|
|**RF-15**|Cerrar la recepción (manual) y<br>cierre automátco al vencer la fecha<br>límite(auto-close).|MVP|
|**RF-16**|Ofertas selladas hasta la Apertura<br>(transparencia).|MVP|
|**RF-17**|Ventana de reapertura entre Cierre<br>y Apertura, para que un proveedor<br>rezagado cargue su oferta — solo si<br>aún no se abrió.|MVP|
|**RF-18**|Cancelar / declarar desierta con<br>justfcación(queda la traza).|MVP|
|**RF-19**|Apertura: acción manual del<br>comprador para ver las ofertas.<br>Desde aquí ya no se puede extender<br>ni reabrir.|MVP|
|**RF-20**|2ª ronda (opcional, tras la<br>Apertura): TODOS los proveedores<br>deben recotzar; oferta anterior<br>versionada, ya no vigente.|MVP|
|**RF-21**|Jefe de Compras: reasignar un caso<br>a otro comprador de su misma área.|MVP|
|**RF-22**|Jefe de Compras: intervenir —<br>actuar sobre un caso de su área con<br>las mismas capacidades del<br>comprador ttular.|MVP|
|**RF-23**|Registrar la adjudicación (puede ser<br>parcial) → estado "Adjudicada". La<br>OC se hace en SAP.|MVP|
|**RF-24**|Repositorio consultable + búsqueda<br>por múltples campos + consulta<br>instantánea ~6 meses.|MVP|
|**RF-25**|Versionado: nunca pisar una oferta<br>(cada re-subida es versión; se marca<br>la vigente).|MVP|
|**RF-26**|Capturar datos crudos (fechas,<br>invitados vs respondieron, precios)<br>para KPIs.|MVP|



|**RF-27**|Hash de integridad por cada Excel<br>subido.|MVP|
|---|---|---|
|**RF-28**|Mail de invitación + aviso de oferta<br>recibida(al comprador).|MVP|
|**RF-29**|Notfcar al proveedor cuando se<br>cierra la recepción de su caso.|MVP|
|**RF-30**|Notfcar al proveedor cuando se<br>lanza una 2ª ronda (debe volver a<br>cotzar).|MVP|
|**RF-31**|Recordatorio de vencimiento al<br>proveedor que todavía no cargó su<br>oferta.|MVP|
|**RF-32**|Aislamiento entre proveedores +<br>confdencialidad entre áreas + roles<br>por área + auditoría de ediciones.|MVP|
|**RF-33**|Almacenamiento de archivos<br>pesados fuera de la base<br>(Drive/GCS)+ disponibilidad 24/7.|MVP|
|**RF-34**|Login interno con cuenta Google<br>(Workspace@bater.net).|MVP|
|**RF-35**|Comparatva dentro del portal<br>(armar/subir/retocar, automátca),<br>aprobación, export y memo (GEM);<br>normalización de moneda.|F2|
|**RF-36**|Conectar la<br>comparatva/adjudicación/memo<br>con SAP.|F2|
|**RF-37**|Buscar dentro de archivos · bajadas<br>masivas · 2FA · tpo de evento<br>(RFI/RFP)· carga masiva de casos.|F2|
|**RF-38**|DEYEL automátco · oferta ítem-por-<br>ítem · proveedor ve resultado ·<br>adjudicación por línea ·<br>scoring/TCO/subastas/envelope ·<br>dashboard KPIs · digests · reglas<br>editables · autoregistro/onboarding<br>de proveedores (gestón en SAP) ·<br>volcar OC a SAP · appmobile natva.|Fuera|



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

- 

## **12 · Fuera de alcance (Fase 2/3)** 

Armar el pedido/QR desde cero · memo GEM · comparativa automática · captura ítem-a-ítem · integración SAP de escritura · DEYEL por API (salvo se decida F1) · rankings/ahorro automático · SLP/onboarding-performance · recordatorios · reverse auctions · envelope bidding · TCO/landed cost. 

## **13 · Criterios de aceptación del MVP** 

7. CA-01: Un comprador crea una licitación (con fecha límite), adjunta el Excel, invita a 3 proveedores y estos reciben acceso — sin enviar un mail manual. 

8. CA-02: Los 3 proveedores entran, aceptan la invitación, descargan el Excel, suben su oferta con adjuntos, hacen una pregunta y uno rechaza — cada uno ve solo lo suyo (ofertas selladas). 

9. CA-03: El comprador cierra la recepción (manual o auto-close); como falta un proveedor y todavía no se abrió, reabre momentáneamente el caso para que cargue su oferta. 

10. CA-04: El comprador abre la licitación (acción manual) y ve las ofertas; a partir de ahí ya no puede extender ni reabrir. Se registra la adjudicación (puede ser parcial) → estado "Adjudicada". 

11. CA-05: El caso queda en el repositorio y se encuentra por QR, proveedor y fecha. 12. CA-06: La auditoría muestra la traza de ediciones del caso. 

13. CA-07 (seguridad): Un proveedor no puede acceder por URL directa a la QR-Proveedor de otro (verificado por revisor de seguridad). 

14. CA-08: Un Jefe de Compras ve todos los casos de su área, reasigna uno a otro comprador, y este puede seguir gestionándolo con el historial intacto. 

15. CA-09: Al cerrarse un caso, el/los proveedor(es) reciben un mail; al lanzarse una 2ª ronda, reciben aviso y deben volver a cotizar (oferta anterior versionada). 

## **14 · Entregables y hitos (sprints)** 

_De menor a mayor riesgo; cada sprint entrega algo funcionando. Estimaciones a cargo de Chivilo._ 

|**Sprint**|**Entrega**|
|---|---|
|**0 · Fundaciones**|Auth interna + externa, modelo de datos, sync de<br>proveedores desde SAP (reusar/extender LFA1-Sync),<br>almacenamiento de archivos.|
|**1 · Alta e invitación**|Crear caso (manual), invitar proveedores, bandejas<br>(comprador + Jefe de Compras), estados. Demo con un<br>caso real.|
|**2 · Interacción del proveedor**|Vista del proveedor, subir oferta + adjuntos, Q&A,<br>rechazar, reconsultas, extensión antes del cierre. El<br>corazón;validar aislamientoysellado.|
|**3 · Cierre, apertura y adjudicación**|Cerrar/auto-close, ventana de reapertura, apertura<br>manual, cancelar/desierta, reasignar/intervenir (Jefe de<br>Compras),registrar adjudicación.|
|**4 · 2ª ronda, repositorio y notfcaciones**|2ª ronda con versionado, búsqueda histórica, mails<br>(invitación,oferta,cierre,2ª ronda,recordatorio),|



<mark>auditoría.</mark> **Gate MVP** Cumplir CA-01..09 + revisión de seguridad → aprobación de Stage-Gate (sponsor Leo). 

## **15 · Restricciones técnicas y de gobierno** 

- Stack: lo decide Chivilo; sugerencia de herramientas Google. Debe cumplir los RNF (24/7, volumen, aislamiento, búsqueda). 

- Propiedad: el código y los datos viven en infraestructura de Batfer (GCP), no del proveedor. Handoff de credenciales por canal seguro; sin secretos en repos. 

- Datos sensibles: precios de proveedores = confidenciales; el aislamiento es requisito legal/reputacional. 

- Gobierno: producto bajo PrMO/Stage-Gate del TMO. Sponsor Leo aprueba pasajes de compuerta. 

## **Anexo A · Supuestos y pendientes** 

### **Resueltas en la ronda v0.4 (dudas cerradas con Renzo)** 

- [Resuelto] El "Aprobador" es el Jefe de Compras, coordinador de área (revisa/interviene/reasigna) — no aprueba comparativas en el MVP. 

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
|**D-01**|Sealed/blind bidding: el proveedor<br>nunca ve ofertas ajenas (blind);<br>ofertas no visibles hasta el cierre<br>(sealed). Diferencial central vs mail.|MVP|
|**D-02**|Audit trail append-only + hash del<br>Excel en cada submission.<br>Complianceydefensa ante|MVP|



||disputas.||
|---|---|---|
|**D-03**|Auto-close por deadline + soporte<br>de ≥2 rondas con archivo de la<br>rondaprevia.|MVP|
|**D-04**|Q&A anonimizado y visible a todos<br>los invitados. Equidad.|MVP|
|**D-05**|Adjudicación por línea con<br>justfcación(+3–7% ahorro).|MVP|
|**D-06**|Versionado de archivos: nunca<br>sobrescribir (re-subida = versión<br>nueva).|MVP|
|**D-07**|Campo event_type en el modelo<br>(RFQ hoy; RFI/RFP a futuro sin<br>repintar).|MVP (schema)|
|**D-08**|Normalización de moneda con FX a<br>fecha (hoy en el Excel externo;<br>interno cuando la comparatva<br>entre alportal).|MVP nota / F2|
|**D-09**|Reconsiderar login del proveedor →<br>magic link: la industria lo favorece<br>para terceros no técnicos; seguro<br>(single-use,expira) ysin fricción.|Decisión abierta|
|**D-10**|Instrumentar KPIs (fechas, invitados<br>vs respondieron, precios) →<br>savings, cycle tme, tasa de<br>respuesta.|MVP|
|**D-11**|Reverse auctons / envelope bidding<br>/ TCO / SLP completo → fuera de<br>alcance,documentados.|F2+|



