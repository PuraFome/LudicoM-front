# LudicoM – Frontend 🎲

**LudicoM Frontend** é a aplicação web (interface de usuário) desenvolvida em **React com TypeScript** para apoiar o gerenciamento de jogos, usuários, eventos e atividades do **Ludico UTFPR**.

Este repositório faz parte do sistema **LudicoM**, que busca modernizar e centralizar o controle das ações do programa de extensão **Ludico**, facilitando a organização e ampliando o alcance do projeto junto à comunidade acadêmica e externa.

---

## 🏛 Sobre o Programa Ludico

O **Ludico** é um programa de extensão da **Universidade Tecnológica Federal do Paraná (UTFPR)**, ativo desde **2016**, com atividades contínuas de caráter mensal. Ele é composto por três frentes principais:  

- 🎲 **Board Games**  
- 🧩 **RPG**  
- 🔐 **Escape Room**  

O programa tem como público-alvo **alunos da UTFPR** e a **comunidade externa** das cidades de **Londrina, Cornélio Procópio** e regiões metropolitanas.  

### 🎯 Objetivos
- Desenvolver **capacidades de aprendizagem** nos participantes.  
- Melhorar **raciocínio lógico** e **análise crítica**.  
- Estimular **organização** e **relações interpessoais**.  
- Atuar como **ferramenta de inserção cultural**.  

### 📅 Atividades
- Eventos mensais desde 2016, realizados alternadamente nos campus de **Cornélio Procópio** e **Londrina**.  
- Média de **160 participantes por evento**.  
- Sessões de **board games**, **RPG**, **escape room** e **jogos em inglês**.  
- **Palestras** com profissionais convidados sobre jogos, cultura e educação.  
- **Sorteios de brindes** em parceria com colaboradores.  
- Eventos online realizados durante a pandemia (palestras, workshops, jogos digitais).  

### 🌍 Reconhecimento
- Único projeto da **América Latina** aprovado no edital internacional **Game in Lab**, que estuda o uso de jogos de tabuleiro para aquisição e manutenção de habilidades sociais.  
- Destaque em veículos como **Folha de Londrina**, **Taberna Role Play (YouTube)** e **Podcast A Taverna do Beholder Cego**.  
- Participações em eventos culturais e acadêmicos como **WRPG Fest**, **Semana do Orgulho Nerd**, **Expo Japão 2018** e **SPIEL Digital 2020**.  

---

## 🚀 Funcionalidades do Frontend

- Interface intuitiva e moderna para gerenciamento de:
  - 🎮 **Jogos** (cadastro, consulta, edição e exclusão)
  - 👥 **Participantes** (registro e gerenciamento)
  - 🏢 **Instituições** (cadastro e controle)
  - 📅 **Eventos** (criação e acompanhamento)
  - 📦 **Empréstimos** (registro e controle de devoluções)
- Sistema de busca e filtros
- Modais para criação, edição, visualização e confirmação
- Notificações Toast para feedback ao usuário
- Layout responsivo e adaptável
- Tema com animações e design moderno
- Code splitting e lazy loading para melhor performance
- Integração com API REST do backend

---

## 🛠 Tecnologias Utilizadas

- **React 19** com TypeScript
- **React Router DOM** para navegação
- **Webpack** para bundling e desenvolvimento
- **Babel** para transpilação
- **CSS Modules** para estilização
- **PropTypes** para validação de props
- **Fetch API** para comunicação com backend

---

## 🧭 Como Executar

### Pré-requisitos
- **Node.js 18+**  
- **npm** ou **yarn**  
- Backend **LudicoM** rodando (opcional para desenvolvimento)

### Passos

```bash
# Clonar o repositório
git clone https://github.com/LudicoM-UTFPR/LudicoM-front.git
cd LudicoM-front

# Instalar dependências
npm install

# Configurar variáveis de ambiente (opcional)
# Crie um arquivo .env na raiz do projeto
# REACT_APP_API_BASE_URL=http://localhost:8080/api

# Rodar em modo desenvolvimento
npm start

# A aplicação estará disponível em http://localhost:8080
```

### Build para Produção

```bash
# Gerar build otimizado
npm run build

# Os arquivos estarão na pasta dist/
```

---

## 📁 Estrutura do Projeto

```
src/
├── app/                    # Configuração principal da aplicação
├── components/             # Componentes reutilizáveis
│   ├── common/            # Componentes comuns (Toast, Autocomplete, etc)
│   ├── icons/             # Ícones SVG personalizados
│   ├── layout/            # Componentes de layout (Header, Footer)
│   ├── modals/            # Modais (Create, Edit, Detail, Confirm)
│   ├── sections/          # Seções da página inicial
│   └── tables/            # Tabelas genéricas
├── pages/                 # Páginas da aplicação
├── shared/                # Recursos compartilhados
│   ├── constants/         # Constantes e configurações
│   ├── data/              # Dados mockados (desenvolvimento)
│   ├── hooks/             # Custom hooks
│   ├── services/          # Serviços de API
│   ├── types/             # Definições de tipos TypeScript
│   └── utils/             # Funções utilitárias
└── styles/                # Estilos CSS globais e por componente
```

---

## 🔗 Integração com Backend

O frontend se comunica com o backend através de serviços organizados em `src/shared/services/`:

- `authService.ts` - Autenticação e autorização
- `jogosService.ts` - Operações CRUD de jogos
- `participanteService.ts` - Gerenciamento de participantes
- `instituicaoService.ts` - Gerenciamento de instituições
- `eventosService.ts` - Gerenciamento de eventos
- `emprestimosService.ts` - Controle de empréstimos

Configure a URL da API no arquivo `.env`:
```env
REACT_APP_API_BASE_URL=http://localhost:8080/api
```

---

## 🎨 Funcionalidades de Interface

### Componentes Principais

- **GenericTable**: Tabela reutilizável com busca, paginação e ações
- **Modals**: Sistema modular de modais para diferentes operações
- **Toast**: Sistema de notificações não-intrusivas
- **Autocomplete**: Campo de busca com sugestões
- **AnimatedBackground**: Fundo animado com tema de jogos

### Páginas

- **Home**: Página inicial com ações rápidas e boas-vindas
- **Jogos**: Gerenciamento completo de jogos
- **Participantes**: Cadastro e controle de participantes
- **Instituições**: Gerenciamento de instituições parceiras
- **Eventos**: Criação e acompanhamento de eventos
- **Empréstimos**: Controle de empréstimos e devoluções
- **Login**: Autenticação de usuários (em desenvolvimento)

---

## 📚 Documentação

Documentação detalhada disponível na pasta [`docs/`](docs/):

| Documento | Descrição |
|---|---|
| [Visão Geral](docs/index.md) | Funcionalidades, tecnologias, como executar |
| [Arquitetura](docs/arquitetura.md) | Bootstrap, roteamento, providers, estilos |
| [Funcionalidades](docs/funcionalidades.md) | CRUD por entidade, matriz de operações |
| [Componentes](docs/componentes.md) | Inventário de 21 componentes com props |
| [API e Serviços](docs/servicos-api.md) | Endpoints, hooks, tipos, utilitários |

---

## 🧪 Scripts Disponíveis

```bash
# Desenvolvimento com hot reload
npm start

# Build de produção
npm run build

# Verificação de tipos TypeScript
npm run type-check

# Verificação contínua de tipos
npm run type-check:watch
```

---

## 📄 Licença

Este projeto está licenciado sob os termos da [Licença MIT](./LICENSE).

---

## 👥 Contribuindo

Contribuições são bem-vindas! Por favor, siga estas diretrizes:

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
3. Commit suas mudanças (`git commit -m 'Adiciona MinhaFeature'`)
4. Push para a branch (`git push origin feature/MinhaFeature`)
5. Abra um Pull Request

---

## 📞 Contato

**Programa Ludico UTFPR**  
- Campus: Cornélio Procópio e Londrina  
- Universidade: UTFPR - Universidade Tecnológica Federal do Paraná

Para mais informações sobre o programa, visite o repositório do backend: [LudicoM-backend](https://github.com/LudicoM-UTFPR/LudicoM-backend)
