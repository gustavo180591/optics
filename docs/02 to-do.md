# To-Do: Plataforma de Óptica

## Prioridades Altas (MVP)

### 1. Configuración Inicial
- [ ] Configurar repositorio Git con estructura de carpetas
- [ ] Configurar Docker y docker-compose para entornos de desarrollo y producción
- [ ] Configurar SvelteKit 2 con TypeScript
- [ ] Configurar Prisma con PostgreSQL
- [ ] Configurar Tailwind CSS 4
- [ ] Configurar ESLint y Prettier
- [ ] Configurar testing (Vitest + Playwright)

### 2. Módulo de Autenticación y Perfiles
- [ ] Sistema de registro/login (email/Google)
- [ ] Perfiles de usuario (cliente, óptico, vendedor, backoffice, admin)
- [ ] Middleware de autorización por roles
- [ ] Protección de rutas

### 3. Gestión de Recetas
- [ ] Formulario de carga de receta (imagen/PDF/datos manuales)
- [ ] Validación de campos obligatorios (OD/OS, DNP/IPD, etc.)
- [ ] Verificación de vigencia (12 meses adultos / 6 meses niños)
- [ ] Almacenamiento seguro de recetas (cumpliendo LPD 25.326)

### 4. Catálogo de Productos
- [ ] CRUD de armazones
- [ ] CRUD de cristales y tratamientos
- [ ] Sistema de compatibilidad entre armazones y graduaciones
- [ ] Gestión de precios y descuentos

### 5. Integración con Obras Sociales
- [ ] Base de datos de obras sociales y planes
- [ ] Cálculo de cobertura y copago
- [ ] Flujo de autorización online/manual
- [ ] Integración con APIs de obras sociales (donde estén disponibles)

### 6. Carrito y Checkout
- [ ] Gestión del carrito de compras
- [ ] Integración con Mercado Pago
- [ ] Cálculo de cuotas e intereses
- [ ] Generación de factura electrónica (Factura A/B)

### 7. Gestión de Órdenes
- [ ] Seguimiento de estado de órdenes
- [ ] Notificaciones por email/WhatsApp
- [ ] Generación de órdenes de laboratorio
- [ ] Gestión de garantías

## Prioridades Medias

### 8. Panel de Administración
- [ ] Dashboard con métricas clave
- [ ] Gestión de usuarios y permisos
- [ ] Reportes de ventas y rentabilidad
- [ ] Gestión de inventario

### 9. Sucursales
- [ ] Gestión de múltiples sucursales
- [ ] Asignación de stock por sucursal
- [ ] Gestión de turnos para retiro

### 10. Integración con Laboratorio
- [ ] API para envío de trabajos al laboratorio
- [ ] Seguimiento de producción
- [ ] Notificaciones de estado

## Prioridades Bajas

### 11. Mejoras de UX/UI
- [ ] Pruebas de usabilidad
- [ ] Optimización de rendimiento
- [ ] Mejoras de accesibilidad (WCAG 2.1 AA)

### 12. Documentación
- [ ] Guía de usuario
- [ ] Documentación técnica
- [ ] Manual de procedimientos

## Tareas Legales y de Cumplimiento
- [ ] Términos y condiciones
- [ ] Política de privacidad
- [ ] Aviso legal
- [ ] Cumplimiento RGPD para clientes internacionales
- [ ] Documentación para habilitación comercial (Misiones/Posadas)

## Técnico
- [ ] Configuración de CI/CD
- [ ] Monitoreo y alertas
- [ ] Backup y recuperación de desastres
- [ ] Auditoría de seguridad

---

## Notas
- Todas las implementaciones deben cumplir con el marco legal detallado en `history.md`
- Priorizar accesibilidad y experiencia móvil
- Implementar métricas de seguimiento (GA4)
- Documentar decisiones técnicas importantes
