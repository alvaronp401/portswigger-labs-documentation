# Lab 1: Retrieving Hidden Data - SQL Injection

## Introdução

Neste laboratório, exploramos uma vulnerabilidade de **injeção SQL** presente na aplicação, que permite a recuperação de dados ocultos. A vulnerabilidade ocorre na cláusula `WHERE` de uma consulta SQL, especificamente no filtro de categoria de produtos.

## Descrição da Vulnerabilidade

A aplicação usa a seguinte consulta SQL para filtrar os produtos pela categoria:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1

## Passo a Passo

### 1. Interceptando a Requisição

Para iniciar o ataque, utilizamos o **Burp Suite** para interceptar a requisição ao clicar em uma categoria de produto na aplicação. A requisição é então enviada para o **Repeater**.

### 2. Modificando a Consulta SQL

No **Burp Suite**, modificamos a consulta SQL para realizar a injeção. A string `'` fecha a string original do parâmetro `category`, e o operador `OR 1=1` força a condição a ser sempre verdadeira, permitindo que o banco de dados retorne todos os produtos, inclusive os não lançados. Além disso, usamos `--` para comentar o restante da consulta SQL, ignorando o filtro de lançamento.

Consulta modificada:

```sql
category = 'Gifts' OR 1=1 --

## Análise da Vulnerabilidade```

A **injeção SQL** ocorre quando o atacante consegue inserir comandos SQL maliciosos em campos de entrada que são utilizados diretamente em consultas SQL. Nesse laboratório, a vulnerabilidade estava na **consulta SQL** que não validava corretamente o valor da categoria do produto, permitindo que um atacante manipulasse a consulta para retornar dados ocultos, como produtos não lançados.

O uso da expressão `OR 1=1` foi uma técnica simples e eficiente para contornar o filtro `released = 1`, que visava apenas retornar produtos que foram lançados. O comando `--` é um comentário SQL, que ignora qualquer parte da consulta que venha após ele, tornando o filtro `released = 1` ineficaz.

### Impacto da Vulnerabilidade

Com essa vulnerabilidade, um atacante poderia:

- **Expor dados sensíveis**: A consulta maliciosa retorna todos os dados, incluindo informações que não deveriam ser acessíveis, como produtos não lançados ou outras informações privadas.
- **Alterar o comportamento da aplicação**: Um atacante pode modificar os resultados da aplicação, impactando a experiência do usuário e a integridade dos dados.
- **Acesso não autorizado a dados**: Dependendo da configuração do banco de dados e da aplicação, o atacante poderia ter acesso a dados confidenciais.


