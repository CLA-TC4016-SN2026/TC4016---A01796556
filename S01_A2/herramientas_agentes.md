# Comparativa de herramientas de agentes de IA

**Alumno:** Israel Agustín Vargas Monroy

**Matrícula:** A01796556

**Fecha de consulta:** 20 de septiembre de 2026

## Alcance y método

Comparé OpenCode, Claude Code y el conjunto GitHub Copilot/Codex utilizando documentación oficial, páginas de precios y licencias vigentes en la fecha indicada. Los modelos y precios cambian con frecuencia, por lo que los datos económicos representan una fotografía de la fecha de consulta y deben verificarse de nuevo antes de contratar un servicio.

La consigna agrupa “GitHub Copilot/Codex”, pero no son el mismo producto. GitHub Copilot es el servicio de asistencia y agentes de GitHub; OpenAI Codex es un agente que puede utilizarse desde terminal, IDE, web o nube y también puede aparecer como integración dentro de planes de Copilot. La tabla conserva la categoría solicitada, pero marca esta diferencia para evitar una comparación engañosa.

## Tabla comparativa

| Criterio | OpenCode | Claude Code | GitHub Copilot / OpenAI Codex |
|---|---|---|---|
| **Modelo base** | No tiene un único modelo obligatorio. Permite seleccionar modelos de más de 75 proveedores y modelos locales; OpenCode Zen ofrece una selección administrada. | Utiliza modelos de la familia Claude. El usuario puede elegir alias como `sonnet` u `opus`, un identificador completo o el modo automático según su plan. | Copilot ofrece un catálogo de modelos que depende del plan y la configuración. Codex permite seleccionar modelos de OpenAI; al 20-09-2026 la documentación recomienda la familia GPT-5.6 y muestra GPT-6 Astra para ciertos accesos. |
| **Modo de uso** | Interfaz de terminal, aplicación de escritorio y extensión de IDE; también se integra con editores compatibles mediante protocolos de agente. | Principalmente terminal, con integraciones para IDE compatibles y flujos no interactivos para automatización. | Copilot funciona en IDE, CLI, GitHub.com, nube y dispositivos compatibles. Codex está disponible en CLI, extensión de IDE, web, escritorio y entornos cloud. |
| **Soporte a lenguajes** | Multilenguaje; depende del modelo seleccionado y puede cargar servidores LSP para aportar contexto del lenguaje. | Multilenguaje; comprende el repositorio y puede trabajar con los lenguajes que soporten los modelos Claude y las herramientas instaladas. | Multilenguaje. Copilot contextualiza archivos, dependencias y repositorios; Codex trabaja con las herramientas y lenguajes disponibles en el entorno local o cloud. |
| **Ejecución de comandos** | Sí. Incluye herramientas para actuar sobre el código y permite configurar permisos de lectura, edición y ejecución. | Sí. Puede usar Bash y otras herramientas; ofrece listas de herramientas permitidas o bloqueadas y modos de permisos. | Sí. Los modos agente de Copilot pueden modificar repositorios y generar ramas o pull requests. Codex inspecciona, edita y ejecuta comandos dentro de un sandbox con políticas de aprobación configurables. |
| **Licencia** | Cliente de código abierto con licencia MIT. Los modelos conectados conservan sus propias licencias y términos. | Software propietario de Anthropic; el repositorio indica derechos reservados y uso sujeto a sus términos comerciales. | GitHub Copilot es un servicio comercial propietario. El cliente de Codex publicado por OpenAI usa Apache-2.0, pero los modelos y servicios alojados se rigen por los términos del plan correspondiente. |
| **Costo al 20-09-2026** | El cliente puede instalarse sin costo. El consumo depende del proveedor elegido; OpenCode Zen opera bajo pago por uso y mantiene algunos modelos gratuitos. | Claude Code se incluye en Pro por USD 20/mes y en Max desde USD 100/mes; también puede usarse con facturación de API por consumo. Precios antes de impuestos y sujetos a región. | Copilot ofrece Free limitado, Student sin costo para estudiantes verificados, Pro USD 10/mes, Pro+ USD 39/mes y Max USD 100/mes. Codex se incluye desde ChatGPT Free con límites; Go cuesta USD 8/mes, Plus USD 20/mes y Pro parte de USD 100/mes, además de uso por API. |

## Análisis de ventajas y limitaciones

### OpenCode

Su principal ventaja es la libertad para seleccionar proveedor, modelo o ejecución local, además de una licencia abierta. Es apropiado cuando se busca evitar dependencia de un solo proveedor o controlar costos con modelos distintos. Como contrapartida, el usuario debe tomar más decisiones sobre proveedores, credenciales, modelos y políticas; la calidad y el costo no dependen solo del cliente OpenCode.

### Claude Code

Ofrece una experiencia de terminal cohesionada con los modelos Claude y controles explícitos sobre herramientas. Puede ser conveniente para tareas de razonamiento prolongado y para usuarios que ya pagan una suscripción compatible. Su principal limitación frente a OpenCode es la dependencia del ecosistema y términos de Anthropic, además de que la API se factura por separado de la suscripción de consumo.

### GitHub Copilot y Codex

GitHub Copilot destaca por su integración con el ciclo de trabajo de GitHub, los IDE y los pull requests. Codex añade un flujo centrado en terminal, revisión de cambios, ejecución de comandos y ambientes locales o cloud. La amplitud de superficies es una ventaja, pero obliga a distinguir qué plan, modelo y política de permisos está activo para no asumir que todas las funciones están disponibles en cualquier cuenta.

## Herramienta elegida

Elegí **OpenAI Codex** porque ya cuento con acceso dentro de ChatGPT y porque se adapta a mi flujo de trabajo en macOS, Python, terminal y repositorios Git. Para esta actividad fue especialmente útil que pudiera inspeccionar el directorio, proponer una estructura, crear únicamente los archivos autorizados y revisar el resultado sin generar código prematuramente. Además, sus controles de sandbox y aprobación permiten revisar comandos antes de ejecutarlos, lo cual favorece un uso responsable del agente.

La elección no significa que Codex sea universalmente superior. OpenCode sería preferible si necesitara independencia de proveedor o modelos locales, mientras que Claude Code sería una alternativa razonable si el equipo estuviera estandarizado en el ecosistema de Anthropic. En este proyecto, Codex reduce fricción porque coincide con las herramientas y el acceso que ya utilizo.

## Referencias

Anthropic. (2026a). *Claude Code overview*. https://docs.anthropic.com/en/docs/claude-code/overview

Anthropic. (2026b). *CLI reference*. https://docs.anthropic.com/en/docs/claude-code/cli-usage

Anthropic. (2026c). *Plans and pricing*. https://claude.com/pricing

Anthropic. (2026d). *Claude Code license*. https://github.com/anthropics/claude-code/blob/main/LICENSE.md

GitHub. (2026a). *GitHub Copilot features*. https://docs.github.com/en/copilot/get-started/features

GitHub. (2026b). *GitHub Copilot licenses*. https://docs.github.com/en/billing/concepts/product-billing/github-copilot-licenses

OpenCode. (2026a). *Intro*. https://opencode.ai/docs/

OpenCode. (2026b). *Models*. https://opencode.ai/docs/models/

OpenCode. (2026c). *Permissions*. https://opencode.ai/docs/permissions/

OpenCode. (2026d). *Zen*. https://opencode.ai/docs/zen

OpenCode. (2026e). *Package license*. https://github.com/anomalyco/opencode/blob/dev/packages/opencode/package.json

OpenAI. (2026a). *Codex CLI*. https://learn.chatgpt.com/docs/codex/cli

OpenAI. (2026b). *Models*. https://learn.chatgpt.com/docs/models

OpenAI. (2026c). *Pricing*. https://learn.chatgpt.com/docs/pricing

OpenAI. (2026d). *Agent approvals and security*. https://learn.chatgpt.com/docs/agent-approvals-security

OpenAI. (2026e). *Codex license*. https://github.com/openai/codex/blob/main/docs/license.md

## Declaración de uso de inteligencia artificial

Utilicé OpenAI Codex para localizar fuentes, organizar la tabla y apoyar la corrección de ortografía y redacción. Verifiqué los datos contra documentación oficial, distinguí hechos de inferencias y asumo la responsabilidad por la comparación y la conclusión.
