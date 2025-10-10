# optics — history.md
> Plataforma de óptica (venta de lentes) orientada a Argentina. Alcance MVP listo para pasar a desarrollo con SvelteKit 2 (Svelte 5), Prisma, PostgreSQL, Docker, Tailwind 4. Ubicación comercial principal: **Misiones, Posadas** (otras sucursales opcionales).

## Marco Legal

### Nacional (Argentina)
- **Ley 24.240** de Defensa del Consumidor
- **Ley 25.326** de Protección de Datos Personales
- **Ley 25.506** de Firma Digital
- **Ley 26.653** de Accesibilidad Web
- **Resolución General 4.247/18** AFIP (Facturación Electrónica)
- **Ley 19.587** de Higiene y Seguridad en el Trabajo
- **Régimen de Sello de Seguridad** para lentes recetados

### Provincial (Misiones)
- **Ley 26.951** de Protección de Datos Personales
- **Código de Faltas de Misiones** (Art. 22 - Protección al Consumidor)
- **Ley VI - N.° 25** (antes Ley 3.626) - Ejercicio de la Óptica

### Municipal (Posadas)
- **Ordenanza II - N.° 49** (antes 2.255) - Comercio e Industria
- **Ordenanza II - N.° 32** (antes 2.121) - Publicidad y Cartelería
- **Ordenanza II - N.° 19** (antes 1.817) - Habilitaciones Comerciales

### Políticas de la Plataforma
- **Protección de Datos**: Cumplimiento estricto de LPD 25.326 y normativa provincial
- **Garantías**: 1 año por defecto, según normativa vigente
- **Devoluciones**: 10 días hábiles desde la recepción (Ley 24.240)
- **Accesibilidad**: Nivel AA de WCAG 2.1
- **Sustentabilidad**: Gestión responsable de residuos según ley 25.916

---

---

## 0. Contexto
- **Objetivo:** permitir a clientes comprar **lentes recetados** con cobertura de **obra social/prepaga**, con **retiro en sucursal** o **envío**, garantizando validaciones clínicas y trazabilidad.
- **Mercado:** Argentina (AFIP, Ley 24.240, Ley 25.326).
- **Modelo operativo:** catálogo curado, reglas de compatibilidad marco–cristal, autorización de obra social (online o manual), laboratorio propio/tercerizado.
- **Ubicación inicial:** Posadas, Misiones (sucursal principal). Sucursales adicionales configurables.

---

## 1. Personas (resumen)
- **Cliente final**: tiene receta vigente; busca precio final con su obra social y plazos claros.
- **Óptico/a**: valida receta y compatibilidad, gestiona orden de laboratorio, post-venta.
- **Vendedor/a**: asiste en selección y checkout, coordina retiro/entrega.
- **Backoffice**: autoriza obras sociales manualmente, controla facturación.
- **Admin**: define precios, políticas y métricas.

---

## 2. Épica
**EP-01 — Compra de lentes recetados con obra social y retiro en sucursal**  
Como cliente con receta vigente en Argentina, quiero cargar mi receta, elegir armazón y cristales con tratamientos, ver el precio final con mi obra social y pagar en cuotas, para retirar mis lentes en la sucursal de Posadas (o recibirlos a domicilio) con garantía y seguimiento.

### Definición de Hecho (DoD) de la épica
- Flujo usable mobile/desktop, **LCP < 2.5s**, A11y **AA**.
- Integraciones: **Mercado Pago** (sandbox+prod), notificaciones (email/WhatsApp), gestor de archivos de receta (PDF/JPG).
- Reglas clínicas y de compatibilidad aplicadas en servidor, con mensajes de error/ayuda.
- Desglose fiscal correcto (Factura B/A).  
- Métricas GA4 del funnel: Receta → Selección → Obra social → Pago → Entrega.

---

## 3. Historias de usuario (MVP)

### US-01 — Carga y validación de receta
**Como** cliente con receta vigente,  
**quiero** subir una foto o PDF de mi receta (o ingresar datos manuales),  
**para** que el sistema valide vigencia y campos clínicos y pueda continuar la compra.

**Criterios de aceptación**  
- Given subo receta, When el sistema procesa, Then valida fecha (≤12 meses adultos; ≤6 meses pediátrico), legibilidad y campos OD/OS (esfera, cilindro, eje, **DNP/IPD**, adición si aplica).  
- Si falta DNP/IPD: mostrar guía casera; **bloquear progresivo** hasta medición presencial.  
- Receta inválida: explicar motivo y opciones (turno, subir nueva).

---

### US-02 — Selección de armazón y compatibilidad
**Como** cliente,  
**quiero** elegir un armazón compatible con mi receta,  
**para** evitar combinaciones inviables y asegurar confort.

**Criterios de aceptación**  
- Dado un armazón seleccionado, el sistema calcula viabilidad según curva base/tamaño y mi graduación.  
- Si |D| > ±4.00 → sugerir índice **1.67**; si |D| > ±6.00 → **1.74** y marcos menos curvos.  
- Infantil: sugerir policarbonato.  
- Bloquear si la combinación es clínicamente inviable (mensaje con recomendación).

---

### US-03 — Configuración de cristales y tratamientos
**Como** cliente,  
**quiero** elegir material (CR-39/PC/1.60/1.67/1.74), diseño (mono/progresivo) y tratamientos (AR, blue, fotocromático),  
**para** obtener el balance entre estética, resistencia y precio.

**Criterios de aceptación**  
- Mostrar sólo combinaciones compatibles (p. ej., fotocromático disponible según proveedor).  
- Cambios de opción recalculan precio y plazo estimado (exprés/estándar).

---

### US-04 — Aplicación de obra social/prepaga
**Como** cliente con obra social,  
**quiero** ingresar mi plan para ver **precio final** y **copago**,  
**para** decidir y pagar con claridad.

**Criterios de aceptación**  
- Ingresar proveedor y plan → mostrar **desglose**: precio lista, **aporte**, **tope**, **copago** y total.  
- Autorización **online**: cálculo inmediato. Autorización **manual**: permitir “**pago reserva**” y confirmar saldo tras auditoría.  
- Si el plan no cubre progresivo premium, ofrecer alternativas.

---

### US-05 — Checkout, pago y cuotas
**Como** cliente,  
**quiero** pagar con Mercado Pago (débito/crédito, cuotas) o transferencia,  
**para** completar la compra sin fricción.

**Criterios de aceptación**  
- Carrito muestra total final con impuestos y cuotas disponibles.  
- Webhook de MP registra estado (aprobado, pendiente, rechazado).  
- Generar **número de pedido** y **Factura B** (o A si corresponde).

---

### US-06 — Entrega: retiro en sucursal Posadas / envío
**Como** cliente,  
**quiero** elegir **retiro en sucursal** (Posadas por defecto) o **envío** (Correo/OCA/moto),  
**para** recibir mis lentes cuando me convenga.

**Criterios de aceptación**  
- Mapa/listado de sucursales (Posadas, futuras adicionales).  
- Envío: costo y plazos por CP; tracking disponible.  
- Notificaciones en estados clave: En laboratorio → Listo para retirar/enviar.

---

### US-07 — Orden de laboratorio y seguimiento
**Como** Óptico/a,  
**quiero** generar y gestionar la **Orden de Laboratorio**,  
**para** asegurar trazabilidad del armado y tiempos.

**Criterios de aceptación**  
- Estados: **En validación → En laboratorio → Montaje → Listo → Entregado**.  
- Estimación de tiempos: estándar 3–5 hábiles (mono con AR), progresivo 5–7; **exprés 24–48 h** si stock.  
- Log de eventos (quién, cuándo, qué).

---

### US-08 — Garantía, ajustes y post-venta
**Como** cliente,  
**quiero** soporte post-venta (ajustes gratuitos, garantía),  
**para** resolver molestias y defectos.

**Criterios de aceptación**  
- Garantía fabricación: **6 meses**; **adaptación progresivos 30 días** (1 cambio o downgrade).  
- Agenda de ajuste en sucursal; registro de RMA/servicio.

---

## 4. Reglas de negocio (AR)
- Receta adulta válida **≤12 meses**; pediátrica **≤6 meses**.  
- Progresivos requieren **medición presencial de DNP/IPD** si falta o es dudosa.  
- |D| > ±4.00 → sugerir índice 1.67; |D| > ±6.00 → 1.74 (si disponible).  
- Obras sociales: mostrar siempre **lista → aporte → copago → total**.  
- Datos personales y de salud conforme **Ley 25.326** (consentimiento, finalidad, retención 24 meses mínimo).  
- Botón de Arrepentimiento (Ley 24.240): aplica a armazón sin personalizar; lentes personalizados sin devolución (sí garantía).

---

## 5. No-funcionales
- **Rendimiento:** LCP < 2.5s, TTFB < 800ms en rutas críticas.  
- **Seguridad:** RBAC (Admin, Óptico, Vendedor, Backoffice), auditoría de cambios, cifrado en tránsito y reposo.  
- **Disponibilidad:** backups automáticos de PostgreSQL, RTO ≤ 4h, RPO ≤ 1h.  
- **Accesibilidad:** WCAG **AA**.  
- **Observabilidad:** logs y métricas por paso del funnel y por estado de orden.

---

## 6. Supuestos iniciales (ajustables en Posadas)
- Sucursal **Posadas** con taller exprés para monofocal en stock.  
- Envíos por Correo Argentino/OCA; moto en casco urbano de Posadas.  
- Autorizaciones de OSDE (online) y Swiss/IOMA/PAMI (manual) como ejemplo inicial (parametrizable).

---

## 7. Criterios de aceptación globales (E2E)
1) **Receta válida** → puedo avanzar sin bloqueos indebidos.  
2) **Compatibilidad** marco–cristal → el sistema **bloquea** lo inviable y **sugiere** alternativas.  
3) **Precio final** con obra social visible antes del pago, con cuotas claras.  
4) **Pago** registrado vía webhook, con comprobante fiscal.  
5) **Seguimiento** del pedido con estados y notificaciones.  
6) **Entrega** confiable (retiro Posadas o envío con tracking).  
7) **Post-venta** accesible (ajustes y garantía).

---

## 8. Escenarios BDD (muestras)

### Escenario 1 — Cobertura aplicada correctamente
**Given** cliente con OSDE 310 y receta válida,  
**When** selecciona armazón $80.000 + monofocal 1.60 $85.000 + AR $25.000,  
**Then** el sistema aplica **tope $70.000** y muestra **copago** y **total** final; ofrece cuotas por Mercado Pago.

### Escenario 2 — Receta sin DNP/IPD
**Given** receta sin DNP/IPD,  
**When** el cliente intenta elegir progresivos,  
**Then** el sistema bloquea la continuación y ofrece **medición presencial en Posadas** o guía casera para monofocal.

### Escenario 3 — Exprés elegible
**Given** monofocal CR-39 + AR con stock de diámetro, ingreso antes de 12:00,  
**When** confirma compra con retiro en Posadas,  
**Then** el sistema estima **24–48 h**, notifica “Listo para retirar” y permite agenda de ajuste.

### Escenario 4 — Autorización manual con pago reserva
**Given** plan con autorización manual,  
**When** el cliente elige “pago reserva”,  
**Then** se confirma el pedido en “En validación” y, al aprobarse, se ajusta el saldo (cobro o reintegro).

---

## 9. Métricas y reportes
- **KPIs**: conversión, tasa de exprés, tiempo en laboratorio por tipo, % recetas con faltantes, NPS post-retiro, margen.  
- **Reportes**: ventas por sucursal/plan, autorizaciones rechazadas, SLA lab, top/slow movers.  
- **Alertas**: pedido >5 días en “En laboratorio”, receta vencida, stock crítico, pagos fallidos.

---

## 10. Entidades (visión para Prisma)
- `User`, `Role`, `Permission`  
- `Customer`  
- `Prescription` (OD/OS: esfera, cilindro, eje, DNP/IPD, adición, fecha, profesional, archivo)  
- `Frame` (marca, modelo, material, tamaño, curva, compatibilidad)  
- `LensType` (material, índice, rango, diseño)  
- `Treatment`  
- `Insurance`, `InsurancePlan` (tope, copago, autorización)  
- `PriceRule` (por rango/paquete/plan)  
- `Branch` (Posadas por defecto), `BranchStock`  
- `Order`, `OrderItem`, `Payment`, `Invoice`  
- `LabWorkOrder`, `OrderStatus`, `OrderEvent`  
- `Shipment` (carrier, tracking)  
- `Warranty`, `RMA`  
- `NotificationTemplate`

---

## 11. Fuera de alcance (MVP)
- Try‑on AR de marcos, agenda de turnos completa, factura electrónica AFIP integrada, progresivos con personalización avanzada (fase 2).

---

## 12. Preguntas abiertas
- ¿Listado final de obras sociales en Posadas y topes reales por plan?  
- ¿Laboratorio propio o tercerizado en cada tipo?  
- ¿Política de devoluciones específica provincial/municipal adicional?

---

**Fin del documento.**  
Este `history.md` es base contractual de alcance MVP para **optics**; actualizar conforme definiciones locales de Posadas, Misiones.
