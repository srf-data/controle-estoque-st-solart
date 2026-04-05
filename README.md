# Sistema de Controle de Estoque e Precificação

**Versão:** 1.0  
**Data:** 30/03/2026  

Sistema web para controle de estoque, gerenciamento de produtos, matérias-primas, fornecedores, precificação e movimentações (entradas e saídas). Desenvolvido como Projeto Integrador (PI) da UNIVESP.

---

## 📑 Histórico de Revisões

| Versão | Data | Autor | Descrição |
|--------|------|-------|----------|
| 1.0 | 30/03/2026 | Equipe PI | Documentação inicial do sistema |
| 1.1 | 05/04/2026 | Shayare | Adição de funcionalidade de recuperação de senha com envio de código por e-mail |

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
- React.js  
- Vite  
- React Router  
- Axios  

## Backend
- Node.js  
- Express.js  
- JWT (autenticação)  
- bcrypt  

## Banco de Dados
- PostgreSQL (Neon)  
- Prisma ORM  

## Interface e Estilo
- Tailwind CSS  
- Recharts  

---

# 4. Setup e Desenvolvimento

## Pré-requisitos

- Node.js 18+  
- Git  

---

## Instalação

```bash
# Clonar repositório
git clone https://github.com/srf-data/controle-estoque-st-solart.git

# Entrar na pasta e instalar dependências
npm install

# Executar projeto
npm run dev
```

# Endereços
Frontend: link
Backend: link

# 5. Funcionalidades
### Autenticação
- Cadastro de usuários
- Login com validação
- Proteção de rotas
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
### Produtos
- Cadastro de produtos
- Listagem
- Edição
- Exclusão
- Definição de preço
### Matérias-Primas
- Cadastro completo
- Gerenciamento de dados
- Integração com estoque
### Fornecedores
- Cadastro de fornecedores
- Listagem e edição
- Informações de contato
### Entradas
- Registro de entrada de estoque
- Atualização automática de quantidade
### Saídas
- Registro de saída
- Controle de estoque em tempo real
### Usuários
- Cadastro
- Listagem
- Edição
- Exclusão

# 6. Estrutura do Projeto
A denifir

# 7. Configuração
### Variáveis de Ambiente
- Backend (.env)
- DATABASE_URL=
- JWT_SECRET=
- PORT=3000
- Frontend (.env)
- VITE_API_URL=http://localhost:3000
### Scripts
# Desenvolvimento
npm run dev

### Build
npm run build

### Produção
npm start

# 8. Deploy
Ambiente de Produção
- Frontend: Vercel
- Backend: Render
- Banco: Neon

### Checklist
- [ ] Configurar variáveis de ambiente
- [ ] Rodar migrations
- [ ] Testar autenticação
- [ ] Testar CRUD completo
- [ ] Validar entradas e saídas
- [ ] Validar dashboard
<div><b>Status do Projeto</b>: Em desenvolvimento</div>

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

