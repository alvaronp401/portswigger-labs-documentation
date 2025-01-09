# Lab 1 - Retrieving Hidden Data (SQL Injection)

## Descrição do Laboratório

Este laboratório demonstra como explorar uma vulnerabilidade de **SQL Injection** em uma aplicação web. O objetivo é manipular a consulta SQL para revelar dados que, normalmente, estariam ocultos para o usuário final. Em particular, estamos interessados em revelar produtos não lançados através de uma falha de injeção SQL.

### Contexto da Aplicação

A aplicação permite filtrar produtos por categoria usando a URL:

```http
GET /filter?category=Gifts HTTP/1.1
Host: <host_do_lab>
Cookie: session=<session_id>
A consulta SQL original, executada no banco de dados para filtrar os produtos da categoria "Gifts", era semelhante a:

sql
Copiar código
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;
O desafio neste laboratório é explorar uma vulnerabilidade de SQL Injection para revelar produtos que não foram lançados, manipulando a consulta SQL da aplicação.

Passos Realizados
Passo 1: Interceptando a Requisição HTTP
Utilizando o Burp Suite, a requisição HTTP original foi interceptada e modificada. A requisição original parecia com:

http
Copiar código
GET /filter?category=Gifts HTTP/1.1
Host: <host_do_lab>
Cookie: session=<session_id>
Para poder manipular a consulta SQL, enviamos a requisição para a aba Repeater do Burp Suite e alteramos o parâmetro category com um payload de SQL Injection.

Passo 2: Realizando a Injeção SQL
Alteramos o valor do parâmetro category para um payload de SQL Injection:

sql
Copiar código
'+OR+1=1-- 
A requisição modificada foi:

http
Copiar código
GET /filter?category='+OR+1=1-- HTTP/1.1
Host: <host_do_lab>
Cookie: session=<session_id>
Explicação do Payload:
': Fecha a string da consulta SQL.
OR 1=1: Esta parte adiciona uma condição que sempre será verdadeira, alterando a lógica da consulta SQL para ignorar o filtro original.
--: Comentário, que descarta o restante da consulta, incluindo o filtro AND released = 1.
Passo 3: Verificando os Resultados
Após enviar a requisição modificada, conseguimos obter todos os produtos, incluindo os não lançados. Isso ocorreu porque a condição 1=1 sempre retornou verdadeiro, fazendo com que a consulta ignorasse a restrição released = 1.

Passo 4: Analisando a Vulnerabilidade
Após a injeção SQL, a consulta SQL foi modificada da seguinte forma:

sql
Copiar código
SELECT * FROM products WHERE category = '' OR 1=1 -- ' AND released = 1;
OR 1=1: A condição 1=1 sempre retorna verdadeiro, permitindo que todos os produtos sejam retornados.
--: O operador de comentário ignora a segunda parte da consulta, incluindo a verificação do filtro released = 1.
Passo 5: Medidas de Mitigação
Para proteger a aplicação contra este tipo de vulnerabilidade, é necessário realizar as seguintes ações:

Validação de Entrada: Validar todos os dados de entrada do usuário antes de serem usados em consultas SQL.
Uso de Prepared Statements: Utilizar instruções preparadas (Prepared Statements) para evitar que a entrada do usuário seja tratada diretamente na consulta SQL.
Escapar Dados: Quando for necessário usar entradas de usuários em consultas, é essencial escapar adequadamente os dados para prevenir injeções SQL.
Ferramentas Utilizadas
Burp Suite: Ferramenta de interceptação e análise de requisições HTTP/HTTPS.
SQL Injection: Técnica usada para injetar código SQL malicioso em uma aplicação e manipular o banco de dados de forma indesejada.
Resultados Obtidos
Antes da Injeção SQL
Antes da injeção, a aplicação filtrava corretamente os produtos por categoria e apenas exibia os produtos lançados.

Após a Injeção SQL
Após a injeção, a aplicação passou a mostrar todos os produtos, incluindo aqueles que estavam ocultos (não lançados), violando a lógica de segurança da aplicação.

Imagens
Aqui estão algumas imagens para ilustrar o processo de exploração:

Imagem 1: Requisição Interceptada no Burp Suite

Imagem 2: Resultados Após a Injeção SQL
