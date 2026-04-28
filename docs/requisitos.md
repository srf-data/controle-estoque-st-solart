# **Documento de Requisitos: Controle de Estoque e Precificação**

Versao: 1.6
Data: 28/04/2026

---

## **Historico de Revisoes**

| Versão | Data       | Autor                       | Descrição                                                                      |
| ------ | ---------- | --------------------------- | ------------------------------------------------------------------------------ |
| 1.0    | 28/03/2026 | Equipe PI UNIVESP | Documento inicial de requisitos para sistema de controle de estoque e produção |
| 1.1    | 05/04/2026 | Shayare | Adição da funcionalidade de recuperação de senha com envio de código por e-mail |
| 1.2    | 06/04/2026 | Shayare | Adição de pop-ups de cadastro/edição, regras de validação de insumos, detalhamento de campos em tabelas (saídas, fornecedores, matérias-primas) e novos indicadores de dashboard |
| 1.3    | 12/04/2026 | Shayare | Atualização de UI/UX (notificações flutuantes, paginação padrão, novo fluxo de exclusão), conversão inteligente de unidades, efeito cascata na produção, bloqueio estrito de estoque negativo e correções de renderização |
| 1.4    | 19/04/2026 | Marco, Shayare | Implementação de persistência de login, configuração de envio de e-mails reais (Gmail), validações de formulário, prevenção de duplos cliques e segurança de repositório |
| 1.5    | 23/04/2026 - 26/04/2026 | Isaque, Shayare | Migração para PostgreSQL (Neon), Prisma ORM, Gestão de imagens em Base64, cálculo inteligente de custos/conversão universal, novos campos financeiros (Comissão e Desconto) e ajustes de segurança e sessão. |
| 1.6    | 28/04/2026 | Fabiano, Shayare | Implementação de Middleware JWT estrito, proteção CORS, Rate Limiting (anti-brute force), sanitização de logs, remoção de arquivos locais vulneráveis e otimização de payload (10MB). |

---

## **Indice**

1. [Introdução](#1-introducao)
2. [Visão Geral do Produto](#2-visao-geral-do-produto)
3. [Arquitetura e Tecnologias](#3-arquitetura-e-tecnologias)
4. [Requisitos Funcionais](#4-requisitos-funcionais)
5. [Regras de Negócio](#5-regras-de-negocio)
6. [Requisitos Não Funcionais](#6-requisitos-nao-funcionais)
7. [Estrutura do Projeto](#7-estrutura-do-projeto)
8. [Deploy](#8-deploy)

---

# **1. Introdução**

## **1.1 Propósito**

Este documento define os requisitos do sistema de **Controle de Estoque e Precificação**, uma aplicação web para controle de estoque, matérias-primas, fornecedores e movimentações (entradas e saidas).

---

## **1.2 Público-alvo**

* Desenvolvedores frontend e backend
* Equipe do projeto PI (UNIVESP)
* Professores avaliadores
* Pequenos negócios que necessitam controle de estoque

---

## **1.3 Escopo**

### **Inclui (V1)**

* Autenticação de usuários
* Dashboard com indicadores
* Cadastro de produtos
* Cadastro de matérias-primas
* Cadastro de fornecedores
* Controle de entradas e saidas
* Gestão de usuários

### **Fora do escopo (V1)**

* Integração com sistemas fiscais
* Controle financeiro completo
* Multiempresa (multi-tenant)
* App mobile

---

# **2. Visão Geral do Produto**

O sistema e uma plataforma web que permite:

* Controle centralizado de estoque
* Registro de movimentações (entrada e saida)
* Visualização de dados via dashboard
* Organizacao de fornecedores e insumos

Objetivo principal: substituir controles manuais (planilhas) por uma solução organizada e automatizada.

---

# **3. Arquitetura e Tecnologias**

### **Frontend**

* React.js (Core do projeto)
* Vite (Build tool e servidor de desenvolvimento)
* Tailwind CSS (Utilizado para estilização moderna e responsiva)
* Recharts (Para os gráficos financeiros e de estoque no dashboard)
* Zod (Validação rigorosa de formulários de usuários e insumos)
* Native Fetch API (Realizar requisições HTTP assíncronas - buscar recursos de rede)

### **Backend**

* Node.js
* Express.js
* JWT (Autenticação segura de rotas)
* bcryptjs (Criptografia de senhas)
* Prisma ORM (Interface de comunicação com o banco de dados)
* Nodemailer (Integração com SMTP do Gmail para recuperação de senha)
* Express Rate Limit (Proteção contra ataques de força bruta no login e recuperação de senha)
* CORS (Gerenciamento de permissões de acesso entre domínios)

### **Banco de Dados**

* PostgreSQL (Hospedado na Neon, que oferece escalabilidade e arquitetura serverless)

### **Hospedagem**

* Render
* GitHub

---

# **4. Requisitos Funcionais**

## **4.1 Autenticação**

* [x] **RF-001**: O sistema deve permitir cadastro de usuário com nome, email e senha apenas dentro da aplicação
* [x] **RF-002**: O sistema deve permitir login com email e senha
* [x] **RF-003**: O sistema deve manter sessão autenticada com token JWT
* [x] **RF-004**: O sistema deve permitir logout
* [x] **RF-032**: O sistema deve permitir que o usuário solicite recuperação de senha informando o e-mail cadastrado
* [x] **RF-033**: O sistema deve gerar um código de verificação temporário para recuperação de senha
* [x] **RF-034**: O sistema deve enviar o código de verificação para o e-mail do usuário
* [x] **RF-035**: O sistema deve permitir que o usuário informe o código recebido para validação
* [x] **RF-036**: O sistema deve validar se o código está correto e dentro do prazo de expiração
* [x] **RF-037**: O sistema deve permitir que o usuário redefina sua senha após validação do código
* [x] **RF-038**: O sistema deve invalidar o código após uso ou expiração
* [x] **RF-057**: O sistema deve utilizar o armazenamento local do navegador (LocalStorage) para persistir o token de sessão do usuário, garantindo que ele continue logado mesmo após atualizações da página (refresh).
* [x] **RF-058**: O envio dos códigos de recuperação de senha deve ser realizado por meio de uma integração com uma conta de e-mail real (Gmail).
* [x] **RF-059**: O layout do e-mail de recuperação deve ser focado em design limpo e tipográfico, garantindo a legibilidade e a exibição correta do branding ("st.solart") em qualquer dispositivo, contornando bloqueios de imagens.
* [x] **RF-062**: O sistema deve realizar logout automático (encerrar sessão) imediatamente caso o usuário delete a própria conta.
* [x] **RF-063**: O sistema deve manter proteção estrita de identidade, bloqueando qualquer redirecionamento (fallback) para contas de administrador caso o usuário logado não seja encontrado.
* [x] **RF-064**: Os campos de "Data de Cadastro" e "Último Acesso" nas listagens de usuários devem exibir formato detalhado contendo horas e minutos (HH:mm).
* [x] **RF-070**: Todas as rotas de CRUD do sistema devem ser protegidas por um Middleware (authenticateToken) que exige e valida um token JWT, rejeitando qualquer requisição não autenticada.
* [x] **RF-071**: O frontend deve utilizar um utilitário de requisição centralizado (apiFetch) para injetar automaticamente o cabeçalho de autorização (Authorization: Bearer <token>) em todas as chamadas à API.

## 4.1 UI/UX e Tabelas (Requisitos Gerais)
* [x] **RF-050**: O sistema deve exibir notificações flutuantes informando o sucesso ou erro de ações. As notificações devem desaparecer automaticamente após 3 segundos.
* [x] **RF-051**: A exclusão de qualquer registro não deve possuir um botão direto nas tabelas principais. O usuário deve primeiro acessar o pop-up de "Editar" e utilizar o botão de confirmação secundária ("Sim, Excluir") localizado no final do painel.
* [x] **RF-052**: Todas as tabelas do sistema (incluindo a de Entradas) devem utilizar um componente de paginação padronizado com botões numéricos clicáveis [ 1 ] [ 2 ] [ ... ].
* [x] **RF-060**: O sistema deve validar ativamente os dados inseridos em formulários, exigindo formatos padronizados (ex: e-mails contendo @ e domínio) e bloqueando o envio de senhas em branco.
* [x] **RF-061**: O sistema deve implementar um estado de processamento (isProcessing) que desativa visualmente e funcionalmente os botões de envio durante o carregamento de uma requisição, prevenindo cliques acidentais duplicados.

---

## **4.2 Dashboard**

* [x] **RF-005**: O sistema deve exibir indicadores gerais (estoque total, entradas, saidas)
* [x] **RF-006**: O sistema deve exibir gráficos de movimentação
* [x] **RF-007**: O sistema deve atualizar dados em tempo real ou sob demanda
* [x] **RF-039**: O sistema deve exibir a Quantidade de Itens Cadastrados (somando insumos e produtos).
* [x] **RF-040**: O sistema deve exibir indicadores específicos para "Insumos com Estoque Mínimo" e "Insumos com Estoque Zerado".

---

## **4.3 Produtos**

* [x] **RF-008**: O sistema deve permitir cadastrar produto
* [x] **RF-009**: O produto deve conter Código, Nome do Produto, Receita (Cálculo), Quantidade Cadastrada, Custo Produto, Preco Venda
* [x] **RF-010**: O sistema deve listar produtos
* [x] **RF-011**: O sistema deve permitir editar produto
* [x] **RF-012**: O sistema deve permitir excluir produto
* [x] **RF-041**: Na tela de gestão, a coluna "Receita (Cálculo)" deve abrir um pop-up detalhando o cálculo originado de como é feito a precificação do produto.
* [x] **RF-042**: O botão "Novo Registro" deve abrir um pop-up para a adição de novos produtos
* [x] **RF-043**: Dentro do pop-up de "Novo Registro", o sistema deve exibir uma lista de insumos pré-cadastrados para seleção
* [x] **RF-053**: O pop-up de "Receita (Cálculo)" deve carregar os dados em tempo real com base nos insumos selecionados pelo usuário, exibindo o "Custo de Produção Total" já recalculado automaticamente.
* [x] **RF-054**: O sistema deve exibir as informações de estoque e consumo sempre acompanhadas de suas respectivas unidades textuais e suporte a visibilidade fracionária com decimais em todas as colunas de estoque e dropdowns.
* [x] **RF-065**: O formulário de cadastro de produtos deve incluir o campo de "Comissão (%)".

---

## **4.4 Materias-Primas**

* [x] **RF-013**: O sistema deve permitir cadastrar e editar matéria-prima através de pop-ups, selecionando o fornecedor e inserindo: nome, unidade de medida, custo unitário e estoque inicial
* [x] **RF-014**: O sistema deve listar materias-primas
* [x] **RF-015**: O sistema deve permitir editar matéria-prima
* [x] **RF-016**: O sistema deve permitir excluir matéria-prima
* [x] **RF-044**: O sistema deve exibir a coluna "Fornecedor" na tabela de matérias-primas
* [x] **RF-045**: O sistema deve destacar visualmente, na tela de matérias-primas, os insumos que estiverem com estoque zerado e com baixo estoque.
* [x] **RF-049**: Os campos da tabela devem ser: Código, Matéria-Prima, Fornecedor, Unidade, Custo, Estoque, Foto

---

## **4.5 Fornecedores**

* [x] **RF-017**: O cadastro e a edição de fornecedores devem ser realizados via pop-up, contendo os seguintes campos: Código, Razão, Nome, CNPJ, Cidade, Estado e Contato
* [x] **RF-018**: O fornecedor deve conter nome, contato e informações adicionais
* [x] **RF-019**: O sistema deve listar fornecedores
* [x] **RF-021**: O sistema deve permitir excluir fornecedor

---

## **4.6 Entradas de Estoque**

* [x] **RF-022**: O sistema deve registrar a inserção de matérias-primas por fornecedor
* [x] **RF-023**: A entrada deve conter quantidade, data e fornecedor
* [x] **RF-024**: O sistema deve atualizar automaticamente o estoque
* [x] **RF-046**: O sistema deve permitir a edição de registros de entrada através de um pop-up que exibe a lista de insumos e o nome do fornecedor
* [x] **RF-055**: Ao registrar uma entrada do tipo "Produção Própria" para um produto, o sistema deve buscar a receita do modelo, multiplicar os insumos pela quantidade produzida e deduzir automaticamente estes insumos do inventário global (Efeito Cascata).

---

## **4.7 Saidas de Estoque**

* [x] **RF-025**: O sistema deve registrar saida de produto
* [x] **RF-026**: A saida deve conter quantidade e data
* [x] **RF-027**: O sistema deve reduzir automaticamente o estoque  
* [x] **RF-047**: A tela de saídas deve conter uma tabela com as seguintes colunas: Ordem, Cliente, Produto, Quantidade, Total e Data
* [x] **RF-056**: No formulário de "Baixa de Insumo", o sistema deve fornecer uma seleção de "Unidade Utilizada", permitindo saídas avulsas modulares e declaração de gasto fracionário exato (ex: em ml) de um estoque maior.
* [x] **RF-066**: O registro de Saídas deve incluir o campo "Desconto (R$)", com o sistema calculando e abatendo o Valor Total de forma automática.
* [x] **RF-067**: O sistema deve permitir a edição de registros de Saídas (tanto de produtos quanto de insumos) diretamente na tabela, atualizando o saldo do estoque em tempo real.

---
Requisito Geral (Tabelas)
* [x] **RF-048**: Todas as tabelas do sistema devem possuir botões de ação para "Editar" o respectivo registro.
---

## **4.8 Usuarios**

* [x] **RF-028**: O sistema deve permitir cadastro de usuários
* [x] **RF-029**: O sistema deve listar usuários
* [x] **RF-030**: O sistema deve permitir editar usuários
* [x] **RF-031**: O sistema deve permitir excluir usuários

## **4.9 Gestão de Imagens**

* [x] **RF-068**: O servidor deve aceitar uploads de arquivos (payload) de até 10MB (otimizado para proteger o servidor contra ataques de esgotamento de memória, garantindo suporte adequado para fotos de perfil).
* [x] **RF-069**: As tabelas devem apresentar um ícone de "seta diagonal" que, ao ser clicado, expande a foto.

---

# **5. Regras de Negocio**

* [x] **RN-001**: Não permitir saida maior que o estoque disponivel
* [x] **RN-002**: Todo produto deve ter quantidade inicial >= 0
* [x] **RN-003**: Entrada sempre aumenta estoque
* [x] **RN-004**: Saida sempre reduz estoque
* [x] **RN-005**: Campos obrigatórios devem ser validados antes do envio
* [x] **RN-006**: Usuários devem estar autenticados para acessar o sistema
* [x] **RN-007**: Senhas devem ser armazenadas criptografadas
* [x] **RN-008**: Exclusões devem ser físicas. Quando exclui um produto ou insumo, ele é removido permanentemente do banco de dados.
* [x] **RN-009**: O código de recuperação de senha deve ser único e aleatório com no mínimo e máximo de 6 digitos numéricos.
* [x] **RN-010**: O código de recuperação deve expirar em até 10 minutos após sua geração
* [x] **RN-011**: O código não pode ser reutilizado após validação
* [x] **RN-012**: A redefinição de senha só deve ser permitida após validação correta do código
* [x] **RN-013**: A nova senha deve ser armazenada de forma criptografada (bcrypt)
* [x] **RN-014**: No pop-up de "Novo Registro" (produtos), a quantidade inserida para um insumo não pode ser superior à quantidade disponível no sistema (tela de entradas).
* [x] **RN-015**: Caso o usuário tente inserir uma quantidade maior do que a disponível no estoque, o sistema deve bloquear a ação e exibir uma mensagem de erro ("Quantidade insuficiente de insumo").
* [x] **RN-016**: O sistema não deve permitir a inserção de quantidades negativas em nenhum campo de insumo ou produto.
* [x] **RN-017**: A tela de matérias-primas deve exibir notificações claras em caso de erro de usuário ou de quantidade excedida durante as movimentações e edições.
* [x] **RN-018**: O saldo de estoque não pode ser editado manualmente. Qualquer alteração deve ser registrada como uma Entrada ou Saída para garantir o histórico.
* [x] **RN-019**: O sistema faz a conversão automática de unidades (ex: de Litro para ml, Quilo para grama), permitindo o abatimento exato de frações nas receitas sem gerar erros.
* [x] **RN-020**: Trava rigorosa do sistema. Nenhuma saída ou produção será permitida se o resultado deixar o estoque menor que zero.
* [x] **RN-021**: O custo de produção de um produto deve ser calculado fracionando o valor do insumo proporcionalmente à unidade utilizada (ex: calculando o gasto em ml de um insumo comprado em Litro).
* [x] **RN-022**: A sugestão de preço de venda gerada pelo sistema deve obrigatoriamente considerar o cálculo da Margem de Lucro somado às taxas cadastradas de Comissão.
* [x] **RN-023**: Ao editar ou excluir um registro de Entrada ou Saída, o sistema deve realizar a compensação automática, devolvendo ou retirando a quantidade exata do estoque global para evitar furos no inventário.
* [x] **RN-024**: O sistema deve aplicar conversão proporcional automática em todas as operações matemáticas que envolvam mudanças de unidade de massa e volume (L ↔ ml e kg ↔ g).
* [x] **RN-025**: É estritamente proibida a verificação de senhas em texto plano (fallback). Todas as validações de login e redefinição devem utilizar exclusivamente criptografia via bcrypt.
* [x] **RN-026**: O sistema não deve possuir chaves de segurança (como o JWT_SECRET) fixas no código-fonte (hardcoded). Elas devem ser injetadas obrigatoriamente através do arquivo .env.

---

# **6. Requisitos Nao Funcionais**

* [x] **RNF-001 (Desempenho)**: Listagens devem carregar em ate 2 segundos
* [x] **RNF-002 (Usabilidade)**: Interface deve ser intuitiva e responsiva
* [x] **RNF-003 (Seguranca)**: Dados devem trafegar via HTTPS
* [x] **RNF-004 (Seguranca)**: Senhas devem usar hash (bcrypt)
* [x] **RNF-005 (Disponibilidade)**: Sistema deve ter alta disponibilidade
* [x] **RNF-006 (Escalabilidade)**: Sistema deve suportar crescimento de dados
* [x] **RNF-007 (Manutenibilidade)**: Código deve ser modular
* [x] **RNF-008 (Segurança)**: O envio de e-mails deve ser feito de forma segura utilizando autenticação SMTP
* [x] **RNF-009 (Segurança)**: O sistema deve limitar tentativas de validação de código para evitar ataques de força bruta
* [x] **RNF-010 (Semântica e UI limpa)**: Exigência do uso de tags HTML corretas (como <button>) em elementos clicáveis para evitar bugs visuais, como bordas soltas ou focos residuais na interface.
* [x] **RNF-011** (Segurança de Credenciais): Informações sensíveis da aplicação (como senhas de e-mail, tokens e URLs de banco de dados) devem ser isoladas estritamente em arquivos de variáveis de ambiente (.env) para testes.
* [x] **RNF-012** (Segurança de Repositório): O sistema de versionamento (GitHub) deve ser configurado com restrições (via .gitignore) para bloquear o envio e a exposição pública dos arquivos de configuração local (.env).
* [x] **RNF-013** (Homologação e Testes): O ambiente de desenvolvimento deve manter uma base de dados local mapeada (ex: usuários de teste) para viabilizar a validação constante do fluxo de login e recuperação real antes de enviar para a produção.
* [x] **RNF-014** (Banco de Dados e Infraestrutura): O sistema deve utilizar um banco de dados relacional de alta performance (PostgreSQL hospedado no Neon) para garantir integridade, disponibilidade em nuvem e proteção dos dados contra perdas locais.
* [x] **RNF-015** (Persistência ORM): Todas as transações com o banco de dados devem ser gerenciadas exclusivamente via Prisma ORM para garantir segurança e integridade referencial nas operações em cascata.
* [x] **RNF-016** (Armazenamento de Imagens): O banco de dados deve ser configurado para suportar strings de grande volume (tipo Text) a fim de salvar imagens convertidas em formato Base64 sem corrompimento.
* [x] **RNF-017** (Clean Code e Manutenibilidade): O código enviado para o ambiente de produção deve estar limpo, com rotinas de faxina que removam comentários de desenvolvimento e logs de debug do console, visando a otimização de performance.
* [x] **RNF-018** (Segurança - CORS): O servidor deve restringir requisições (Cross-Origin Resource Sharing), aceitando conexões apenas de ambientes autorizados (ex: localhost:5173, 3000, 3005) ou da URL de produção definida nas variáveis de ambiente.
* [x] **RNF-019** (Segurança - Rate Limiting Global): O sistema deve aplicar um limitador de tráfego global (ex: 200 requisições a cada 15 minutos por IP) para prevenir sobrecarga e ataques de negação de serviço.
* [x] **RNF-020** (Segurança - Anti-Brute Force): As rotas sensíveis de autenticação (Login e Recuperação de Senha) devem possuir um limite rigoroso de tentativas (ex: 10 tentativas por hora) para impedir o descobrimento de senhas e spam de e-mails.
* [x] **RNF-021** (Privacidade e Sanitização de Logs): O sistema de logs do servidor deve omitir dados sensíveis em caso de erros, mascarando senhas e ocultando cargas pesadas (como imagens base64) para evitar vazamento de dados.
* [x] **RNF-022** (Segurança de Repositório e Arquivos): Arquivos de dados locais vulneráveis (como db.json) e logs de depuração (error_debug.json) devem ser permanentemente deletados do projeto e bloqueados via .gitignore para impedir exposição pública no GitHub.

---

# **7. Estrutura do Projeto**

```text
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

---

# **8. Deploy**

### **Ambiente de desenvolvimento**

* Frontend & Backend: Render
* Banco de Dados: Neon (PostgreSQL)

### **Ambiente de produção**

* Frontend: Vercel
* Backend: Render
* Banco: Supabase

---

### **Checklist de Deploy**

* [x] Configurar variáveis de ambiente
* [x] Subir banco de dados
* [x] Rodar migrations
* [x] Testar login
* [x] Testar CRUD completo
* [x] Testar entradas e saidas
* [x] Testar dashboard
