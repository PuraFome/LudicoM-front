# Funcionalidades do LudicoM Frontend

## Sumário

- [Home](#home)
- [Jogos](#jogos)
- [Participantes](#participantes)
- [Instituições](#instituições)
- [Eventos](#eventos)
- [Empréstimos](#empréstimos)
- [Login](#login)
- [Padrões Comuns](#padrões-comuns)
- [Matriz CRUD Completa](#matriz-crud-completa)
- [Interface PageResponse&lt;T&gt;](#interface-pageresponset)

---

## Home

Dashboard principal (página inicial do sistema).

### O que renderiza

- **WelcomeSection**: Saudação ao usuário.
- **QuickActions**: Três botões de acesso rápido:
  - Adicionar Participante
  - Consultar Jogo
  - Registrar Empréstimo
- **Tabela "Empréstimos Ativos"**: Lista de empréstimos filtrada pelo evento ativo, mostrando apenas os não devolvidos.
- **Banner de evento ativo**: Exibido quando há um evento em andamento, com contagem regressiva se faltar 30 minutos ou menos para o término.

### Operações CRUD

| Operação | Presente | Observação |
|---|---|---|
| Criar | ✅ | Atalho para registro de empréstimo |
| Listar | ✅ | Lista empréstimos ativos do evento corrente |
| Detalhes | ❌ | |
| Editar | ✅ | Ação de devolução via botão |
| Excluir | ✅ | Exclusão de empréstimo ativo |

### Modais utilizados

1. **CreateModal**: Registro de empréstimo (via QuickActions).
2. **ConsultModal**: Consulta de jogos (via QuickActions).
3. **ConfirmModal (devolução)**: Confirmação antes de marcar devolução.
4. **ConfirmModal (exclusão)**: Confirmação antes de excluir empréstimo ativo.

---

## Jogos

CRUD completo do catálogo de jogos.

### O que renderiza

- Lista paginada com busca por nome, nome alternativo ou código de barras.
- Cada linha exibe nome, disponibilidade e ações.

### Campos

| Campo | Tipo | Validação |
|---|---|---|
| nome | Texto | Mínimo 2 caracteres |
| nomeAlternativo | Texto | Opcional |
| anoPublicacao | Número | Entre 1800 e ano atual |
| tempoDeJogo | Número | Entre 1 e 1440 minutos |
| minimoJogadores | Número | Entre 1 e 50 |
| maximoJogadores | Número | Entre 1 e 100 |
| codigoDeBarras | Texto | Mínimo 8 caracteres |
| isDisponivel | Booleano | Indicador de disponibilidade |

### Operações CRUD

| Operação | Presente | Observação |
|---|---|---|
| Criar | ✅ | Modal de criação |
| Listar | ✅ | Paginação server-side |
| Detalhes | ✅ | Modal com 8 campos + timestamps |
| Editar | ✅ | Modal de edição |
| Excluir | ✅ | Modal de confirmação |

### Modais utilizados

1. **Create**: Formulário com todos os campos do jogo.
2. **Detail**: Exibe todos os campos incluindo timestamps de criação e atualização.
3. **Edit**: Mesmo formulário do Create, pré-preenchido.
4. **Confirm**: Confirmação antes de excluir.

---

## Participantes

CRUD com criação inline de instituição.

### O que renderiza

- Lista paginada com busca por nome, email ou documento.
- Cada linha exibe nome, email, documento e ações.

### Campos

| Campo | Tipo | Validação |
|---|---|---|
| nome | Texto | Obrigatório |
| email | Texto | Validação via regex |
| documento | Texto (CPF) | Obrigatório |
| ra | Texto | Opcional (obrigatório apenas se instituição selecionada) |
| instituicao | Autocomplete | Seleção de instituição existente |

### Validações especiais

- Unicidade de email, documento e RA (erro 409 retorna toast de aviso).

### Criação inline de instituição

- Botão "+" ao lado do campo instituição abre um sub-modal "Criar Instituição".
- Após criar a instituição, o campo é preenchido automaticamente no formulário principal.

### Operações CRUD

| Operação | Presente | Observação |
|---|---|---|
| Criar | ✅ | Modal com criação inline de instituição |
| Listar | ✅ | Paginação server-side |
| Detalhes | ✅ | Modal com dados completos |
| Editar | ✅ | Modal de edição |
| Excluir | ✅ | Modal de confirmação |

### Modais utilizados

1. **Create (principal)**: Formulário do participante.
2. **Create (instituição)**: Sub-modal para criar instituição inline.
3. **Detail**: Exibe dados completos do participante.
4. **Edit**: Formulário pré-preenchido.
5. **Confirm**: Confirmação antes de excluir.

---

## Instituições

CRUD com bloqueio de exclusão se vinculada a eventos.

### O que renderiza

- Lista paginada com busca por nome.
- Cada linha exibe nome, endereço e ações.

### Campos

| Campo | Tipo | Validação |
|---|---|---|
| nome | Texto | Máximo 200 caracteres |
| endereco | Texto | Máximo 255 caracteres |

### Validações especiais

- Unicidade de nome (erro 409 retorna toast de aviso).
- Exclusão bloqueada se a instituição estiver vinculada a eventos. O backend retorna erro informativo.

### Operações CRUD

| Operação | Presente | Observação |
|---|---|---|
| Criar | ✅ | Modal de criação |
| Listar | ✅ | Paginação server-side |
| Detalhes | ✅ | Modal com dados completos |
| Editar | ✅ | Modal de edição |
| Excluir | ✅ | Com verificação de vínculo com eventos |

### Modais utilizados

1. **Create**: Formulário com campos nome e endereço.
2. **Detail**: Exibe dados completos da instituição.
3. **Edit**: Formulário pré-preenchido.
4. **Confirm**: Confirmação antes de excluir.

---

## Eventos

CRUD com criação inline de instituição.

### O que renderiza

- Lista paginada com busca por data ou instituição.
- Cada linha exibe data, instituição, horários e ações.

### Campos

| Campo | Tipo | Valor padrão |
|---|---|---|
| data | Data (dd/mm/yyyy) | Hoje |
| instituicao | Autocomplete | — |
| horaInicio | Hora (HH:mm) | 14:00 |
| horaFim | Hora (HH:mm) | 18:00 |

### Criação inline de instituição

- Botão "+" ao lado do campo instituição abre um sub-modal "Criar Instituição".
- Após criar a instituição, o campo é preenchido automaticamente no formulário principal.

### Operações CRUD

| Operação | Presente | Observação |
|---|---|---|
| Criar | ✅ | Modal com criação inline de instituição |
| Listar | ✅ | Paginação server-side |
| Detalhes | ✅ | Modal com dados completos |
| Editar | ✅ | Modal de edição |
| Excluir | ✅ | Modal de confirmação |

### Modais utilizados

1. **Create (principal)**: Formulário do evento com data formatada dd/mm/yyyy e horários HH:mm.
2. **Create (instituição)**: Sub-modal para criar instituição inline.
3. **Detail**: Exibe dados completos do evento.
4. **Edit**: Formulário pré-preenchido.
5. **Confirm**: Confirmação antes de excluir.

---

## Empréstimos

Página mais complexa do sistema (467 linhas). Gerencia o ciclo completo de empréstimos de jogos.

### O que renderiza

Duas abas de listagem paginada:

1. **Empréstimos Ativos** (`isDevolvido = false`)
   - Ações disponíveis: Devolver e Excluir.
2. **Histórico** (`isDevolvido = true`)
   - Ações disponíveis: Detalhes, Editar e Excluir.

### Funcionalidades especiais

- **Devolução por código de barras**: É possível registrar a devolução escaneando o código de barras do jogo via `POST /emprestimo/devolver`.
- **Criação inline de participante**: Botão "+" ao lado do campo participante no modal de criação, similar ao padrão de instituição nas outras páginas.
- **Auto-vinculação ao evento ativo**: O empréstimo é automaticamente associado ao evento em andamento.

### Operações CRUD

| Operação | Presente | Observação |
|---|---|---|
| Criar | ✅ | Modal com criação inline de participante |
| Listar | ✅ | Paginação server-side com duas abas (ativos/histórico) |
| Detalhes | ✅ | Disponível na aba Histórico |
| Editar | ✅ | Disponível na aba Histórico |
| Excluir | ✅ | Disponível em ambas as abas |

### Modais utilizados

1. **Create (principal)**: Formulário de empréstimo com autocomplete para jogo e participante.
2. **Create (participante)**: Sub-modal para criar participante inline.
3. **Detail**: Exibe dados completos do empréstimo (apenas no Histórico).
4. **Edit**: Formulário pré-preenchido (apenas no Histórico).
5. **Confirm (devolução)**: Confirmação antes de registrar devolução.
6. **Confirm (exclusão)**: Confirmação antes de excluir.

---

## Login

Página placeholder.

### O que renderiza

Apenas o componente `<ComingSoon pageName="Login" />`.

### Operações CRUD

Nenhuma. Página não implementada.

---

## Padrões Comuns

### Busca com debounce de 300ms

Todos os campos de busca utilizam debounce de 300ms para evitar disparar requisições a cada caractere digitado. A busca só é executada após o usuário parar de digitar por 300ms.

### Paginação server-side

Todas as listagens seguem o mesmo padrão de requisição:

```
GET /api/entidade?paginated=true&page=0&size=10&search=termo
```

Parâmetros:

| Parâmetro | Tipo | Descrição |
|---|---|---|
| paginated | boolean | Habilita resposta paginada |
| page | int | Número da página (0-based) |
| size | int | Itens por página |
| search | string | Termo de busca |

### Tratamento de erros

Três níveis de tratamento:

1. **Erro 409 (Conflito)**: Exibe toast de aviso (amarelo) com a mensagem do servidor. Usado para violação de unicidade (email, documento, RA, nome de instituição).
2. **Lista de erros**: Quando o backend retorna múltiplos erros, exibe via `showErrorList()` com marcadores (bullets).
3. **Erro genérico**: Para demais falhas, exibe toast de erro (vermelho) com mensagem padrão.

### Feedback visual

| Tipo | Cor | Função |
|---|---|---|
| Success | Verde | `showSuccess()` |
| Error | Vermelho | `showError()` |
| Warning | Amarelo | `showWarning()` |
| Info | Azul | `showInfo()` |

---

## Matriz CRUD Completa

| Página | Criar | Listar | Detalhes | Editar | Excluir | Criação Inline |
|---|---|---|---|---|---|---|
| Home | ✅ (empréstimo) | ✅ (ativos) | ❌ | ✅ (devolver) | ✅ | ❌ |
| Jogos | ✅ | ✅ paginada | ✅ | ✅ | ✅ | — |
| Participantes | ✅ | ✅ paginada | ✅ | ✅ | ✅ | ✅ Instituição |
| Instituições | ✅ | ✅ paginada | ✅ | ✅ | ✅ (com verif.) | — |
| Eventos | ✅ | ✅ paginada | ✅ | ✅ | ✅ | ✅ Instituição |
| Empréstimos | ✅ | ✅ paginada (2 abas) | ✅ (histórico) | ✅ (histórico) | ✅ (ambas) | ✅ Participante |

**Legenda:**

- ✅ (empréstimo): Atalho na Home para criar empréstimo.
- ✅ (devolver): Ação de devolução na Home (considerada edição do estado do empréstimo).
- ✅ (com verif.): Exclusão condicionada à verificação de vínculo com eventos.
- ✅ paginada: Listagem com paginação server-side.
- ✅ (2 abas): Listagem dividida em abas "Ativos" e "Histórico".
- ✅ (histórico): Operação disponível apenas na aba Histórico.
- ✅ (ambas): Operação disponível em ambas as abas.
- ❌: Operação não disponível.
- —: Operação não se aplica.

---

## Interface PageResponse&lt;T&gt;

Todas as respostas paginadas seguem esta estrutura:

```typescript
interface PageResponse<T> {
  content: T[];
  totalPages: number;
  totalElements: number;
  number: number;
  size: number;
  first: boolean;
  last: boolean;
  empty: boolean;
}
```

| Campo | Tipo | Descrição |
|---|---|---|
| content | T[] | Array com os itens da página atual |
| totalPages | number | Total de páginas disponíveis |
| totalElements | number | Total de registros no servidor |
| number | number | Número da página atual (0-based) |
| size | number | Tamanho da página (itens por página) |
| first | boolean | Indica se é a primeira página |
| last | boolean | Indica se é a última página |
| empty | boolean | Indica se a página está vazia |
