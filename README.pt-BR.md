# AgentRelay

[English](README.md) · [简体中文](README.zh-CN.md) · [Português (Brasil)](README.pt-BR.md) · [Español](README.es.md)

Um serviço local de IA para extensões de navegador, páginas web e fluxos de trabalho no desktop. Conecte clientes compatíveis de Codex, Grok Bot, Antigravity, Grok CLI e DeepSeek Harness através de uma única interface HTTP unificada, escolha uma conversa existente e continue utilizando seu contexto entre tarefas, sem precisar reenviar todo o histórico a cada solicitação.

[GoodExts](https://goodexts.com/) · [Detalhes do produto](https://goodexts.com/agentrelay) · [Tutorial de integração com o XFollower](https://goodexts.com/blog/xfollower-agentrelay)

![Configurações de conexão do AgentRelay](https://goodexts.com/images/agentrelay/en/home.png)

Este repositório contém documentação do produto, skills de integração e arquivos de distribuição binária. O código-fonte da aplicação é mantido em repositório privado. Componentes de terceiros mantêm suas respectivas licenças.

## Skill de Integração para IA (AI Integration Skill)

Se você precisa conectar sua extensão de navegador, site ou ferramenta desktop ao AgentRelay utilizando assistentes de programação por IA (Codex, Antigravity, Claude Code, Cursor), utilize a nossa skill padrão de integração localizada diretamente na raiz deste repositório:

- 📖 **Documentação da Skill**: [`agentrelay-integration/SKILL.md`](agentrelay-integration/SKILL.md)
- 📦 **Download do pacote completo**: [`agentrelay-integration.zip`](agentrelay-integration.zip)
- 🔌 **Componentes inclusos**:
  - `agentrelay-integration/scripts/client.mjs` (Cliente ES module nativo, pronto para uso em service workers MV3 do Chrome)
  - `agentrelay-integration/scripts/probe.mjs` (Ferramenta de diagnóstico CLI e verificação completa)
  - `agentrelay-integration/scripts/self-test.mjs` (Testes locais com mock sem realizar chamadas reais de IA)

Coloque a pasta `agentrelay-integration/` em seu projeto ou em `~/.codex/skills/` e envie a instrução ao seu assistente de IA:
> *"Leia `agentrelay-integration/SKILL.md` e integre este projeto ao AgentRelay seguindo as regras de branches..."*

## Novidades na versão 2.8.3

- **Motores Nativos Grok CLI e DeepSeek Harness (DSH)**: Descoberta completa de sessões persistentes em terminal e workspaces locais, decodificação de fluxos comprimidos multi-frame e encaminhamento bidirecional de comandos entre 5 clientes locais de IA (Codex, Grok Bot, Antigravity, Grok CLI, DeepSeek Harness).
- **Grok Bot atualizado para 0.12.5 (PR166) e Recuperação de Leitura**: Atualização do binário upstream eliminando o erro de bloqueio 401 "This version of Grok Bot is no longer supported", além de fila de recuperação de leitura prevenindo timeouts intermitentes 502 Bad Gateway.
- **Compactação Nativa de Sessão e Isolamento de Filas por Proprietário**: Suporte dedicado à compactação nativa e isolamento de filas de execução para tarefas contínuas de alta frequência (marcação e comentários).
- **Proteção do Instalador Windows e Integridade de Processos**: Novo handshake de encerramento seguro durante instalação e atualização local, resolvendo alertas falsos de processos em uso e conflitos de integridade Low Integrity.

## Download 2.8.3

| Plataforma | Instalador | Arquivo portátil |
| --- | --- | --- |
| Windows 10/11 x64 | [Instalador Windows](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-win-x64.exe) | [Windows ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-win-x64.zip) |
| macOS Apple Silicon | [macOS DMG](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-mac-arm64.dmg) | [macOS ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-mac-arm64.zip) |

[Todas as versões e hashes SHA-256](https://github.com/wangsen2020/agentrelay-releases/releases)

Mantenha a pasta portátil do Windows ou o pacote da aplicação macOS completo. O Windows inclui o runtime Python necessário para o adaptador do Antigravity; o macOS atualmente utiliza o Python 3 já instalado no sistema. Instale e faça login nos clientes de IA separadamente.

O aplicativo não possui assinatura de código OS ou autenticação da Apple (notarization); o sistema pode exibir um aviso de desenvolvedor não identificado. A validação para Mac ARM64 é executada no GitHub CI com fixtures controladas.

## Como começar a usar

1. Instale ou extraia o AgentRelay e execute-o. As verificações iniciais e migração de dados ocorrem antes da janela principal abrir.
2. Inicie o cliente de IA desejado ou use a tela de inicialização para iniciá-lo. Você pode ignorar a verificação ou aguardar 10 segundos para avançar.
3. Na página inicial, copie o endereço do serviço local e o token de acesso para a ferramenta compatível. O endereço padrão é `http://127.0.0.1:8787`.
4. Escolha um motor e conversa na aba Test para verificar a conexão.

O aplicativo desktop abre sem exigir formulário de conta ou token. Fechar a janela principal apenas minimiza a interface enquanto o serviço de segundo plano continua ativo. Clique no ícone da bandeja para restaurar a janela. Utilize a opção explícita **Sair** para aguardar a finalização de tarefas pendentes e encerrar.

## Uma conversa, múltiplas tarefas

Abra a opção **Sessions** na barra lateral para pesquisar títulos, IDs originais e workspaces. Filtre por cliente de IA e intervalo de datas, selecione várias conversas e utilize as ações compatíveis de cada cliente:

| Cliente | Gerenciamento de conversas |
| --- | --- |
| Codex | Arquivar, restaurar e excluir |
| Antigravity IDE | Excluir conversa e resumo |
| Grok Bot | Navegar pelos bots existentes e copiar IDs disponíveis |

![Gerenciamento de conversas do AgentRelay com dados de demonstração](https://goodexts.com/images/agentrelay/en/sessions.png)

A janela de sessões é aberta de forma ampla, adaptando-se ao espaço útil da tela. O tema e o idioma acompanham a janela principal.

| Ação | Grok Bot | Codex | Antigravity |
| --- | --- | --- | --- |
| Verificar status | `GET /grokbot/health` | `GET /codex/health` | `GET /antigravity/health` |
| Listar conversas | `GET /grokbot/sessions` | `GET /codex/sessions` | `GET /antigravity/sessions` |
| Criar conversa | Somente bots existentes | `POST /codex/sessions` | `POST /antigravity/sessions` |
| Enviar tarefa | `POST /grokbot/jobs` | `POST /codex/jobs` | `POST /antigravity/jobs` |
| Consultar tarefa | `GET /grokbot/jobs/{jobId}` | `GET /codex/jobs/{jobId}` | `GET /antigravity/jobs/{jobId}` |

Chamadas de API exigem o cabeçalho `Authorization: Bearer <token>`. Requisições JSON utilizam `Content-Type: application/json`.

```json
{"jobId":"id-unico-gerado-pelo-cliente","sessionId":"id-da-conversa-selecionada","text":"Conteúdo da mensagem","timeoutMs":120000}
```

Realize polling consultando pelo `jobId` original. Quando o status estiver como `completed`, leia o campo `result.text`. Reenviar o mesmo ID com o mesmo conteúdo retorna a tarefa existente; dados diferentes retornam erro 409. As conversas são executadas em fila serial. Um estado de envio `unknown` não autoriza reenviar imediatamente com outro ID.

Rotas legadas do Grok Bot, como `/ask`, permanecem compatíveis. Codex e Antigravity reutilizam a conversa selecionada em mensagens subsequentes. As rotas ACP são integrações experimentais configuradas separadamente das rotas nativas padrão.

Para um fluxo prático em extensões de navegador, consulte o [Tutorial do XFollower com AgentRelay](https://goodexts.com/blog/xfollower-agentrelay) e explore mais utilitários na [Página inicial do GoodExts](https://goodexts.com/).

## Dados e Atualizações Automáticas

No Windows, os dados ficam em `%APPDATA%\AgentRelay`; no macOS, em `~/Library/Application Support/AgentRelay`. Instalações e atualizações preservam configurações, tokens, histórico de tarefas e mapeamentos de sessão. Caminhos legados de dados são migrados antes da abertura da interface.

A versão 1.x utilizava um atualizador em arquivo único diferente. Instale ou extraia a versão 2.x manualmente uma vez; atualizações posteriores usam este repositório de lançamentos.

Verifique atualizações em **Config → Software updates**. O AgentRelay valida o manifesto assinado e o hash SHA-256 do arquivo da plataforma, aguarda a conclusão de tarefas ativas, faz backup da aplicação e dos dados locais e executa a atualização. Caso haja falha na inicialização, a versão anterior é restaurada automaticamente.
