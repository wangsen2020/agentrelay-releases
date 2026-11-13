# AgentRelay

[English](README.md) · [简体中文](README.zh-CN.md) · [Português (Brasil)](README.pt-BR.md) · [Español](README.es.md)

**2.13.3 · Windows 10/11 x64 · Solo instalador EXE**

Conecta Codex, Grok Bot, Antigravity, Grok CLI, DeepSeek Harness y WorkBuddy locales. Codex, Antigravity, Harness, Grok CLI y Antigravity CLI admiten mensajes nativos en turnos activos (`POST /steer`); WorkBuddy se incorpora como sexto motor con reutilización de sesiones de escritorio, inicio de sesión aislado por QR oficial, ventana de chat `/ui/workbuddy`, barra rápida de copiado por mensaje y rutas públicas `/workbuddy/jobs`.

## Descarga y actualización

[EXE para Windows x64](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.13.3/AgentRelay-2.13.3-win-x64.exe) · [SHA-256](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.13.3/SHA256SUMS.txt) · [Notas de versión](https://github.com/wangsen2020/agentrelay-releases/releases/tag/v2.13.3)

Espera a que terminen las tareas, cierra la aplicación y ejecuta el EXE. La configuración y los registros se conservan en el AppData existente. Esta versión se instala manualmente; el actualizador anterior basado en ZIP no es su vía de actualización.

## Elige la skill

- [agentrelay-session-bridge](agentrelay-session-bridge/SKILL.md): asignación entre sesiones existentes, recibos directos y colaboración por eventos.

- [agentrelay-integration](agentrelay-integration/SKILL.md): integración de extensiones/clientes mediante jobs y recuperación de la tarea original.

- [agentrelay-local-rebuild](agentrelay-local-rebuild/SKILL.md): reconstrucción Windows para responsables con acceso autorizado al código privado.

## Instala la colaboración directa

Copia toda la carpeta `agentrelay-session-bridge` a `~/.codex/skills/` o al directorio de skills de tu herramienta. Conserva scripts, references y agents. Requiere Node.js 22+, AgentRelay 2.11.0+ local y clientes nativos con sus propias sesiones iniciadas.

Indica a la IA: “Usa $agentrelay-session-bridge para asignar esta tarea a la sesión existente elegida. Incluye la sesión actual del coordinador para devolver el resultado y envía DONE o BLOCKED de forma proactiva.”

Incluye el provider/sessionId real del coordinador actual. Steer estricto entra en un turno activo; los jobs ordinarios activan una sesión inactiva. Solo encola una notificación al mismo coordinador tras no_active_turn explícito y autorización del contrato. No reenvíes entregas inciertas.

## Verificación y límites

En Windows se verificaron las seis direcciones entre tres clientes, con nonces coincidentes y dos recibos DONE en el mismo turno activo de Codex. Aceptación, recepción y finalización son evidencias distintas.

La prueba no demuestra continuación automática de Codex inactivo, recuperación de fallos, ejecución ilimitada ni un porcentaje fijo de ahorro. El contexto no se comparte automáticamente; se mantienen las cuentas, cuotas y permisos de cada producto.

El código de la aplicación es privado. Las skills públicas contienen herramientas de API e instrucciones, sin credenciales ni código del servicio.

[Producto y guías](https://goodexts.com/es/agentrelay)
