# Componentes do LudicoM Frontend

> Total: **21 componentes** em 6 categorias.

---

## Common (6)

Componentes compartilhados e reutilizáveis por toda a aplicação.

### AnimatedBackground

**Arquivo:** `src/components/common/AnimatedBackground.tsx`

Fundo animado com partículas flutuantes no tema do Ludico. Renderiza um canvas que desenha dados, cartas e peões usando `requestAnimationFrame`. As partículas se movem em velocidade constante, rotacionam e fazem wrap ao redor da tela.

- **Props:** Nenhuma (sem interface de props, usa refs internas)
- **Comportamento:** Canvas `position: fixed` com `z-index: -1` e `pointer-events: none`
- **Partículas:** 3 tipos (dice, card, pawn), densidade adaptativa baseada no tamanho da tela
- **Cor:** Usa a primary color `#E76F51`
- **Cleanup:** Remove event listener e cancela animation frame ao desmontar

---

### Autocomplete

**Arquivo:** `src/components/common/Autocomplete.tsx`

Input de texto com dropdown de sugestões. Suporta busca secundária pelo campo `searchValue` da opção.

**Interface AutocompleteOption:**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `value` | `string \| number \| boolean` | Valor da opção |
| `label` | `string` | Texto exibido |
| `searchValue?` | `string` | Valor adicional para busca (ex: código de barras) |

**Props (AutocompleteProps):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `value` | `string` | — | Valor atual do input |
| `onChange` | `(value: string) => void` | — | Callback ao digitar ou selecionar |
| `options` | `AutocompleteOption[]` | — | Lista de opções |
| `placeholder?` | `string` | — | Placeholder do input |
| `className?` | `string` | — | Classe CSS adicional |

**Comportamento:** Filtra opções por label e searchValue. Fecha ao clicar fora. Abre ao focar.

---

### ComingSoon

**Arquivo:** `src/components/common/ComingSoon.tsx`

Placeholder memoizado para páginas não implementadas.

**Props (ComingSoonProps, de `src/shared/types`):**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `pageName` | `string` | Nome da página a exibir |

**Renderiza:** `<h1>{pageName} será implementado em breve</h1>`

---

### EmptyState

**Arquivo:** `src/components/common/EmptyState.tsx`

Estado vazio para tabelas e listas sem dados.

**Props (EmptyStateProps):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `message?` | `string` | `"Nenhum dado encontrado."` | Mensagem exibida |
| `icon?` | `React.ReactNode` | — | Ícone opcional acima da mensagem |

**Renderiza:** Ícone condicional + mensagem centralizados.

---

### Toast

**Arquivo:** `src/components/common/Toast.tsx`

Notificação individual com auto-dismiss.

**Props (ToastProps):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `id` | `string` | — | Identificador único |
| `message` | `string` | — | Texto da notificação |
| `type?` | `'error' \| 'success' \| 'warning' \| 'info'` | `'info'` | Tipo visual |
| `duration?` | `number` | `5000` | Tempo em ms antes de fechar |
| `onClose` | `(id: string) => void` | — | Callback ao fechar |

**Comportamento:** Fecha automaticamente após `duration` ms. Botão manual de fechamento.

---

### ToastContainer / ToastProvider

**Arquivo:** `src/components/common/ToastContainer.tsx`

Contexto de notificações que gerencia a fila de toasts.

**ToastProvider Props:**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `children` | `ReactNode` | Componentes filhos |

**Contexto (ToastContextType):**
| Método | Assinatura | Descrição |
|--------|-----------|-----------|
| `showToast` | `(message: string, type?, duration?) => void` | Notificação genérica (padrão: info, 5000ms) |
| `showError` | `(message: string) => void` | Notificação de erro (6000ms) |
| `showSuccess` | `(message: string) => void` | Notificação de sucesso (4000ms) |
| `showWarning` | `(message: string) => void` | Notificação de aviso (5000ms) |
| `showErrorList` | `(errors: Record<string, string>, type?) => void` | Exibe múltiplos erros como toasts individuais |

**Hook:** `useToast()` — retorna o contexto. Dispara erro se usado fora de `ToastProvider`.

---

## Ícones (3)

Ícones SVG personalizados.

### MenuIcon

**Arquivo:** `src/components/icons/MenuIcon.tsx`

Hamburger que anima para X quando aberto.

**Props:**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `open?` | `boolean` | `false` | `true` anima para X, `false` mostra hamburger |

**Detalhes:** 4 retângulos SVG com transições CSS (`transform` e `opacity`). O retângulo do meio vira o traço central do X, os traços superior e inferior somem com translate.

---

### SearchIcon

**Arquivo:** `src/components/icons/SearchIcon.tsx`

Lupa de busca memoizada.

**Props (SearchIconProps, de `src/shared/types`):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `width?` | `number` | `24` | Largura do SVG |
| `height?` | `number` | `24` | Altura do SVG |
| `stroke?` | `string` | `"#666666"` | Cor do traço |
| `strokeWidth?` | `number` | `2` | Espessura do traço |

---

### ThemeIcon

**Arquivo:** `src/components/icons/ThemeIcon.tsx`

Ícone condicional: sol quando `isDark=true`, lua quando `isDark=false`.

**Props:**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `isDark` | `boolean` | `true` mostra sol (tema escuro ativo), `false` mostra lua |

---

## Layout (3)

Componentes estruturais da aplicação.

### Header

**Arquivo:** `src/components/layout/Header.tsx`

Navegação principal com menu responsivo.

**Props:** Nenhuma (sem interface de props)

**Funcionalidades:**
- Botão hamburger (MenuIcon) à esquerda
- Logo central (Link para Home, alterna entre `/logo.svg` e `/logo-claro.svg`)
- 6 links de navegação: Home, Empréstimos, Jogos, Instituição, Evento, Participantes
- Theme toggle (salva em `localStorage`, define `data-theme` no `<html>`)
- Scroll visibility via hook `useScrollVisibility` (threshold de `UI_CONSTANTS.SCROLL_THRESHOLD`)
- Menu fecha automaticamente ao trocar de rota (`location.pathname`)
- Acessibilidade: `aria-label`, `aria-expanded`, `aria-controls`

---

### Footer

**Arquivo:** `src/components/layout/Footer.tsx`

Rodapé memoizado com copyright dinâmico.

**Props:** Nenhuma

**Renderiza:**
```
© {anoAtual} LudicoM - UTFPR. Todos os direitos reservados.
Sistema de Gerenciamento de Atividades Lúdicas
```
O ano é calculado via `new Date().getFullYear()` com `useMemo`.

---

### PageHeader

**Arquivo:** `src/components/layout/PageHeader.tsx`

Cabeçalho de página memoizado com título e botão CTA opcional.

**Props (PageHeaderProps, de `src/shared/types`):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `title` | `string` | — | Título da página |
| `buttonText?` | `string` | — | Texto do botão CTA |
| `onButtonClick?` | `() => void` | — | Callback do botão |
| `showButton?` | `boolean` | `true` | Controla visibilidade do botão |

**Renderiza:** `<h1>` com classe `gradient-title` + botão condicional com classe `btn--xlarge btn--primary`.

---

## Modais (5)

Sistema de modais genéricos e específicos.

### CreateModal\<T\>

**Arquivo:** `src/components/modals/CreateModal.tsx`

Modal de criação genérico com formulário dinâmico baseado em campos.

**Interface CreateField\<T\>:**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `key` | `keyof T` | Chave do campo no objeto |
| `label` | `string` | Rótulo exibido |
| `type` | `'text' \| 'email' \| 'number' \| 'date' \| 'time' \| 'boolean' \| 'select' \| 'autocomplete'` | Tipo de input |
| `required?` | `boolean` | Campo obrigatório |
| `placeholder?` | `string` | Placeholder |
| `defaultValue?` | `any` | Valor padrão (pode ser função) |
| `options?` | `{ value: string \| number \| boolean; label: string }[]` | Opções para select/autocomplete |
| `validation?` | `(value: any) => string \| null` | Função de validação customizada |
| `dataListId?` | `string` | ID para autocomplete interno |

**Props (CreateModalProps\<T\>):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `isOpen` | `boolean` | — | Controla visibilidade |
| `onClose` | `() => void` | — | Fecha o modal |
| `onSave` | `(newItem: Omit\<T, 'id'\>) => void` | — | Callback de salvamento |
| `fields` | `CreateField\<T\>[]` | — | Configuração dos campos |
| `title?` | `string` | `'Criar Novo Item'` | Título do modal |
| `inlineFieldActions?` | `Record<string, { label?; title?; icon?; onClick }>` | — | Ações inline por campo (botão "+") |
| `prefill?` | `Partial\<Omit\<T, 'id'\>\>` | — | Valores pré-preenchidos (mescla sem reinicializar) |
| `infoMessage?` | `string \| React.ReactNode` | — | Mensagem informativa no topo |

**Tipos de campo:**
| Type | Renderização |
|------|-------------|
| `text` / `email` | `<input type="text\|email">` |
| `number` | `<input type="number">` |
| `date` | `<input type="date">` |
| `time` | `<input type="time">` |
| `boolean` | `<input type="checkbox">` |
| `select` | `<select>` com options |
| `autocomplete` | Componente `Autocomplete` |

**Funcionalidades:**
- Validação obrigatória e customizada por campo
- Botão "+" inline por campo via `inlineFieldActions`
- Prefill dinâmico (mescla valores sem resetar o formulário)
- Bloqueio de scroll da página quando aberto
- Esconde campo `horaDevolucao` se `isDevolvido` não estiver marcado
- ID único de formulário por instância
- Fecha ao clicar no backdrop

---

### EditModal\<T\>

**Arquivo:** `src/components/modals/EditModal.tsx`

Modal de edição genérico. Mesmos tipos de campo do CreateModal.

**Interface EditField\<T\>:**
Idêntica a `CreateField<T>`.

**Props (EditModalProps\<T\>):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `isOpen` | `boolean` | — | Controla visibilidade |
| `onClose` | `() => void` | — | Fecha o modal |
| `onSave` | `(updatedItem: T) => void` | — | Callback de salvamento |
| `item` | `T \| null` | — | Item a ser editado |
| `fields` | `EditField\<T\>[]` | — | Configuração dos campos |
| `title?` | `string` | `'Editar Item'` | Título do modal |

**Diferenças do CreateModal:**
- Não tem `inlineFieldActions`, `prefill` ou `infoMessage`
- Inicializa com valores do `item` recebido
- Normaliza objetos em campos autocomplete (extrai `.nome` do objeto)
- Botão "Salvar" em vez de "Criar"

---

### DetailModal\<T\>

**Arquivo:** `src/components/modals/DetailModal.tsx`

Modal de visualização somente-leitura.

**Interface DetailField\<T\>:**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `key` | `keyof T` | — | Chave do campo |
| `label` | `string` | — | Rótulo exibido |
| `type?` | `'text' \| 'date' \| 'boolean' \| 'number' \| 'array' \| 'custom'` | `'text'` | Tipo de renderização |
| `render?` | `(value: T[keyof T], item: T) => React.ReactNode` | — | Render function customizada |

**Props (DetailModalProps\<T\>):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `isOpen` | `boolean` | — | Controla visibilidade |
| `onClose` | `() => void` | — | Fecha o modal |
| `item` | `T \| null` | — | Item a exibir |
| `fields` | `DetailField\<T\>[]` | — | Configuração dos campos |
| `title?` | `string` | `'Detalhes'` | Título do modal |
| `onEdit?` | `(item: T) => void` | — | Callback ao clicar "Editar" |
| `onDelete?` | `(item: T) => void` | — | Callback ao clicar "Excluir" |

**Tipos de renderização:**
| Type | Formatação |
|------|-----------|
| `text` | `String(value)` |
| `date` | `new Date(value).toLocaleDateString('pt-BR')` |
| `boolean` | `'Sim'` ou `'Não'` |
| `number` | `value.toLocaleString('pt-BR')` |
| `array` | `value.join(', ')` |
| `custom` | Executa `field.render(value, item)` |

**Fallback:** Se `type` não especificado, usa `String(value)`. Se valor for `null` ou `undefined`, exibe `'-'`.

---

### ConfirmModal

**Arquivo:** `src/components/modals/ConfirmModal.tsx`

Modal de confirmação genérico.

**Props (ConfirmModalProps):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `isOpen` | `boolean` | — | Controla visibilidade |
| `title?` | `string` | `'Confirmar Ação'` | Título do modal |
| `message` | `string \| React.ReactNode` | — | Mensagem de confirmação |
| `confirmLabel?` | `string` | `'Confirmar'` | Texto do botão confirmar |
| `cancelLabel?` | `string` | `'Cancelar'` | Texto do botão cancelar |
| `onConfirm` | `() => void \| Promise<void>` | — | Callback de confirmação |
| `onCancel` | `() => void` | — | Callback de cancelamento |
| `variant?` | `'danger' \| 'primary' \| 'default'` | `'danger'` | Estilo do botão confirmar |

**CSS classes do botão confirmar:**
- `danger`: `btn--medium btn--danger`
- `primary`: `btn--medium btn--primary`
- `default`: `btn--medium btn--default`

---

### ConsultModal

**Arquivo:** `src/components/modals/ConsultModal.tsx`

**Único modal não genérico da aplicação.** Específico para a entidade Jogos.

**Props (ConsultModalProps):**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `isOpen` | `boolean` | Controla visibilidade |
| `onClose` | `() => void` | Fecha o modal |
| `onJogoUpdated?` | `(jogo: Jogo) => void` | Callback após editar um jogo |
| `onJogoDeleted?` | `(jogoId: string \| number) => void` | Callback após deletar um jogo |

**Funcionalidades:**
- Embuti `GenericTable<Jogo>` com colunas `JOGO_COLUMNS`
- Paginação server-side com `Pagination` (busca debounced 300ms)
- Ações: Detalhes (primary), Editar (secondary)
- Abre `DetailModal<Jogo>` e `EditModal<Jogo>` internamente
- Usa hook `useCrudOperations<Jogo>` para gerenciar estado dos modais
- Usa `fetchJogosPaginated`, `updateJogo`, `deleteJogo` do serviço
- Modal grande: `maxWidth: 90vw`, `maxHeight: 90vh`
- Loading state com opacidade reduzida para não piscar a tabela

---

## Sections (2)

Seções da página inicial.

### WelcomeSection

**Arquivo:** `src/components/sections/WelcomeSection.tsx`

Boas-vindas memoizado.

**Props:** Nenhuma

**Renderiza:** `<h1 class="welcome-title gradient-title">Bem Vindo ao Ludico Manager</h1>`

---

### QuickActions

**Arquivo:** `src/components/sections/QuickActions.tsx`

**Componente mais complexo da aplicação.** Gerencia 3 ações rápidas com fluxo de criação encadeada.

**Props:**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `onEmprestimoCreated?` | `() => void` | Callback após criar empréstimo |

**Ações:**
1. **Adicionar Participante** — Abre CreateModal com `participanteCreateFields`
2. **Consultar Jogo** — Abre ConsultModal
3. **Registrar Empréstimo** — Abre CreateModal com `emprestimoCreateFields`

**Fluxo encadeado (modal dentro de modal):**
```
Empréstimo → Participante → Instituição
```

O modal de empréstimo permite criar participante inline (botão "+" no campo participante), que por sua vez permite criar instituição inline (botão "+" no campo instituição). O nome criado retorna como prefill para o modal anterior.

**Funcionalidades do empréstimo:**
- Valida unicidade de email, documento e RA do participante
- Detecta evento ativo no momento (data + horário atual)
- Exibe alerta amarelo se faltam 30 minutos ou menos para o término do evento
- Filtra apenas jogos disponíveis (`j.isDisponivel`)
- Bloqueia empréstimo se não houver evento ativo

**Modais gerenciados:** Até 5 CreateModals simultaneamente (participante principal, instituição from participante, empréstimo, participante inline, instituição inline) + ConsultModal.

---

## Tables (2)

Componentes de tabela e paginação.

### GenericTable\<T\>

**Arquivo:** `src/components/tables/GenericTable.tsx`

Tabela genérica com busca, escape HTML e EmptyState.

**Props (GenericTableProps\<T\>):**
| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| `data` | `T[]` | — | Dados a exibir |
| `columns` | `TableColumn\<T\>[]` | — | Configuração das colunas |
| `actions?` | `TableAction\<T\>[]` | `[]` | Ações por linha |
| `searchPlaceholder?` | `string` | `'Buscar...'` | Placeholder do campo de busca |
| `searchFields` | `(keyof T)[]` | — | Campos usados na busca |
| `tableTitle` | `string` | — | Título acessível da tabela (caption) |
| `emptyMessage?` | `string` | `'Nenhum dado encontrado.'` | Mensagem quando vazio |
| `controlledSearchValue?` | `string` | — | Valor controlado da busca (externa) |
| `onControlledSearchChange?` | `(value: string) => void` | — | Callback da busca controlada |

**Tipos auxiliares (de `src/shared/types`):**
```typescript
interface TableColumn<T> {
  key: keyof T;
  label: string;
  render?: (value: any, item: T) => React.ReactNode;
}

interface TableAction<T> {
  label: string;
  onClick: (item: T) => void;
  variant?: 'primary' | 'secondary' | 'danger';
}
```

**Funcionalidades:**
- Escape HTML via utilitário `escapeHtml` (segurança XSS)
- Fallback para `EmptyState` quando lista vazia
- Filtro local com `filterByMultipleFields` (ou controlado externamente)
- Input de busca com ícone SearchIcon
- Coluna de ações com botões estilizados por variant
- Limitação de texto nas 4 primeiras colunas
- `useMemo` nos dados filtrados para performance

---

### Pagination

**Arquivo:** `src/components/tables/Pagination.tsx`

Paginação server-side com ellipsis inteligente.

**Props (PaginationProps):**
| Prop | Tipo | Descrição |
|------|------|-----------|
| `page` | `number` | Página atual (0-indexed) |
| `totalPages` | `number` | Total de páginas |
| `totalElements` | `number` | Total de registros |
| `pageSize` | `number` | Itens por página |
| `onPageChange` | `(page: number) => void` | Callback ao trocar página |
| `onPageSizeChange` | `(size: number) => void` | Callback ao trocar tamanho |

**Funcionalidades:**
- Seletor de tamanho: 10, 20, 50, 100
- Botões: Primeira, Anterior, números, Próxima, Última
- Ellipsis (`...`) para intervalos grandes de páginas
- Exibe "Página X de Y (Z registros)"
- Retorna `null` se `totalPages <= 1` (oculta completamente)
- Botão desabilitado na primeira/última página

---

## Resumo

| Categoria | Componentes | Arquivos |
|-----------|-------------|----------|
| Common (6) | AnimatedBackground, Autocomplete, ComingSoon, EmptyState, Toast, ToastContainer | `src/components/common/*` |
| Ícones (3) | MenuIcon, SearchIcon, ThemeIcon | `src/components/icons/*` |
| Layout (3) | Header, Footer, PageHeader | `src/components/layout/*` |
| Modais (5) | CreateModal, EditModal, DetailModal, ConfirmModal, ConsultModal | `src/components/modals/*` |
| Sections (2) | WelcomeSection, QuickActions | `src/components/sections/*` |
| Tables (2) | GenericTable, Pagination | `src/components/tables/*` |
