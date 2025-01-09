# SQL Injection Lab - Exploração de Vulnerabilidade

## Descrição do Lab

Neste laboratório, exploramos uma vulnerabilidade de **SQL Injection** em uma aplicação web que utiliza uma consulta SQL para filtrar produtos por categoria. O objetivo é manipular essa consulta para exibir produtos que, por padrão, estão ocultos (não lançados).

### Consulta Original

A consulta SQL usada na aplicação para filtrar os produtos por categoria e verificar se foram lançados era semelhante a:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;
