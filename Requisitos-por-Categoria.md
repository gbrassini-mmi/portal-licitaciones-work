# **Portal de Cotizaciones — Requisitos por categoría** 

Documento de trabajo — Compras Batfer (v0.3, tras revisión interna) 

_DOCUMENTO EDITABLE — dejanos tus comentarios_ 

**Alcance del MVP:** el portal es SOLO para la interacción con el proveedor y correr el proceso de licitación lo más regulado posible. La comparativa, la aprobación y el memo se hacen hoy en SAP (desconectado); el portal los incorpora en Fase 2 y luego se conecta con SAP. 

### **Hitos de la licitación** 

|**Hito**|**Quépasa**|
|---|---|
|**1 · Creación**|Se arma la licitación (borrador). Fecha límite SIEMPRE<br>obligatoria.<br>|
|**2 · Lanzamiento**|Se envía la licitación a losproveedores → cotzan.|
|**Extensión (antes del cierre)**|Se pueden sumar proveedores nuevos sin ver precios;<br>losqueya cotzaron NO tenenque volver a cotzar.|
|**3 · Cierre**|Bloqueo por plataforma: manual o automátco (vence la<br>fecha límite). No se cotza más ni entran proveedores<br>nuevos.|
|**Reapertura (ventana)**|El comprador puede reabrir momentáneamente el caso<br>para que un proveedor rezagado cargue su oferta —<br>SOLO si todavía no se hizo la Apertura.|
|**4 · Apertura**|Acción manual del comprador: recién acá ve las ofertas<br>(antes estaban selladas). Por transparencia, a partr de<br>la aperturaya no sepuede extender ni reabrir.|
|**5 · Comparatva**|En el MVP se hace fuera (SAP); el portal la incorpora en<br>Fase 2.|
|**2ª ronda (opcional, tras la comparatva)**|TODOS los proveedores que partciparon deben volver<br>a cotzar; la oferta anterior queda versionada, ya no<br>vigente. Vuelve a la etapa de Lanzamiento.|
|**6 · Adjudicación**|Se registra el proveedor elegido (puede ser parcial) →<br>estado 'Adjudicada'. La OC se hace en SAP.|



### **Categorías** 

|**Mínimo indispensable**<br>🟢|Sin esto el portal no reemplaza al mail. Es lo que se<br>construye primero.|
|---|---|
|**🔵 Agregar en versión 2**|Mejora clara (incluye<br>comparatva/adjudicación/memo dentro del portal +<br>conexión con SAP).|
|**Fuera de la plataforma**<br>**⚪**|Se consideró pero deliberadamente NO va (o lo<br>maneja SAP). Queda registrado.|



**Resumen: 42 mínimas · 11 para versión 2 · 17 fuera (70 en total).** 

_Cómo usar: revisá cada historia y anotá en la columna "Comentarios". Podés usar Control de cambios / Comentarios de Word._ 

## 🟢 **Mínimo indispensable** 

|**Área**<br>|**Historia de usuario**|**Comentarios del equipo**|
|---|---|---|
|1 · Alta y gestón del caso|Como**Comprador**, quiero crear un<br>caso de cotzación cargando nº<br>identfcador, unidad (opcional),<br>comprador, código y descripción<br>(Excel adjunto) y moneda, para<br>iniciar formalmente un pedido de<br>cotzación.||
|1 · Alta y gestón del caso|Como**Comprador**, quiero que el<br>sistema asigne un número único<br>cuando el caso nace a mano, para<br>identfcar cada cotzación sin<br>ambigüedad.||
|1 · Alta y gestón del caso<br>|Como**Comprador**, quiero adjuntar<br>la planilla Excel de ítems y los<br>planos/especifcaciones, para que<br>los proveedores tengan todo lo<br>necesariopara cotzar.||
|1 · Alta y gestón del caso<br>|Como**Comprador**, quiero ver y<br>seguir el estado del caso (creación<br>→ … → adjudicada), para saber en<br>qué etapa está cada cotzación.||
|1 · Alta y gestón del caso|Como**Comprador**, quiero tener un<br>listado (bandeja) de las necesidades<br>pendientes de cotzar, para<br>gestonar mi trabajopendiente.||
|2 · Proveedores e invitación|Como**Comprador**, quiero tener<br>disponible la lista de proveedores<br>de SAP (export/sync) para elegir al<br>invitar (desplegable/buscador), para<br>invitar sin recargar datos.||
|2 · Proveedores e invitación|Como**Comprador**, quiero invitar a<br>varios proveedores (del listado de<br>SAP) a un caso, para recibir ofertas<br>de distntas fuentes.||
|3 · Interacción con el proveedor|Como**Proveedor**, quiero recibir la<br>invitación por mail, acceder al<br>portal y aceptar o rechazar la<br>invitación, viendo solo lo mío, para<br>partcipar sin ver información de<br>otros.||
|3 · Interacción con el proveedor|Como**Proveedor**, quiero descargar<br>la planilla, completarla y re-subir mi<br>oferta con adjuntos, para enviar mi<br>cotzación de forma trazable.||
|3 · Interacción con el proveedor|<br>Como**Proveedor**, quiero usar el<br>portal en español o inglés, para<br>entender todo en mi idioma.||
|3 · Interacción con el proveedor|Como**Proveedor**, quiero<br>declinar/rechazar la cotzación<br>dejando constancia, para avisar que<br>nopartcipo sin usar mail.||



|3 · Interacción con el proveedor|Como**Comprador**, quiero<br>reconsultar (dentro de la misma<br>ronda, antes del cierre) a todos o a<br>algunos proveedores, para pedir<br>mejora o aclaración de la oferta.|
|---|---|
|3 · Interacción con el proveedor|Como**Proveedor / Comprador**,<br>quiero hacer y responder preguntas<br>dentro del portal, para registrar las<br>aclaraciones formalmente.|
|3 · Interacción con el proveedor|Como**Comprador**, quiero antes del<br>Cierre (sin haber visto precios),<br>poder sumar un proveedor nuevo<br>— extensión, para ampliar la<br>competencia mientras se mantenga<br>la equidad.|
|3 · Interacción con el proveedor|Como**Comprador**, quiero ver el<br>estado de cada cotzación sin ver el<br>detalle ni los precios, para hacer<br>seguimiento sin romper el sellado.|
|3 · Interacción con el proveedor|Como**Proveedor**, quiero recibir un<br>recordatorio antes del vencimiento<br>si todavía no cargué mi oferta, para<br>noperder la oportunidad de cotzar.|
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero cerrar la<br>recepción de ofertas, para pasar a la<br>etapa de apertura/comparación.|
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero que el<br>caso se cierre automátcamente al<br>vencer el plazo, para evitar ofertas<br>fuera de término.|
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero reabrir el<br>caso entre el Cierre y la Apertura,<br>para que un proveedor rezagado<br>cargue su oferta, para no perder<br>una oferta por un cierre<br>automátco.|
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero abrir la<br>licitación (acción manual) para ver<br>las ofertas de todos los<br>proveedores, para pasar a la etapa<br>de comparación.|
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero lanzar<br>una 2ª ronda tras la Apertura,<br>donde TODOS los proveedores que<br>partciparon deben volver a cotzar,<br>para negociar mejores condiciones<br>sinperder trazabilidad.|
|4 · Cierre, comparatva y adjudicación|Como**Jefe de Compras /**<br>**Comprador**, quiero cancelar/anular<br>un caso con registro (archivar o<br>declarar desierta), para descartar<br>cotzaciones sinperder la traza.|
|4 · Cierre, comparatva y adjudicación|<br>Como**Comprador**, quiero registrar<br>la adjudicación (elegir proveedor,<br>puede ser parcial), para dejar<br>constancia de la decisión.|
|4 · Cierre, comparatva y adjudicación|Como**Jefe de Compras**, quiero<br>tener una bandeja con TODOS los|



||casos de mi área (no solo los<br>propios), para actuar como<br>coordinador de mi equipo.|
|---|---|
|4 · Cierre, comparatva y adjudicación<br>|Como**Jefe de Compras**, quiero<br>reasignar un caso a otro comprador<br>de mi área, para redistribuir carga<br>de trabajo o cubrir una ausencia.|
|4 · Cierre, comparatva y adjudicación|Como**Jefe de Compras**, quiero<br>intervenir/actuar sobre un caso de<br>mi área con las mismas capacidades<br>que el comprador ttular, para<br>resolver algo sin esperar al<br>comprador ttular.|
|5 · Repositorio e histórico|<br>Como**Comprador**, quiero que todo<br>el caso quede guardado y<br>consultable, para tener un<br>repositorio único.|
|5 · Repositorio e histórico|Como**Comprador**, quiero buscar en<br>el histórico por varios campos, para<br>encontrar cotzacionespasadas.|
|5 · Repositorio e histórico|<br>Como**Comprador**, quiero consultar<br>al instante lo de los últmos ~6<br>meses, para trabajar con lo reciente<br>sin demora.|
|5 · Repositorio e histórico|Como**Comprador**, quiero que<br>nunca se pise una oferta<br>(versionado), para no perder<br>versiones anteriores.|
|5 · Repositorio e histórico|Como**Responsable de datos**, quiero<br>que se capturen datos crudos<br>(fechas, invitados vs respondieron,<br>precios), para poder medir<br>indicadores.|
|6 · Notfcaciones|Como**Comprador / Proveedor**,<br>quiero recibir mail de invitación (y<br>aviso de oferta recibida), para<br>enterarme sin revisar elportal.|
|6 · Notfcaciones|Como**Proveedor**, quiero recibir un<br>aviso cuando se cierra la recepción<br>de mi caso, para saber que ya no<br>puedo cargar cambios.|
|6 · Notfcaciones|Como**Proveedor**, quiero recibir un<br>aviso cuando se lanza una 2ª ronda,<br>para saber que debo volver a<br>cotzar.|
|7 · Seguridad y trazabilidad|Como**Responsable de seguridad**,<br>quiero garantzar el aislamiento<br>total entre proveedores, para que<br>nadie vea ofertas ajenas.|
|7 · Seguridad y trazabilidad|Como**Responsable de seguridad**,<br>quiero asegurar confdencialidad<br>entre áreas/unidades, para que no<br>se cruce info entre compradores.|
|7 · Seguridad y trazabilidad|Como**Admin**, quiero defnir roles<br>con alcance por área/región, para<br>que cada uno vea/haga solo lo suyo.|
|7 · Seguridad y trazabilidad|Como**Responsable de datos**, quiero<br>tener auditoría completa de|



||ediciones, para saber quién hizo<br>quéycuándo.|
|---|---|
|7 · Seguridad y trazabilidad|Como**Responsable de seguridad**,<br>quiero que las ofertas queden<br>selladas hasta la apertura, para<br>garantzar la transparencia (nadie ve<br>precios antes del cierre).|
|7 · Seguridad y trazabilidad|Como**Responsable de seguridad**,<br>quiero tener un hash de integridad<br>por cada Excel subido, para detectar<br>archivos alterados.|
|8 · Base técnica|Como**Responsable de datos**, quiero<br>guardar los archivos pesados fuera<br>de la base (Drive/GCS), para<br>soportar el volumen de archivos.|
|8 · Base técnica|Como**Responsable de datos**, quiero<br>tener disponibilidad 24/7, para que<br>proveedores de otros husos puedan<br>operar.|



## **🔵 Agregar en versión 2** 

|**Área**|**Historia de usuario**|**Comentarios del equipo**|
|---|---|---|
|1 · Alta y gestón del caso<br>|Como**Comprador**, quiero cargar<br>varias cotzaciones juntas desde un<br>Excel, para ahorrar tempo en<br>cargas masivas.||
|4 · Cierre, comparatva y adjudicación<br>|Como**Comprador**, quiero<br>armar/subir la comparatva dentro<br>del portal y retocarla, para dejar el<br>análisis formal en elportal.||
|4 · Cierre, comparatva y adjudicación<br>|Como**Aprobador**, quiero aprobar o<br>rechazar la comparatva en el<br>portal, para dar el visto bueno antes<br>de adjudicar.||
|4 · Cierre, comparatva y adjudicación<br>|Como**Comprador**, quiero exportar<br>la comparatva a Excel, para<br>compartrla o adjuntarla a la OC.||
|4 · Cierre, comparatva y adjudicación<br>|Como**Comprador**, quiero que el<br>portal arme la comparatva<br>automátcamente desde las ofertas,<br>para evitar el armado manual.||
|4 · Cierre, comparatva y adjudicación<br>|Como**Comprador**, quiero que las<br>ofertas en distnta moneda se<br>normalicen (USD/ARS, según área),<br>para comparar en una base común.||
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero generar el<br>memo de adjudicación (GEM) desde<br>elportal, para completar el circuito.||
|5 · Repositorio e histórico|Como**Comprador**, quiero buscar<br>dentro del contenido de los<br>archivos, para encontrar un part<br>number dentro de un PDF.||
|5 · Repositorio e histórico|Como**IT**, quiero hacer bajadas<br>masivas del histórico, para||



||respaldosymigraciones.|
|---|---|
|7 · Seguridad y trazabilidad|Como**Admin**, quiero que el ingreso<br>interno sea con cuenta de Google (y<br>2FA a futuro), para reforzar la<br>seguridad de cuentas internas.|
|8 · Base técnica|Como**Responsable de datos**, quiero<br>dejar previsto el 'tpo de evento' en<br>el modelo (RFI/RFP a futuro), para<br>no rehacer el modelo después.|



## **⚪ Fuera de la plataforma** 

_Lo siguiente NO va en la plataforma (o lo maneja SAP). Se deja registrado para constancia._ 

|**Área**<br>|**Historia de usuario**|**Comentarios del equipo**|
|---|---|---|
|1 · Alta y gestón del caso|Como**Comprador**, quiero que el<br>caso traiga datos automátcamente<br>desde DEYEL.||
|2 · Proveedores e invitación|Como**Jefe de Desarrollo de**<br>**Proveedores**, quiero dar de alta /<br>editar / actvar proveedores en el<br>portal.||
|2 · Proveedores e invitación|Como**Proveedor**, quiero<br>registrarme por mi cuenta en el<br>portal.||
|2 · Proveedores e invitación|Como**Jefe de Desarrollo de**<br>**Proveedores**, quiero gestonar el<br>onboarding completo (datos<br>bancarios, califcación,<br>performance).||
|3 · Interacción con el proveedor|Como**Comprador**, quiero que las<br>preguntas se anonimicen y la<br>respuesta llegue a todos los<br>invitados.||
|3 · Interacción con el proveedor|Como**Proveedor**, quiero cargar mi<br>oferta ítempor ítem en elportal.||
|3 · Interacción con el proveedor|Como**Proveedor**, quiero ver si mi<br>oferta fue elegida.||
|4 · Cierre, comparatva y adjudicación<br>|Como**Comprador**, quiero adjudicar<br>por línea a distntosproveedores.||
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero evaluar<br>con scoring ponderado técnico +<br>comercial.||
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero ver el<br>costo total (TCO / landed cost por<br>Incoterm).||
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero correr<br>subastas inversas.||
|4 · Cierre, comparatva y adjudicación|Como**Comprador**, quiero usar<br>sobre técnico/comercial separado<br>(envelope bidding).||
|5 · Repositorio e histórico|Como**Gerencia**, quiero ver un<br>dashboard de indicadores(ahorro,||



||tempos,tasa de respuesta).|
|---|---|
|6 · Notfcaciones|Como**Comprador**, quiero enviar<br>recordatorios y resúmenes<br>automátcos(digests).|
|7 · Seguridad y trazabilidad|Como**Admin**, quiero editar las<br>reglas de negocio sin reprogramar.|
|8 · Base técnica|Como**Comprador**, quiero volcar la<br>adjudicación a SAP<br>automátcamente.|
|8 · Base técnica|Como**Proveedor**, quiero usar una<br>appmobile natva.|



