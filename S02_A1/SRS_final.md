# Especificación de Requerimientos de Software — StockIV

**Versión:** 1.0

**Estado:** Validado para diseño académico; pendiente de validación comercial

**Alumno:** Israel Agustín Vargas Monroy

**Matrícula:** A01796556

**Fecha:** 27 de septiembre de 2026

## 1. Introducción

### 1.1 Propósito del documento

Este documento especifica los requerimientos de la primera versión de StockIV mediante la estructura simplificada de una especificación de requerimientos de software (IEEE, 1998). Su finalidad es servir como contrato verificable entre las necesidades del stakeholder y las siguientes etapas del proyecto: diseño de API, implementación y pruebas. Los identificadores RF-XX y RF-XX-AC-Y deberán conservarse en los artefactos posteriores para mantener la trazabilidad, precisión y verificabilidad de los requisitos (IEEE Computer Society, 2024; Wiegers & Beatty, 2013).

### 1.2 Alcance del sistema

StockIV será una aplicación web para centralizar el inventario de una tienda de ropa, calzado y artículos deportivos que opera inicialmente con dos o tres sucursales. Permitirá autenticar usuarios, administrar productos y sucursales, consultar existencias, registrar entradas, salidas y transferencias, configurar mínimos, visualizar alertas y revisar el historial de movimientos.

La primera versión no procesará ventas, facturas, cobros, pagos, contabilidad, órdenes de compra ni pronósticos. Tampoco incluirá una aplicación móvil nativa, lectores de códigos de barras o importaciones masivas. Estas exclusiones mantienen un alcance realizable durante las 12 semanas del curso.

### 1.3 Definiciones y acrónimos

| Término | Definición |
|---|---|
| SRS | Especificación de Requerimientos de Software. |
| SKU | Identificador único asignado a un producto. |
| Existencia | Cantidad disponible de un producto en una sucursal. |
| Movimiento | Operación que incrementa o disminuye una existencia. |
| Transferencia | Salida de una sucursal y entrada equivalente en otra, ejecutadas de forma atómica. |
| Umbral mínimo | Cantidad definida para determinar que una existencia es baja. |
| RBAC | Control de acceso basado en roles. |
| Product Owner | Representante de las necesidades y prioridades del producto. |
| Criterio de aceptación | Condición verificable en formato Dado-Cuando-Entonces. |

## 2. Descripción general

### 2.1 Perspectiva del producto

StockIV será un sistema web independiente compuesto por una interfaz de usuario, una API de backend y una base de datos relacional. Sustituirá hojas de cálculo y registros manuales dispersos. La existencia se controlará por la combinación de producto y sucursal, mientras que cada cambio quedará registrado como un movimiento auditable.

### 2.2 Funciones del producto

El sistema proporcionará las siguientes funciones principales:

- Autenticación y cierre de sesión.
- Administración de usuarios y roles.
- Administración de productos y sucursales.
- Consulta y filtrado de existencias.
- Registro de entradas y salidas.
- Transferencias atómicas entre sucursales.
- Configuración de umbrales y visualización de alertas.
- Consulta del historial de movimientos.

### 2.3 Características de los usuarios

| Rol | Perfil | Permisos principales |
|---|---|---|
| Administrador | Propietario o responsable general con conocimientos básicos de aplicaciones web. | Gestionar usuarios, sucursales y productos; consultar y modificar inventario; revisar historial y alertas. |
| Encargado de inventario | Personal operativo que recibe, acomoda o transfiere mercancía. | Consultar productos, registrar movimientos y transferencias, configurar mínimos y revisar historial y alertas. No puede administrar usuarios ni desactivar sucursales. |

### 2.4 Restricciones

- La primera versión deberá completarse dentro de las 12 semanas del curso.
- El acceso requerirá una cuenta activa y autenticada.
- El alcance se limitará al inventario; se excluyen ventas, facturación, pagos y contabilidad.
- Las cantidades se manejarán como unidades enteras; no se admitirán fracciones.
- El despliegue deberá ser accesible desde un navegador web moderno.
- Los movimientos confirmados no podrán editarse ni eliminarse desde la interfaz.

### 2.5 Supuestos y dependencias

- Cada producto tendrá un SKU definido por el negocio.
- Las sucursales contarán con conexión a internet durante la operación.
- El personal recibirá una inducción de hasta 30 minutos.
- Las reglas obtenidas del Product Owner simulado se validarán con un representante comercial antes de un despliegue productivo.

## 3. Requerimientos específicos

### 3.1 Requerimientos funcionales y criterios de aceptación

Los criterios se expresan en formato Dado-Cuando-Entonces para describir ejemplos de comportamiento observables y derivar pruebas posteriores, conforme al enfoque BDD propuesto por North (2006).

#### RF-01 — Autenticar usuarios

El sistema deberá permitir que un usuario activo inicie sesión con correo electrónico y contraseña, obtenga los permisos de su rol y cierre la sesión. Los mensajes de error no deberán revelar si una cuenta existe.

**RF-01-AC-1 — Acceso válido**

- **Dado que** existe un usuario activo con credenciales válidas,
- **cuando** envía su correo y contraseña,
- **entonces** el sistema inicia una sesión, muestra la pantalla de inventario y habilita únicamente las funciones de su rol.

**RF-01-AC-2 — Acceso rechazado**

- **Dado que** el correo o la contraseña son incorrectos, o la cuenta está inactiva,
- **cuando** se intenta iniciar sesión,
- **entonces** el sistema rechaza el acceso, no crea una sesión y muestra el mensaje genérico “Credenciales inválidas”.

#### RF-02 — Administrar usuarios y roles

El sistema deberá permitir al administrador crear usuarios, asignarles el rol Administrador o Encargado de inventario y activar o desactivar sus cuentas. El nombre tendrá de 2 a 100 caracteres; el correo deberá tener formato válido y ser único sin distinguir mayúsculas; y la contraseña temporal tendrá al menos 12 caracteres, una mayúscula, una minúscula y un dígito.

**RF-02-AC-1 — Creación de usuario**

- **Dado que** un administrador autenticado proporciona un nombre de 2 a 100 caracteres, un correo válido no registrado, un rol permitido y una contraseña temporal que cumple la política definida en RF-02,
- **cuando** confirma el registro,
- **entonces** el sistema crea la cuenta activa con el rol seleccionado y registra quién realizó la operación.

**RF-02-AC-2 — Protección por rol**

- **Dado que** un Encargado de inventario tiene una sesión activa,
- **cuando** intenta crear, cambiar el rol o desactivar a otro usuario,
- **entonces** el sistema rechaza la operación con estado de autorización denegada y no modifica ninguna cuenta.

#### RF-03 — Administrar productos

El sistema deberá permitir registrar y editar productos con SKU, nombre, descripción, categoría, talla, color, precio y estado. El SKU admitirá de 1 a 40 caracteres en mayúsculas, números o guion; el nombre tendrá de 2 a 120 caracteres; la categoría será obligatoria; la descripción admitirá hasta 500 caracteres; talla y color hasta 50 caracteres cada uno; y el precio cumplirá RD-06. Administradores y encargados podrán registrar y editar; solo el administrador podrá desactivar. Un producto con movimientos conservará su historial y no se eliminará físicamente.

**RF-03-AC-1 — Registro correcto de producto**

- **Dado que** un Administrador o Encargado captura un SKU no registrado y todos los campos dentro de las longitudes y formatos definidos en RF-03 y RD-06,
- **cuando** confirma el registro,
- **entonces** el sistema crea un producto activo y lo hace consultable sin modificar existencias.

**RF-03-AC-2 — SKU duplicado**

- **Dado que** ya existe un producto activo o inactivo con el mismo SKU,
- **cuando** se intenta registrar otro producto con ese SKU,
- **entonces** el sistema rechaza el registro, identifica el campo duplicado y conserva sin cambios el catálogo.

**RF-03-AC-3 — Desactivación con historial**

- **Dado que** un producto tiene al menos un movimiento y un administrador solicita eliminarlo,
- **cuando** confirma la acción,
- **entonces** el sistema lo marca como inactivo, conserva sus existencias e historial y evita que se use en movimientos nuevos.

#### RF-04 — Consultar inventario

El sistema deberá mostrar productos y existencias, con búsqueda por SKU o nombre y filtros por categoría, estado y sucursal. Para cada producto deberá presentar la cantidad por sucursal y el total general.

**RF-04-AC-1 — Consulta por filtros**

- **Dado que** existen productos de distintas categorías y sucursales,
- **cuando** el usuario combina un texto de búsqueda con categoría y sucursal,
- **entonces** el sistema muestra únicamente los productos que cumplen todos los filtros y sus existencias en la sucursal seleccionada.

**RF-04-AC-2 — Total consolidado**

- **Dado que** un producto tiene 8 unidades en la sucursal A, 5 en B y 2 en C,
- **cuando** el usuario consulta su detalle sin filtrar una sucursal,
- **entonces** el sistema muestra el desglose 8, 5 y 2 y un total de 15 unidades.

#### RF-05 — Registrar entradas y salidas

El sistema deberá permitir registrar entradas y salidas indicando producto activo, sucursal activa, cantidad entera positiva y motivo. Cada operación deberá actualizar la existencia y producir un movimiento inmutable.

**RF-05-AC-1 — Entrada válida**

- **Dado que** un producto activo tiene 10 unidades en una sucursal activa,
- **cuando** un usuario autorizado registra una entrada de 4 unidades con motivo,
- **entonces** la existencia queda en 14 y el historial conserva usuario, fecha y hora, producto, sucursal, tipo, cantidad y motivo.

**RF-05-AC-2 — Salida sin disponibilidad**

- **Dado que** un producto tiene 3 unidades disponibles en una sucursal,
- **cuando** se solicita una salida de 4 unidades,
- **entonces** el sistema rechaza la operación, informa que la existencia es insuficiente y mantiene la cantidad en 3 sin crear movimiento.

**RF-05-AC-3 — Salida válida**

- **Dado que** un producto activo tiene 9 unidades en una sucursal activa,
- **cuando** un Administrador o Encargado registra una salida de 4 unidades con un motivo válido,
- **entonces** la existencia queda en 5 y el historial conserva usuario, fecha y hora, producto, sucursal, tipo, cantidad y motivo.

#### RF-06 — Transferir existencias entre sucursales

El sistema deberá transferir una cantidad entera positiva de un producto activo entre dos sucursales activas distintas. El decremento en origen y el incremento en destino deberán confirmarse o revertirse como una sola transacción.

**RF-06-AC-1 — Transferencia válida**

- **Dado que** un producto tiene 10 unidades en la sucursal A y 2 en la sucursal B,
- **cuando** el usuario transfiere 4 unidades de A hacia B,
- **entonces** A queda con 6, B con 6 y el historial relaciona ambos movimientos mediante un identificador único de transferencia.

**RF-06-AC-2 — Transferencia no confirmada**

- **Dado que** el origen no tiene cantidad suficiente o falla el registro de cualquiera de las dos partes,
- **cuando** se intenta confirmar la transferencia,
- **entonces** el sistema revierte la operación completa, mantiene ambas existencias originales y no conserva un movimiento parcial.

#### RF-07 — Configurar mínimos y visualizar alertas

El sistema deberá permitir definir un umbral mínimo entero no negativo para cada combinación de producto y sucursal. Deberá marcar como baja toda existencia menor o igual al umbral y permitir filtrar esas alertas.

**RF-07-AC-1 — Activación de alerta**

- **Dado que** el mínimo de un producto en una sucursal es 5,
- **cuando** una salida reduce la existencia de 6 a 5,
- **entonces** el sistema marca esa combinación como existencia baja y la incluye en el filtro de alertas.

**RF-07-AC-2 — Eliminación de alerta**

- **Dado que** una combinación está alertada con mínimo 5 y existencia 3,
- **cuando** una entrada eleva la existencia a 7,
- **entonces** el sistema deja de mostrarla como existencia baja sin eliminar su historial.

#### RF-08 — Consultar historial de movimientos

El sistema deberá permitir consultar movimientos por intervalo de fechas, producto, sucursal, usuario y tipo. El detalle deberá conservar fecha y hora, usuario, producto, sucursal, tipo, cantidad, motivo y referencia de transferencia cuando corresponda.

**RF-08-AC-1 — Consulta trazable**

- **Dado que** existen movimientos de varios productos y fechas,
- **cuando** el usuario filtra por un SKU y un intervalo inclusivo de fechas,
- **entonces** el sistema muestra solo los movimientos coincidentes, ordenados del más reciente al más antiguo, con todos los campos de auditoría.

**RF-08-AC-2 — Inmutabilidad desde la interfaz**

- **Dado que** existe un movimiento confirmado,
- **cuando** cualquier usuario intenta editarlo o eliminarlo desde la interfaz o la API,
- **entonces** el sistema rechaza la solicitud y conserva intactos el movimiento y la existencia resultante.

#### RF-09 — Administrar sucursales

El sistema deberá permitir al administrador registrar y editar sucursales con código único, nombre y estado, así como desactivarlas cuando no conserven existencias. El código admitirá de 2 a 12 caracteres en mayúsculas, números o guion y el nombre tendrá de 2 a 100 caracteres.

**RF-09-AC-1 — Registro de sucursal**

- **Dado que** un administrador captura un código y nombre válidos que no están registrados,
- **cuando** confirma la operación,
- **entonces** el sistema crea una sucursal activa disponible para consultas y movimientos con existencia inicial de cero por producto.

**RF-09-AC-2 — Desactivación con historial**

- **Dado que** una sucursal tiene historial de movimientos y todas sus existencias están en cero,
- **cuando** un administrador confirma su desactivación,
- **entonces** el sistema la marca como inactiva, conserva el historial y evita movimientos nuevos en ella.

**RF-09-AC-3 — Desactivación rechazada por existencias**

- **Dado que** una sucursal conserva al menos una unidad de cualquier producto,
- **cuando** un administrador intenta desactivarla,
- **entonces** el sistema rechaza la operación, identifica que debe transferirse o ajustarse la existencia y mantiene la sucursal activa.

### 3.2 Requerimientos no funcionales

- **RNF-01 — Rendimiento:** bajo una carga de 50 usuarios concurrentes, 10 000 productos y 100 000 movimientos, al menos el 95 % de las consultas de inventario e historial deberá responder en un máximo de 2 segundos, medidos en el servidor y sin contar la latencia de internet.

- **RNF-02 — Seguridad:** las contraseñas deberán almacenarse mediante un algoritmo de hash adaptativo como Argon2id o bcrypt; todo tráfico deberá usar HTTPS; y cada endpoint protegido deberá validar sesión y rol en el backend.

- **RNF-03 — Gestión de sesión:** una sesión deberá expirar después de 30 minutos de inactividad y el cierre de sesión deberá invalidar el token o la sesión del lado del servidor.

- **RNF-04 — Disponibilidad:** el servicio deberá alcanzar 99.5 % de disponibilidad mensual, excluyendo mantenimientos anunciados con al menos 24 horas de anticipación.

- **RNF-05 — Respaldo y recuperación:** la base de datos deberá respaldarse al menos una vez cada 24 horas. El objetivo de punto de recuperación será de 24 horas y el objetivo de tiempo de recuperación será de 4 horas.

- **RNF-06 — Usabilidad:** después de una inducción máxima de 30 minutos, al menos 4 de 5 usuarios representativos deberán completar sin ayuda las tareas de consultar existencia, registrar una entrada y transferir mercancía; cada tarea deberá requerir como máximo dos errores corregibles.

- **RNF-07 — Compatibilidad:** la interfaz deberá funcionar en las dos versiones estables más recientes de Chrome, Edge, Firefox y Safari, con resoluciones desde 768 × 1024 píxeles y superiores.

- **RNF-08 — Mantenibilidad:** las reglas de dominio del backend deberán contar con pruebas automatizadas y una cobertura de sentencias mínima de 80 %, medida en la integración continua.

### 3.3 Requerimientos de dominio

- **RD-01:** el SKU deberá ser único en todo el sistema, sin importar si el producto está activo o inactivo.

- **RD-02:** la existencia de una combinación producto-sucursal nunca podrá ser negativa.

- **RD-03:** cada movimiento deberá usar una cantidad entera mayor que cero e incluir un motivo no vacío de entre 3 y 200 caracteres.

- **RD-04:** una transferencia deberá tener sucursales de origen y destino distintas y ejecutarse de forma atómica.

- **RD-05:** los movimientos confirmados serán inmutables; una corrección se representará mediante un movimiento compensatorio que haga referencia al movimiento original.

- **RD-06:** los precios se expresarán en pesos mexicanos (MXN), admitirán dos decimales y no podrán ser negativos.

- **RD-07:** un producto o sucursal con historial no se eliminará físicamente; se cambiará a estado inactivo.

- **RD-08:** ventas, facturación, cobros, pagos, contabilidad y órdenes de compra no forman parte de la primera versión.

- **RD-09:** el código de sucursal deberá ser único sin distinguir mayúsculas; una sucursal solo podrá desactivarse cuando todas sus existencias sean cero.

## 4. Trazabilidad

Los casos de uso y sus relaciones se representan con la notación UML 2.5.1 (Object Management Group, 2017).

| Requerimiento | Caso de uso | Criterios de aceptación | Fuente principal |
|---|---|---|---|
| RF-01 | UC-01 Autenticarse | RF-01-AC-1, RF-01-AC-2 | Proyecto base y entrevista |
| RF-02 | UC-02 Administrar usuarios | RF-02-AC-1, RF-02-AC-2 | Entrevista; necesidad de permisos |
| RF-03 | UC-03 Administrar productos | RF-03-AC-1 a RF-03-AC-3 | Proyecto base y entrevista |
| RF-04 | UC-04 Consultar inventario | RF-04-AC-1, RF-04-AC-2 | Proyecto base y entrevista |
| RF-05 | UC-05 Registrar entrada; UC-06 Registrar salida | RF-05-AC-1, RF-05-AC-2 | Proyecto base y entrevista |
| RF-06 | UC-07 Transferir existencias | RF-06-AC-1, RF-06-AC-2 | Contexto multisucursal y entrevista |
| RF-07 | UC-08 Configurar mínimos; UC-10 Visualizar alertas | RF-07-AC-1, RF-07-AC-2 | Necesidad implícita validada |
| RF-08 | UC-09 Consultar historial | RF-08-AC-1, RF-08-AC-2 | Necesidad implícita validada |
| RF-09 | UC-12 Administrar sucursales | RF-09-AC-1 a RF-09-AC-3 | Contexto multisucursal y entrevista |

## 5. Referencias

IEEE. (1998). *IEEE Std 830-1998: IEEE recommended practice for software requirements specifications*. https://doi.org/10.1109/IEEESTD.1998.88286

IEEE Computer Society. (2024). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide V4.0a)*. https://www.computer.org/education/bodies-of-knowledge/software-engineering

North, D. (2006). *Introducing BDD*. https://dannorth.net/introducing-bdd/

Object Management Group. (2017). *OMG Unified Modeling Language (OMG UML), version 2.5.1*. https://www.omg.org/spec/UML/2.5.1/PDF

Wiegers, K. E., & Beatty, J. (2013). *Software requirements* (3rd ed.). Microsoft Press.

## 6. Declaración de uso de inteligencia artificial

Se utilizó OpenAI Codex para apoyar la clasificación de requerimientos, proponer criterios de aceptación, revisar la consistencia técnica y realizar correcciones gramaticales y de redacción. El alumno evaluó cada propuesta, sustituyó formulaciones genéricas por condiciones verificables, contrastó el contenido con la evidencia del equipo y asume la responsabilidad por la especificación final.
