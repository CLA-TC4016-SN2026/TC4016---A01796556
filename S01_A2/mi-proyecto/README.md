# Sistema web de inventario

Estructura inicial del proyecto de la materia **Análisis, Diseño y Construcción del Software**. En esta etapa solo se preparan las carpetas principales y se documenta su propósito; el backend y el frontend se implementarán en semanas posteriores.

## Alcance previsto

- Catálogo de productos.
- Consulta de existencias.
- Registro de entradas y salidas.
- Historial de movimientos.
- Alertas de stock bajo.

## Stack planeado

- Backend: Python y FastAPI.
- Base de datos: PostgreSQL.
- Frontend: React, TypeScript y Vite.
- Entorno: Docker Compose.
- Versionado: Git y GitHub.

## Estructura

```text
mi-proyecto/
├── backend/
│   ├── contenido.txt
│   └── src/
│       └── contenido.txt
├── frontend/
│   ├── contenido.txt
│   └── src/
│       └── contenido.txt
├── docker/
│   └── contenido.txt
├── docs/
│   └── contenido.txt
├── .gitignore
├── contenido.txt
└── README.md
```

## Estado actual

- [x] Stack tecnológico definido.
- [x] Carpetas raíz creadas.
- [x] Propósito de cada carpeta documentado.
- [x] Patrones de exclusión de Git preparados.
- [ ] Scaffolding del backend, programado para semanas 5–6.
- [ ] Scaffolding del frontend, programado para semanas 7–8.

## Uso responsable del repositorio

No deben confirmarse archivos `.env`, credenciales, tokens, entornos virtuales, dependencias descargadas ni productos de compilación. Antes de cada commit se recomienda ejecutar `git status` y `git diff --cached`.

## Primer commit sugerido

```text
chore(setup): initialize inventory project structure
```
