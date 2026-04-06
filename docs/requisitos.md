# **Documento de Requisitos: Controle de Estoque e Precificação**

Versao: 1.0
Data: 28/03/2026

---

## **Historico de Revisoes**

| Versão | Data       | Autor                       | Descrição                                                                      |
| ------ | ---------- | --------------------------- | ------------------------------------------------------------------------------ |
| 1.0    | 28/03/2026 | Equipe PI UNIVESP | Documento inicial de requisitos para sistema de controle de estoque e produção |
| 1.1    | 05/04/2026 | Shayare | Adição da funcionalidade de recuperação de senha com envio de código por e-mail |
| 1.2    | 06/04/2026 | Shayare | Adição de pop-ups de cadastro/edição, regras de validação de insumos, detalhamento de campos em tabelas (saídas, fornecedores, matérias-primas) e novos indicadores de dashboard |

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

* React.js
* Vite
* Tailwind CSS
* Axios
* React Router
* Recharts

### **Backend**

* Node.js
* Express.js
* JWT (autenticacao)
* bcrypt
* Prisma ORM
* Nodemailer (envio de e-mails)
* Crypto (geração de códigos aleatórios)

### **Banco de Dados**

* PostgreSQL

### **Hospedagem**

* Vercel (frontend)
* Render (backend)
* Supabase (banco)

---

# **4. Requisitos Funcionais**

## **4.1 Autenticação**

* [ ] **RF-001**: O sistema deve permitir cadastro de usuário com nome, email e senha
* [ ] **RF-002**: O sistema deve permitir login com email e senha
* [ ] **RF-003**: O sistema deve manter sessão autenticada com token JWT
* [ ] **RF-004**: O sistema deve permitir logout
* [ ] **RF-032**: O sistema deve permitir que o usuário solicite recuperação de senha informando o e-mail cadastrado
* [ ] **RF-033**: O sistema deve gerar um código de verificação temporário para recuperação de senha
* [ ] **RF-034**: O sistema deve enviar o código de verificação para o e-mail do usuário
* [ ] **RF-035**: O sistema deve permitir que o usuário informe o código recebido para validação
* [ ] **RF-036**: O sistema deve validar se o código está correto e dentro do prazo de expiração
* [ ] **RF-037**: O sistema deve permitir que o usuário redefina sua senha após validação do código
* [ ] **RF-038**: O sistema deve invalidar o código após uso ou expiração

---

## **4.2 Dashboard**

* [ ] **RF-005**: O sistema deve exibir indicadores gerais (estoque total, entradas, saidas)
* [ ] **RF-006**: O sistema deve exibir gráficos de movimentação
* [ ] **RF-007**: O sistema deve atualizar dados em tempo real ou sob demanda
* [ ] **RF-039**: O sistema deve exibir a Quantidade de Itens Cadastrados (somando insumos e produtos).
* [ ] **RF-040**: O sistema deve exibir indicadores específicos para "Insumos com Estoque Mínimo" e "Insumos com Estoque Zerado".

---

## **4.3 Produtos**

* [ ] **RF-008**: O sistema deve permitir cadastrar produto
* [ ] **RF-009**: O produto deve conter nome, descrição, preço e quantidade
* [ ] **RF-010**: O sistema deve listar produtos
* [ ] **RF-011**: O sistema deve permitir editar produto
* [ ] **RF-012**: O sistema deve permitir excluir produto
* [ ] **RF-041**: Na tela de gestão, a coluna "Receita (Cálculo)" deve abrir um pop-up detalhando o cálculo originado na tela de "Entrada"
* [ ] **RF-042**: O botão "Novo Registro" deve abrir um pop-up para a adição de novos produtos
* [ ] **RF-043**: Dentro do pop-up de "Novo Registro", o sistema deve exibir uma lista de insumos pré-cadastrados para seleção

---

## **4.4 Materias-Primas**

* [ ] **RF-013**: O sistema deve permitir cadastrar e editar matéria-prima através de pop-ups, selecionando o fornecedor e inserindo: nome, unidade de medida, custo unitário e estoque inicial
* [ ] **RF-014**: O sistema deve listar materias-primas
* [ ] **RF-015**: O sistema deve permitir editar matéria-prima
* [ ] **RF-016**: O sistema deve permitir excluir matéria-prima
* [ ] **RF-044**: O sistema deve exibir a coluna "Fornecedor" na tabela de matérias-primas
* [ ] **RF-045**: O sistema deve destacar visualmente, na tela de matérias-primas, os insumos que estiverem com estoque zerado e com baixo estoque.

---

## **4.5 Fornecedores**

* [ ] **RF-017**: O cadastro e a edição de fornecedores devem ser realizados via pop-up, contendo os seguintes campos: Razão, Nome, CNPJ, Cidade, Estado e Contato
* [ ] **RF-018**: O fornecedor deve conter nome, contato e informações adicionais
* [ ] **RF-019**: O sistema deve listar fornecedores
* [ ] **RF-021**: O sistema deve permitir excluir fornecedor

---

## **4.6 Entradas de Estoque**

* [ ] **RF-022**: O sistema deve registrar a inserção de matérias-primas por fornecedor
* [ ] **RF-023**: A entrada deve conter quantidade, data e fornecedor
* [ ] **RF-024**: O sistema deve atualizar automaticamente o estoque
* [ ] **RF-046**: O sistema deve permitir a edição de registros de entrada através de um pop-up que exibe a lista de insumos e o nome do fornecedor

---

## **4.7 Saidas de Estoque**

* [ ] **RF-025**: O sistema deve registrar saida de produto
* [ ] **RF-026**: A saida deve conter quantidade e data
* [ ] **RF-027**: O sistema deve reduzir automaticamente o estoque
* [ ] **RF-047**: A tela de saídas deve conter uma tabela com as seguintes colunas: Ordem, Cliente, Produto, Quantidade, Total e Data

---
Requisito Geral (Tabelas)
* [ ] **RF-048**: Todas as tabelas do sistema devem possuir botões de ação para "Editar" o respectivo registro.
---

## **4.8 Usuarios**

* [ ] **RF-028**: O sistema deve permitir cadastro de usuários
* [ ] **RF-029**: O sistema deve listar usuários
* [ ] **RF-030**: O sistema deve permitir editar usuários
* [ ] **RF-031**: O sistema deve permitir excluir usuários

---

# **5. Regras de Negocio**

* [ ] **RN-001**: Não permitir saida maior que o estoque disponivel
* [ ] **RN-002**: Todo produto deve ter quantidade inicial >= 0
* [ ] **RN-003**: Entrada sempre aumenta estoque
* [ ] **RN-004**: Saida sempre reduz estoque
* [ ] **RN-005**: Campos obrigatórios devem ser validados antes do envio
* [ ] **RN-006**: Usuários devem estar autenticados para acessar o sistema
* [ ] **RN-007**: Senhas devem ser armazenadas criptografadas
* [ ] **RN-008**: Exclusões podem ser lógicas (soft delete)
* [ ] **RN-009**: O código de recuperação de senha deve ser único e aleatório com no mínimo e máximo de 6 digitos.
* [ ] **RN-010**: O código de recuperação deve expirar em até 10 minutos após sua geração
* [ ] **RN-011**: O código não pode ser reutilizado após validação
* [ ] **RN-012**: A redefinição de senha só deve ser permitida após validação correta do código
* [ ] **RN-013**: A nova senha deve ser armazenada de forma criptografada (bcrypt)
* [ ] **RN-014**: No pop-up de "Novo Registro" (produtos), a quantidade inserida para um insumo não pode ser superior à quantidade disponível no sistema (tela de entradas).
* [ ] **RN-015**: Caso o usuário tente inserir uma quantidade maior do que a disponível no estoque, o sistema deve bloquear a ação e exibir uma mensagem de erro ("Quantidade insuficiente de insumo").
* [ ] **RN-016**: O sistema não deve permitir a inserção de quantidades negativas em nenhum campo de insumo ou produto.
* [ ] **RN-017**: A tela de matérias-primas deve exibir notificações claras em caso de erro de usuário ou de quantidade excedida durante as movimentações e edições.

---

# **6. Requisitos Nao Funcionais**

* [ ] **RNF-001 (Desempenho)**: Listagens devem carregar em ate 2 segundos
* [ ] **RNF-002 (Usabilidade)**: Interface deve ser intuitiva e responsiva
* [ ] **RNF-003 (Seguranca)**: Dados devem trafegar via HTTPS
* [ ] **RNF-004 (Seguranca)**: Senhas devem usar hash (bcrypt)
* [ ] **RNF-005 (Disponibilidade)**: Sistema deve ter alta disponibilidade
* [ ] **RNF-006 (Escalabilidade)**: Sistema deve suportar crescimento de dados
* [ ] **RNF-007 (Manutenibilidade)**: Código deve ser modular
* [ ] **RNF-008 (Segurança)**: O envio de e-mails deve ser feito de forma segura utilizando autenticação SMTP
* [ ] **RNF-009 (Segurança)**: O sistema deve limitar tentativas de validação de código para evitar ataques de força bruta

---

# **7. Estrutura do Projeto**

```text
A definir
```

---

# **8. Deploy**

### **Ambiente de desenvolvimento**

* Localhost
* Banco Neon

### **Ambiente de produção**

* Frontend: Vercel
* Backend: Render
* Banco: Supabase

---

### **Checklist de Deploy**

* [ ] Configurar variáveis de ambiente
* [ ] Subir banco de dados
* [ ] Rodar migrations
* [ ] Testar login
* [ ] Testar CRUD completo
* [ ] Testar entradas e saidas
* [ ] Testar dashboard
