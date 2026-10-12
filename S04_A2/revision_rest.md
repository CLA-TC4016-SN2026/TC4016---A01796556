# Revisión de principios REST — API StockIV

**Alumno:** Israel Agustín Vargas Monroy  
**Matrícula:** A01796556  
**Fecha:** 12 de octubre de 2026  
**Artefacto revisado:** `openapi.yaml`, versión 1.0.0

## 1. Método de revisión

Se realizó una primera propuesta del contrato a partir de los requerimientos RF-01 a RF-09 y de sus 21 criterios de aceptación. Después se evaluaron las rutas, los verbos, la idempotencia, los códigos de estado, la seguridad y la consistencia de los esquemas. La revisión tomó como referencia el estilo arquitectónico REST de Fielding (2000), el diseño de APIs orientadas a recursos de Richardson y Amundsen (2013) y OpenAPI 3.0.3 (OpenAPI Initiative, 2021).

## 2. Observaciones del agente y acciones correctivas

| # | Observación detectada en la propuesta | Riesgo o principio afectado | Acción correctiva aplicada | Evidencia en `openapi.yaml` |
|---:|---|---|---|---|
| 1 | La propuesta inicial utilizaba rutas con acciones, como `/login`, `/getProducts` y `/deactivateBranch`. | Las rutas expresaban procedimientos en vez de recursos; además mezclaban mayúsculas y verbos. | Se cambiaron por sustantivos en plural, minúsculas y rutas jerárquicas: sesiones, productos y sucursales. | `POST /api/v1/auth/sessions`, `GET /api/v1/products` y `DELETE /api/v1/branches/{branchId}`. |
| 2 | Se propuso `PUT` para cambios parciales de usuario. | `PUT` debe representar un reemplazo completo e idempotente; usarlo para un cambio parcial vuelve ambiguo el contrato. | Se reservó `PATCH` para modificar solo nombre, rol o estado del usuario. `PUT` se conservó donde existe reemplazo completo o *upsert* idempotente. | `PATCH /api/v1/users/{userId}`; `PUT /api/v1/products/{productId}`; `PUT /api/v1/inventory-thresholds/{productId}/{branchId}`. |
| 3 | Entradas, salidas y transferencias podían reintentarse con `POST` y duplicar existencias. | `POST` no es idempotente por definición; un reintento de red podía crear dos movimientos. | Se añadió el encabezado obligatorio `Idempotency-Key` a operaciones que cambian inventario. El servidor debe devolver la misma respuesta al repetir la clave con el mismo cuerpo y `409` si cambia el cuerpo. | Parámetro reutilizable `IdempotencyKey` en `POST /movements` y `POST /transfers`. |
| 4 | La propuesta trataba una transferencia como dos solicitudes independientes para restar y sumar. | Una falla intermedia dejaría existencias inconsistentes y violaría RF-06-AC-2 y la atomicidad. | Se modeló una transferencia como un recurso creado mediante una sola operación. El backend abre una transacción, bloquea ambas existencias, crea los dos movimientos y confirma o revierte todo. | `POST /api/v1/transfers` responde `201` o `409`; `TransferResult` devuelve transferencia, salida, entrada y ambas existencias. |
| 5 | Se usaba `400` para credenciales, permisos, duplicados y faltantes. | Un único código impedía distinguir errores de autenticación, autorización, existencia, validación y conflicto. | Se separaron `401` (token/credenciales), `403` (rol), `404` (recurso), `409` (unicidad o estado), `422` (campos/reglas semánticas) y `500` (falla no prevista). | Respuestas reutilizables en `components.responses` y respuestas específicas por operación. |
| 6 | Se planteó borrar físicamente productos, sucursales y movimientos. | La eliminación física rompe la auditoría y contradice RF-03-AC-3, RF-08-AC-2, RF-09-AC-2, RD-05 y RD-07. | `DELETE` en productos y sucursales significa baja lógica; no existen métodos de escritura en `/movements/{movementId}`. Las correcciones se representan como movimientos compensatorios. | Descripciones de `DELETE /products/{productId}` y `DELETE /branches/{branchId}`; solo `GET` en el detalle de movimientos. |
| 7 | Los listados no tenían límites ni filtros uniformes. | Sin paginación, el rendimiento se degrada al aproximarse a 10 000 productos y 100 000 movimientos (RNF-01). | Se estandarizaron `page` y `pageSize` (máximo 100), un objeto `PageMeta` y filtros explícitos por recurso. | Parámetros `Page` y `PageSize`; esquemas `ProductPage`, `InventoryPage`, `MovementPage` y otros. |
| 8 | La seguridad se describía solamente en texto. | Sin un esquema formal, los clientes generados no saben enviar el token y queda incierto qué ruta es pública. | Se definió `bearerAuth` HTTP con formato JWT como seguridad global; el inicio de sesión sobrescribe la seguridad con `security: []`. | `components.securitySchemes.bearerAuth`, seguridad global y excepción en `POST /auth/sessions`. |
| 9 | La primera versión extendía `ProductInput`, `BranchInput` y `Problem` mediante `allOf`, aunque los esquemas base prohibían propiedades adicionales. | En OpenAPI 3.0, `allOf` no fusiona las propiedades permitidas; una respuesta podía invalidarse por contener `id` o `errors`. | Se sustituyeron esas composiciones por objetos de respuesta completos y cerrados. | Esquemas finales `Product`, `Branch` y `ValidationProblem`, todos sin `allOf` conflictivo. |
| 10 | `MovementCreate` solo describía en texto cuándo era obligatorio `compensatesMovementId`. | El contrato permitía un ajuste sin referencia o una entrada normal con referencia, contradiciendo RD-05. | Se separaron variantes excluyentes con `oneOf`: movimiento normal y movimiento compensatorio. | `NormalMovementCreate`, `CompensatingMovementCreate` y `MovementCreate.oneOf`. |

## 3. Comprobación de verbos e idempotencia

| Verbo | Uso en StockIV | Semántica e idempotencia |
|---|---|---|
| `GET` | Consultas de catálogos, inventario, historial, transferencias y alertas. | Seguro e idempotente; no cambia estado. |
| `POST` | Creación de sesión, usuarios, productos, sucursales, movimientos y transferencias. | No idempotente por naturaleza. Las escrituras de inventario exigen `Idempotency-Key` para reintentos seguros. |
| `PUT` | Reemplazo completo de producto/sucursal y creación o reemplazo de umbral. | Idempotente: repetir el mismo cuerpo produce el mismo estado observable. |
| `PATCH` | Cambio parcial de un usuario. | El documento solo permite asignar valores absolutos, por lo que repetirlo no acumula cambios. |
| `DELETE` | Cierre de la sesión y baja lógica de producto o sucursal. | Idempotente en intención. Una repetición puede responder `204` o `409/404` según la política, sin restaurar el recurso. |

## 4. Criterios de aceptación de la revisión

- Todas las rutas comienzan con `/api/v1` para mantener una frontera de versión estable.
- Los nombres de recurso son sustantivos, minúsculos y, cuando representan colecciones, plurales.
- Cada operación contiene `x-acceptance-criteria`. Cuando satisface directamente un escenario, la lista contiene sus IDs; cuando implementa un comportamiento del RF que no recibió un Given-When-Then específico —por ejemplo, cierre de sesión o edición de catálogos— la lista queda vacía y `x-supports-requirements` identifica el RF sin inventar trazabilidad.
- Los cuerpos de error usan `application/problem+json` con una estructura uniforme.
- Las colecciones grandes están paginadas y sus filtros se expresan como parámetros de consulta.
- Las transferencias y los movimientos describen una unidad transaccional implementable.
- El historial no expone `PUT`, `PATCH` ni `DELETE`.

## 5. Resultado de la validación

El archivo se comprobó como YAML y como documento OpenAPI 3.0.3 mediante `openapi-spec-validator` 0.7.2. También se verificaron referencias internas, parámetros de ruta, unicidad de los 27 `operationId`, presencia de `x-acceptance-criteria` en las 27 operaciones, cobertura de los 21 criterios y seguridad JWT. Después de una revisión docente simulada se corrigieron la composición de esquemas, las variantes de movimientos y la trazabilidad indirecta. El resultado final no contiene errores de sintaxis ni referencias sin resolver. Como último control de entrega, el alumno deberá abrir el mismo archivo en Swagger Editor y confirmar que no aparezcan errores en su entorno.

## Referencias

Fielding, R. T. (2000). *Architectural styles and the design of network-based software architectures* [Tesis doctoral, University of California, Irvine]. https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm

OpenAPI Initiative. (2021). *OpenAPI specification, version 3.0.3*. https://spec.openapis.org/oas/v3.0.3

Richardson, L., & Amundsen, M. (2013). *RESTful web APIs*. O’Reilly Media.

## Declaración de uso de inteligencia artificial

Se utilizó OpenAI Codex para proponer una primera versión de la especificación, revisar principios REST, detectar inconsistencias y apoyar la corrección gramatical y la redacción. El alumno contrastó las propuestas con el SRS de StockIV, verificó los cambios reflejados en `openapi.yaml` y asume la responsabilidad por el contenido final.
