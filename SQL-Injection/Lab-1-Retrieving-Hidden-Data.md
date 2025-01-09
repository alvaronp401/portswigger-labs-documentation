Lab 01: SQL Injection Básica na Cláusula WHERE

Descrição do Lab
O objetivo deste laboratório é explorar uma vulnerabilidade de SQL Injection presente na cláusula WHERE da aplicação. A aplicação permite que os usuários filtrem produtos por categorias, mas não valida corretamente os parâmetros fornecidos pelo cliente, permitindo manipulações maliciosas na consulta SQL.

A consulta SQL da aplicação, ao selecionar uma categoria como "Gifts", seria algo como:

sql
Copiar código
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
Nosso desafio é explorar essa vulnerabilidade para forçar a exibição de produtos não lançados, que estão ocultos por padrão.

## Passos Realizados
1. Identificação da vulnerabilidade.
2. Teste com payloads básicos.
3. Exploração bem-sucedida.

## Resultado
(Descreva o que foi obtido, incluindo capturas de tela ou payloads utilizados.)

## Medidas de Mitigação
- Validação das entradas do usuário.
- Consultas parametrizadas no banco de dados.
