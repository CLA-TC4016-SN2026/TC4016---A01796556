# Decisión del stack tecnológico

**Alumno:** Israel Agustín Vargas Monroy

**Matrícula:** A01796556

**Materia:** Análisis, Diseño y Construcción del Software

**Actividad:** S01-A2 — Configuración del Entorno de Desarrollo

**Fecha:** 20 de septiembre de 2026

## 1. Descripción del proyecto

El proyecto será un **sistema web de inventario**. En sus siguientes etapas permitirá registrar productos, consultar existencias, capturar entradas y salidas, conservar el historial de movimientos y mostrar alertas de stock bajo. La arquitectura tendrá un frontend independiente que consumirá una API REST y un backend responsable de las reglas de negocio y del acceso a datos.

En esta actividad únicamente se define el stack y se prepara la estructura raíz. No se genera todavía el scaffolding detallado ni código funcional, porque la consigna reserva esas actividades para las semanas 5–8.

## 2. Restricciones reales consideradas

- Equipo de desarrollo: MacBook Pro con procesador Apple Silicon M3 y macOS.
- Lenguaje con mayor experiencia: Python.
- Experiencia disponible: desarrollo de APIs, inteligencia artificial, manejo de datos y fundamentos de JavaScript.
- Tipo de aplicación: web con backend, frontend y base de datos relacional.
- Alcance académico: debe ser sencillo de instalar, probar y explicar, pero permitir crecimiento posterior.
- Portabilidad: el entorno deberá poder reproducirse en otros equipos mediante contenedores.
- Seguridad: no se almacenarán credenciales ni archivos `.env` en Git.

## 3. Stack seleccionado

| Capa | Tecnología elegida | Función prevista |
|---|---|---|
| Backend | Python 3.12+ y FastAPI | API REST, validación de solicitudes y reglas de inventario. |
| Acceso a datos | SQLAlchemy y Alembic | Mapeo objeto-relacional y migraciones versionadas. |
| Base de datos | PostgreSQL 16+ | Productos, existencias y movimientos con integridad transaccional. |
| Frontend | React, TypeScript y Vite | Interfaz de productos, movimientos, búsquedas y alertas. |
| Pruebas | pytest y Vitest | Pruebas del backend y del frontend cuando inicie el desarrollo. |
| Entorno | Docker Desktop para Apple Silicon y Docker Compose | Ejecución reproducible de servicios sin depender de instalaciones locales distintas. |
| Control de versiones | Git, GitHub y Conventional Commits | Historial, colaboración y publicación de la entrega. |
| Agente utilizado | OpenAI Codex | Análisis del stack, generación controlada de archivos y revisión de comandos. |

Las versiones exactas de las dependencias de aplicación se fijarán cuando se realice el scaffolding. De esta forma no se crean archivos de configuración prematuros ni se confunde esta actividad con la implementación.

## 4. Justificación técnica comparativa

### 4.1 Productividad y facilidad de uso

FastAPI aprovecha anotaciones de tipos estándar de Python para validar datos y generar documentación OpenAPI interactiva. Esto reduce código repetitivo y se alinea con el lenguaje que conozco mejor, por lo que puedo concentrarme en las reglas de inventario y no en aprender simultáneamente un ecosistema de backend distinto (FastAPI, 2026a, 2026b).

Flask también utiliza Python y es ligero, pero su núcleo WSGI deja más decisiones de validación, documentación y organización a extensiones externas. Express ofrece una base mínima y flexible en Node.js, mientras que NestJS proporciona una arquitectura robusta con TypeScript; ambas son alternativas válidas, pero aumentarían la curva de aprendizaje del backend sin aportar una ventaja decisiva para el alcance actual (Express.js, 2026; Pallets, 2026; NestJS, 2026).

### 4.2 Rendimiento y adecuación al problema

FastAPI se ejecuta sobre ASGI y está orientado a APIs de alto rendimiento. Para este proyecto, el objetivo no es maximizar solicitudes por segundo, sino evitar que el framework sea un cuello de botella y facilitar operaciones concurrentes de consulta y actualización. El rendimiento real se validará posteriormente con pruebas; no se asume que la elección del framework sustituye la medición.

PostgreSQL resulta apropiado porque los movimientos de inventario exigen transacciones, restricciones y control de concurrencia. Una entrada o salida debe actualizar existencias y registrar el movimiento como una unidad consistente; PostgreSQL trata cada sentencia dentro de una transacción y ofrece mecanismos de aislamiento y control multiversión (PostgreSQL Global Development Group, 2026a, 2026b). SQLite sería útil para un prototipo individual, pero ofrece menos margen para concurrencia y crecimiento multiusuario.

### 4.3 Ecosistema, mantenibilidad y calidad

Python dispone de un ecosistema maduro para APIs, pruebas y análisis de datos. FastAPI integra tipos, validación y contratos OpenAPI, mientras que SQLAlchemy y Alembic permiten separar las reglas del dominio de la persistencia y registrar la evolución del esquema. Esta combinación facilita pruebas unitarias y evita depender de consultas SQL dispersas en los controladores.

En frontend, React permite dividir la interfaz en componentes reutilizables; esto encaja con pantallas repetitivas como tablas, filtros, formularios y alertas (React, 2026). Se usará TypeScript para detectar errores de tipos antes de ejecutar la aplicación y para mantener contratos claros con la API.

### 4.4 Experiencia de desarrollo

Vite ofrece un servidor de desarrollo con recarga rápida y una compilación optimizada para producción (Vite, 2026a). Frente a Next.js, React con Vite mantiene una separación clara entre el frontend y la API de FastAPI y evita introducir renderizado del lado del servidor que el sistema interno de inventario no necesita inicialmente. Vue 3 con Vite sería igualmente viable y tiene una curva de entrada amigable, pero React aporta mayor continuidad con el ecosistema y experiencia profesional que planeo fortalecer.

### 4.5 Portabilidad y reproducibilidad

Docker Compose permitirá definir más adelante frontend, backend y PostgreSQL como servicios coordinados en un solo archivo. Docker lo describe como una herramienta para definir y ejecutar aplicaciones de múltiples contenedores, redes y volúmenes desde una configuración común (Docker, 2026). Esto reduce diferencias entre macOS, Linux y los equipos de evaluación.

### 4.6 Costo y riesgo de adopción

Las tecnologías centrales seleccionadas son de código abierto y no requieren licencias comerciales para el desarrollo académico. El principal costo es el tiempo de aprendizaje de React y TypeScript. Se mitigará mediante componentes pequeños, contratos OpenAPI, pruebas y entregas iterativas; no se añadirá complejidad de microservicios, Kubernetes o renderizado del lado del servidor mientras el alcance no lo justifique.

## 5. Evaluación crítica de la recomendación del agente

La recomendación inicial del agente fue coherente con mis restricciones, pero no la acepté sin revisión. Confirmé en documentación oficial que FastAPI ofrece validación y documentación automática, que Vite está orientado a una experiencia de desarrollo rápida y que PostgreSQL proporciona las propiedades transaccionales necesarias para movimientos de inventario. También comparé las alternativas de la consigna para evitar justificar la decisión únicamente con “ya conozco Python”.

El agente propuso inicialmente generar archivos de configuración y ejemplos de endpoints. Rechacé esa parte porque excedía el alcance de la actividad: en esta semana solo se requiere una estructura básica. La salida final conserva únicamente carpetas raíz, archivos `contenido.txt`, un README y un `.gitignore`; no contiene código funcional.

## 6. Decisión final

El stack elegido es **FastAPI + PostgreSQL + React/TypeScript con Vite**, acompañado por Docker Compose, Git y GitHub. La decisión se apoya en seis criterios: productividad, rendimiento suficiente, integridad de datos, mantenibilidad, experiencia de desarrollo y portabilidad. Para un sistema de inventario académico ofrece un equilibrio adecuado entre simplicidad inicial y capacidad de evolución.

## Referencias

Docker. (2026). *Docker Compose*. https://docs.docker.com/compose/

Express.js. (2026). *Express: Node.js web application framework*. https://expressjs.com/

FastAPI. (2026a). *FastAPI*. https://fastapi.tiangolo.com/

FastAPI. (2026b). *Features*. https://fastapi.tiangolo.com/features/

NestJS. (2026). *Documentation*. https://docs.nestjs.com/

Pallets. (2026). *Flask documentation (3.1.x)*. https://flask.palletsprojects.com/en/stable/

PostgreSQL Global Development Group. (2026a). *Transactions*. https://www.postgresql.org/docs/18/tutorial-transactions.html

PostgreSQL Global Development Group. (2026b). *Transaction isolation*. https://www.postgresql.org/docs/17/transaction-iso.html

React. (2026). *React: The library for web and native user interfaces*. https://react.dev/

Vite. (2026a). *Getting started*. https://vite.dev/guide/

Vite. (2026b). *Why Vite*. https://vite.dev/guide/why

## Declaración de uso de inteligencia artificial

Utilicé OpenAI Codex para explorar alternativas, crear la estructura solicitada y apoyar la revisión de ortografía y redacción. Revisé los comandos antes de autorizarlos, contrasté las recomendaciones con las fuentes oficiales citadas y asumo la responsabilidad académica por la selección final.
