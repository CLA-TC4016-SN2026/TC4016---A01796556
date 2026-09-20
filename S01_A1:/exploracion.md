# Prompts y respuestas

**Alumno:** Israel Agustín Vargas Monroy  
**Matrícula:** A01796556  
**Materia:** Análisis, Diseño y Construcción del Software  
**Modalidad:** Individual  
**Fecha de elaboración:** 20 de septiembre de 2026

## Configuración documentada y procedimiento

### Entorno utilizado

- **Agente:** OpenAI Codex, utilizado desde ChatGPT Work. La interfaz no mostró un identificador público más específico del modelo, por lo que no se inventó un número de versión.
- **Fuente primaria:** edición oficial vigente del *Guide to the Software Engineering Body of Knowledge*, versión 4.0a, descargada de `https://ieeecs-media.computer.org/media/education/swebok/swebok-v4.pdf` el 20 de septiembre de 2026.
- **Identificación del archivo consultado:** portada “Released August 2026”, 411 páginas y SHA-256 `0c9db4da0b2425c58cf778b99131eb31bb6c188e98b67163a549a1366d96e511`.
- **Fuentes de contraste:** *The Scrum Guide* (2020) para precisar qué es Scrum y el Manifiesto Ágil (2001) para contrastar sus valores.
- **Formato de salida:** Markdown con codificación UTF-8.

### Pasos realizados

1. Descargué la versión vigente del SWEBOK desde el sitio oficial de IEEE Computer Society y revisé la portada, el prólogo, la introducción, la tabla I.1 y el capítulo 10.
2. Confirmé en la tabla I.1 los nombres y el orden de las 18 áreas de conocimiento; no utilicé listas de blogs ni de versiones anteriores como fuente de verdad.
3. Redacté un primer borrador de cada prompt e identifiqué ambigüedades que podían producir respuestas superficiales o mezclar SWEBOK v3 con v4.0a.
4. Refiné los prompts para exigir versión, alcance, formato, terminología y referencias específicas.
5. Contrasté las respuestas del agente con las secciones citadas de las fuentes primarias y corregí las afirmaciones que podían inducir a error.
6. Revisé manualmente la coherencia, la ortografía, las citas y el cumplimiento de los criterios solicitados.

### Problemas encontrados y solución aplicada

| Problema | Riesgo | Solución aplicada |
|---|---|---|
| La bibliografía de la actividad fecha SWEBOK v4.0a como 2024, mientras que el archivo oficial consultado indica “Released August 2026”. | Citar una edición distinta de la realmente consultada. | Se cita como 2026 la versión 4.0a descargada y se aclara que la versión 4.0 fue aprobada originalmente en 2024. |
| Algunas páginas relacionadas con certificación todavía resumen 15 áreas. | Mezclar SWEBOK v3 con las 18 áreas de v4.0a. | Se verificó la lista contra la tabla I.1 de la versión 4.0a, donde aparecen 18 áreas. |
| La consigna coloca a Scrum junto con modelos de proceso. | Presentar Scrum erróneamente como un modelo de ciclo de vida equivalente a cascada. | Se conserva la comparación solicitada, pero se aclara que Scrum es un marco de trabajo ligero; puede emplearse dentro de un enfoque ágil, iterativo e incremental. |
| Una comparación rápida podía repetir que en Agile “no se documenta”. | Reforzar un estereotipo incorrecto. | Se verificó el capítulo 10, que identifica esa afirmación como una concepción errónea, y se describió la documentación como suficiente y adaptada al contexto. |

### Evidencia de iteración y refinamiento

| Consulta | Debilidad del borrador inicial | Refinamiento incorporado al prompt final |
|---|---|---|
| 1 | “¿Qué es SWEBOK?” podía generar una definición genérica. | Se exigió distinguir la guía del cuerpo de conocimiento, explicar su propósito profesional y sustentar la respuesta con la introducción oficial. |
| 2 | “Resume las áreas” podía recuperar las 15 áreas de v3 o alterar sus nombres. | Se indicó v4.0a, el orden oficial, exactamente 18 áreas, nombre original, traducción y una sola oración por área. |
| 3 | “Compara cascada, iterativo y Scrum” podía tratar los tres como conceptos equivalentes. | Se pidió aclarar la naturaleza de cada uno, usar los cuatro criterios obligatorios y evitar el mito de que Agile elimina la documentación. |

## Prompt 01

> Actúa como tutor de posgrado en ingeniería de software. Con base principalmente en la introducción del SWEBOK Guide v4.0a, explica en español qué es el SWEBOK y cuál es su propósito dentro de la ingeniería de software profesional. Distingue explícitamente entre el cuerpo de conocimiento y la guía que lo organiza; indica quién la publica, qué significa “conocimiento generalmente aceptado” y menciona sus cinco objetivos oficiales. No presentes el SWEBOK como una metodología ni como una norma obligatoria. Redacta entre 300 y 450 palabras e incluye citas parentéticas en formato APA 7 con sección o página cuando sea posible.

### Respuesta

El **SWEBOK** (*Software Engineering Body of Knowledge*) se refiere al cuerpo de conocimiento de la ingeniería de software, mientras que el **SWEBOK Guide** es la guía que organiza y facilita el acceso a una parte de ese conocimiento. La distinción es importante: la guía no contiene todo lo que se sabe sobre ingeniería de software ni sustituye la literatura especializada; funciona como un mapa consensuado de los temas que la comunidad profesional reconoce como relevantes. La publica la IEEE Computer Society y su versión 4.0a estructura el campo en 18 áreas de conocimiento, descritas mediante conceptos, procesos, prácticas, entradas, salidas, herramientas y técnicas (IEEE Computer Society, 2026, “Introduction to the Guide”, pp. xxxvii–xxxix).

La expresión **conocimiento generalmente aceptado** alude al conocimiento respaldado por un consenso amplio entre profesionales y académicos, aplicable en numerosos contextos y documentado en literatura reconocida. Esto no significa que todas sus prácticas deban utilizarse en todos los proyectos. La selección concreta depende del dominio, el riesgo, el tamaño, las restricciones y el ciclo de vida del producto. Por ello, SWEBOK no es una metodología de desarrollo, una receta paso a paso ni una norma de cumplimiento obligatorio.

La guía declara cinco objetivos oficiales:

1. Promover una visión consistente de la ingeniería de software en todo el mundo.
2. Especificar su alcance y aclarar su relación con disciplinas como ciencias de la computación, dirección de proyectos, ingeniería informática y matemáticas.
3. Caracterizar los contenidos de la disciplina.
4. Proporcionar acceso temático al cuerpo de conocimiento de la ingeniería de software.
5. Servir como base para el desarrollo curricular y para materiales de certificación y licenciamiento profesional (IEEE Computer Society, 2026, “Introduction to the Guide”, p. xxxviii).

En la práctica profesional, SWEBOK ofrece un vocabulario común para que equipos, organizaciones, docentes y organismos de certificación puedan comunicarse con mayor precisión. También permite identificar qué conocimientos necesita fortalecer una persona o un equipo, relacionar actividades del ciclo de vida y evaluar prácticas con una perspectiva integral. Su valor no radica en imponer una única forma de desarrollar software, sino en delimitar la disciplina y orientar el aprendizaje y la toma de decisiones con una base compartida.

## Prompt 02

> Usa la tabla I.1 y los capítulos del SWEBOK Guide v4.0a como fuente principal. Resume exactamente las 18 áreas de conocimiento en el orden oficial. Para cada área incluye: número, nombre oficial en inglés, traducción al español y una sola oración que describa con precisión su alcance. Distingue las áreas propias de ingeniería de software (capítulos 1–15) de las áreas fundacionales (capítulos 16–18) e identifica cuáles se añadieron en la versión 4. No mezcles la lista con SWEBOK v3 ni inventes áreas. Termina con una observación breve sobre la relación entre las áreas y una cita APA 7.

### Respuesta

El SWEBOK v4.0a organiza el conocimiento en **15 áreas de ingeniería de software** y **3 áreas fundacionales** (IEEE Computer Society, 2026, tabla I.1, p. xxxix).

#### Áreas de ingeniería de software

1. **Software Requirements — Requisitos de software:** comprende la elicitación, análisis, especificación, validación y gestión de las necesidades y restricciones que debe satisfacer el software.
2. **Software Architecture — Arquitectura de software:** estudia las estructuras fundamentales del sistema, sus elementos y relaciones, las preocupaciones de los interesados, las vistas arquitectónicas y la evaluación de decisiones de arquitectura.
3. **Software Design — Diseño de software:** transforma requisitos y decisiones arquitectónicas en una descripción implementable mediante principios, estrategias, modelos, patrones y evaluación de la calidad del diseño.
4. **Software Construction — Construcción de software:** abarca la creación detallada de software mediante codificación, verificación, pruebas unitarias y de integración, depuración, reutilización y aplicación de estándares.
5. **Software Testing — Pruebas de software:** aborda la evaluación dinámica del producto a través de niveles, técnicas, medidas y procesos de prueba para descubrir fallos y aportar evidencia de calidad.
6. **Software Engineering Operations — Operaciones de ingeniería de software:** cubre la planeación, entrega, despliegue, operación, monitoreo, soporte, continuidad y control de software en ambientes productivos.
7. **Software Maintenance — Mantenimiento de software:** trata la modificación, análisis, evolución, soporte, migración y eventual retiro del software después de su entrega inicial.
8. **Software Configuration Management — Gestión de la configuración del software:** controla la identificación, versionado, cambio, estado, auditoría, construcción y liberación de los elementos de configuración.
9. **Software Engineering Management — Gestión de la ingeniería de software:** integra iniciación, alcance, planeación, ejecución, revisión, control, cierre y medición del trabajo de ingeniería de software.
10. **Software Engineering Process — Proceso de ingeniería de software:** estudia la definición e implementación de procesos, los modelos de ciclo de vida y la evaluación y mejora de dichos procesos.
11. **Software Engineering Models and Methods — Modelos y métodos de ingeniería de software:** reúne principios y tipos de modelado, análisis de modelos y métodos heurísticos, formales, de prototipado y ágiles.
12. **Software Quality — Calidad del software:** comprende fundamentos, planificación, aseguramiento, control, medición y mejora de la calidad del producto y de los procesos relacionados.
13. **Software Security — Seguridad del software:** integra la seguridad a lo largo del ciclo de vida mediante requisitos, diseño, construcción, pruebas, gestión de vulnerabilidades y prácticas para dominios específicos.
14. **Software Engineering Professional Practice — Práctica profesional de la ingeniería de software:** aborda profesionalismo, ética, responsabilidad, trabajo en equipo, dinámica de grupos y habilidades de comunicación.
15. **Software Engineering Economics — Economía de la ingeniería de software:** aplica análisis económico, valor, costo, beneficio, riesgo y decisiones de inversión al ciclo de vida del software.

#### Áreas fundacionales

16. **Computing Foundations — Fundamentos de computación:** reúne bases de programación, algoritmos, estructuras de datos, sistemas, bases de datos, redes, interacción humano-computadora e inteligencia artificial necesarias para la ingeniería de software.
17. **Mathematical Foundations — Fundamentos matemáticos:** proporciona lógica, técnicas de demostración, estructuras discretas, probabilidad, estadística, métodos numéricos y teoría de grafos para razonar con precisión.
18. **Engineering Foundations — Fundamentos de ingeniería:** aporta procesos de ingeniería, resolución de problemas, medición, métodos empíricos, estándares y análisis de causa raíz aplicables al trabajo profesional.

Las tres áreas incorporadas en la versión 4 son **Software Architecture**, **Software Engineering Operations** y **Software Security**. Aunque cada área tiene un alcance propio, en un proyecto real interactúan: por ejemplo, un cambio de requisitos puede afectar la arquitectura, el diseño, las pruebas, la configuración, la seguridad, las operaciones y la economía del producto. Por ello, la lista debe interpretarse como una organización del conocimiento, no como una secuencia rígida de fases (IEEE Computer Society, 2026, “Introduction to the Guide”, pp. xxxvii–xxxix).

## Prompt 03

> Compara en español el modelo en cascada, un ciclo de vida iterativo y Scrum con base en el capítulo 10 del SWEBOK Guide v4.0a y en The Scrum Guide 2020. Antes de la tabla, aclara en dos o tres oraciones que cascada e iterativo describen modelos o características de ciclo de vida, mientras que Scrum es un marco de trabajo ligero para generar valor mediante soluciones adaptativas. Después presenta una tabla con exactamente estos criterios: flexibilidad, visibilidad del progreso, adecuación para requisitos cambiantes y documentación requerida. Evita afirmar que Scrum elimina la documentación y distingue la flexibilidad entre sprints de la protección del Sprint Goal durante un sprint. Cierra con una conclusión sobre cuándo resulta más apropiado cada enfoque e incluye citas APA 7.

### Respuesta

El **modelo en cascada** describe un ciclo de vida predictivo y principalmente secuencial, mientras que un **ciclo de vida iterativo** desarrolla el producto mediante ciclos repetidos que permiten ajustar estimaciones y decisiones conforme aumenta el aprendizaje. **Scrum no es un ciclo de vida idéntico a los dos anteriores**, sino un marco de trabajo ligero que implementa control empírico mediante transparencia, inspección y adaptación para generar valor en problemas complejos (IEEE Computer Society, 2026, cap. 10, pp. 10-5–10-7; Schwaber & Sutherland, 2020).

| Criterio | Modelo en cascada | Modelo iterativo | Scrum |
|---|---|---|---|
| **Flexibilidad** | **Baja.** El alcance y los requisitos se definen temprano; las fases avanzan en orden y volver a una etapa anterior suele requerir control formal de cambios y retrabajo. | **Media a alta.** Cada iteración permite revisar el producto y modificar estimaciones, prioridades o decisiones con base en lo aprendido. | **Alta, con límites deliberados.** El Product Backlog puede evolucionar; durante el Sprint se negocia el alcance sin poner en riesgo el Sprint Goal. |
| **Visibilidad del progreso** | **Basada en fases, hitos y documentos.** El avance puede observarse mediante entregables aprobados, aunque el software integrado y utilizable suele aparecer tarde. | **Frecuente.** Cada ciclo produce resultados parciales, prototipos o incrementos que facilitan retroalimentación y evaluación temprana. | **Muy frecuente y estructurada.** Los artefactos, compromisos, Daily Scrum, Sprint Review y un Increment usable permiten inspeccionar el avance en cada Sprint. |
| **Adecuación para requisitos cambiantes** | **Baja.** Funciona mejor cuando los requisitos son estables, comprendidos y sujetos a controles formales, especialmente en entornos predictivos. | **Alta.** Es apropiado cuando la comprensión del problema crece durante el proyecto y los requisitos pueden refinarse entre ciclos. | **Muy alta para problemas complejos.** La revisión del Increment y la adaptación del Product Backlog permiten responder continuamente al cambio, sin abandonar el foco del Sprint actual. |
| **Documentación requerida** | **Generalmente alta y formal.** Es un enfoque orientado por documentos, con especificaciones y aprobaciones que sirven como entradas y salidas de las fases. | **Moderada y evolutiva.** Se documenta lo necesario para conservar decisiones, trazabilidad y continuidad, actualizando los artefactos en cada ciclo. | **Suficiente y adaptada al producto.** Scrum define artefactos y compromisos, pero no prescribe toda la documentación técnica; el equipo conserva la necesaria para calidad, regulación, operación y mantenimiento. |

El enfoque de **cascada** puede ser apropiado cuando existe alta estabilidad de requisitos, fuertes obligaciones contractuales o regulatorias y un cambio costoso. Un enfoque **iterativo** resulta conveniente cuando se necesita aprender gradualmente, reducir incertidumbre o validar decisiones técnicas mediante ciclos. **Scrum** es especialmente útil ante problemas complejos que requieren entregas frecuentes, colaboración continua y adaptación, siempre que la organización comprenda que agilidad no significa ausencia de disciplina ni de documentación. El propio SWEBOK advierte que considerar Agile como un método sin documentos es una concepción errónea (IEEE Computer Society, 2026, p. 10-7).

## Referencias

Beck, K., Beedle, M., van Bennekum, A., Cockburn, A., Cunningham, W., Fowler, M., Grenning, J., Highsmith, J., Hunt, A., Jeffries, R., Kern, J., Marick, B., Martin, R. C., Mellor, S., Schwaber, K., Sutherland, J., & Thomas, D. (2001). *Manifesto for Agile Software Development*. https://agilemanifesto.org/

IEEE Computer Society. (2026). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide)* (Version 4.0a; H. Washizaki, Ed.). https://ieeecs-media.computer.org/media/education/swebok/swebok-v4.pdf

Schwaber, K., & Sutherland, J. (2020). *The Scrum Guide: The definitive guide to Scrum: The rules of the game*. https://scrumguides.org/docs/scrumguide/v2020/2020-Scrum-Guide-US.pdf

## Declaración de uso de inteligencia artificial

Conforme al propósito de la actividad, utilicé OpenAI Codex para ejecutar las consultas documentadas, organizar la información y apoyar la revisión de ortografía y redacción. Contrasté las respuestas con las fuentes primarias citadas, seleccioné las correcciones necesarias y asumo la responsabilidad académica por el contenido final de la entrega.
