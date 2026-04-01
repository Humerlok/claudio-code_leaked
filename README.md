# Snapshot do Código Fonte do Claude Code para Pesquisa de Segurança

> Este repositório reflete um **snapshot do código fonte do Claude Code exposto publicamente** que se tornou acessível em **31 de março de 2026** por meio de uma exposição de source map na distribuição npm. Ele é mantido para fins de **educação, pesquisa de segurança defensiva e análise da cadeia de suprimentos de software**.

---

## Contexto de Pesquisa

Este repositório é mantido por um **estudante universitário** que estuda:

- exposição da cadeia de suprimentos de software e vazamentos de artefatos de build
- práticas de engenharia de software segura
- arquitetura de ferramentas de desenvolvedor baseadas em agentes (agentics)
- análise defensiva de sistemas CLI do mundo real

Este arquivo destina-se a apoiar:

- estudo educacional
- prática de pesquisa de segurança
- revisão de arquitetura
- discussão de falhas nos processos de empacotamento e lançamento

Ele **não** reivindica a propriedade do código original e não deve ser interpretado como um repositório oficial da Anthropic.

---

## Como o Snapshot Público se Tornou Acessível

[Chaofan Shou (@Fried_rice)](https://x.com/Fried_rice) observou publicamente que o material de origem do Claude Code estava acessível através de um arquivo `.map` exposto no pacote npm:

> **"O código fonte do Claude code foi vazado através de um arquivo map em seu registro npm!"**
>
> — [@Fried_rice, 31 de março de 2026](https://x.com/Fried_rice/status/2038894956459290963)

O source map publicado referenciava fontes TypeScript não obfoscadas hospedadas no bucket R2 da Anthropic, o que tornou o snapshot da pasta `src/` publicamente disponível para download.

---

## Escopo do Repositório

O Claude Code é a CLI da Anthropic para interagir com o Claude a partir do terminal para realizar tarefas de engenharia de software, como editar arquivos, executar comandos, pesquisar bases de código e coordenar fluxos de trabalho.

Este repositório contém um espelhamento do snapshot da pasta `src/` para pesquisa e análise.

- **Exposição pública identificada em**: 31/03/2026
- **Linguagem**: TypeScript
- **Runtime**: Bun
- **Interface de Terminal (UI)**: React + [Ink](https://github.com/vadimdemedes/ink)
- **Escala**: ~1.900 arquivos, 512.000+ linhas de código

---

## Estrutura de Diretórios

```text
src/
├── main.tsx                 # Orquestração do ponto de entrada (CLI baseada em Commander.js)
├── commands.ts              # Registro de comandos
├── tools.ts                 # Registro de ferramentas
├── Tool.ts                  # Definições de tipos de ferramentas
├── QueryEngine.ts           # Mecanismo de consulta LLM
├── context.ts               # Coleta de contexto do sistema/usuário
├── cost-tracker.ts          # Rastreamento de custo de tokens
│
├── commands/                # Implementações de comandos slash (~50)
├── tools/                   # Implementações de ferramentas de agente (~40)
├── components/              # Componentes de interface Ink (~140)
├── hooks/                   # Hooks do React
├── services/                # Integrações de serviços externos
├── screens/                 # Interfaces de tela cheia (Doctor, REPL, Resume)
├── types/                   # Definições de tipos TypeScript
├── utils/                   # Funções utilitárias
│
├── bridge/                  # Ponte para IDE e controle remoto
├── coordinator/             # Coordenador de múltiplos agentes
├── plugins/                 # Sistema de plugins
├── skills/                  # Sistema de habilidades (skills)
├── keybindings/             # Configuração de atalhos de teclado
├── vim/                     # Modo Vim
├── voice/                   # Entrada de voz
├── remote/                  # Sessões remotas
├── server/                  # Modo servidor
├── memdir/                  # Diretório de memória persistente
├── tasks/                   # Gerenciamento de tarefas
├── state/                   # Gerenciamento de estado
├── migrations/              # Migrações de configuração
├── schemas/                 # Esquemas de configuração (Zod)
├── entrypoints/             # Lógica de inicialização
├── ink/                     # Wrapper do renderizador Ink
├── buddy/                   # Sprite de acompanhante
├── native-ts/               # Utilitários nativos de TypeScript
├── outputStyles/            # Estilização de saída
├── query/                   # Pipeline de consulta
└── upstreamproxy/           # Configuração de proxy
```

---

## Resumo da Arquitetura

### 1. Sistema de Ferramentas (`src/tools/`)

Cada ferramenta que o Claude Code pode invocar é implementada como um módulo independente. Cada ferramenta define seu esquema de entrada, modelo de permissão e lógica de execução.

| Ferramenta | Descrição |
|---|---|
| `BashTool` | Execução de comandos shell |
| `FileReadTool` | Leitura de arquivos (imagens, PDFs, notebooks) |
| `FileWriteTool` | Criação / sobrescrita de arquivos |
| `FileEditTool` | Modificação parcial de arquivos (substituição de strings) |
| `GlobTool` | Busca por padrões de arquivos |
| `GrepTool` | Busca de conteúdo baseada em ripgrep |
| `WebFetchTool` | Busca conteúdo de URLs |
| `WebSearchTool` | Pesquisa na web |
| `AgentTool` | Criação de sub-agentes |
| `SkillTool` | Execução de habilidades |
| `MCPTool` | Invocação de ferramentas de servidor MCP |
| `LSPTool` | Integração com Language Server Protocol |
| `NotebookEditTool` | Edição de notebooks Jupyter |
| `TaskCreateTool` / `TaskUpdateTool` | Criação e gerenciamento de tarefas |
| `SendMessageTool` | Mensagens entre agentes |
| `TeamCreateTool` / `TeamDeleteTool` | Gerenciamento de agentes de equipe |
| `EnterPlanModeTool` / `ExitPlanModeTool` | Alternar modo de planejamento |
| `EnterWorktreeTool` / `ExitWorktreeTool` | Isolamento em worktrees do Git |
| `ToolSearchTool` | Descoberta diferida de ferramentas |
| `CronCreateTool` | Criação de gatilhos agendados |
| `RemoteTriggerTool` | Gatilho remoto |
| `SleepTool` | Espera em modo proativo |
| `SyntheticOutputTool` | Geração de saída estruturada |

### 2. Sistema de Comandos (`src/commands/`)

Comandos slash voltados para o usuário, invocados com o prefixo `/`.

| Comando | Descrição |
|---|---|
| `/commit` | Cria um commit no git |
| `/review` | Revisão de código |
| `/compact` | Compressão de contexto |
| `/mcp` | Gerenciamento de servidor MCP |
| `/config` | Gerenciamento de configurações |
| `/doctor` | Diagnóstico do ambiente |
| `/login` / `/logout` | Autenticação |
| `/memory` | Gerenciamento de memória persistente |
| `/skills` | Gerenciamento de habilidades |
| `/tasks` | Gerenciamento de tarefas |
| `/vim` | Alternar modo Vim |
| `/diff` | Visualizar alterações |
| `/cost` | Verificar custo de uso |
| `/theme` | Alterar tema |
| `/context` | Visualização de contexto |
| `/pr_comments` | Visualizar comentários de PR |
| `/resume` | Restaurar sessão anterior |
| `/share` | Compartilhar sessão |
| `/desktop` | Transferência para o app de desktop |
| `/mobile` | Transferência para o app mobile |

### 3. Camada de Serviço (`src/services/`)

| Serviço | Descrição |
|---|---|
| `api/` | Cliente da API Anthropic, API de arquivos, bootstrap |
| `mcp/` | Conexão e gerenciamento de servidores Model Context Protocol |
| `oauth/` | Fluxo de autenticação OAuth 2.0 |
| `lsp/` | Gerenciador de Language Server Protocol |
| `analytics/` | Feature flags e análises baseadas no GrowthBook |
| `plugins/` | Carregador de plugins |
| `compact/` | Compressão de contexto de conversa |
| `policyLimits/` | Limites de política da organização |
| `remoteManagedSettings/` | Configurações gerenciadas remotamente |
| `extractMemories/` | Extração automática de memórias |
| `tokenEstimation.ts` | Estimativa de contagem de tokens |
| `teamMemorySync/` | Sincronização de memória de equipe |

### 4. Sistema de Ponte (`src/bridge/`)

Uma camada de comunicação bidirecional que conecta extensões de IDE (VS Code, JetBrains) com a CLI do Claude Code.

- `bridgeMain.ts` — Loop principal da ponte
- `bridgeMessaging.ts` — Protocolo de mensagens
- `bridgePermissionCallbacks.ts` — Callbacks de permissão
- `replBridge.ts` — Ponte de sessão REPL
- `jwtUtils.ts` — Autenticação baseada em JWT
- `sessionRunner.ts` — Gerenciamento de execução de sessão

### 5. Sistema de Permissões (`src/hooks/toolPermission/`)

Verifica permissões em cada invocação de ferramenta. Solicita a aprovação/negação do usuário ou resolve automaticamente com base no modo de permissão configurado (`default`, `plan`, `bypassPermissions`, `auto`, etc.).

### 6. Feature Flags (Sinalizadores de Funcionalidade)

Eliminação de código morto por meio de feature flags do `bun:bundle`:

```typescript
import { feature } from 'bun:bundle'

// Código inativo é completamente removido no momento da build
const voiceCommand = feature('VOICE_MODE')
  ? require('./commands/voice/index.js').default
  : null
```

Flags notáveis: `PROACTIVE`, `KAIROS`, `BRIDGE_MODE`, `DAEMON`, `VOICE_MODE`, `AGENT_TRIGGERS`, `MONITOR_TOOL`

---

## Arquivos Chave em Detalhes

### `QueryEngine.ts` (~46 mil linhas)

O motor principal para chamadas de API de LLM. Lida com respostas em streaming, loops de chamada de ferramentas, modo de pensamento (thinking mode), lógica de re-tentativa e contagem de tokens.

### `Tool.ts` (~29 mil linhas)

Define tipos base e interfaces para todas as ferramentas — esquemas de entrada, modelos de permissão e tipos de estado de progresso.

### `commands.ts` (~25 mil linhas)

Gerencia o registro e a execução de todos os comandos slash. Usa importações condicionais para carregar diferentes conjuntos de comandos por ambiente.

### `main.tsx`

Parser CLI baseado em Commander.js e inicialização do renderizador React/Ink. Na inicialização, ele sobrepõe configurações de MDM, pré-carregamento de keychain e inicialização do GrowthBook para um boot mais rápido.

---

## Pilha Tecnológica (Tech Stack)

| Categoria | Tecnologia |
|---|---|
| Runtime | [Bun](https://bun.sh) |
| Linguagem | TypeScript (strict) |
| Interface de Terminal | [React](https://react.dev) + [Ink](https://github.com/vadimdemedes/ink) |
| Parsing de CLI | [Commander.js](https://github.com/tj/commander.js) (extra-typings) |
| Validação de Esquema | [Zod v4](https://zod.dev) |
| Busca de Código | [ripgrep](https://github.com/BurntSushi/ripgrep) |
| Protocolos | [MCP SDK](https://modelcontextprotocol.io), LSP |
| API | [Anthropic SDK](https://docs.anthropic.com) |
| Telemetria | OpenTelemetry + gRPC |
| Feature Flags | GrowthBook |
| Autenticação | OAuth 2.0, JWT, macOS Keychain |

---

## Padrões de Design Notáveis

### Pré-carregamento Paralelo (Parallel Prefetch)

O tempo de inicialização é otimizado através do pré-carregamento paralelo de configurações de MDM, leitura de keychain e pré-conexão de API antes do início da avaliação pesada de módulos.

```typescript
// main.tsx — disparado como efeitos colaterais antes de outros imports
startMdmRawRead()
startKeychainPrefetch()
```

### Carregamento Lento (Lazy Loading)

Módulos pesados (OpenTelemetry, gRPC, analytics e alguns subsistemas protegidos por flags) são adiados via `import()` dinâmico até que sejam realmente necessários.

### Enxames de Agentes (Agent Swarms)

Sub-agentes são criados via `AgentTool`, com o `coordinator/` lidando com a orquestração multi-agente. `TeamCreateTool` permite o trabalho paralelo em nível de equipe.

### Sistema de Habilidades (Skill System)

Fluxos de trabalho reutilizáveis definidos em `skills/` são executados através da `SkillTool`. Os usuários podem adicionar habilidades personalizadas.

### Arquitetura de Plugins

Plugins nativos e de terceiros são carregados através do subsistema `plugins/`.

---

## Aviso de Pesquisa / Propriedade

- Este repositório é um **arquivo de pesquisa de segurança educacional e defensiva** mantido por um estudante universitário.
- Existe para estudar a exposição de fontes, falhas de empacotamento e a arquitetura de sistemas CLI baseados em agentes modernos.
- O código fonte original do Claude Code permanece propriedade da **Anthropic**.
- Este repositório **não é afiliado, endossado ou mantido pela Anthropic**.
