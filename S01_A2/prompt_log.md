# Bitácora de prompts y revisión responsable

**Alumno:** Israel Agustín Vargas Monroy

**Matrícula:** A01796556

**Agente:** OpenAI Codex con acceso controlado a terminal y archivos del espacio de trabajo

**Fecha:** 20 de septiembre de 2026

## Propósito

Esta bitácora documenta cómo se utilizó el agente, cómo se refinaron los prompts y qué revisión se realizó antes de permitir cambios. Los textos entre bloques de cita corresponden a los prompts utilizados.

## Prompt 01 — Recomendación inicial del stack

> Actúa como arquitecto de software. Recomiéndame un stack para un sistema web de inventario con backend, frontend y base de datos. El sistema administrará productos, existencias, entradas, salidas, historial de movimientos y alertas de stock bajo. Desarrollo en una MacBook Pro M3 con macOS; mi lenguaje más fuerte es Python y tengo fundamentos de JavaScript. El proyecto es académico, debe ser fácil de instalar y explicar, pero permitir crecimiento posterior. Compara FastAPI, Flask, Express/NestJS; React/Vite, Vue/Vite y Next.js. Justifica la decisión con rendimiento, facilidad de uso, ecosistema, mantenibilidad, integridad de datos, portabilidad y costo. No generes código ni scaffolding detallado todavía.

### Resultado evaluado

El agente recomendó FastAPI, PostgreSQL y React con Vite. La propuesta fue coherente con las restricciones, pero la primera respuesta daba demasiado peso al rendimiento teórico y no explicaba suficientemente la integridad de los movimientos de inventario.

## Prompt 02 — Refinamiento crítico

> Revisa críticamente la recomendación anterior. No asumas que la herramienta más rápida es automáticamente la mejor. Explica por qué PostgreSQL y sus transacciones son relevantes para evitar movimientos de inventario inconsistentes; señala en qué condiciones Flask, NestJS, Vue o Next.js serían mejores opciones; separa hechos documentados de preferencias personales. Conserva una solución monolítica y evita microservicios o Kubernetes porque exceden el alcance del curso.

### Resultado evaluado

El agente mejoró la recomendación al vincular PostgreSQL con transacciones y concurrencia, y reconoció escenarios donde las alternativas serían razonables. Se aceptó el stack con la condición de posponer versiones exactas y archivos de configuración hasta las semanas de scaffolding.

## Prompt 03 — Estructura mínima

> Dentro de `S01_A2/`, crea únicamente la estructura `mi-proyecto/backend/src`, `mi-proyecto/frontend/src`, `mi-proyecto/docker` y `mi-proyecto/docs`. Agrega un archivo `contenido.txt` en cada carpeta, incluida la raíz `mi-proyecto`, explicando qué contendrá. Agrega también un README útil y un `.gitignore` apropiado para Python, Node, macOS y archivos de entorno. No crees endpoints, componentes, manifiestos de dependencias, Dockerfiles ni archivos de código.

### Revisión previa de operaciones

Antes de crear archivos se verificó que:

- las rutas estaban dentro de `S01_A2/`;
- no existían comandos destructivos como `rm`, sobrescrituras masivas o cambios fuera del espacio autorizado;
- no se solicitaban `sudo`, contraseñas, tokens ni secretos;
- la operación no generaría código fuera del alcance;
- los archivos de entorno quedarían excluidos mediante `.gitignore`.

### Resultado evaluado

Se creó la estructura completa. Posteriormente se comprobó que todas las carpetas solicitadas contienen `contenido.txt` y que no existen archivos de código fuente.

## Prompt 04 — Comparativa de agentes

> Investiga OpenCode, Claude Code y GitHub Copilot/Codex usando fuentes oficiales vigentes al 20 de septiembre de 2026. Compara modelo base, modo de uso, soporte a lenguajes, ejecución de comandos, licencia y costo. Distingue GitHub Copilot de OpenAI Codex aunque la consigna los agrupe. No afirmes que una herramienta soporta un precio, modelo o licencia sin una fuente oficial; marca como variable cualquier dato dependiente del plan.

### Resultado evaluado

El agente produjo una tabla inicial. Se corrigió para diferenciar la licencia abierta del cliente Codex de los términos comerciales del servicio y para aclarar que OpenCode no incluye gratuitamente el consumo de todos los modelos conectados.

## Prompt 05 — Mensaje de commit

> Propón un único mensaje para el primer commit de esta actividad usando Conventional Commits 1.0.0. El cambio configura la estructura inicial de un sistema de inventario y agrega documentación, sin implementar todavía funcionalidades.

### Respuesta seleccionada

```text
chore(setup): initialize inventory project structure
```

El tipo `chore` es adecuado porque el commit configura la estructura y documentación inicial sin introducir todavía una funcionalidad de usuario. La forma `<type>(<scope>): <description>` cumple la especificación de Conventional Commits (Conventional Commits, 2026).

## Comandos Git revisados

Estos comandos deben ejecutarse desde la raíz del repositorio del curso `TC4016-A01796556`, después de copiar `S01_A2/`. La URL real del repositorio debe sustituir el marcador; no se inventó una dirección remota.

```bash
git status
git add S01_A2
git diff --cached
git commit -m "chore(setup): initialize inventory project structure"
git remote -v
git remote add origin <URL_REAL_DEL_REPOSITORIO>
git push -u origin main
```

Si `origin` ya existe, **no** debe ejecutarse de nuevo `git remote add origin`; se verifica primero con `git remote -v`. Si la rama principal del repositorio no se llama `main`, se debe sustituir por su nombre real. El comando `git diff --cached` permite revisar exactamente qué se enviará antes de crear el commit.

## Criterio de uso responsable

No se ejecutaron automáticamente operaciones externas, instalación de paquetes ni `git push` porque requieren la URL y autenticación reales del repositorio del alumno. Esta limitación evita fabricar evidencia o enviar cambios a un destino no verificado. El usuario conserva la decisión final sobre el commit y el push.

## Referencias

Chacon, S., & Straub, B. (2014). *Pro Git* (2.ª ed.). Apress. https://git-scm.com/book/en/v2

Conventional Commits. (2026). *Conventional Commits 1.0.0*. https://www.conventionalcommits.org/es/v1.0.0/

## Declaración de uso de inteligencia artificial

Utilicé OpenAI Codex para generar recomendaciones y operaciones dentro de un espacio controlado. Revisé rutas, alcance, posibles efectos y contenido antes de aceptar las modificaciones; también utilicé el agente como apoyo para ortografía y redacción.
