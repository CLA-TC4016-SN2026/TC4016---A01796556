# ADR-001: Selección del estilo arquitectónico para StockIV

**Fecha:** 2026-10-12  
**Estado:** Aceptado  
**Responsable:** Israel Agustín Vargas Monroy  
**Matrícula:** A01796556  
**Proyecto:** StockIV  
**SRS de referencia:** versión 1.0, validada para diseño académico

## 1. Contexto

StockIV es una aplicación web para controlar el inventario de una tienda de ropa, calzado y artículos deportivos con dos o tres sucursales. El sistema debe autenticar usuarios, aplicar permisos por rol, administrar productos y sucursales, consultar existencias, registrar entradas y salidas, ejecutar transferencias atómicas, configurar mínimos, mostrar alertas y conservar un historial inmutable.

El sistema será desarrollado por una sola persona durante el curso. Esta restricción favorece una arquitectura que permita implementar y probar rápidamente el producto sin asumir el costo operativo de múltiples servicios. Al mismo tiempo, el diseño debe conservar límites internos claros para evitar un código monolítico difícil de mantener.

### 1.1 Impulsores arquitectónicos derivados del SRS

| Requerimiento | Implicación para la arquitectura |
|---|---|
| RNF-01 — 95 % de consultas en ≤ 2 s con 50 usuarios, 10 000 productos y 100 000 movimientos | Consultas paginadas, índices en base de datos, conexiones agrupadas y ausencia de llamadas distribuidas innecesarias. |
| RNF-02 — Contraseñas con hash adaptativo, HTTPS y autorización en backend | Capa de seguridad central, terminación TLS, autenticación JWT y RBAC aplicado antes de los casos de uso. |
| RNF-03 — Sesión expira tras 30 minutos de inactividad y cierre invalida token | Servicio de autenticación cohesivo y registro de identificadores de token revocados o sesiones activas. |
| RNF-04 — 99.5 % de disponibilidad mensual | Contenedores reiniciables, health checks, monitoreo y despliegue reproducible. No exige todavía redundancia multirregión. |
| RNF-05 — Respaldo diario, RPO 24 h y RTO 4 h | PostgreSQL con volumen persistente o servicio administrado y trabajo programado de respaldo. |
| RNF-06 y RNF-07 — Usabilidad y navegadores modernos | Frontend web desacoplado de la lógica de negocio, con interfaz responsiva. |
| RNF-08 — Reglas de dominio con ≥ 80 % de cobertura | Dominio y servicios de aplicación independientes de HTTP y persistencia para permitir pruebas unitarias. |
| RF-06/RD-04 — Transferencia todo-o-nada | Transacción local ACID mediante un patrón Unit of Work; evita coordinación distribuida. |
| RF-08/RD-05 — Movimientos inmutables | Modelo de dominio que no expone actualización ni eliminación de movimientos; las correcciones son compensatorias. |

## 2. Alternativas consideradas

### Alternativa A — Cliente-servidor con monolito modular en capas

Un frontend React se comunica mediante HTTPS/JSON con una API FastAPI. El backend se despliega como una sola unidad, pero se divide internamente en módulos de autenticación, usuarios, catálogo, inventario, movimientos, transferencias y alertas. Cada módulo sigue las capas de presentación, aplicación, dominio e infraestructura. PostgreSQL conserva el estado transaccional.

### Alternativa B — Microservicios por capacidad de negocio

Autenticación, catálogo, inventario, transferencias y alertas se separarían como servicios desplegables, cada uno con API y, de ser posible, almacenamiento propio. La comunicación combinaría HTTP y eventos.

### Alternativa C — Arquitectura orientada a eventos y funciones serverless

Las operaciones se recibirían mediante funciones independientes; los movimientos publicarían eventos para actualizar existencias, alertas y auditoría. Servicios administrados ejecutarían las funciones y el procesamiento asíncrono.

### 2.1 Comparación de trade-offs

| Criterio | A. Monolito modular en capas | B. Microservicios | C. Eventos y serverless |
|---|---|---|---|
| Escalabilidad | Escala verticalmente y puede replicar frontend/API. Para 50 usuarios y el volumen del SRS ofrece margen suficiente. La API completa escala como una unidad. | Permite escalar inventario o consultas de forma independiente, pero el volumen actual no justifica esa granularidad. | Escala automáticamente por función y absorbe picos, aunque la consistencia entre consumidores puede ser eventual. |
| Mantenibilidad | Módulos y dependencias dirigidas permiten cambiar una capacidad sin distribuir el código en varios repositorios. Una sola migración y un solo contrato operativo. | Límites fuertes, pero exige versionar contratos, rastreo distribuido y sincronizar cambios entre servicios. | Componentes pequeños, pero los flujos quedan repartidos entre eventos, colas, funciones y políticas cloud. |
| Complejidad de implementación | Baja a media: una API, una base de datos y transacciones ACID locales. Docker Compose reproduce el entorno. | Alta: descubrimiento, gateway, observabilidad, despliegues, tolerancia a fallas y consistencia distribuida. | Alta para este dominio: idempotencia de eventos, reintentos, orden, *dead-letter queues* y depuración asíncrona. |
| Adecuación a un desarrollador | Alta. Permite completar el alcance en 12 semanas y mantener pruebas y documentación en un solo flujo. | Baja. El costo operativo y cognitivo desplaza esfuerzo fuera de las funciones evaluadas. | Media-baja. Reduce administración de servidores, pero crea dependencia de plataforma y una curva de aprendizaje innecesaria. |
| Transferencias de inventario | Una transacción PostgreSQL confirma o revierte origen, destino y movimientos. | Requeriría saga, compensaciones o base compartida, reduciendo la independencia prometida. | La actualización asíncrona dificulta garantizar que el usuario observe inmediatamente el estado todo-o-nada. |
| Disponibilidad y operación | Suficiente con health checks, reinicio de contenedores, respaldo y monitoreo. El backend es un punto único que debe vigilarse. | Puede aislar fallas, pero introduce más puntos de falla y mayor esfuerzo de operación. | El proveedor administra gran parte de la disponibilidad; los límites y fallas transitorias deben manejarse explícitamente. |

## 3. Decisión

Se adopta una **arquitectura cliente-servidor con un monolito modular en capas**, desplegado en contenedores. La decisión combina dos niveles:

1. **Nivel arquitectónico:** Browser → frontend React/Vite → API REST FastAPI → PostgreSQL.
2. **Nivel interno del backend:** presentación/API → servicios de aplicación → dominio → infraestructura/persistencia.

La API será el único punto de acceso a las reglas de negocio. El frontend nunca se conectará directamente a PostgreSQL. Los módulos de backend tendrán interfaces explícitas y compartirán una base de datos relacional, pero cada módulo será propietario lógico de sus tablas y reglas.

La elección satisface el tamaño actual, la necesidad de consistencia fuerte y la capacidad de un desarrollador. También conserva una ruta de evolución: si las métricas futuras demuestran una necesidad real, un módulo con límites estables puede extraerse como servicio sin diseñar desde ahora una plataforma distribuida.

## 4. Vista lógica por capas

| Capa | Responsabilidad | Dependencias permitidas |
|---|---|---|
| Presentación | Endpoints FastAPI, validación de DTO, autenticación, traducción de errores a HTTP. | Aplicación. |
| Aplicación | Orquesta casos de uso: crear producto, registrar movimiento, transferir inventario y consultar historial. | Dominio y puertos. |
| Dominio | Entidades, objetos de valor, invariantes y políticas de inventario. | No depende de FastAPI, SQLAlchemy ni PostgreSQL. |
| Infraestructura | Implementa repositorios, Unit of Work, JWT, hash de contraseñas, PostgreSQL y observabilidad. | Interfaces definidas por aplicación/dominio. |
| Frontend | Presenta pantallas, estado de sesión, formularios, tablas, filtros y errores. | Contrato OpenAPI mediante HTTPS; no contiene reglas críticas de autorización o inventario. |

Esta dirección de dependencias aplica separación de responsabilidades e inversión de dependencias: el dominio expresa reglas y los detalles externos implementan interfaces, en lugar de que las reglas dependan de frameworks (IEEE Computer Society, 2024).

## 5. Patrones seleccionados

| Patrón | Aplicación en StockIV | Justificación |
|---|---|---|
| Repository | `UserRepository`, `ProductRepository`, `BranchRepository`, `StockRepository` y `MovementRepository`. | Aísla consultas SQLAlchemy y permite probar servicios con dobles de prueba. |
| Service Layer / Application Service | Servicios por caso de uso, sin lógica de presentación. | Centraliza transacciones y permisos sin duplicarlos entre endpoints. |
| Unit of Work | Una unidad transaccional controla movimientos y transferencias. | RF-06 exige confirmar o revertir origen, destino y ambos movimientos como un conjunto. |
| Dependency Injection | FastAPI proporciona repositorios, UoW, autenticador y configuración. | Reduce acoplamiento y facilita pruebas. |
| DTO | Modelos Pydantic de entrada y salida separados de entidades persistentes. | Mantiene estable el contrato OpenAPI y evita exponer campos internos. |
| RBAC | Políticas para Administrador y Encargado de inventario. | Implementa RF-02 y RNF-02 en backend. |
| Movimiento compensatorio | Una corrección crea un nuevo movimiento que referencia al original. | Preserva inmutabilidad y auditoría de RD-05. |

No se adopta CQRS completo, Event Sourcing ni una saga distribuida. El historial de movimientos es una bitácora de dominio, pero la existencia actual sigue siendo un estado transaccional explícito para mantener consultas sencillas y rápidas.

## 6. Consecuencias

### 6.1 Consecuencias positivas

- Las transferencias usan una sola transacción ACID, lo que reduce el riesgo de movimientos parciales.
- Las reglas se pueden probar sin levantar servidor web ni base de datos, apoyando RNF-08.
- El despliegue y diagnóstico son manejables para una persona.
- La separación frontend/API permite evolucionar la experiencia de usuario sin duplicar reglas críticas.
- PostgreSQL aporta restricciones, índices y consultas relacionales adecuadas para inventario e historial.
- Los módulos reducen el acoplamiento y facilitan una posible extracción futura.

### 6.2 Consecuencias negativas y mitigaciones

| Consecuencia | Mitigación |
|---|---|
| El backend completo escala como una unidad. | Medir antes de optimizar; replicar el contenedor API detrás del proxy y mantenerlo sin estado cuando el volumen lo requiera. |
| Una falla de la API afecta todas las capacidades. | Dos réplicas sin estado detrás de Nginx, health checks, reinicio automático, registros estructurados y monitoreo. |
| Una base compartida puede permitir accesos indebidos entre módulos. | Repositorios por módulo, revisión de dependencias y migraciones controladas con Alembic. |
| El token JWT debe poder invalidarse al cerrar sesión. | `jti`, expiración corta y registro de sesiones/revocaciones consultado por el backend. |
| El despliegue inicial usa un solo host y una sola región. | Se seleccionará una instancia con SLA del proveedor de al menos 99.5 %, se medirá disponibilidad y se conservarán respaldos externos. Si no se cumple el objetivo, el ADR se revisará para distribuir réplicas entre hosts o zonas. |

## 7. Interfaces y decisiones de despliegue

- Browser ↔ frontend: HTTPS.
- Frontend ↔ backend: REST/JSON sobre HTTPS, contrato OpenAPI 3.0.3 y Bearer JWT.
- Backend ↔ PostgreSQL: SQL mediante SQLAlchemy y conexión cifrada cuando la plataforma lo permita.
- Proxy ↔ contenedores: red privada; solo el proxy publica puertos externos.
- Disponibilidad de aplicación: Nginx balancea dos réplicas sin estado de FastAPI dentro del host inicial; el riesgo residual del host único se acepta y monitorea en la primera versión.
- Respaldo ↔ almacenamiento externo: trabajo diario cifrado, con retención y prueba de restauración.
- Configuración y secretos: variables de entorno o almacén de secretos; nunca dentro del repositorio.

## 8. Revisión crítica de la propuesta del agente

La propuesta inicial del agente fue revisada antes de aceptar el modelo. Se realizaron las siguientes decisiones propias:

### Decisión 1 — Existencia por producto y sucursal

La primera propuesta colocaba `quantity` y `minimumStock` dentro de `Product`. Se sustituyó por la clase asociativa `StockLevel`, identificada por producto y sucursal. La modificación es necesaria porque RF-04 exige desglose multisucursal y RF-06 transfiere unidades entre dos ubicaciones. Un atributo único en `Product` no podría representar cantidades distintas ni umbrales por sucursal.

### Decisión 2 — Alertas derivadas, no persistidas

El agente propuso una entidad `Alert` con estado activo/resuelto. Se reemplazó por `LowStockAlert <<derived>>`, calculada cuando `quantity <= minimumThreshold`. Esto evita sincronizar una segunda fuente de verdad. RF-07 solicita mostrar u ocultar la alerta según existencia y umbral; no exige un ciclo de vida independiente.

### Decisión 3 — Movimientos inmutables y compensatorios

La propuesta CRUD incluía editar y eliminar `Movement`. Se eliminaron esas operaciones y se añadió la asociación opcional `compensates`, porque RF-08-AC-2 y RD-05 exigen conservar los movimientos confirmados. Una corrección genera otro movimiento y mantiene la cadena de auditoría.

### Decisión 4 — Transferencia como agregado transaccional

El agente representaba la transferencia únicamente como una salida seguida de una entrada. Se creó `Transfer` como raíz que compone exactamente dos movimientos relacionados. Así existe un identificador común y un límite transaccional explícito para RF-06-AC-1 y RF-06-AC-2.

### Decisión 5 — Monolito modular en lugar de microservicios

Se rechazó separar autenticación, catálogo e inventario desde la primera versión. Con un desarrollador y 50 usuarios concurrentes, la separación física no mejora un atributo medible del SRS y sí complica las transferencias atómicas, las pruebas y el despliegue.

### Decisión 6 — Creación controlada del primer administrador

La relación de auditoría entre usuarios permite un creador opcional únicamente para la primera cuenta. El administrador inicial se crea mediante una migración controlada, por lo que `createdByUserId` puede ser nulo solo en ese caso. Todas las cuentas posteriores requieren un Administrador creador, conforme a RF-02-AC-1. Así se evita una autorreferencia artificial y se conserva la trazabilidad operativa.

## 9. Criterios para revisar esta decisión

El ADR deberá reconsiderarse si ocurre al menos una de estas condiciones:

- un módulo requiere escalar al menos cinco veces por encima del resto durante un periodo sostenido;
- equipos independientes necesitan desplegar capacidades con calendarios diferentes;
- la disponibilidad requerida supera lo razonable para una sola unidad de backend;
- integraciones externas necesitan eventos de dominio confiables;
- las métricas muestran que una carga particular no puede aislarse mediante índices, caché o réplicas de lectura.

## Referencias

Bass, L., Clements, P., & Kazman, R. (2021). *Software architecture in practice* (4th ed.). Addison-Wesley.

Brown, S. (2018). *The C4 model for visualising software architecture*. https://c4model.com/

IEEE Computer Society. (2024). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide V4.0a), Chapters 2–3*. IEEE. https://www.computer.org/education/bodies-of-knowledge/software-engineering

Nygard, M. (2011). *Documenting architecture decisions*. Cognitect. https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions

## Declaración de uso de inteligencia artificial

Se utilizó OpenAI Codex para analizar alternativas arquitectónicas, producir borradores de los diagramas y apoyar la corrección gramatical y de redacción. El alumno contrastó las propuestas con el SRS de StockIV, modificó el modelo del dominio, rechazó complejidad no justificada y asume la responsabilidad por las decisiones y los artefactos finales.
