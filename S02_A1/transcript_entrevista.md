# Transcript de entrevistas de elicitación — StockIV

**Alumno:** Israel Agustín Vargas Monroy

**Matrícula:** A01796556

**Fecha:** 27 de septiembre de 2026

La separación entre necesidades explícitas, implícitas y restricciones sigue el proceso de elicitación y análisis descrito en SWEBOK (IEEE Computer Society, 2024). Las preguntas y repreguntas buscan descubrir reglas del dominio y excepciones, no solo funciones visibles (Wiegers & Beatty, 2013).

## 1. Sesión simulada con agente en rol de Product Owner

**Contexto proporcionado al agente:** StockIV será una aplicación web para administrar el inventario de una tienda de ropa, calzado y artículos deportivos con dos o tres sucursales. La primera versión debe ser viable en 12 semanas y no incluirá ventas, facturación ni pagos.

**Instrucción de rol:** Responde como Product Owner de la tienda. Expresa necesidades realistas, reglas operativas, prioridades y restricciones; no diseñes la solución técnica.

### Pregunta 1

**Entrevistador:** ¿Cómo controlan actualmente los productos y existencias, qué dificultades enfrentan y qué resultado esperan obtener con StockIV?

**Product Owner:** Cada sucursal lleva hojas de cálculo separadas y también existen anotaciones manuales. Cuando se mueve mercancía entre tiendas, los archivos no se actualizan al mismo tiempo. Por eso no siempre sabemos qué cantidad está realmente disponible. Esperamos consultar una sola fuente, identificar en qué sucursal está cada producto y reducir las diferencias entre el inventario registrado y el físico.

### Pregunta 2

**Entrevistador:** ¿Quiénes utilizarán el sistema y qué acciones debería poder realizar cada tipo de usuario?

**Product Owner:** Habrá administradores y encargados de inventario. El administrador podrá gestionar usuarios, sucursales, productos y todas las operaciones de existencias. El encargado podrá consultar productos y registrar entradas, salidas y transferencias, pero no crear usuarios ni desactivar sucursales. Todos deberán autenticarse y cada operación deberá quedar asociada con el usuario que la realizó.

### Pregunta 3

**Entrevistador:** ¿Qué información debe conservarse de cada producto y cómo se distinguen productos, categorías, tallas, colores y sucursales?

**Product Owner:** Cada producto necesita un SKU único, nombre, descripción breve, categoría, talla, color, precio y estado activo o inactivo. La existencia debe manejarse por producto y sucursal, no como una cifra global. En la primera versión operaremos con hasta tres sucursales, pero no queremos que la estructura impida agregar más después.

### Pregunta 4

**Entrevistador:** ¿Qué movimientos modifican las existencias y qué debe ocurrir ante cantidades insuficientes, errores de captura, transferencias o intentos de eliminar productos con historial?

**Product Owner:** Deben existir entradas, salidas y transferencias. Toda cantidad será un número entero positivo y se pedirá un motivo. El sistema nunca debe permitir existencias negativas. Una transferencia debe descontar en el origen y sumar en el destino como una sola operación; si falla una parte, ninguna debe conservarse. Los productos con movimientos no deben borrarse físicamente, solo desactivarse. Si alguien comete un error, debe registrarse un movimiento compensatorio para conservar la trazabilidad.

### Pregunta 5

**Entrevistador:** ¿Qué consultas, alertas e historial necesitan para tomar decisiones durante la operación diaria? ¿Qué datos serían indispensables en esas vistas?

**Product Owner:** Necesitamos buscar por SKU, nombre, categoría y sucursal; ver existencia por sucursal y total; y filtrar productos con nivel bajo. Cada combinación de producto y sucursal debe admitir una existencia mínima. Cuando la cantidad sea igual o menor a ese mínimo, debe aparecer una alerta. El historial debe mostrar fecha y hora, usuario, producto, sucursal, tipo, cantidad, motivo y, si fue transferencia, el origen y destino.

### Pregunta 6

**Entrevistador:** ¿Qué expectativas existen sobre rapidez, seguridad, facilidad de uso, disponibilidad, respaldo y dispositivos o navegadores compatibles?

**Product Owner:** Las consultas habituales deberían responder en aproximadamente dos segundos. Las contraseñas no deben almacenarse en texto legible y los permisos deben aplicarse en el servidor, no solo ocultando botones. La interfaz debe funcionar en computadoras y tabletas con navegadores actuales. Queremos respaldos diarios y poder recuperar el servicio en la misma jornada laboral ante una falla. El sistema debe ser suficientemente claro para que un encargado nuevo complete las tareas principales después de una capacitación breve.

### Pregunta 7

**Entrevistador:** Para una primera versión realizable en 12 semanas, ¿qué funciones son obligatorias, cuáles pueden posponerse y qué procesos deben quedar explícitamente fuera?

**Product Owner:** Son obligatorios el inicio de sesión, los roles, la gestión de productos y sucursales, la consulta de existencias, los movimientos, las transferencias, los mínimos, las alertas y el historial. Podemos dejar para después lectores de código de barras, importaciones masivas, aplicación móvil nativa, pronósticos y compras a proveedores. Ventas, facturación, cobros, pagos y contabilidad quedan fuera de esta versión.

## 2. Evidencia real del stakeholder proxy del equipo

Esta sección reconstruye las respuestas a las mismas siete preguntas utilizando exclusivamente la conversación real del equipo en Microsoft Teams del 25 al 27 de septiembre de 2026. Los participantes que definieron el producto fueron Luis Fernando Baeza Rosales y Flavio César Palacios Salas. Se considera una **validación con stakeholder proxy**, no una entrevista a un propietario real de una tienda. No se agregan respuestas ficticias; cuando el chat no contiene un dato, se registra como pendiente.

### Pregunta 1

**Respuesta reconstruida:** El equipo eligió un sistema de inventario por considerarlo suficientemente claro y realizable dentro del tiempo del curso. El problema acordado es evitar registros manuales o archivos separados que pueden quedar desactualizados y concentrar productos, precios y existencias en un solo sistema.

**Evidencia:** Luis propuso un sistema donde el cliente pudiera ingresar productos y modificar inventario. En la descripción compartida posteriormente indicó la necesidad de centralizar productos, precios y existencias (L. F. Baeza Rosales, comunicación personal, 26 de septiembre de 2026).

### Pregunta 2

**Respuesta reconstruida:** Los usuarios serán el propietario y el personal encargado del inventario. Cada usuario iniciará sesión. El chat no definió permisos diferentes por rol; este punto se dejó para especificación y validación posterior.

**Evidencia:** El documento acordado por el equipo menciona expresamente al propietario, al personal de inventario y el inicio de sesión obligatorio.

### Pregunta 3

**Respuesta reconstruida:** El negocio será una tienda de ropa, calzado y artículos deportivos. Flavio propuso contextualizarla con dos o tres sucursales y el equipo aceptó continuar con la tienda deportiva. El chat confirmó productos, precios y existencias, pero no definió todavía campos como SKU, categoría, talla o color.

**Evidencia:** Flavio propuso la tienda deportiva con dos o tres sucursales; Luis confirmó la elección y redactó la descripción base (F. C. Palacios Salas & L. F. Baeza Rosales, comunicación personal, 26 de septiembre de 2026).

### Pregunta 4

**Respuesta reconstruida:** El equipo acordó registrar productos, actualizar cantidades, modificar información y eliminar registros cuando sea necesario. No se acordaron reglas para transferencias, existencias negativas, errores de captura o productos con historial; estos vacíos se identificaron para la sesión con el Product Owner simulado.

### Pregunta 5

**Respuesta reconstruida:** La consulta de artículos disponibles y existencias es indispensable. La conversación no definió alertas, filtros ni campos del historial. Estos elementos se trataron como necesidades implícitas y se sometieron a validación en la especificación.

### Pregunta 6

**Respuesta reconstruida:** La herramienta debe ser sencilla y accesible desde un navegador. El chat no fijó métricas de rendimiento, respaldo o disponibilidad. Sí estableció autenticación como condición de acceso.

### Pregunta 7

**Respuesta reconstruida:** La prioridad es el manejo de productos y existencias. Se excluyen expresamente ventas, facturación y pagos. El criterio de selección fue mantener una complejidad razonable para el tiempo del curso.

## 3. Necesidades identificadas

| ID | Tipo | Necesidad | Evidencia o inferencia |
|---|---|---|---|
| NE-01 | Explícita | Registrar, consultar y modificar productos. | Mencionada por el equipo y por el Product Owner. |
| NE-02 | Explícita | Actualizar existencias mediante entradas, salidas y transferencias. | Actualización indicada por el equipo; tipos de movimiento precisados en la entrevista simulada. |
| NE-03 | Explícita | Exigir autenticación para utilizar el sistema. | Incluida en `proyecto_base.md`. |
| NE-04 | Explícita | Consultar existencias por producto y sucursal. | Contexto de dos o tres sucursales y validación del Product Owner. |
| NI-01 | Implícita | Conservar un historial auditable de movimientos. | Se infiere de la necesidad de resolver discrepancias y atribuir cambios. |
| NI-02 | Implícita | Impedir existencias negativas y transferencias parciales. | Se infiere de la integridad necesaria para que el inventario sea confiable. |
| NI-03 | Implícita | Desactivar productos con historial en vez de eliminarlos físicamente. | Se infiere de la trazabilidad requerida. |
| RT-01 | Restricción | Entregar una primera versión viable en 12 semanas. | Restricción académica y criterio expresado por el equipo. |
| RN-01 | Restricción de negocio | No incluir ventas, facturación, pagos ni contabilidad. | Exclusión expresa de `proyecto_base.md` y validación del Product Owner. |

## 4. Observación de validez

La sesión del agente es una simulación de elicitación y la conversación de Teams es evidencia real del acuerdo del equipo. Antes de implementar el sistema en un entorno comercial, las reglas añadidas por el Product Owner simulado deberán validarse con un propietario o encargado real de una tienda deportiva.

## 5. Referencias

IEEE Computer Society. (2024). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide V4.0a)*. https://www.computer.org/education/bodies-of-knowledge/software-engineering

Wiegers, K. E., & Beatty, J. (2013). *Software requirements* (3rd ed.). Microsoft Press.

## 6. Declaración de uso de inteligencia artificial

Se utilizó OpenAI Codex para representar al Product Owner ficticio solicitado por la actividad, ordenar la evidencia de Teams y apoyar la corrección gramatical y la redacción. El alumno revisó las respuestas, distinguió expresamente la simulación de la evidencia real y asume la responsabilidad sobre el transcript final. No se inventó ni presentó una persona ficticia como cliente real.
