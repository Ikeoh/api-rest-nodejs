# API REST de Transações Financeiras

Esta é uma API REST desenvolvida em Node.js com Fastify e Knex.js para gerenciar transações financeiras. A API permite criar, listar, visualizar e resumir transações, garantindo que cada usuário tenha acesso apenas às suas próprias informações.

## Funcionalidades

### Requisitos Funcionais (RF)

- **Criar nova transação:** O usuário pode registrar uma nova transação informando título, valor e tipo (crédito ou débito).
- **Obter resumo da conta:** O usuário pode visualizar o saldo total de sua conta, consolidando todas as transações.
- **Listar todas as transações:** O usuário tem acesso a uma lista completa de todas as suas transações já realizadas.
- **Visualizar transação única:** É possível consultar os detalhes de uma transação específica por meio de seu ID.

### Regras de Negócio (RN)

- **Tipos de Transação:**
  - **Crédito:** Adiciona valor ao saldo total.
  - **Débito:** Subtrai valor do saldo total.
- **Identificação de Usuário:** A API utiliza cookies (`sessionId`) para identificar e autenticar o usuário entre as requisições, garantindo a segurança e a privacidade dos dados.
- **Privacidade de Dados:** O usuário só pode acessar e visualizar as transações que ele mesmo criou.

## Tecnologias Utilizadas

- **Node.js:** Ambiente de execução JavaScript no servidor.
- **Fastify:** Framework web de alta performance para Node.js.
- **Knex.js:** SQL query builder para Node.js, compatível com diversos bancos de dados.
- **TypeScript:** Superset do JavaScript que adiciona tipagem estática.
- **Zod:** Biblioteca para validação de esquemas e tipos de dados.
- **Vitest:** Framework de testes para projetos em Vite, mas amplamente utilizado com TypeScript.
- **ESLint:** Ferramenta para linting de código, garantindo a padronização e a qualidade do código.
- **tsx:** Executa arquivos TypeScript e ESNext em Node.js de forma nativa e eficiente.

## Estrutura do Banco de Dados

A tabela `transactions` armazena os dados das transações e possui a seguinte estrutura:

- `id` (UUID, Chave Primária): Identificador único da transação.
- `session_id` (UUID, Index): Identificador da sessão do usuário para agrupar suas transações.
- `title` (String): Descrição da transação.
- `amount` (Decimal): Valor da transação. Valores de débito são armazenados como negativos.
- `created_at` (Timestamp): Data e hora de criação da transação.

## Endpoints da API

O prefixo base para todos os endpoints de transações é `/transactions`.

### `POST /transactions`

Cria uma nova transação.

**Corpo da Requisição:**

```json
{
  "title": "Salário",
  "amount": 5000,
  "type": "credit"
}
```

- `title` (string, obrigatório): Título da transação.
- `amount` (number, obrigatório): Valor da transação.
- `type` (enum, obrigatório): Tipo da transação (`credit` ou `debit`).

**Resposta de Sucesso:**

- **Código:** `201 Created`

### `GET /transactions`

Lista todas as transações do usuário autenticado.

**Resposta de Sucesso:**

- **Código:** `200 OK`
- **Corpo:**
  ```json
  {
    "transactions": [
      {
        "id": "uuid-da-transacao",
        "title": "Salário",
        "amount": 5000,
        "created_at": "timestamp",
        "session_id": "uuid-da-sessao"
      }
    ]
  }
  ```

### `GET /transactions/:id`

Retorna os detalhes de uma transação específica.

**Parâmetros da URL:**

- `id` (UUID, obrigatório): ID da transação.

**Resposta de Sucesso:**

- **Código:** `200 OK`
- **Corpo:**
  ```json
  {
    "transaction": {
      "id": "uuid-da-transacao",
      "title": "Salário",
      "amount": 5000,
      "created_at": "timestamp",
      "session_id": "uuid-da-sessao"
    }
  }
  ```

### `GET /transactions/summary`

Retorna o resumo financeiro do usuário.

**Resposta de Sucesso:**

- **Código:** `200 OK`
- **Corpo:**
  ```json
  {
    "summary": {
      "amount": 3500.50
    }
  }
  ```

## Como Executar o Projeto

1. **Clone o repositório:**
   ```bash
   git clone <url-do-repositorio>
   cd <nome-do-diretorio>
   ```

2. **Instale as dependências:**
   ```bash
   npm install
   ```

3. **Configure as variáveis de ambiente:**
   - Renomeie o arquivo `.env.example` para `.env`.
   - Preencha as variáveis de ambiente necessárias, como `DATABASE_URL` e `PORT`.

4. **Execute as migrações do banco de dados:**
   ```bash
   npm run knex migrate:latest
   ```

5. **Inicie o servidor de desenvolvimento:**
   ```bash
   npm run dev
   ```

   O servidor estará disponível em `http://localhost:3333` (ou na porta configurada).

## Como Executar os Testes

Para garantir a qualidade e o funcionamento correto da API, execute os testes automatizados:

```bash
npm test
```
