# Servicos de API e Hooks

> Documentacao dos servicos de API, hooks, tipos e utilitarios do LudicoM Frontend.

## Autenticacao

Todas as requisicoes usam **Basic Auth**. As credenciais sao lidas via variaveis de ambiente:

| Variavel | Descricao | Padrao |
|---|---|---|
| `REACT_APP_AUTH_USERNAME` | Usuario para autenticacao | — |
| `REACT_APP_AUTH_PASSWORD` | Senha para autenticacao | — |
| `REACT_APP_API_BASE_URL` | URL base da API | `http://localhost:8080/api` |

O cabecalho `Authorization` e construido como `Basic base64(username:password)` pela funcao `getAuthHeaders()` em `authService.ts`. Quando as credenciais estao ausentes, um aviso e exibido em desenvolvimento e a requisicao segue sem cabecalho de autenticacao.

```typescript
// Exemplo do cabecalho gerado
{
  'Authorization': 'Basic YWRtaW46YWRtaW4xMjM=',
  'Content-Type': 'application/json'
}
```

## Endpoints por Entidade

Todas as entidades seguem o mesmo padrao CRUD com suporte a paginacao. A URL base e definida por `API_BASE_URL` (`/api`). Os endpoints devem ser concatenados com a base.

### Jogos (`/api/jogo`)

| Funcao | Metodo | Endpoint | Descricao |
|---|---|---|---|
| `fetchJogos(signal?)` | GET | `/jogo` | Lista todos os jogos |
| `fetchJogosPaginated(page, size, search?, signal?)` | GET | `/jogo?paginated=true` | Lista paginada com busca |
| `createJogo(payload)` | POST | `/jogo` | Cria um novo jogo |
| `updateJogo(id, payload)` | PUT | `/jogo/{id}` | Atualiza um jogo existente |
| `deleteJogo(id)` | DELETE | `/jogo/{id}` | Remove um jogo |

**Normalizacao**: `uid` do backend mapeado para `id` como string (UUID). Campos numericos (`anoPublicacao`, `tempoDeJogo`, `minimoJogadores`, `maximoJogadores`) convertidos com coerção segura.

### Participantes (`/api/participante`)

| Funcao | Metodo | Endpoint | Descricao |
|---|---|---|---|
| `fetchParticipantes(signal?)` | GET | `/participante` | Lista todos os participantes |
| `fetchParticipantesPaginated(page, size, search?, signal?)` | GET | `/participante?paginated=true` | Lista paginada com busca |
| `createParticipante(payload)` | POST | `/participante` | Cria um novo participante |
| `updateParticipante(id, changes)` | PUT | `/participante/{id}` | Atualiza um participante |
| `deleteParticipante(id)` | DELETE | `/participante/{id}` | Remove um participante |

**Normalizacao**: `uid` para `id`, `instituicao` objeto normalizado com shape padrao (`uid`, `nome`, `endereco`), `idInstituicao` extraido do objeto quando presente.

### Instituicoes (`/api/instituicao`)

| Funcao | Metodo | Endpoint | Descricao |
|---|---|---|---|
| `fetchInstituicoes(signal?)` | GET | `/instituicao` | Lista todas as instituicoes |
| `fetchInstituicoesPaginated(page, size, search?, signal?)` | GET | `/instituicao?paginated=true` | Lista paginada com busca |
| `createInstituicao(payload)` | POST | `/instituicao` | Cria uma nova instituicao |
| `updateInstituicao(id, changes)` | PUT | `/instituicao/{id}` | Atualiza uma instituicao |
| `deleteInstituicao(id)` | DELETE | `/instituicao/{id}` | Remove uma instituicao |

### Eventos (`/api/evento`)

| Funcao | Metodo | Endpoint | Descricao |
|---|---|---|---|
| `fetchEventos(signal?)` | GET | `/evento` | Lista todos os eventos |
| `fetchEventosPaginated(page, size, search?, signal?)` | GET | `/evento?paginated=true` | Lista paginada com busca |
| `createEvento(payload)` | POST | `/evento` | Cria um novo evento |
| `updateEvento(id, changes)` | PUT | `/evento/{id}` | Atualiza um evento |
| `deleteEvento(id)` | DELETE | `/evento/{id}` | Remove um evento |

**Normalizacao**: `uid` para `id`, horarios (`horaInicio`, `horaFim`) truncados para `HH:mm` (5 caracteres).

### Emprestimos (`/api/emprestimo`)

| Funcao | Metodo | Endpoint | Descricao |
|---|---|---|---|
| `fetchEmprestimos(signal?)` | GET | `/emprestimo` | Lista todos os emprestimos |
| `fetchEmprestimosPaginated(page, size, isDevolvido?, search?, signal?)` | GET | `/emprestimo?paginated=true` | Lista paginada com filtro por status |
| `createEmprestimo(payload)` | POST | `/emprestimo` | Registra um novo emprestimo |
| `updateEmprestimo(id, changes)` | PUT | `/emprestimo/{id}` | Atualiza um emprestimo |
| `deleteEmprestimo(id)` | DELETE | `/emprestimo/{id}` | Remove um emprestimo |
| `devolverEmprestimo(codigoDeBarras)` | POST | `/emprestimo/devolver` | Devolucao por codigo de barras |

O endpoint `POST /emprestimo/devolver` recebe `{ codigoDeBarras: string }` e retorna o `Emprestimo` atualizado ou `null` se nao encontrado.

**Normalizacao**: `uid` para `id`, nomes de jogo/participante resolvidos a partir de objetos aninhados, horarios truncados para `HH:mm`.

## Interfaces TypeScript

Arquivo: `src/shared/types/index.ts`

```typescript
export interface Instituicao {
  uid: string;
  nome: string;
  endereco: string;
}

export interface Emprestimo {
  id: string;
  idJogo: string;
  idParticipante: string;
  idEvento: string;
  horaEmprestimo: string;
  horaDevolucao: string | null;
  isDevolvido: boolean;
  observacoes?: string;
  // Campos computados para exibicao (nao persistidos)
  jogo?: string;
  participante?: string;
  horario?: string;
}

export interface Evento {
  id: string;
  data: string;
  idInstituicao?: string;
  instituicao: Instituicao;
  horaInicio: string;
  horaFim: string;
}

export interface Jogo {
  id: string;
  nome: string;
  nomeAlternativo: string;
  anoPublicacao: number;
  tempoDeJogo: number;
  minimoJogadores: number;
  maximoJogadores: number;
  codigoDeBarras: string;
  isDisponivel: boolean;
  criadoQuando: string;
  atualizadoQuando: string;
}

export interface Participante {
  id: string;
  nome: string;
  email: string;
  documento: string;
  ra: string;
  idInstituicao?: string;
  instituicao?: Instituicao;
}

export interface PageResponse<T> {
  content: T[];
  totalPages: number;
  totalElements: number;
  number: number;
  size: number;
  first: boolean;
  last: boolean;
  empty: boolean;
}

export interface TableColumn<T> {
  key: keyof T;
  label: string;
  render?: (value: any, item: T) => React.ReactNode;
}

export interface TableAction<T> {
  label: string;
  onClick: (item: T) => void;
  variant?: 'primary' | 'secondary' | 'danger';
}

export interface RouteConfig {
  path: string;
  name: string;
  component: React.ComponentType;
}

export type RouteKey = 'HOME' | 'EVENTOS' | 'JOGOS' | 'PARTICIPANTES' | 'EMPRESTIMOS' | 'LOGIN';

export interface UIConstants {
  HEADER_HEIGHT: number;
  SCROLL_THRESHOLD: number;
  SEARCH_DEBOUNCE_DELAY: number;
}

export interface AppMessages {
  LOGIN_PLACEHOLDER: string;
  CRIAR_EVENTO: string;
  ADICIONAR_PARTICIPANTE: string;
  CONSULTAR_JOGO: string;
  REGISTRAR_EMPRESTIMO: string;
  CONFIRM_RETURN: string;
  SEARCH_PLACEHOLDER: string;
}

export interface SearchIconProps {
  width?: number;
  height?: number;
  stroke?: string;
  strokeWidth?: number;
}

export interface ComingSoonProps {
  pageName: string;
}

export interface PageHeaderProps {
  title: string;
  buttonText?: string;
  onButtonClick?: () => void;
  showButton?: boolean;
}
```

### Notas sobre as Interfaces

- **Instituicao** usa `uid` (UUID) como identificador, diferente das demais entidades que usam `id`.
- **Emprestimo** possui campos computados (`jogo`, `participante`, `horario`) preenchidos pelo servico de normalizacao, nao persisti dos no backend.
- **Jogo** contem timestamps `criadoQuando` e `atualizadoQuando` retornados pelo backend.
- **PageResponse** e generica e usada por todas as entidades com paginacao.

### Campos de Configuracao

Arquivos em `src/shared/constants/` que definem a configuracao de campos para modais de detalhe, edicao e criacao:

- **detailFields.ts** — `jogoDetailFields`, `participanteDetailFields`, `eventoDetailFields`, `emprestimoDetailFields`, `instituicaoDetailFields`
- **editFields.ts** — `jogoEditFields`, `participanteEditFields`, `eventoEditFields`, `emprestimoEditFields`, `instituicaoEditFields`
- **createFields.ts** — `jogoCreateFields`, `participanteCreateFields`, `eventoCreateFields`, `emprestimoCreateFields`, `instituicaoCreateFields`

Cada configuracao define os campos exibidos no modal correspondente, com tipo, label e ordem de exibicao.

## Hooks de Dados

Arquivo: `src/shared/hooks/useEntityData.ts`

### useParticipantes

Carrega todos os participantes via `fetchParticipantes()`.

```typescript
{
  participantes: Participante[],
  loading: boolean,
  error: string | null,
  createParticipante: (payload: Partial<Participante>) => Promise<Participante>,
  updateParticipante: (id: string, changes: Partial<Participante>) => Promise<Participante>,
  deleteParticipante: (id: string) => Promise<void>
}
```

### useJogos

Carrega jogos com **cache no sessionStorage por 5 minutos** (cache-first: exibe cache imediato, atualiza em background se stale).

```typescript
{
  jogos: Jogo[],
  loading: boolean,
  error: string | null,
  createJogo: (payload: Partial<Jogo>) => Promise<Jogo>,
  updateJogo: (id: string, changes: Partial<Jogo>) => Promise<Jogo>,
  deleteJogo: (id: string) => Promise<void>,
  setDisponibilidadeLocal: (id: string, disponivel: boolean) => void,
  refetchJogos: () => Promise<void>
}
```

`setDisponibilidadeLocal` atualiza `isDisponivel` no estado local e no cache sem chamar a API. `createJogo` possui fallback local (cria com id gerado) se a requisicao falhar.

### useEventos

Carrega eventos sem cache. Similar ao padrao basico.

```typescript
{
  eventos: Evento[],
  loading: boolean,
  error: string | null,
  createEvento: (payload: Partial<Evento>) => Promise<Evento>,
  updateEvento: (id: string, changes: Partial<Evento>) => Promise<Evento>,
  deleteEvento: (id: string) => Promise<void>,
  refetchEventos: () => Promise<void>
}
```

### useEmprestimos

Carrega emprestimos com **cache no sessionStorage por 2 minutos** (cache mais curto para dados que mudam com frequencia).

```typescript
{
  emprestimos: Emprestimo[],
  loading: boolean,
  error: string | null,
  refetchEmprestimos: () => Promise<void>
}
```

Nota: `useEmprestimos` nao expoe metodos create/update/delete. Usa `fetchEmprestimos` com dynamic `import()`.

### useInstituicoes

Carrega instituicoes sem cache.

```typescript
{
  instituicoes: Instituicao[],
  loading: boolean,
  error: string | null,
  createInstituicao: (payload: Partial<Instituicao>) => Promise<Instituicao>,
  updateInstituicao: (id: string, changes: Partial<Instituicao>) => Promise<Instituicao>,
  deleteInstituicao: (id: string) => Promise<void>
}
```

### useCrudOperations

Arquivo: `src/shared/hooks/useCrudOperations.ts`

Hook generico para gerenciamento de estado de modais. **Opera apenas em estado local, nao faz chamadas API.**

```typescript
// T extends { id: number | string }
{
  // Estados
  selectedItem: T | null,
  isModalOpen: boolean,
  isEditModalOpen: boolean,
  isCreateModalOpen: boolean,

  // Handlers basicos
  handleDetalhes: (item: T) => void,
  handleEditar: (item: T) => void,
  handleCriar: () => void,

  // Factories para handlers customizados
  createHandleExcluir: (items, setItems, getConfirmMessage) => (item: T) => void,
  createHandleSalvarEdicao: (items, setItems, updateItemData?) => (item: T) => void,
  createHandleSalvarCriacao: (items, setItems, generateId?, prepareNewItem?) => (item: T) => void,

  // Handlers para fechar modais
  closeDetailModal: () => void,
  closeEditModal: () => void,
  closeCreateModal: () => void,

  // Setters para casos especiais
  setSelectedItem,
  setIsModalOpen,
  setIsEditModalOpen,
  setIsCreateModalOpen
}
```

## Utilitarios

Arquivo: `src/shared/utils/index.ts`

| Funcao | Descricao |
|---|---|
| `escapeHtml(str: unknown): string` | Escapa caracteres HTML (`&`, `<`, `>`, `"`, `'`) para prevenir XSS |
| `filterByMultipleFields(items, termo, campos)` | Filtra array de objetos por termo de busca em multiplos campos |
| `debounce(func, wait)` | Retorna funcao com debounce padrao (ex: 300ms para busca) |
| `formatDateTime(date)` | Formata data para locale pt-BR com hora (ex: `01/06/2026 14:30`) |
| `formatTimeHHMM(input?)` | Extrai hora no formato `HH:mm` de um Date, string ISO ou timestamp |
| `ensureHHMMSS(time?)` | Converte `HH:mm` para `HH:mm:00` para envio ao backend |
| `isoToHHMM(iso?)` | Converte string ISO para `HH:mm`, retorna `null` se invalido |
| `generateId(prefix?)` | Gera UUIDv4 com prefixo opcional (ex: `jogo-xxxx-xxxx`) |
| `isNotNullish(value)` | Type guard que verifica se valor nao e null nem undefined |
| `handleError(error, context?)` | Loga erro no console com contexto (ex: `[jogosService.fetchJogos]: ...`) |
| `validateEntityData(rawData, entitySchema)` | Valida e converte dados brutos conforme schema, garantindo tipos corretos |
| `getDevolvidosLocal()` | Le do localStorage mapa de ids de emprestimos devolvidos localmente |
| `markEmprestimoDevolvidoLocal(id, hora?)` | Marca emprestimo como devolvido no localStorage |

### ENTITY_SCHEMAS

Schemas de validacao para cada entidade, usados por `validateEntityData`:

```typescript
{
  jogo:       { id, nome, nomeAlternativo, anoPublicacao, tempoDeJogo, minimoJogadores,
                maximoJogadores, codigoDeBarras, isDisponivel, criadoQuando, atualizadoQuando },
  instituicao:  { uid, nome, endereco },
  participante: { id, nome, email, documento, ra, idInstituicao, instituicao },
  evento:       { id, data, idInstituicao, instituicao, horaInicio, horaFim },
  emprestimo:   { id, idJogo, idParticipante, idEvento, horaEmprestimo,
                  horaDevolucao, isDevolvido, observacoes }
}
```

Cada campo e tipado como `'number' | 'string' | 'boolean' | 'date' | 'object'`.

## Validacoes

Arquivo: `src/shared/utils/validations.ts`

### VALIDATION_PATTERNS

| Padrao | Regex | Uso |
|---|---|---|
| `TIME` | `/^([0-1]?[0-9]\|2[0-3]):[0-5][0-9]$/` | Horario `HH:mm` (formato 24h) |
| `EMAIL` | `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` | Formato basico de email |
| `DOCUMENT` | `/^\d{3}\.\d{3}\.\d{3}-\d{2}$/` | CPF no formato `000.000.000-00` |
| `RA` | `/^[0-9]{7,12}$/` | RA (7 a 12 digitos) |

### ValidationUtils

| Metodo | Descricao |
|---|---|
| `isValidEmail(email)` | Testa email contra `VALIDATION_PATTERNS.EMAIL` |
| `isValidTime(time)` | Testa horario contra `VALIDATION_PATTERNS.TIME` |
| `isValidDocument(document)` | Testa CPF contra `VALIDATION_PATTERNS.DOCUMENT` |
| `isValidRA(ra)` | Testa RA contra `VALIDATION_PATTERNS.RA` |
| `isValidYear(year)` | Verifica se ano esta entre 1800 e o ano atual |

### VALIDATION_MESSAGES

Mensagens de erro centralizadas para formularios, incluindo validacoes de nome, email, horario, documento, RA, jogo, instituicao e data.

## Constantes e Configuracoes

Arquivo: `src/shared/constants/index.ts`

### ROUTES

Objeto com os paths das 7 rotas da aplicacao, usado em `App.tsx` e `Header`:

```typescript
export const ROUTES = {
  HOME: "/",
  INSTITUICOES: "/instituicoes",
  EVENTOS: "/eventos",
  JOGOS: "/jogos",
  PARTICIPANTES: "/participantes",
  EMPRESTIMOS: "/emprestimos",
  LOGIN: "/login",
} as const;

// Tipos derivados
type RouteValues = typeof ROUTES[keyof typeof ROUTES];
type RouteKeys = keyof typeof ROUTES;
```

### UI_CONSTANTS

Constantes de interface do usuario:

```typescript
export const UI_CONSTANTS: UIConstants = {
  HEADER_HEIGHT: 64,
  SCROLL_THRESHOLD: 64,
  SEARCH_DEBOUNCE_DELAY: 300,
};
```

### MESSAGES

Mensagens da aplicacao, exibidas em componentes de placeholder e confirmacao:

```typescript
export const MESSAGES: AppMessages = {
  LOGIN_PLACEHOLDER: "Funcionalidade de login sera implementada",
  CRIAR_EVENTO: "Funcionalidade de Criar Evento sera implementada",
  ADICIONAR_PARTICIPANTE: "Funcionalidade de Adicionar Participante sera implementada",
  CONSULTAR_JOGO: "Funcionalidade de Consultar Jogo sera implementada",
  REGISTRAR_EMPRESTIMO: "Funcionalidade de Registrar Emprestimo sera implementada",
  CONFIRM_RETURN: "Confirmar devolucao?",
  SEARCH_PLACEHOLDER: "Buscar por jogo ou participante...",
};
```

### API_BASE_URL

URL base da API, lida da variavel de ambiente `REACT_APP_API_BASE_URL` com fallback para `http://localhost:8080/api`. Um aviso e exibido no console em desenvolvimento quando a variavel nao esta definida.

```typescript
export const API_BASE_URL: string =
  (process.env.REACT_APP_API_BASE_URL as string) || 'http://localhost:8080/api';
```

### Re-exportacoes

O arquivo `src/shared/constants/index.ts` tambem re-exporta as configuracoes de campos e colunas de tabelas atraves dos modulos `detailFields`, `editFields`, `createFields` e `tableColumns`.

## Dados Mockados

Tres arquivos JSON em `src/shared/data/` usados durante o desenvolvimento:

| Arquivo | Quantidade | Finalidade |
|---|---|---|
| `jogos.json` | 10 registros | Jogos para teste de CRUD |
| `participantes.json` | 10 registros | Participantes para teste de CRUD |
| `emprestimos.json` | 10 registros | Emprestimos para teste de fluxo |

Cada registro segue a estrutura das interfaces TypeScript correspondentes.
