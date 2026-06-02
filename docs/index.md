# LudicoM Frontend — Documentação

> **Versão:** 1.0.0
> **Stack:** React 19 + TypeScript + Webpack 5
> **Homepage:** [purafome.github.io/LudicoM-front](https://purafome.github.io/LudicoM-front)

## Índice da Documentação

| Documento | Descrição |
|---|---|
| [`index.md`](index.md) | Visão geral, funcionalidades, tecnologia |
| [`arquitetura.md`](arquitetura.md) | Estrutura do projeto, roteamento, bootstrap |
| [`funcionalidades.md`](funcionalidades.md) | Funcionalidades por entidade, CRUD, modais |
| [`componentes.md`](componentes.md) | Inventário de componentes reutilizáveis |
| [`servicos-api.md`](servicos-api.md) | Serviços de API, endpoints, hooks |

## Sobre o LudicoM

O **LudicoM** é um sistema de gerenciamento para o programa de extensão **Lúdico** da UTFPR, ativo desde 2016 com eventos mensais nos campi de Cornélio Procópio e Londrina, atendendo uma média de **160 participantes por evento**.

O programa atua em três frentes principais:
- 🎲 **Board Games**
- 🧩 **RPG**
- 🔐 **Escape Room**

Este repositório contém o **frontend** da aplicação, uma SPA (Single Page Application) desenvolvida em React com TypeScript.

## Funcionalidades Implementadas

| Funcionalidade | Status | Documentação |
|---|---|---|
| 🎮 **CRUD Jogos** — Cadastro, consulta, edição e exclusão | ✅ Completo | [funcionalidades.md](funcionalidades.md#jogos) |
| 👥 **CRUD Participantes** — Registro e gerenciamento | ✅ Completo | [funcionalidades.md](funcionalidades.md#participantes) |
| 🏢 **CRUD Instituições** — Cadastro de instituições | ✅ Completo | [funcionalidades.md](funcionalidades.md#instituições) |
| 📅 **CRUD Eventos** — Criação e acompanhamento | ✅ Completo | [funcionalidades.md](funcionalidades.md#eventos) |
| 📦 **CRUD Empréstimos** — Controle de empréstimos e devoluções | ✅ Completo | [funcionalidades.md](funcionalidades.md#empréstimos) |
| 🏠 **Dashboard Home** — Ações rápidas, empréstimos ativos | ✅ Completo | [funcionalidades.md](funcionalidades.md#home) |
| 🔐 **Login** — Autenticação de usuários | ⏳ Placeholder | — |
| 🎨 **Tema Dark/Light** — Alternância entre temas | ✅ Completo | [componentes.md](componentes.md#layout) |
| 📱 **Design Responsivo** — 5 breakpoints (1440px a 480px) | ✅ Completo | [arquitetura.md](arquitetura.md#estilos) |
| 🔔 **Sistema de Notificações Toast** | ✅ Completo | [componentes.md](componentes.md#common) |
| 🔍 **Busca com Debounce + Paginação** (300ms) | ✅ Completo | [funcionalidades.md](funcionalidades.md#padrões-comuns) |
| 💾 **Cache SessionStorage** (jogos 5min, empréstimos 2min) | ✅ Completo | [servicos-api.md](servicos-api.md#hooks-de-dados) |

## Tecnologias

| Tecnologia | Versão | Finalidade |
|---|---|---|
| **React** | ^19.1.1 | Biblioteca de UI |
| **TypeScript** | ^5.9.3 | Tipagem estática |
| **React Router DOM** | ^7.9.3 | Roteamento SPA |
| **Webpack** | ^5.101.3 | Bundler e dev server |
| **Babel** | ^7.28.4 | Transpilação JS/TS |
| **CSS Modules** | — | Estilização |
| **PropTypes** | ^15.8.1 | Validação de props |
| **Fetch API** | nativa | Comunicação com backend |
| **gh-pages** | ^6.3.0 | Deploy GitHub Pages |

## Como Executar

```bash
npm install
npm start         # Dev na porta 3000
npm run build     # Build produção → dist/
npm run type-check # Verificação de tipos
npm run deploy    # Deploy GitHub Pages
```

### Configuração de Ambiente

Crie um arquivo `.env` na raiz com as variáveis:

```
REACT_APP_API_BASE_URL=http://localhost:8080/api
REACT_APP_AUTH_USERNAME=admin
REACT_APP_AUTH_PASSWORD=admin123
```

## Estrutura do Projeto

```
src/
├── app/                    # Bootstrap (entry point, App raiz)
├── components/             # Componentes reutilizáveis
│   ├── common/            # Toast, Autocomplete, AnimatedBackground
│   ├── icons/             # Ícones SVG (Menu, Search, Theme)
│   ├── layout/            # Header, Footer, PageHeader
│   ├── modals/            # CreateModal, EditModal, DetailModal, ConfirmModal, ConsultModal
│   ├── sections/          # WelcomeSection, QuickActions (home)
│   └── tables/            # GenericTable, Pagination
├── pages/                 # 7 páginas (Home, Jogos, Participantes, etc.)
├── shared/                # Código compartilhado
│   ├── constants/         # Rotas, campos, colunas
│   ├── data/              # Dados mockados
│   ├── hooks/             # useCrudOperations, useEntityData, useScrollVisibility
│   ├── services/          # authService, jogosService, etc.
│   ├── types/             # Interfaces TypeScript
│   └── utils/             # Utilitários, validações
└── styles/                # CSS global e de componentes
    ├── base/              # Reset, variáveis (design tokens), responsivo
    └── components/        # 15 folhas de estilo específicas
```

## Licença

MIT — conforme arquivo [LICENSE](../LICENSE).
