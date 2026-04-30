# Sistema de Controle de Estoque e Precificação

**Versão:** 1.6
**Data:** 28/04/2026

Sistema web para controle de estoque, gerenciamento de produtos, matérias-primas, fornecedores, precificação e movimentações (entradas e saídas). Desenvolvido como Projeto Integrador (PI) da UNIVESP.

---

## Histórico de Revisões

| Versão | Data | Autor | Descrição |
|--------|------|-------|----------|
| 1.0 | 30/03/2026 | Equipe PI | Documentação inicial do sistema |
| 1.1 | 05/04/2026 | Shayare | Adição de funcionalidade de recuperação de senha com envio de código por e-mail |
| 1.2 | 06/04/2026 | Shayare | Adição de pop-ups de cadastro/edição, regras de validação de insumos, detalhamento de tabelas e novos indicadores de dashboard |
| 1.3 | 12/04/2026 | Shayare | Melhorias de UI/UX, correção de unidades, baixa automática de insumos (efeito cascata), conversor de medidas e travas de estoque |
| 1.4 | 19/04/2026 | Marco, Shayare | Persistência de login, e-mails reais, validações de formulário e prevenção de duplos clique | 
| 1.5 | 26/04/2026 | Isaque, Shayare | Migração para PostgreSQL (Neon), Prisma ORM, Gestão de imagens Base64, cálculo inteligente de custos e novos campos financeiros | 
| 1.6 | 28/04/2026 | Fabiano, Shayare | Middleware JWT estrito, proteção CORS, Rate Limiting (anti-brute force), sanitização de logs e otimização de payload (10MB) |

---

## Índice

- Introdução  
- Visão Geral do Produto  
- Arquitetura e Tecnologias  
- Setup e Desenvolvimento  
- Funcionalidades  
- Estrutura do Projeto  
- Configuração  
- Deploy  

---

# 1. Introdução

## 1.1 Propósito

Este documento descreve a arquitetura, funcionalidades e processo de desenvolvimento do **Sistema de Controle de Estoque e Precificação**, com o objetivo de orientar a equipe na construção e evolução da aplicação.

---

## 1.2 Público-alvo

- Desenvolvedores frontend e backend  
- Equipe do Projeto Integrador (UNIVESP)  
- Professores avaliadores  

---

## 1.3 Escopo

O sistema contempla:

- Controle de estoque  
- Cadastro de produtos e matérias-primas  
- Gestão de fornecedores  
- Registro de entradas e saídas  
- Precificação de produtos  
- Dashboard com indicadores  

---

# 2. Visão Geral do Produto

O sistema é uma aplicação web que centraliza o controle de estoque e precificação, substituindo planilhas manuais por uma solução digital organizada.

Permite:

- Visualização de dados em tempo real  
- Controle de movimentações  
- Gestão de recursos do estoque  
- Definição de preços com base em dados  
- Melhor tomada de decisão  

---

# 3. Arquitetura e Tecnologias

## Frontend
- React.js (Core do projeto)
- Vite (Build tool e servidor)
- Tailwind CSS (Estilização)
- Recharts (Gráficos do dashboard)
- Zod (Validação rigorosa de formulários)
- Native Fetch API (Requisições HTTP)

## Backend
- Node.js & Express.js
- JWT (Autenticação) & bcryptjs (Criptografia)
- Prisma ORM
- Nodemailer (E-mails via Gmail)
- Express Rate Limit & CORS (Segurança de tráfego e acesso)

## Banco de Dados
- PostgreSQL (Neon)  

---

# 4. Setup e Desenvolvimento

## Pré-requisitos

- Node.js 18+  
- Git  

---

## Instalação

```bash
# Clonar repositório
git clone https://github.com/srf-data/App_St.git

# Entrar na pasta e instalar dependências
npm install

# Executar projeto
npm run dev
```

# 5. Funcionalidades
### Autenticação
- Cadastro de usuários
- Login com validação
- Proteção de rotas
- Persistência de sessão utilizando LocalStorage.
- Proteção estrita de rotas no backend via Middleware JWT.
- Logout automático imediato em caso de exclusão da própria conta.
- Bloqueio de rotas não autorizadas com CORS e Rate Limiting global (prevenção contra força bruta).

### Recuperação de Senha
- Solicitação de redefinição de senha via e-mail  
- Envio de código temporário para o e-mail cadastrado  
- Validação do código informado pelo usuário  
- Definição de nova senha com confirmação  
- Expiração automática do código de verificação  
### Dashboard
- Indicadores de estoque
- Gráficos de entradas e saídas
- Visualização de dados em tempo real
- Exibição da quantidade total de itens cadastrados (insumos + produtos).
- Indicadores visuais específicos e destaques para "Insumos com Estoque Mínimo" e "Estoque Zerado".
### Produtos
- Cadastro de produtos
- Listagem
- Edição
- Exclusão
- Definição de preço
- Criação via pop-up de "Novo Registro" com lista de seleção dinâmica de insumos.
- Pop-up de "Receita (Cálculo)" carregando dados e custos de produção em tempo real.
- Exibição padronizada de valores fracionários vinculados às suas unidades de medida (ex: 0,9 L, 100 ml, 5,4 Kg).
- Cálculo de Custo Inteligente: o custo é calculado fracionando o valor do insumo proporcionalmente à unidade utilizada.
### Matérias-Primas
- Cadastro completo
- Gerenciamento de dados
- Integração com estoque
- Cadastro e edição integrados via pop-ups (relacionando Fornecedor, Unidade de Medida, Custo Unitário e Estoque Inicial).
- Destaque visual na tabela para itens com estoque crítico.
### Fornecedores
- Cadastro de fornecedores
- Listagem e edição
- Informações de contato
- Formulário de cadastro/edição via pop-up com campos detalhados (Razão, Nome, CNPJ, Cidade, Estado e Contato).
### Entradas
- Registro de entrada de estoque
- Atualização automática de quantidade
- Registro de matérias-primas vinculadas por fornecedor com edição via pop-up.
- Efeito Cascata (Produção Própria): Ao registrar a produção de um item, o sistema lê a receita e deduz os insumos automaticamente do estoque global.
### Saídas
- Registro de saída
- Controle de estoque em tempo real
- Tabela com detalhamento de Ordem, Cliente, Produto, Quantidade, Total e Data.
- Baixas Avulsas Modulares: Opção de selecionar a "Unidade Utilizada" para declarar gastos fracionários exatos (ex: baixar ml de um estoque em Litros).
- Inclusão do campo Desconto (R$) com recálculo automático do Valor Total.
- Edição de registros de saídas diretamente na tabela, com atualização de saldo em tempo real.
### Usuários
- Cadastro
- Listagem
- Edição
- Exclusão
### UI/UX e Navegação
- **Notificações Flutuantes:** Banners de feedback sutis no topo da tela que desaparecem após 3 segundos.
- **Exclusão Segura:** Remoção de botões de exclusão direta nas tabelas; a exclusão agora é confirmada apenas dentro da tela de "Editar" para evitar cliques acidentais.
- **Paginação Padronizada:** Todas as tabelas operam em um sistema unificado numérico [ 1 ] [ 2 ] [ ... ].
- **Acessibilidade Visual:** Correção de focos residuais utilizando semântica estrita HTML (`<button>`) em elementos interativos.
- Exigência de formatos padronizados de dados via Zod.
- Prevenção de Duplicidade: Estado de processamento (isProcessing) que desativa botões durante o envio de requisições.
### Regras de Negócio e Automação
- **Bloqueio de Estoque Falso:** O campo "Estoque" é de leitura bloqueada em edições manuais. Alterações de quantidade exigem registro formal em entradas/saídas para garantir rastreabilidade.
- **Motor Conversor de Receitas:** Inteligência que processa unidades diferentes (ex: Litro para ml, Quilo para gramas) para abater frações exatas sem gerar falsos erros de quantidade.
- **Travas Anti-Quebra (Bloqueio Fatal):** O sistema impede e barra imediatamente movimentações (vendas ou produções) que resultem em estoque negativo (abaixo de zero).
- **Reconciliação de Estoque**: Ao editar ou excluir entradas/saídas, o sistema compensa o inventário automaticamente.
- **Conversão Universal Ativa**: O sistema realiza conversão matemática automática entre unidades de massa e volume (L ↔ ml e kg ↔ g) em todas as operações.

# 6. Estrutura do Projeto
```
App_St/
├── server/
│   ├── index.cjs          # Servidor Monolítico (Express + Prisma + Rotas)
│   └── prisma/
│       └── schema.prisma  # Modelagem do Banco de Dados PostgreSQL
├── src/
│   ├── assets/            # Imagens e ícones estáticos
│   ├── utils/             # api.js (Fetch), validators.js (Zod)
│   ├── App.jsx            # Gerenciador de Rotas/Navegação
│   ├── PaginaInicial.jsx  # Dashboard com Recharts
│   ├── Produtos.jsx       # Gestão de Produtos e Receitas
│   ├── MateriasPrimas.jsx # Gestão de Insumos e Estoque
│   ├── Users.jsx          # Gestão de Usuários e Perfil
│   └── ...                # Outros componentes (Saidas, Entradas, Fornecedores)
├── dist/                  # Build final para produção
└── package.json           # Dependências e Scripts (Vite)
```

# 7. Configuração
### Variáveis de Ambiente
Backend (.env)
- DATABASE_URL=postgresql://usuario:senha@host:porta/banco?sslmode=require
- JWT_SECRET=chave_secreta_aqui
- PORT=3005
- EMAIL_USER=email@gmail.com
- EMAIL_PASS=senha_de_app_gerada
Frontend (.env)
- VITE_API_URL=http://localhost:3005

# 8. Deploy
Ambiente de Produção
- Frontend: Vercel
- Backend: Render
- Banco: Neon  

## Equipe

Projeto desenvolvido por equipe de 8 integrantes para o Projeto Integrador (PI) da UNIVESP:
- CAIO DAUMAS TREVISAN 
- FABIANO SANNINO 
- GILVAN CARDOSO 
- ISAQUE ALVES MENDES 
- LARISSA MOREIRA DO NASCIMENTO PRADO 
- MAIARA DIAS DOS SANTOS 
- MARCO ANTONIO DE LIMA PRADO 
- SHAYARE ROCHA FERREIRA

