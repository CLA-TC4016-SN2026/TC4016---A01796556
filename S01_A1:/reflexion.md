# Actividad S01-A1: Reflexión y análisis crítico

**Alumno:** Israel Agustín Vargas Monroy  
**Matrícula:** A01796556  
**Materia:** Análisis, Diseño y Construcción del Software  
**Fecha de elaboración:** 20 de septiembre de 2026

## 1. ¿En qué aspectos el agente fue útil para comprender los fundamentos del SWEBOK?

El agente fue útil para convertir un documento técnico de más de cuatrocientas páginas en una primera estructura de estudio manejable. Me permitió relacionar las 18 áreas de conocimiento y reconocer que no representan fases consecutivas de un proyecto, sino perspectivas que se conectan a lo largo del ciclo de vida; por ejemplo, una decisión de requisitos puede repercutir en arquitectura, seguridad, pruebas, operaciones y economía. Esta síntesis me ayudó a construir un mapa inicial antes de profundizar en los capítulos específicos del SWEBOK.

También resultó útil para comparar conceptos que suelen presentarse de manera aislada. Al pedir una tabla con criterios explícitos, pude observar que las diferencias entre cascada, un ciclo iterativo y Scrum no se limitan a “tradicional frente a ágil”, sino que incluyen la forma de gestionar el cambio, mostrar progreso y conservar documentación. En un contexto profesional, esta capacidad de síntesis puede acelerar la preparación de una discusión técnica o la identificación de preguntas relevantes, siempre que la respuesta se trate como un punto de partida y no como autoridad final.

La calidad de la respuesta mejoró al delimitar versión, fuente, extensión y formato en cada prompt. Esto me mostró que interactuar con un agente requiere habilidades similares a especificar requisitos: una petición ambigua produce resultados difíciles de verificar, mientras que una instrucción con alcance, restricciones y criterios de aceptación facilita una salida útil. Por tanto, la elaboración del prompt también funcionó como ejercicio de análisis y comunicación técnica.

## 2. ¿En qué aspectos fue insuficiente o impreciso? ¿Cómo lo detectaste?

El agente podía responder de forma convincente aun cuando existían ambigüedades de versión. Un riesgo concreto fue recuperar la estructura de SWEBOK v3, que contiene 15 áreas, en lugar de las 18 áreas de v4.0a. Detecté y resolví este riesgo comparando la lista generada con la tabla I.1 de la introducción oficial, donde los capítulos 1 al 15 corresponden a áreas de ingeniería de software y los capítulos 16 al 18 a áreas fundacionales (IEEE Computer Society, 2026, p. xxxix).

Otra imprecisión potencial consistía en clasificar Scrum simplemente como un “modelo de proceso”, al mismo nivel conceptual que cascada. El capítulo 10 del SWEBOK presenta cascada dentro de los ciclos predictivos y describe los ciclos iterativos por sus repeticiones; además, señala a Scrum como un método popular para la gestión de proyectos dentro del contexto ágil (IEEE Computer Society, 2026, pp. 10-5–10-7). La guía oficial de Scrum lo define con mayor precisión como un marco de trabajo ligero, por lo que ajusté la comparación sin ignorar el formato solicitado por la actividad (Schwaber & Sutherland, 2020).

También identifiqué que la afirmación “en Agile no se documenta” es una simplificación incorrecta. El SWEBOK la menciona expresamente como una concepción errónea y aclara que los documentos siguen siendo necesarios, aunque su cantidad y forma dependan del contexto (IEEE Computer Society, 2026, pp. 10-6–10-7). Finalmente, la bibliografía de la consigna asocia v4.0a con 2024, mientras que la portada del archivo oficial vigente consultado indica “Released August 2026”; para evitar ocultar la discrepancia, registré la edición realmente usada y distinguí su fecha de la aprobación original de v4.0 en 2024. Estos casos muestran que una respuesta fluida no garantiza precisión y que la verificación debe revisar tanto el contenido como la versión de la fuente.

## 3. ¿Cómo planeas usar el agente durante el resto del curso para maximizar su utilidad sin depender ciegamente de él?

Planeo utilizar el agente en tres momentos: exploración, práctica y revisión. Primero le pediré mapas de conceptos, preguntas y comparaciones para ubicar rápidamente un tema; después lo usaré para generar escenarios, contraejemplos o ejercicios que me obliguen a aplicar el conocimiento; al final, le solicitaré una revisión contra una rúbrica explícita. En ninguno de estos momentos consideraré su respuesta como evidencia suficiente por sí sola.

Para verificar contenido técnico, priorizaré fuentes primarias como SWEBOK, normas IEEE/ISO, documentación oficial de marcos de trabajo y artículos académicos. Registraré la versión y fecha de consulta, especialmente en documentos que evolucionan, y comprobaré afirmaciones sensibles mediante capítulo, sección o página. Si encuentro una discrepancia, documentaré qué dijo el agente, qué establece la fuente y por qué acepté o rechacé la respuesta; esta trazabilidad es importante en la práctica profesional porque una decisión errónea puede afectar seguridad, costo, cumplimiento o mantenibilidad.

También evitaré delegar al agente decisiones que requieran juicio profesional sin proporcionarle contexto suficiente. Formularé prompts con objetivo, audiencia, alcance, restricciones, formato y criterios de aceptación, y conservaré la responsabilidad sobre la selección final. Usado de esta manera, el agente puede reducir el tiempo de búsqueda y mejorar la claridad de mi trabajo, mientras que la lectura crítica, la validación y la argumentación permanecen bajo mi control.

## Contraste adicional

La revisión se realizó en dos pasadas: una primera generación orientada a responder la consigna y una segunda evaluación crítica con el rol simulado de profesor. Ambas coincidieron en la utilidad general de la síntesis, pero la segunda pasada detectó tres puntos que requerían mayor precisión: la versión exacta del SWEBOK, la naturaleza de Scrum y el tratamiento de la documentación en enfoques ágiles. Esta coincidencia parcial confirma que pedir al mismo agente una crítica separada puede ayudar a encontrar omisiones, pero no sustituye el contraste con una fuente independiente, porque ambas pasadas pueden compartir los mismos sesgos o errores.

## Referencias

IEEE Computer Society. (2026). *Guide to the Software Engineering Body of Knowledge (SWEBOK Guide)* (Version 4.0a; H. Washizaki, Ed.). https://ieeecs-media.computer.org/media/education/swebok/swebok-v4.pdf

Schwaber, K., & Sutherland, J. (2020). *The Scrum Guide: The definitive guide to Scrum: The rules of the game*. https://scrumguides.org/docs/scrumguide/v2020/2020-Scrum-Guide-US.pdf

## Declaración de uso de inteligencia artificial

Conforme al propósito de la actividad, utilicé OpenAI Codex para ejecutar las consultas documentadas, organizar ideas y apoyar la revisión de ortografía y redacción. Verifiqué las afirmaciones técnicas con las fuentes primarias citadas, realicé la selección crítica del contenido y asumo la responsabilidad académica por la versión final.
