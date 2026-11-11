# AgentRelay

[English](README.md) · [简体中文](README.zh-CN.md) · [Português (Brasil)](README.pt-BR.md) · [Español](README.es.md)

Un servicio local de IA para extensiones de navegador, sitios web y flujos de trabajo de escritorio. Conecta clientes compatibles de Codex, Grok Bot, Antigravity, Grok CLI y DeepSeek Harness a través de una única interfaz HTTP unificada, selecciona una conversación existente y aprovecha su contexto de manera continua en múltiples tareas, sin tener que reconstruir el historial en cada petición.

[GoodExts](https://goodexts.com/) · [Detalles del producto](https://goodexts.com/agentrelay) · [Tutorial de integración con XFollower](https://goodexts.com/blog/xfollower-agentrelay)

![Configuración de conexión de AgentRelay](https://goodexts.com/images/agentrelay/en/home.png)

Este repositorio contiene documentación del producto, habilidades (skills) de integración y distribuciones binarias. El código fuente de la aplicación se mantiene de forma privada. Los componentes de terceros conservan sus respectivas licencias.

## Habilidad de Integración para IA (AI Integration Skill)

Si necesitas que un asistente de programación con IA (Codex, Antigravity, Claude Code, Cursor) integre tu extensión de navegador, aplicación web o cliente de escritorio con AgentRelay, utiliza nuestra habilidad estándar ubicada directamente en la raíz de este repositorio:

- 📖 **Documentación de la Skill**: [`agentrelay-integration/SKILL.md`](agentrelay-integration/SKILL.md)
- 📦 **Descarga del paquete completo**: [`agentrelay-integration.zip`](agentrelay-integration.zip)
- 🔌 **Herramientas incluidas**:
  - `agentrelay-integration/scripts/client.mjs` (Cliente ES module nativo, listo para service workers en Chrome MV3)
  - `agentrelay-integration/scripts/probe.mjs` (Herramienta de diagnóstico CLI y verificación integral)
  - `agentrelay-integration/scripts/self-test.mjs` (Pruebas unitarias locales simuladas sin llamadas reales a IA)

Copia la carpeta `agentrelay-integration/` en tu proyecto o en `~/.codex/skills/`, y dale la instrucción a tu IA:
> *"Lee `agentrelay-integration/SKILL.md` y conecta este proyecto a AgentRelay siguiendo las reglas de ramas..."*

## Novedades en la versión 2.8.3

- **Motores Nativos Grok CLI y DeepSeek Harness (DSH)**: Detección completa de sesiones persistentes en terminal y espacios de trabajo locales, decodificación de flujos comprimidos multi-frame y reenvío bidireccional de tareas entre los 5 clientes locales de IA (Codex, Grok Bot, Antigravity, Grok CLI, DeepSeek Harness).
- **Grok Bot actualizado a 0.12.5 (PR166) y Recuperación de Lectura**: Actualización del binario original que soluciona el error 401 "This version of Grok Bot is no longer supported", e incorpora una cola de recuperación de lectura que evita timeouts intermitentes 502 Bad Gateway.
- **Compactación Nativa de Sesión y Aislamiento de Colas por Propietario**: Soporte dedicado para compactación nativa a nivel de sesión y aislamiento de colas de ejecución para flujos continuos de alta frecuencia (etiquetado y comentarios).
- **Protección del Instalador de Windows e Integridad de Procesos**: Protocolo optimizado de cierre ordenado durante la instalación y actualización, evitando alertas falsas de procesos bloqueados y conflictos con niveles Low Integrity.

## Descargar 2.8.3

| Plataforma | Instalador | Archivo portátil |
| --- | --- | --- |
| Windows 10/11 x64 | [Instalador Windows](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-win-x64.exe) | [Windows ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-win-x64.zip) |
| macOS Apple Silicon | [macOS DMG](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-mac-arm64.dmg) | [macOS ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-mac-arm64.zip) |

[Todas las versiones y sumas de verificación SHA-256](https://github.com/wangsen2020/agentrelay-releases/releases)

Conserva la carpeta portátil completa de Windows o el paquete de la aplicación en macOS. Windows incluye el entorno de ejecución Python requerido por el adaptador de Antigravity; macOS utiliza la instalación existente de Python 3. Instala e inicia sesión en los clientes de IA por separado.

La aplicación no cuenta con firma digital de código del sistema ni notarización de Apple; el sistema operativo puede mostrar una advertencia de editor no identificado. La validación en Mac ARM64 se realiza mediante GitHub CI con entornos de prueba controlados.

## Guía de inicio rápido

1. Instala o extrae AgentRelay y ejecútalo. Las comprobaciones de inicio y la migración de datos se realizan antes de abrir la ventana principal.
2. Abre el cliente de IA que desees utilizar o inícialo desde la pantalla de bienvenida. Puedes omitir la verificación o esperar 10 segundos para continuar.
3. En la página de inicio, copia la dirección del servicio local y el token de acceso en tu herramienta compatible. La dirección predeterminada es `http://127.0.0.1:8787`.
4. Selecciona un motor y conversación en la pestaña Test para verificar la conexión.

La aplicación de escritorio se abre sin solicitar registro ni formulario de token. Al cerrar la ventana principal, esta solo se oculta mientras el servicio y las tareas en segundo plano siguen activas. Haz clic en el icono de la bandeja para restaurar la interfaz. Usa la opción **Salir** para finalizar las tareas pendientes y cerrar la aplicación por completo.

## Una conversación, múltiples tareas

Accede a **Sessions** en la barra lateral para buscar títulos, identificadores originales y espacios de trabajo. Filtra por cliente de IA y rangos de fechas, selecciona varias conversaciones y utiliza las acciones disponibles según el cliente:

| Cliente | Gestión de conversaciones |
| --- | --- |
| Codex | Archivar, restaurar y eliminar |
| Antigravity IDE | Eliminar la conversación y su resumen |
| Grok Bot | Explorar bots existentes y copiar identificadores disponibles |

![Gestión de conversaciones de AgentRelay con datos de demostración](https://goodexts.com/images/agentrelay/en/sessions.png)

La ventana de sesiones se abre con mayor amplitud adaptándose al espacio disponible en pantalla. El tema visual y el idioma se sincronizan con la ventana principal.

| Acción | Grok Bot | Codex | Antigravity |
| --- | --- | --- | --- |
| Comprobar estado | `GET /grokbot/health` | `GET /codex/health` | `GET /antigravity/health` |
| Listar conversaciones | `GET /grokbot/sessions` | `GET /codex/sessions` | `GET /antigravity/sessions` |
| Crear conversación | Solo bots existentes | `POST /codex/sessions` | `POST /antigravity/sessions` |
| Enviar tarea | `POST /grokbot/jobs` | `POST /codex/jobs` | `POST /antigravity/jobs` |
| Consultar tarea | `GET /grokbot/jobs/{jobId}` | `GET /codex/jobs/{jobId}` | `GET /antigravity/jobs/{jobId}` |

Las peticiones de API requieren el encabezado `Authorization: Bearer <token>`. Los cuerpos JSON utilizan `Content-Type: application/json`.

```json
{"jobId":"id-unico-generado-por-cliente","sessionId":"id-de-conversacion-seleccionada","text":"Contenido del mensaje","timeoutMs":120000}
```

Realiza sondeo (polling) consultando por el `jobId` original. Cuando el estado sea `completed`, lee el campo `result.text`. Reenviar el mismo identificador con el mismo contenido devuelve la tarea original; un contenido distinto genera un error 409. Las tareas en una misma conversación se ejecutan en serie. Un estado de entrega `unknown` no autoriza el reenvío inmediato con un nuevo ID.

Las rutas heredadas de Grok Bot, como `/ask`, siguen siendo compatibles. Codex y Antigravity reutilizan la conversación elegida en peticiones continuas. Las rutas ACP son integraciones experimentales configuradas de forma independiente a las rutas de clientes nativos por defecto.

Para ver un ejemplo práctico con extensiones de navegador, consulta el [Tutorial de XFollower con AgentRelay](https://goodexts.com/blog/xfollower-agentrelay) y descubre más utilidades en la [Página principal de GoodExts](https://goodexts.com/).

## Datos y Actualizaciones Automáticas

En Windows, los datos residen en `%APPDATA%\AgentRelay`; en macOS, en `~/Library/Application Support/AgentRelay`. Las instalaciones y actualizaciones conservan configuraciones, tokens, historial de tareas y mapeos de sesión. Las rutas de datos anteriores se migran antes de iniciar la interfaz.

La versión 1.x utilizaba un actualizador de archivo único distinto. Instala o descomprime la versión 2.x manualmente la primera vez; las actualizaciones posteriores se descargan automáticamente desde este repositorio.

Comprueba actualizaciones en **Config → Software updates**. AgentRelay valida el manifiesto firmado y el hash SHA-256 del archivo de la plataforma, espera la finalización de tareas activas, realiza copia de seguridad de la aplicación y datos de negocio y efectúa el reemplazo. En caso de fallo durante el arranque, se restaura automáticamente la versión anterior.
