# Arquitetura do LudicoM Frontend

## Bootstrap da Aplicacao

```
public/index.html
  └── <div id="root"></div>
      └── webpack (entry: src/app/index.tsx)
          └── createRoot() — React 19 Concurrent API
              └── <App />
```

O `React.StrictMode` foi **intencionalmente desabilitado** para evitar dupla montagem que causava requisicoes canceladas.

### ResizeObserver para Header Height

O `App.tsx` possui um `useEffect` que observa dinamicamente a altura do `<header>` usando `ResizeObserver` (com fallback para `window.resize`) e atualiza a variavel CSS `--header-height` no `<html>`. Isso garante que o `main-content` sempre respeite o espaco ocupado pelo header, mesmo em mudancas de layout responsivo ou quebra de linha.

## Componente Raiz (App.tsx)

```
<ToastProvider>                    ← Contexto de notificacoes
  <Router basename="/LudicoM-front/">
    <div className="app">
      <AnimatedBackground />         ← Fundo animado decorativo
      <Header />                     ← Barra de navegacao principal
      <main className="main-content">
        <Suspense fallback={<div className="lazy-fallback">Carregando...</div>}>
          <Routes>
            <Route path="/"            → Home />
            <Route path="/jogos"       → Jogos />
            <Route path="/participantes" → Participantes />
            <Route path="/instituicoes"  → Instituicoes />
            <Route path="/eventos"     → Eventos />
            <Route path="/emprestimos" → Emprestimos />
            <Route path="/login"       → Login />
          </Routes>
        </Suspense>
      </main>
      <Footer />
    </div>
  </Router>
</ToastProvider>
```

## Padrao Arquitetural por Pagina CRUD

Todas as paginas CRUD seguem o mesmo padrao:

```
Pagina
├── PageHeader (titulo + botao "Criar")
├── Campo de busca (debounce 300ms)
├── GenericTable (dados paginados + acoes)
│   ├── DetailModal (visualizacao)
│   ├── EditModal (edicao)
│   └── ConfirmModal (exclusao)
├── CreateModal (criacao)
└── Pagination (controles server-side)
```

## Roteamento

Os paths sao definidos como constantes tipadas em `src/shared/constants/index.ts` (`ROUTES.HOME`, `ROUTES.JOGOS`, etc.) e usados tanto no `App.tsx` quanto nas navegacoes do `Header`.

| Path | Constante | Componente | Lazy Load | Descricao |
|---|---|---|---|---|
| `/` | `ROUTES.HOME` | Home | Sim | Dashboard |
| `/jogos` | `ROUTES.JOGOS` | Jogos | Sim | CRUD jogos |
| `/participantes` | `ROUTES.PARTICIPANTES` | Participantes | Sim | CRUD participantes |
| `/instituicoes` | `ROUTES.INSTITUICOES` | Instituicoes | Sim | CRUD instituicoes |
| `/eventos` | `ROUTES.EVENTOS` | Eventos | Sim | CRUD eventos |
| `/emprestimos` | `ROUTES.EMPRESTIMOS` | Emprestimos | Sim | CRUD emprestimos (2 abas) |
| `/login` | `ROUTES.LOGIN` | Login | Sim | Placeholder |

## Gerenciamento de Estado

- **Context API**: notificacoes (ToastContext) — autenticacao via Basic Auth em `authService.ts` (sem Context API)
- **Estado local (useState/useReducer)**: paginas CRUD individuais, controle de modais e formularios
- **Cache externo**: sessionStorage para jogos (5 minutos) e emprestimos (2 minutos)

## Estilos

### Design System

O design system e baseado em variaveis CSS customizadas com uma paleta de cores consistente:

- **Primaria**: Laranja `#E76F51`
- **Secundaria**: Azul `#457B9D`
- **Terciaria**: Verde `#2A9D8F`
- **Perigo**: Vermelho `#DC3545`

### Botoes — 4 tamanhos x 6 variantes

| Tamanho | Padding | Font Size | Altura |
|---|---|---|---|
| small | 6px 12px | 14px | 32px |
| medium | 8px 16px | 16px | 36px |
| large | 12px 24px | 18px | 44px |
| xlarge | 16px 32px | 24px | 48px |

| Variante | Cor | Uso |
|---|---|---|
| primary | Laranja `#E76F51` | Acao principal |
| secondary | Azul `#457B9D` | Acao secundaria |
| tertiary | Verde `#2A9D8F` | Acao terciaria |
| danger | Vermelho `#DC3545` | Exclusao |
| outline | Borda | Acao leve |
| ghost | Transparente | Contextual |

### Responsividade — 6 breakpoints

| Breakpoint | Alvo | Principais mudancas |
|---|---|---|
| <= 1440px | Desktop Large | Padding em acoes rapidas e tabela |
| <= 1200px | Desktop | Nav gap, titulos menores |
| <= 992px | Tablet | Nav compacta, tabela com scroll |
| <= 768px | Tablet Pequeno | Header wrap, fontes menores |
| <= 600px | Mobile | Botoes em coluna, padding zero |
| <= 480px | Mobile Pequeno | Tudo no tamanho minimo |

### Animacoes

- **Toast**: `@keyframes slideIn` — desliza da direita
- **Header mobile**: `max-height` animado para abrir/fechar menu
- **Hover geral**: `translateY(-2px)` com sombra elevada
- **Fundo animado**: `requestAnimationFrame` com particulas em canvas

## Performance

- **Code splitting**: lazy loading com `React.lazy()` em todas as 7 paginas
- **Cache sessionStorage**: jogos (5 min), emprestimos (2 min)
- **Debounce**: 300ms nas buscas dos CRUDs
- **Memorizacao**: `React.memo` em 5 componentes
- **RAF throttle**: no hook `useScrollVisibility`
- **Webpack**: `splitChunks` para separar vendors + `contenthash` nos bundles
