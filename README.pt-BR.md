# AgentRelay

[English](README.md) · [简体中文](README.zh-CN.md) · [Português (Brasil)](README.pt-BR.md) · [Español](README.es.md)

**2.11.0 · Windows 10/11 x64 · Somente instalador EXE**

Conecte Codex, Grok Bot, Antigravity, Grok CLI e DeepSeek Harness locais. Codex, Antigravity e Harness aceitam mensagens nativas durante turnos ativos: o coordenador delega, cada agente executa e retorna resultados proativamente.

## Download e atualização

[EXE para Windows x64](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.11.0/AgentRelay-2.11.0-win-x64.exe) · [SHA-256](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.11.0/SHA256SUMS.txt) · [Notas da versão](https://github.com/wangsen2020/agentrelay-releases/releases/tag/v2.11.0)

Aguarde as tarefas terminarem, encerre o aplicativo e execute o EXE. A configuração e os registros permanecem no AppData existente. Esta versão requer instalação manual; o atualizador antigo baseado em ZIP não é o caminho de atualização.

## Escolha a skill

- [agentrelay-session-bridge](agentrelay-session-bridge/SKILL.md): delegação entre sessões existentes, recibos diretos e colaboração orientada a eventos.

- [agentrelay-integration](agentrelay-integration/SKILL.md): integração de extensões/clientes por jobs e recuperação da tarefa original.

- [agentrelay-local-rebuild](agentrelay-local-rebuild/SKILL.md): reconstrução Windows para mantenedores com acesso autorizado ao código privado.

## Instale a colaboração direta

Copie a pasta inteira `agentrelay-session-bridge` para `~/.codex/skills/` ou o diretório de skills da sua ferramenta. Preserve scripts, references e agents. Requer Node.js 22+, AgentRelay 2.11.0+ local e clientes nativos conectados às respectivas contas.

Peça à IA: “Use $agentrelay-session-bridge para delegar esta tarefa à sessão existente escolhida. Inclua a sessão atual do coordenador como endereço de retorno e envie DONE ou BLOCKED proativamente.”

Use o provider/sessionId real do coordenador atual. Steer estrito entrega no turno ativo; jobs comuns ativam uma sessão ociosa. Só enfileire uma notificação de resultado para o mesmo coordenador após no_active_turn explícito e autorização no contrato. Não reenvie entregas incertas.

## Verificação e limites

As seis direções entre três clientes foram verificadas no Windows com nonces correspondentes e dois recibos DONE no mesmo turno ativo do Codex. Aceitação, recebimento e conclusão são estados diferentes.

O teste não comprova continuação automática do Codex ocioso, recuperação de falhas, execução ilimitada ou um percentual fixo de economia. Contextos não são compartilhados automaticamente; contas, cotas e permissões continuam válidas.

O código do aplicativo permanece privado. As skills públicas contêm ferramentas de API e instruções, sem credenciais ou código do serviço.

[Produto e guias](https://goodexts.com/pt-br/agentrelay)
