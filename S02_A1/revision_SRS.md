# Revisión crítica del SRS — StockIV

**Alumno:** Israel Agustín Vargas Monroy

**Matrícula:** A01796556

**Fecha:** 27 de septiembre de 2026

## 1. Método de revisión

La revisión se realizó en cuatro pasos: extracción inicial de necesidades, clasificación en requerimientos funcionales, no funcionales y de dominio, revisión humana de precisión y consistencia, y revisión cruzada con un agente en rol de profesor. Se compararon `proyecto_base.md`, el guion, ambas secciones del transcript, el diagrama y el SRS. La revisión aplicó criterios de calidad, trazabilidad y verificabilidad de requerimientos (IEEE Computer Society, 2024; Wiegers & Beatty, 2013).

## 2. Revisión de la clasificación inicial

| Propuesta inicial del agente | Problema encontrado | Acción tomada |
|---|---|---|
| “El sistema permitirá eliminar productos.” | La eliminación física contradice la trazabilidad cuando existen movimientos. | Se cambió por desactivación y se añadieron RF-03-AC-3, RD-05 y RD-07. |
| “El sistema actualizará el inventario.” | No distinguía entradas, salidas, transferencias ni sucursales. | Se separaron RF-05 y RF-06; la existencia se modeló por producto y sucursal. |
| “El sistema será rápido.” | No era medible. | RNF-01 fijó volumen, concurrencia, percentil y tiempo máximo. |
| “El sistema será seguro.” | No indicaba controles verificables. | RNF-02 y RNF-03 especificaron hash de contraseñas, HTTPS, autorización en backend y expiración de sesión. |
| “El sistema será fácil de usar.” | No definía población, tareas ni umbral de éxito. | RNF-06 estableció una prueba con cinco usuarios, tres tareas y límites de ayuda y errores. |
| Una sola cifra global de existencias. | Contradecía el contexto de dos o tres sucursales. | RF-04, RF-06 y RD-04 incorporaron el manejo multisucursal. |
| Transferencia como dos cambios independientes. | Podía producir inventario inconsistente ante una falla parcial. | RF-06 y RF-06-AC-2 exigieron atomicidad y reversión completa. |
| Historial editable. | Permitía perder evidencia de quién modificó el inventario. | RF-08-AC-2 y RD-05 establecieron inmutabilidad y movimientos compensatorios. |

## 3. Clasificación final

La versión final contiene:

- **9 requerimientos funcionales**, todos vinculados con casos de uso y criterios de aceptación.
- **8 requerimientos no funcionales**, con métricas de rendimiento, seguridad, disponibilidad, recuperación, usabilidad, compatibilidad y mantenibilidad.
- **9 requerimientos de dominio**, centrados en integridad del inventario y límites del negocio.
- **21 criterios de aceptación** con identificadores únicos y resultados verificables.

## 4. Ambigüedades, contradicciones y gaps detectados

| ID | Hallazgo | Tipo | Corrección aplicada |
|---|---|---|---|
| H-01 | “Eliminar registros” podía significar borrado físico o desactivación. | Ambigüedad | Se definió desactivación para entidades con historial. |
| H-02 | El proyecto base no especificaba si la existencia era global o por sucursal. | Gap | Se adoptó existencia por producto-sucursal conforme al contexto del equipo. |
| H-03 | No se habían definido roles ni límites de permisos. | Gap | Se definieron Administrador y Encargado de inventario y se añadió RF-02. |
| H-04 | No existía regla ante salidas mayores a la existencia. | Gap de dominio | RD-02 y RF-05-AC-2 prohíben existencias negativas. |
| H-05 | Una transferencia podía quedar aplicada solo en una sucursal. | Riesgo de contradicción | Se exigió una transacción atómica en RF-06 y RD-04. |
| H-06 | “Accesible” y “sencilla” no eran verificables. | Ambigüedad no funcional | RNF-06 y RNF-07 agregaron prueba de usabilidad y matriz de navegadores y resoluciones. |
| H-07 | No había política de corrección de errores. | Gap | RD-05 estableció movimientos compensatorios referenciados. |
| H-08 | Alertas e historial no aparecían en el acuerdo base. | Necesidad implícita | Se documentaron como inferencias y se validaron en la sesión simulada; requieren validación comercial antes de producción. |
| H-09 | Faltaba una frontera explícita con procesos comerciales. | Alcance | Se incorporaron las exclusiones en 1.2, 2.4 y RD-08. |
| H-10 | La administración de sucursales se mencionaba como función obligatoria, pero no tenía requisito ni caso de uso. | Gap de trazabilidad | Se añadieron RF-09, RD-09, UC-12 y tres criterios de aceptación. |
| H-11 | RF-05 verificaba una entrada exitosa y una salida rechazada, pero no una salida exitosa. | Gap de prueba | Se añadió RF-05-AC-3 con cantidades inicial y final verificables. |

## 5. Verificación de criterios Given-When-Then

Se comprobó que cada RF tenga al menos dos criterios en formato Dado-Cuando-Entonces, siguiendo el enfoque de especificación por ejemplos de BDD (North, 2006), con:

- una precondición observable;
- una acción única y reproducible;
- un resultado medible;
- datos de ejemplo cuando el cálculo o la validación lo requieren;
- comportamiento esperado tanto en escenarios exitosos como de rechazo.

Los identificadores siguen el patrón `RF-XX-AC-Y`. No se reutiliza ningún ID. RF-03, RF-05 y RF-09 incluyen un tercer criterio para cubrir decisiones relevantes de desactivación, salida válida y desactivación de sucursales.

## 6. Revisión del diagrama

Se verificó que los dos roles concretos correspondan a los usuarios del SRS y que todos los RF tengan representación. El actor general “Usuario” concentra los permisos compartidos por Administrador y Encargado de inventario mediante generalización UML. Las relaciones se interpretan de la siguiente forma:

- Registrar una salida incluye validar la existencia disponible.
- Transferir existencias incluye una salida y una entrada dentro de la misma operación.
- Visualizar alertas extiende la consulta de inventario cuando la existencia es menor o igual al mínimo.

La autenticación se representa como caso de uso del actor general “Usuario”; los dos roles heredan esa asociación. Los demás casos protegidos requieren una sesión activa, de acuerdo con el SRS.

La notación de actores, casos de uso, generalización y relaciones `<<include>>` y `<<extend>>` sigue UML 2.5.1 (Object Management Group, 2017).

## 7. Revisión cruzada en rol de profesor

Un segundo agente revisó los seis entregables con la rúbrica de la actividad. La primera evaluación estricta fue de **78/90 puntos (86.7 %)**. Después de aplicar las correcciones técnicas, la proyección es **85/90 puntos (94.4 %)**; los cinco puntos restantes dependen de incorporar respuestas obtenidas directamente de un propietario o encargado real.

| Hallazgo del revisor | Acción correctiva | Evidencia en la versión final |
|---|---|---|
| No existe entrevista con un cliente comercial real. | Se mantuvo la distinción entre Product Owner simulado y stakeholder proxy; no se fabricó evidencia. Se dejó la validación comercial como pendiente. | `transcript_entrevista.md`, secciones 2 y 4; SRS 2.5. |
| La gestión de sucursales estaba declarada, pero no especificada. | Se añadió el requerimiento, reglas, criterios, caso de uso y trazabilidad. | RF-09, RD-09, UC-12 y matriz de trazabilidad. |
| El estado “Validado para diseño” contradecía las decisiones pendientes. | Se precisó que la validación es académica y que falta validación comercial. | Encabezado de `SRS_final.md`. |
| “Contraseña válida” y “los demás datos válidos” eran ambiguos. | Se definieron longitud y composición de contraseña, formatos, longitudes y campos obligatorios del producto. | RF-02, RF-02-AC-1, RF-03 y RF-03-AC-1. |
| RF-05 no cubría una salida exitosa. | Se agregó un escenario con existencia inicial, cantidad retirada y existencia final. | RF-05-AC-3. |
| “Usuario autenticado” aparecía ejecutando “Autenticarse”. | Se renombró el actor general como “Usuario” y se conservaron los roles mediante generalización. | `casos_de_uso.puml` y `casos_de_uso.png`. |
| Las citas requerían consistencia. | Se normalizó SWEBOK como versión 4.0a y se incorporaron citas en el cuerpo para BDD y UML. | Referencias y citas de los documentos individuales. |

La revisión confirma que `proyecto_base.md` reproduce el título y los tres párrafos compartidos por Luis sin agregar citas, datos personales ni disclaimer, lo cual conserva la igualdad requerida entre integrantes.

## 8. Decisiones pendientes para validación comercial

Aunque el SRS es suficiente para el diseño académico, antes de un uso productivo deben confirmarse con un propietario o encargado real:

- los campos obligatorios de cada categoría de producto;
- la política exacta para correcciones y ajustes físicos;
- los umbrales de rendimiento y disponibilidad;
- el periodo de conservación del historial;
- si el encargado puede desactivar productos o solo el administrador.

La especificación deliberada de unidades enteras, moneda y atomicidad responde a la necesidad de evitar incompatibilidades y supuestos no documentados. El caso Mars Climate Orbiter ilustra el costo que puede tener una discrepancia de unidades no detectada en una interfaz técnica (NASA, 1999).

## 9. Referencias

IEEE Computer Society. (2024). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide V4.0a)*. https://www.computer.org/education/bodies-of-knowledge/software-engineering

NASA. (1999). *Mars Climate Orbiter mishap investigation board phase I report*. National Aeronautics and Space Administration.

North, D. (2006). *Introducing BDD*. https://dannorth.net/introducing-bdd/

Object Management Group. (2017). *OMG Unified Modeling Language (OMG UML), version 2.5.1*. https://www.omg.org/spec/UML/2.5.1/PDF

Wiegers, K. E., & Beatty, J. (2013). *Software requirements* (3rd ed.). Microsoft Press.

## 10. Declaración de uso de inteligencia artificial

Se utilizó OpenAI Codex como apoyo para simular al Product Owner, estructurar el borrador, revisar consistencia, proponer criterios de aceptación y realizar correcciones gramaticales y de redacción. El alumno contrastó las propuestas con la conversación real del equipo, corrigió ambigüedades, definió el alcance y conserva la responsabilidad sobre la versión final. La sección de stakeholder proxy se reconstruyó únicamente con información presente en Teams y no se presenta como entrevista a un cliente comercial.
