

Notebooklm py es · MD
notebooklm-py
<p align="left"> <img src="https://raw.githubusercontent.com/teng-lin/notebooklm-py/main/notebooklm-py.png" alt="logo de notebooklm-py" width="128"> </p>
Una skill integral para Google Gemini Notebook y una API de Python no oficial. Acceso programático completo a las funciones de NotebookLM —incluidas capacidades que la interfaz web no expone— a través de Python, CLI y agentes de IA como Claude Code, Codex y OpenClaw.

Nota (julio de 2026): Google renombró NotebookLM como Gemini Notebook. Sigue siendo el mismo producto independiente (ahora también accesible desde la app de Gemini), los enlaces existentes redirigen automáticamente y esta librería controla el mismo servicio subyacente sin cambios. El paquete conserva el nombre notebooklm-py.

Versión en PyPI Versión de Python Licencia: MIT Tests

<p> <a href="https://trendshift.io/repositories/19116" target="_blank"><img src="https://trendshift.io/api/badge/repositories/19116" alt="teng-lin%2Fnotebooklm-py | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a> </p>
Fuente y desarrollo: https://github.com/teng-lin/notebooklm-py

⚠️ Librería no oficial - Uso bajo su propio riesgo

Esta librería utiliza APIs internas no documentadas de Google que pueden cambiar sin previo aviso.

No afiliada a Google - Es un proyecto de la comunidad
Las APIs pueden romperse - Google puede modificar los endpoints internos en cualquier momento
Aplican límites de uso - El uso intensivo puede ser limitado
Ideal para prototipos, investigación y proyectos personales. Consultar Solución de problemas para consejos de depuración.

Qué se puede construir
🤖 Herramientas para agentes de IA - Integra NotebookLM en Claude Code, Codex y otros agentes LLM. Incluye una skill raíz de NotebookLM para GitHub y para el descubrimiento vía npx skills add, soporte local mediante notebooklm skill install para Claude Code y directorios de skills .agents, y una guía a nivel de repositorio para Codex en AGENTS.md.

📚 Automatización de investigación - Importación masiva de fuentes (URLs, PDFs, YouTube, Google Drive), ejecución de consultas de investigación web/Drive con importación automática, y extracción programática de información. Permite construir pipelines de investigación repetibles.

🎙️ Generación de contenido - Genera audiodescripciones (podcasts), videos, presentaciones de diapositivas, cuestionarios, tarjetas de estudio, infografías, tablas de datos, mapas mentales y guías de estudio. Control total sobre formatos, estilos y resultados.

📥 Descargas y exportación - Descarga localmente todos los artefactos generados (MP3, MP4, PDF, PNG, CSV, JSON, Markdown). Exporta a Google Docs/Sheets. Funciones que la interfaz web no ofrece: descargas por lotes, exportación de cuestionarios/tarjetas en múltiples formatos, extracción de mapas mentales en JSON.

Casos de uso y recetas
NotebookLM es un motor fundamentado (grounded): Gemini hace la lectura pesada y responde a partir de tus propias fuentes con citas. El patrón ganador es dejar que haga el análisis costoso mientras tu agente (Claude Code, Codex, …) orquesta y se encarga del último tramo — usando NotebookLM como una capa de síntesis y memoria sin costo de tokens que un agente controla en un bucle, y extrayendo artefactos estructurados hacia afuera en formatos más completos y automatizables. Recetas que la comunidad construye sobre esta librería, agrupadas según para qué usan NotebookLM:

Ahorrar tokens — dejar que NotebookLM haga el razonamiento costoso:

🪙 Delegación de investigación sin costo de tokens — Cargar 30 documentos en un notebook, dejar que Gemini haga el análisis pesado, y que el agente gaste tokens solo en el pulido final. El agente solo orquesta (create → source add → ask); el razonamiento ocurre del lado del servidor. En la práctica: una guía de cuatro flujos de trabajo para que Claude Code deje de gastar tokens en NotebookLM.
🧠 Destilación de conocimiento → una skill permanente — Ejecutar Deep Research (source add-research "tu tema" --mode deep) o cargar un corpus de documentos, dejar que Gemini de NotebookLM lo condense, e incorporar el resultado en un SKILL.md que el agente carga al iniciar — se construye una vez y se reutiliza sin gasto de tokens ni llamadas de red en tiempo de ejecución, versionado en git e inmune a los cambios de la interfaz. Un experto de dominio empaquetado sin tener que curar fuentes a mano. (Volcar documentos en bruto en una skill aplana la jerarquía; que NotebookLM los condense primero es lo que lo hace funcionar).
✅ Skills autoevaluables — Hacer que NotebookLM genere el conjunto de evaluación — un cuestionario directo de las fuentes — para calificar una skill de agente contra una verdad de referencia en vez de preguntas de prueba que uno mismo podría sesgar. Construir la skill, evaluarla contra los exámenes generados por NotebookLM, iterar hasta aprobar. En la práctica: una skill que obtuvo 4/10 en el primer intento y 10/10 tras una iteración, evaluada por un cuestionario generado por NotebookLM.
Darle memoria al agente — recuperación persistente y fundamentada:

💾 Memoria persistente entre sesiones — Mantener un notebook "Cerebro Maestro"; un paso de cierre agrega las decisiones y correcciones de cada sesión como notas (note create / ask --save-as-note), y una línea en el CLAUDE.md lo consulta (ask) al inicio de la siguiente sesión. El almacenamiento y la recuperación quedan en la infraestructura de Google.
🧩 Memoria fundamentada para agentes de código — Exponer un notebook con la documentación interna, RFCs y arquitectura propios a través del servidor MCP (o simplemente ask) para que un agente responda a partir del código propio con citas, en lugar de suposiciones que solo suenan plausibles — una alternativa sin infraestructura a montar una base de datos vectorial y un pipeline de embeddings propios. En la práctica: convertir un notebook en el "cerebro del proyecto" fundamentado en fuentes que un agente de código consulta antes de escribir.
🪞 Consultar las propias notas / diario — Cargar años de notas diarias, actas de reuniones o un diario y usar ask para obtener respuestas citadas a partir del propio historial — revelando patrones de largo plazo que una búsqueda por palabras clave no puede (por ejemplo, un resumen semanal sintetizado a partir de 282 notas diarias, con cada afirmación enlazada a la entrada de origen). En la práctica: conversar con un año de notas diarias como base de conocimiento citada.
Convertir las fuentes en respuestas y artefactos — respuestas citadas, medios generados y exportaciones:

📞 Base de conocimiento fundamentada / oráculo de soporte técnico (RAG) — Cargar documentación de producto, preguntas frecuentes, RFCs y tickets pasados, y luego usar ask --json para obtener respuestas fundamentadas en las fuentes y citadas para soporte, guardias o consultas internas. O hacer que un agente apunte a toda la documentación de una herramienta en rápida evolución — más de lo que el agente puede mantener en contexto — como un oráculo de resolución de problemas que consulta apenas encuentra un error. En la práctica: OpenClaw hizo que la librería scrapeara las 524 páginas de docs.openclaw.ai, eliminara traducciones duplicadas, y las depurara hasta 269 fuentes limpias (faltantes/sobrantes/duplicadas = 0).
🔁 Reutilización de contenido multiformato — Un mismo conjunto de fuentes, todos los formatos: generate audio (podcast), generate video, generate slide-deck, además de un borrador de blog con generate report, generate quiz y generate flashcards — un solo notebook desplegado en múltiples canales.
📤 Exportaciones masivas y automatizables — Extraer mapas mentales como JSON, cuestionarios/tarjetas como JSON/Markdown/HTML, tablas de datos como CSV, e informes como Markdown — en lote, a archivos locales, directo a Anki, a la herramienta de mapas mentales propia, o a un repositorio (download <tipo> / download <tipo> --all). La mitad programática de "sacar datos", no solo de "meter fuentes".
🕸️ Sincronización con Obsidian / grafo de conocimiento — Ejecutar la CLI desde la raíz del vault para que los artefactos descargados (informes, JSON de mapas mentales, transcripciones) queden como archivos en el grafo de conocimiento propio; algunas skills de la comunidad construidas sobre esta librería incluso resuelven los marcadores de citas de NotebookLM en [[wikilinks]] de Obsidian. Combinar con una audiodescripción tipo podcast para un resumen sonoro de las propias notas. En la práctica: "Claude Code + NotebookLM + Obsidian = GOD MODE".
Ejecutarlo sin supervisión, a escala o desde el celular — programado, sin interfaz gráfica y remoto:

🚨 Generador de manuales de incidentes — Ante una alerta, crear un notebook con la documentación relevante, hacer preguntas de diagnóstico específicas, y generar un informe tipo briefing (generate report --format briefing-doc --wait, luego download report) como manual automatizado.
📚 Constructor de currículos / conjuntos de estudio — Extraer un programa de estudios o una hoja de ruta de desarrollo, crear un notebook por tema (espaciando las llamadas para evitar límites de uso), y generar en bloque podcasts, cuestionarios y tarjetas de estudio para cada uno.
📰 Boletines de audio programados — Combinar auth refresh --quiet (cron/launchd/systemd) con generate audio para publicar un boletín personalizado y actualizado en un feed de podcast según un horario.
📱 NotebookLM desde el celular, controlado por un agente — Alojar el conector MCP remoto detrás de un túnel de Cloudflare/Tailscale y agregarlo como conector personalizado en la web (Conectores de claude.ai, o ChatGPT con Modo Desarrollador). Luego controlar todo el conjunto de herramientas — investigación profunda, ingesta de fuentes, generación de contenido del estudio, preguntas y respuestas citadas — desde la app móvil de claude.ai en movimiento (los conectores MCP de ChatGPT son solo para web), encadenado con otras herramientas MCP propias, en lugar de saltar entre aplicaciones.
Todo esto combina primitivas comunes de la librería — ver la Referencia de la CLI y la API de Python. El pegamento del lado del agente (skills, programación de tareas, estructura del vault) vive en la configuración propia de cada quien, no en este paquete. La cantidad de fuentes por notebook depende del nivel de la cuenta de Google — dividir entre varios notebooks si se alcanza un límite.

¿Recién llega? Empezar con un recorrido: Claude Code + NotebookLM = CHEAT CODE (video) · 5 demos + 50 casos de uso, con prompts.

Formas de uso
Método	Ideal para
API de Python	Integración en aplicaciones, flujos asíncronos, pipelines personalizados
CLI	Scripts de shell, tareas rápidas, automatización de CI/CD
Servidor MCP	Claude Desktop/Code, Codex, etc. — localmente vía stdio, o como conector remoto autoalojado (detrás de un túnel de Cloudflare/Tailscale) accesible desde claude.ai y ChatGPT, incluido el celular.
Servidor REST	Automatización local sobre rutas HTTP protegidas sin lanzar un proceso de CLI por cada llamada
Integración con agentes	Claude Code, Codex, agentes LLM, automatización en lenguaje natural
Funciones
Cobertura completa de NotebookLM
Categoría	Capacidades
Notebooks	Crear, listar, renombrar, eliminar
Fuentes	URLs, YouTube, archivos (PDF, texto, Markdown, Word, EPUB, audio, video, imágenes), Google Drive, texto pegado; actualizar, obtener guía/texto completo
Chat	Preguntas, historial de conversación, personas personalizadas, prompts iniciales sugeridos
Notas	Crear, listar, renombrar, eliminar, guardar respuestas del chat, guardar historial de conversación
Etiquetas de fuentes	Etiquetas de tema generadas por IA o manuales; agregar/quitar fuentes de una etiqueta; filtrar fuentes por etiqueta
Investigación	Agentes de investigación web y de Drive (modos rápido/profundo) con importación automática
Compartir	Enlaces públicos/privados, permisos de usuario (lector/editor), control del nivel de vista
Generación de contenido (todos los tipos de artefactos)
Tipo	Opciones	Formato de descarga
Audiodescripción	4 formatos (análisis profundo, breve, crítica, debate), 3 duraciones, más de 50 idiomas	MP3
Videodescripción	4 formatos (explicativo, breve, cinematográfico, corto), 8 estilos visuales (+ auto/personalizado), además de un alias de CLI dedicado cinematic-video	MP4
Presentación de diapositivas	Formato detallado o de presentador, duración ajustable; revisión de diapositivas individuales	PDF, PPTX
Infografía	3 orientaciones, 3 niveles de detalle	PNG
Cuestionario	Cantidad y dificultad configurables	JSON, Markdown, HTML
Tarjetas de estudio	Cantidad y dificultad configurables	JSON, Markdown, HTML
Informe	Documento de briefing, guía de estudio, entrada de blog, o prompt personalizado	Markdown
Tabla de datos	Estructura personalizada mediante lenguaje natural	CSV
Mapa mental	Árbol de nodos jerárquico — dos variantes: JSON basado en notas o el mapa interactivo más nuevo del estudio (--kind / MindMapKind)	JSON
Más allá de la interfaz web
Capacidades programáticas, por lotes y de archivo local que facilitan la API/CLI — varias en formatos más completos, o a mayor escala, que haciendo clic en la app web:

Descargas por lotes - Descargar todos los artefactos de un tipo a la vez
Exportación de cuestionarios/tarjetas - Obtener archivos JSON, Markdown o HTML estructurados
Extracción de datos de mapas mentales - Exportar árboles jerárquicos en JSON para herramientas de visualización
Exportación de tablas de datos a CSV - Descargar tablas estructuradas como hojas de cálculo
Presentación de diapositivas como PPTX o PDF - Descargar archivos editables de PowerPoint o PDF
Revisión de diapositivas - Modificar diapositivas individuales con prompts en lenguaje natural
Personalización de plantillas de informes - Agregar instrucciones adicionales a las plantillas de formato incorporadas
Guardar el historial del chat como notas - Persistir toda una conversación de preguntas y respuestas (no solo una respuesta puntual) como nota del notebook
Acceso al texto completo de la fuente - Recuperar el contenido de texto indexado de cualquier fuente
Compartir de forma programática - Gestionar permisos sin usar la interfaz
Instalación
La guía de instalación completa — seis perfiles (agente, usuario final, librería, sin interfaz gráfica, colaborador, usuario avanzado), matriz de extras opcionales, notas por plataforma — está en docs/installation.md.

Inicio más rápido (usuarios de CLI y agentes de IA) — instalar la CLI con uv tool (recomendado) o pipx:

bash
uv tool install "notebooklm-py[browser]"   # o: pipx install "notebooklm-py[browser]"
notebooklm login                           # la primera ejecución descarga Chromium automáticamente (~170 MB), luego inicio de sesión de Google
notebooklm auth check --test --json        # verificar: se espera "status": "ok"
¿Por qué uv tool / pipx? Instalan la CLI en su propio entorno aislado y ponen notebooklm en el PATH — sin conflictos de dependencias con otras herramientas, una actualización de una sola línea (uv tool upgrade notebooklm-py) o desinstalación, y, algo crucial, funcionan en macOS moderno (Python de Homebrew) y en Debian/Ubuntu, donde un pip install a nivel de sistema queda bloqueado con error: externally-managed-environment (PEP 668). ¿Todavía no tenés uv? curl -LsSf https://astral.sh/uv/install.sh | sh (o brew install uv / winget install astral-sh.uv).

¿Prefiere pip común? Funciona igual dentro de un entorno virtual (y directamente en Windows, donde Python no está gestionado externamente):

bash
python3 -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install "notebooklm-py[browser]"
Como librería (incorporada en la aplicación propia — sin Playwright, sin Chromium):

bash
uv add notebooklm-py                    # o, dentro de un entorno virtual: pip install notebooklm-py
Si playwright install chromium falla en Linux con TypeError: onExit is not a function, ver la solución alternativa para Linux. Colaboradores: ver CONTRIBUTING.md.

Autenticación y acceso
Autenticación flexible para desarrollo local, servidores sin interfaz gráfica y configuraciones multiusuario:

Tres formas de obtener cookies - Inicio de sesión interactivo con Playwright (predeterminado), importación desde un navegador ya autenticado (login --browser-cookies chrome, sin Playwright), o un token maestro duradero.
Autenticación con token maestro - Genera cookies web frescas bajo demanda sin necesidad de un navegador por sesión (login --master-token --account tucorreo@ejemplo.com), de modo que se autorrepara ante sesiones vencidas sin supervisión — el modelo de autenticación para servidores, CI y el conector MCP remoto (claude.ai / ChatGPT).
Perfiles multicuenta - Cambiar entre cuentas de Google sin necesidad de volver a autenticarse.
Configuración del agente
Opción 1 — Instalación por CLI:

bash
notebooklm skill install
Instala la skill en ~/.claude/skills/notebooklm y en ~/.agents/skills/notebooklm.

Opción 2 — Instalación con npx (a través del ecosistema abierto de skills):

bash
npx skills add teng-lin/notebooklm-py
Obtiene el SKILL.md canónico directamente desde GitHub.

Inicio rápido
<p align="center"> <a href="https://asciinema.org/a/767284" target="_blank"><img src="https://asciinema.org/a/767284.svg" width="600" /></a> <br> <em>Sesión de 16 minutos comprimida a 30 segundos</em> </p>
CLI
bash
# 1. Autenticarse (abre el navegador)
notebooklm login
# O usar Microsoft Edge (para organizaciones que requieren Edge para SSO)
# notebooklm login --browser msedge
# O reutilizar cookies de una sesión de navegador ya iniciada
# notebooklm login --browser-cookies chrome
# notebooklm login --browser-cookies 'chrome::Profile 1'  # un solo perfil de Chromium
# (combinar con --profile para completar un perfil específico;
#  usar --account / --all-accounts después de auth inspect cuando hay
#  varias cuentas de Google conectadas)

# 2. Crear un notebook y agregar fuentes
notebooklm create "Mi investigación"
notebooklm use <id_del_notebook>
notebooklm source add "https://es.wikipedia.org/wiki/Inteligencia_artificial"
notebooklm source add "./paper.pdf"

# 3. Conversar con las fuentes
notebooklm ask "¿Cuáles son los temas clave?"
notebooklm ask --prompt-file ./pregunta_larga.txt  # Leer la pregunta desde un archivo

# 4. Generar contenido (usar --prompt-file para prompts largos)
notebooklm generate audio "hacerlo entretenido" --wait
notebooklm generate video --style whiteboard --wait
notebooklm generate cinematic-video "resumen estilo documental" --wait
notebooklm generate quiz --difficulty hard
notebooklm generate flashcards --quantity more
notebooklm generate slide-deck
notebooklm generate infographic --orientation portrait
notebooklm generate mind-map                       # mapa interactivo del estudio (predeterminado); --kind note-backed para el árbol JSON
notebooklm generate data-table "comparar conceptos clave"

# 5. Descargar artefactos
notebooklm download audio ./podcast.m4a
notebooklm download video ./resumen.mp4
notebooklm download cinematic-video ./documental.mp4
notebooklm download quiz --format markdown ./cuestionario.md
notebooklm download flashcards --format json ./tarjetas.json
notebooklm download slide-deck ./diapositivas.pdf
notebooklm download infographic ./infografia.png
notebooklm download mind-map ./mapamental.json
notebooklm download data-table ./datos.csv
Otros comandos útiles de la CLI:

bash
notebooklm auth check --test         # Diagnosticar problemas de autenticación/cookies
notebooklm auth refresh --quiet      # Renovación puntual de cookies (para cron / launchd / systemd)
notebooklm auth refresh --browser-cookies chrome  # Reextraer y reparar el enrutamiento de cuentas
notebooklm auth inspect --browser 'chrome::Profile 1'  # Previsualizar un perfil de Chromium
notebooklm agent show codex          # Mostrar las instrucciones incluidas para Codex
notebooklm agent show claude         # Mostrar la plantilla de skill incluida para Claude Code
notebooklm language list             # Listar los idiomas de salida disponibles
notebooklm metadata --json           # Exportar metadatos y fuentes del notebook
notebooklm share status              # Ver el estado de lo compartido
notebooklm source add-research "IA" --import-all  # investigación web + importar las fuentes encontradas
notebooklm skill status              # Verificar la instalación local de la skill del agente
notebooklm profile list              # Listar todos los perfiles de cuentas de Google
notebooklm profile switch work       # Cambiar al perfil de cuenta activo
Usar --prompt-file RUTA con ask, los comandos generate basados en prompt, y source add-research cuando el texto sea demasiado largo para la línea de comandos de la shell. Esto lee el texto del prompt/consulta desde un archivo y es independiente de source add ./archivo.pdf, que sigue subiendo ese archivo como fuente de NotebookLM.

API de Python
python
import asyncio
from notebooklm import NotebookLMClient, MindMapKind


async def main():
    async with NotebookLMClient.from_storage() as client:
        # Crear notebook y agregar fuentes
        nb = await client.notebooks.create("Investigación")
        await client.sources.add_url(nb.id, "https://example.com", wait=True)

        # Conversar con las fuentes
        result = await client.chat.ask(nb.id, "Resumir esto")
        print(result.answer)

        # Generar contenido (podcast, video, cuestionario, etc.)
        status = await client.artifacts.generate_audio(nb.id, instructions="hacerlo divertido")
        await client.artifacts.wait_for_completion(nb.id, status.task_id)
        await client.artifacts.download_audio(nb.id, "podcast.m4a")

        # Generar cuestionario y descargarlo como JSON
        status = await client.artifacts.generate_quiz(nb.id)
        await client.artifacts.wait_for_completion(nb.id, status.task_id)
        await client.artifacts.download_quiz(nb.id, "cuestionario.json", output_format="json")

        # Generar un mapa mental mediante la API unificada client.mind_maps (issue #1256) —
        # dos variantes: el mapa interactivo más nuevo del estudio MindMapKind.INTERACTIVE (mostrado;
        # sondeado hasta su finalización por defecto) o el JSON basado en notas MindMapKind.NOTE_BACKED.
        # Ambos se exportan mediante:
        mm = await client.mind_maps.generate(nb.id, kind=MindMapKind.INTERACTIVE)
        await client.artifacts.download_mind_map(nb.id, "mapamental.json", mm.id)


asyncio.run(main())
Documentación
Referencia de la CLI - Documentación completa de comandos
API de Python - Referencia completa de la API
Guía de MCP - Configuración del servidor MCP, transportes y referencia de herramientas
Servidor de API REST - Servidor FastAPI experimental en localhost
Configuración - Almacenamiento y ajustes
Límites de cuota y nivel de cuenta - Límites de notebooks/fuentes/estudio por nivel de cuenta y cómo se corresponden con AccountLimits.tier
Guía de lanzamiento - Lista de verificación de lanzamiento y verificación del empaquetado
Solución de problemas - Problemas comunes y soluciones
Estabilidad de la API - Política de versionado y garantías de estabilidad
Actualización a la versión 0.8.0 - Guía de migración de cambios incompatibles para el contrato de errores y retornos de la v0.8.0
Para colaboradores
Arquitectura - Visión general de la arquitectura y principios de diseño
Guía de desarrollo - Arquitectura, pruebas y lanzamientos
Desarrollo de RPC - Captura y depuración de protocolo
Referencia de RPC - Estructuras de payload
Registro de cambios - Historial de versiones y notas de lanzamiento
Seguridad - Política de seguridad y manejo de credenciales
Licencia
Licencia MIT. Ver LICENSE para más detalles.

