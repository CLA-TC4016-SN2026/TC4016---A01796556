# Decisiones de diseño y trazabilidad — API StockIV

**Alumno:** Israel Agustín Vargas Monroy  
**Matrícula:** A01796556  
**Fecha:** 12 de octubre de 2026  
**Base del equipo:** SRS de StockIV, versión 1.0

## Sección A — Trazabilidad especificación ↔ criterios de aceptación

La tabla conserva exactamente los 21 identificadores del SRS. Un criterio se considera cubierto cuando el contrato contiene la operación, las respuestas y los esquemas necesarios para comprobar su resultado. En RF-08-AC-2 la cobertura también depende de una restricción deliberada: el recurso de movimientos es de solo lectura después de su creación.

| ID del criterio | Descripción resumida | Endpoint | Método HTTP | ¿Cubierto? | Evidencia o decisión |
|---|---|---|---|---|---|
| RF-01-AC-1 | Usuario activo inicia sesión y recibe permisos de su rol. | `/api/v1/auth/sessions` | `POST` | ✅ | Devuelve JWT y usuario con rol; `200`. |
| RF-01-AC-2 | Credenciales incorrectas o cuenta inactiva no crean sesión. | `/api/v1/auth/sessions` | `POST` | ✅ | Devuelve `401` y el mensaje genérico “Credenciales inválidas”. |
| RF-02-AC-1 | Administrador crea usuario válido y queda registrado el creador. | `/api/v1/users` | `POST` | ✅ | `UserCreate` valida campos; `User.createdBy` conserva auditoría; `201`. |
| RF-02-AC-2 | Encargado no puede crear, cambiar rol ni desactivar usuarios. | `/api/v1/users`; `/api/v1/users/{userId}` | `POST`, `PATCH` | ✅ | Ambas operaciones documentan `403`; autorización se valida en backend. |
| RF-03-AC-1 | Administrador o encargado registra producto válido sin alterar existencias. | `/api/v1/products` | `POST` | ✅ | `ProductInput`; respuesta `201`; la descripción declara que no modifica inventario. |
| RF-03-AC-2 | SKU duplicado se rechaza sin cambiar catálogo. | `/api/v1/products` | `POST` | ✅ | `409` específico y unicidad global del SKU. |
| RF-03-AC-3 | Producto con historial se desactiva sin borrarlo. | `/api/v1/products/{productId}` | `DELETE` | ✅ | Baja lógica `204`; conserva existencias e historial. |
| RF-04-AC-1 | Consulta combina texto, categoría y sucursal. | `/api/v1/inventory` | `GET` | ✅ | Parámetros `search`, `category`, `branchId` y `productActive`. |
| RF-04-AC-2 | Detalle muestra cantidades por sucursal y total consolidado. | `/api/v1/inventory/{productId}` | `GET` | ✅ | `InventoryItem.levels` y `totalQuantity`. |
| RF-05-AC-1 | Entrada válida aumenta existencia y crea historial. | `/api/v1/movements` | `POST` | ✅ | Tipo `ENTRY`; `MovementResult` devuelve movimiento y existencia resultante; `201`. |
| RF-05-AC-2 | Salida mayor a existencia se rechaza sin movimiento. | `/api/v1/movements` | `POST` | ✅ | `409` por existencia insuficiente; la operación se define atómica. |
| RF-05-AC-3 | Salida válida disminuye existencia y conserva auditoría. | `/api/v1/movements` | `POST` | ✅ | Tipo `EXIT`; movimiento incluye usuario, fecha, producto, sucursal, cantidad y motivo. |
| RF-06-AC-1 | Transferencia válida actualiza origen y destino y relaciona movimientos. | `/api/v1/transfers` | `POST` | ✅ | `TransferResult` contiene identificador, movimientos de salida/entrada y ambas existencias. |
| RF-06-AC-2 | Transferencia fallida revierte ambas partes. | `/api/v1/transfers` | `POST` | ✅ | Una sola operación transaccional; `409` no confirma movimientos parciales. |
| RF-07-AC-1 | Salida que alcanza el mínimo activa alerta. | `/api/v1/movements`; `/api/v1/alerts` | `POST`, `GET` | ✅ | La respuesta del movimiento expone `lowStock` derivado; la consulta devuelve combinaciones `quantity <= threshold`. |
| RF-07-AC-2 | Entrada por encima del mínimo elimina la alerta sin borrar historial. | `/api/v1/movements`; `/api/v1/alerts` | `POST`, `GET` | ✅ | La alerta es una vista derivada del inventario; el movimiento permanece inmutable. |
| RF-08-AC-1 | Historial filtrado por SKU y fechas, con auditoría y orden descendente. | `/api/v1/movements` | `GET` | ✅ | Incluye filtros `sku`, `dateFrom`, `dateTo` y campos completos en `Movement`. |
| RF-08-AC-2 | Un movimiento confirmado no puede editarse ni eliminarse. | `/api/v1/movements/{movementId}` | `GET` | ✅ | Cobertura por restricción: solo existe `GET`; no se definen `PUT`, `PATCH` ni `DELETE`. Las correcciones son compensatorias. |
| RF-09-AC-1 | Administrador crea sucursal válida con existencias iniciales en cero. | `/api/v1/branches` | `POST` | ✅ | `BranchInput`, autorización por rol y respuesta `201`; la descripción fija cantidad inicial cero. |
| RF-09-AC-2 | Sucursal con historial y existencias cero se desactiva. | `/api/v1/branches/{branchId}` | `DELETE` | ✅ | Baja lógica `204`; conserva historial e impide movimientos posteriores. |
| RF-09-AC-3 | Sucursal con existencia positiva no se desactiva. | `/api/v1/branches/{branchId}` | `DELETE` | ✅ | Devuelve `409` y mantiene el recurso activo. |

### Resultado de cobertura

- Criterios totales del SRS: **21**.
- Criterios cubiertos por una o más operaciones: **21 (100 %)**.
- Criterios parciales: **0**.
- Criterios sin cobertura: **0**.

No se detectó un *gap* de endpoint después de las correcciones. RF-08-AC-2 no requiere una operación de escritura: se satisface precisamente al no exponerla y al permitir solamente movimientos compensatorios. Este tipo de trazabilidad vincula requerimientos, diseño y pruebas y reduce la pérdida de intención entre etapas (IEEE Computer Society, 2024).

### Gaps inversos detectados en el SRS

Aunque todos los criterios tienen cobertura, el texto de algunos requerimientos incluye comportamientos sin un criterio Given-When-Then propio: cierre de sesión (RF-01), consulta de usuarios (RF-02), edición de productos (RF-03), configuración explícita de mínimos (RF-07) y consulta/edición de sucursales (RF-09). Estas operaciones siguen siendo necesarias para implementar el requerimiento, pero asignarles el criterio “más cercano” produciría trazabilidad falsa.

Por ello, cada una conserva `x-acceptance-criteria: []` y agrega `x-supports-requirements` con el RF correspondiente. La decisión recomendada para la siguiente revisión del SRS es añadir criterios específicos para esos comportamientos; no se modificó unilateralmente la base compartida del equipo en esta actividad.

## Sección B — Decisiones de diseño

### 1. Recursos y endpoints seleccionados

Se eligieron ocho grupos de recursos porque corresponden a capacidades estables del dominio y a los agregados principales del SRS:

- **Sesiones:** representa inicio y cierre de sesión sin introducir un verbo en la ruta.
- **Usuarios:** concentra identidad, rol y estado; sus escrituras están reservadas al Administrador.
- **Productos y sucursales:** son catálogos maestros, con baja lógica para preservar trazabilidad.
- **Inventario y umbrales:** modelan la existencia por la pareja producto-sucursal. El total consolidado es una proyección y no una cantidad almacenada independiente.
- **Movimientos:** es el libro auditable de entradas, salidas y ajustes compensatorios. Una vez creado es inmutable.
- **Transferencias:** es una unidad de negocio distinta porque exige dos movimientos coordinados dentro de una sola transacción.
- **Alertas:** es una colección calculada a partir de la existencia y el umbral; no tiene operaciones de creación o eliminación manual.

La API sigue una separación en capas coherente con la arquitectura de StockIV: Browser → frontend React/Vite → API FastAPI → servicios de aplicación/dominio → repositorios SQLAlchemy → PostgreSQL. El contrato no obliga a implementar microservicios; los recursos pueden residir en un monolito modular, apropiado para el alcance académico y para mantener transacciones locales.

### 2. Funcionalidades fuera del alcance

Se excluyeron ventas, facturación, cobros, pagos, contabilidad, órdenes de compra, pronósticos, importación masiva, lectores de códigos de barras y aplicación móvil nativa porque RD-08 y el alcance del SRS los reservan para versiones futuras. Tampoco se incluyeron endpoints para borrar movimientos, fijar directamente una existencia o ejecutar dos pasos separados de transferencia, pues esos diseños violarían RD-02, RD-04 y RD-05.

La recuperación de contraseña, el refresco de tokens, la carga de imágenes de productos y la administración de categorías tampoco tienen criterios de aceptación en la versión 1.0. Incluirlos ahora ampliaría el contrato sin evidencia del stakeholder. Podrán evaluarse en una versión posterior mediante nuevos criterios y una evolución compatible del contrato.

### 3. Patrones de diseño incluidos

| Patrón | Decisión | Motivación concreta |
|---|---|---|
| Versionado por URI | Se incluye `/api/v1`. | Permite evolucionar cambios incompatibles sin alterar a clientes de la primera versión. |
| Paginación | Se incluyen `page` y `pageSize`, con máximo de 100. | Evita respuestas no acotadas y apoya RNF-01 con 10 000 productos y 100 000 movimientos. |
| Filtrado | Se incluyen filtros propios de cada colección. | Evita descargar todos los datos y satisface RF-04 y RF-08. |
| Idempotencia de comandos | `Idempotency-Key` es obligatorio en movimientos y transferencias. | Hace seguros los reintentos de red sin duplicar cantidades. |
| Errores uniformes | Se usa `application/problem+json` con `Problem` y `ValidationProblem`. | Los clientes pueden tratar errores de forma consistente sin depender de mensajes libres. |
| Baja lógica | `DELETE` desactiva productos y sucursales. | Conserva historial y cumple RD-07. |
| RBAC y JWT | Bearer JWT global con verificación de rol en el backend. | Cumple la seguridad requerida y diferencia `401` de `403`. |
| Operación transaccional | Una transferencia se crea mediante un único `POST`. | Garantiza todo-o-nada y cumple RF-06-AC-2. |

### 4. Patrones excluidos o diferidos

- **Cursor pagination:** se eligió paginación por página porque facilita la interfaz académica y el volumen inicial es moderado. Si los movimientos crecen mucho o cambian durante la navegación, una versión futura puede incorporar cursor sin alterar los recursos.
- **HATEOAS completo:** no se incluyeron enlaces hipermedia en cada respuesta. El alcance prioriza un contrato explícito consumido por un frontend conocido; el versionado, los códigos y los esquemas aportan suficiente navegabilidad para la primera versión.
- **Versionado por encabezado:** se descartó por ser menos visible al probar manualmente en Swagger Editor.
- **Borrado físico:** se excluyó para catálogos con historial y para movimientos.
- **Operaciones masivas:** se difirieron porque no están en el SRS y requieren reglas adicionales de atomicidad y reporte de errores parciales.

### 5. Propuestas del agente que no seguían buenas prácticas REST

El agente sugirió inicialmente rutas con verbos (`/getProducts`), `PUT` para actualizaciones parciales, códigos `400` para cualquier falla y dos solicitudes independientes para una transferencia. Se detectaron los problemas al comprobar tres preguntas: (1) si la URI identifica un recurso y no una acción; (2) si repetir la solicitud conserva la semántica declarada; y (3) si el código HTTP permite distinguir autenticación, autorización, validación, ausencia y conflicto. La comparación contra Fielding (2000), Richardson y Amundsen (2013) y el SRS permitió corregir estas propuestas.

Una revisión posterior en rol de profesor detectó otros dos defectos menos visibles. Primero, `allOf` no fusionaba correctamente esquemas base que tenían `additionalProperties: false`; se reemplazaron por objetos completos. Segundo, se habían asociado criterios a operaciones de soporte que no demostraban realmente ese escenario; se eliminaron esas asociaciones y se documentaron los gaps inversos. Finalmente, `MovementCreate` se dividió con `oneOf` para que un ajuste compensatorio siempre incluya la referencia obligatoria y un movimiento normal no pueda enviarla.

También se rechazó la idea de exponer `DELETE /movements/{id}`. Aunque el verbo parecería válido en un CRUD genérico, sería incorrecto en este dominio porque el historial es un registro auditable. La buena práctica no se decidió solo por la forma de HTTP, sino por la invariante de negocio RD-05.

### 6. Implementabilidad y correspondencia con las pruebas futuras

Cada `operationId` puede convertirse en una función de controlador FastAPI. Los esquemas pueden mapearse a modelos Pydantic y las entidades a SQLAlchemy/PostgreSQL. Las claves únicas de SKU, correo y código de sucursal deben reforzarse en base de datos; las transferencias deben usar una transacción y bloqueos de las filas de inventario; las políticas RBAC deben probarse en el backend. Los identificadores de `x-acceptance-criteria` permitirán derivar pruebas funcionales en S09-A1 sin reinterpretar el SRS.

## Referencias

Fielding, R. T. (2000). *Architectural styles and the design of network-based software architectures* [Tesis doctoral, University of California, Irvine]. https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm

IEEE Computer Society. (2024). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide V4.0a), Chapter 3: Software Design*. IEEE. https://www.computer.org/education/bodies-of-knowledge/software-engineering

OpenAPI Initiative. (2021). *OpenAPI specification, version 3.0.3*. https://spec.openapis.org/oas/v3.0.3

Richardson, L., & Amundsen, M. (2013). *RESTful web APIs*. O’Reilly Media.

## Declaración de uso de inteligencia artificial

Se utilizó OpenAI Codex para apoyar la elaboración del contrato OpenAPI, la revisión de decisiones REST, la comprobación de trazabilidad y la corrección gramatical y de redacción. El alumno revisó cada recomendación, la contrastó con el SRS y la arquitectura de StockIV, corrigió propuestas que no respetaban las invariantes del dominio y asume la responsabilidad por el contenido final.
