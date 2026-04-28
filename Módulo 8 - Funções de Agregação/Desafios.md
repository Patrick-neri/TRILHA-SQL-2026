# 🚀 Desafios SQL - Módulo 8 - Funções de Agregação
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Funções%20de%20Agregação-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto

Este conjunto de desafios aborda o uso de funções de agregação em SQL, aplicadas na geração de métricas e análises essenciais para tomada de decisão baseada em dados.

💡 Nesses desafios trabalhei com:

- COUNT para contagem de registros
- SUM para cálculos de totais
- AVG para médias de valores
- MIN e MAX para identificação de extremos
- DISTINCT para análise de diversidade de dados
- GROUP BY para agrupamento de informações
- Construção de relatórios analíticos com múltiplas métricas

## 🧠 Habilidades Desenvolvidas

✔️ Escrita de queries SQL com funções de agregação  
✔️ Análise de métricas e indicadores (KPIs)  
✔️ Construção de relatórios estratégicos   
✔️ Pensamento analítico  
✔️ Construção de relatórios  


---


## 📌 Aula 33

🔹 Desafio 1  
   - Contar quantos produtos existem no banco
```sql
SELECT COUNT(p.produto_id ) FROM produtos p 
```
🔹 Desafio 2
   - Contar quantos pedidos foram feitos em 2025
```sql
SELECT 
	COUNT(data_pedido)
FROM 
	pedidos 
WHERE EXTRACT(YEAR FROM data_pedido) = 2025
```

## 📌 Aula 34

🔹 Desafio 1
   - Contar quantas marcas diferentes de produtos existem
```sql
SELECT * FROM produtos LIMIT 2

SELECT COUNT(DISTINCT marca) FROM produtos 
```
🔹 Desafio 2
   - Contar em quantas cidades diferentes temos clientes
```sql
SELECT * FROM clientes LIMIT 5

SELECT COUNT(DISTINCT cidade) FROM clientes
```

## 📌 Aula 35

🔹 Desafio 1
   - Calcular o valor total de todos os pedidos
```sql
SELECT * FROM pedidos LIMIT 5

SELECT ROUND(SUM(valor_total),2) AS "Valor total dos pedidos" FROM pedidos
```
🔹 Desafio 2
   - Calcular o estoque total de todos os produtos
```sql
SELECT SUM(estoque) AS "Estoque Total" FROM produtos

SELECT
	DISTINCT(marca), 
	SUM(estoque) AS "Estoque Total" 
FROM 
	produtos
GROUP BY 
	marca
```

## 📌 Aula 36

🔹 Desafio 1
  - Calcular o preço médio dos produtos
```sql
SELECT
	ROUND(AVG(preco),2) AS "Preço médio dos produtos" 
FROM 
	produtos
```
🔹 Desafio 2
   - Calcular o valor médio dos pedidos
```sql
SELECT 
	ROUND(AVG(valor_total), 2) AS "Valor médio dos pedidos"
FROM
	pedidos

SELECT 
	ROUND(AVG(preco), 2) AS "Valor médio do produto"
FROM
	produtos
WHERE
	marca = 'Samsung'
```

## 📌 Aula 37

🔹 Desafio 1
   - Encontre o menor preço dos produto da categoria Automotivo
```sql
SELECT * FROM categorias
	
SELECT 
	MIN(preco) AS "Menor preço do Automotivo"
FROM 
	produtos
WHERE 
	categoria_id = '6'
```
🔹 Desafio 2
   - Identifique o menor desconto aplicado em pedidos com desconto
```sql
SELECT * FROM pedidos LIMIT 5

SELECT 
	MIN(desconto) AS "Desconto Mínimo Aplicado"
FROM 
	pedidos
```

## 📌 Aula 38

🔹 Desafio 1
   - Encontrar o produto mais caro
```sql
SELECT
	MAX(preco) AS "Produto mais Caro"
FROM
	produtos
```
🔹 Desafio 2
   - Encontrar o valor do maior pedido
```sql
SELECT 
	MAX(valor_total) AS "Valor do maior pedido"
FROM 
	pedidos
```
## 🧠 Desafios Finais
---
## 🏁 Desafio Final 1 - Visão Geral do Catálogo
  - Mostre em uma única consulta:
    - Total de produtos cadastrados
    - Quantidade total em estoque
    - Preço médio dos produtos (arredondado para 2 casas)
    - Produto mais barato
    - Produto mais caro
```sql
SELECT
	COUNT(*) AS "Total de Produtos",
	SUM(estoque) AS "Total em Estoque",
	ROUND(AVG(preco),2) AS "Preço Médio (R$)",
	MIN(preco) AS "Menor Preço (R$)",
	MAX(preco) AS "Maior Preço (R$)"
FROM
	produtos
```

## 🏁 Desafio Final 2 - Análise de Vendas
  - Mostre em uma única consulta:
    - Total de pedidos realizados
    - Valor total vendido
    - Ticket médio (valor médio por pedido)
    - Menor pedido
    - Maior pedido
```sql
SELECT * FROM pedidos LIMIT 3
	
SELECT
	COUNT(pedido_id) AS "Total de Pedidos",
	ROUND(SUM(valor_total),2) AS "Total Vendido",
	ROUND(AVG(valor_total),2) AS "Ticket Médio",
	MIN(valor_total) AS "Menor Pedido",
	MAX(valor_total) AS "Maior Pedido"
FROM
	pedidos
```

## 🏁 Desafio Final 3 - Diversidade de Clientes
  - Descubra:
    - Quantos clientes estão cadastrados
    - Em quantas cidades diferentes
    - Em quantos estados diferentes
```sql
SELECT * FROM CLIENTES LIMIT 5

SELECT
	COUNT(DISTINCT cliente_id) AS "Total de Clientes",
	COUNT(DISTINCT cidade) AS "Total de Cidades",
	COUNT(DISTINCT estado) AS "Total de Estados"
FROM
	clientes
```

## 🏁 Desafio Final 4 - Estatísticas de Avaliações
  - Mostre:
    - Total de avaliações
    - Nota média (arredondada para 1 casa decimal)
    - Menor nota
    - Maior nota
```sql
SELECT
	COUNT(comentario) AS "Total de Avaliações",
	ROUND(AVG(nota),1) AS "Nota Média",
	MIN(nota) AS "Menor Nota",
	MAX(nota) AS "Maior Nota"
FROM 
	avaliacoes
```

## 🏁 Desafio Final 5 - Análise de Pagamentos (Desafio Avançado)
  - Para pagamentos com status "aprovado":
    - Quantos pagamentos foram aprovados
    - Valor total aprovado
    - Valor médio aprovado
    - Quantos métodos de pagamento diferentes foram usados
```sql
SELECT * FROM pagamentos LIMIT 5

SELECT
	COUNT(status) AS "Total de Pagamentos",
	status,
	SUM(valor) AS "Valor Total",
	ROUND(AVG(valor),2) AS "Valor Médio",
	COUNT(DISTINCT metodo) AS "Métodos de Pag. Utilizados"
FROM
	pagamentos
WHERE status = 'aprovado'
GROUP BY status
```

## 🏁 Desafio Final 6 - Relatório Completo (Boss Final!)
  - Crie uma consulta que mostre para produtos da marca "Samsung":
    - Quantidade de produtos Samsung
    - Estoque total de produtos Samsung
    - Preço médio (arredondado)
    - Produto Samsung mais barato
    - Produto Samsung mais caro
```sql
SELECT
	marca AS "Produto",
	COUNT(marca) AS "Qtd. de Produtos",
	SUM(estoque) AS "Estoque Total",
	ROUND(AVG(preco),2),
	MIN(preco) AS "Produto mais barato",
	MAX(preco) AS "Produto mais caro"
FROM
	produtos
WHERE marca='Samsung'
GROUP BY marca
```

