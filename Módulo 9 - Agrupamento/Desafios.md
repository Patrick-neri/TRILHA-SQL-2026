# 🚀 Desafios SQL - Módulo 9 - Agrupamento (GROUP BY & HAVING)
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Agrupamento%20de%20Dados-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este conjunto de desafios aborda o uso de **agrupamento de dados em SQL**, utilizando GROUP BY e HAVING para gerar métricas e análises essenciais para tomada de decisão baseada em dados.

💡 Nesses desafios trabalhei com:

- GROUP BY para agrupamento de informações  
- HAVING para filtros em dados agregados  
- COUNT para contagem de registros  
- SUM para cálculos de totais  
- AVG para médias de valores  
- MIN e MAX para identificação de extremos  
- Construção de relatórios analíticos com múltiplas métricas  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL com agrupamentos  
✔️ Análise de métricas e indicadores (KPIs)  
✔️ Construção de relatórios estratégicos  
✔️ Pensamento analítico aplicado a dados  
✔️ Interpretação de dados agregados  

---

## 📌 Aula 39

🔹 Desafio 1 - Contar quantos produtos existem por categoria
```sql
SELECT * FROM produtos LIMIT 2

SELECT
	categoria_id,
	COUNT(categoria_id) AS "Qtd. por categoria"
FROM 
	produtos
GROUP BY 
	categoria_id
ORDER BY
	categoria_id
```

🔹 Desafio 2 - Calcular o valor total de pedidos por cliente
```sql
SELECT * FROM pedidos LIMIT 4

SELECT
	cliente_id,
	SUM(valor_total) AS "Valor total do cliente"
FROM 
	pedidos
GROUP BY
	cliente_id
ORDER BY 
	cliente_id
```

---

## 📌 Aula 40

🔹 Desafio 1 - Contar quantos clientes existem por estado e cidade
```sql
SELECT * FROM clientes LIMIT 2

SELECT
	cidade,
	estado,
	COUNT(*) AS "Qtd de clientes"
FROM
	clientes
GROUP BY
	cidade, estado
ORDER BY
	cidade
```

🔹 Desafio 2 - Calcular o valor médio de pedidos por mês e ano
```sql
SELECT * FROM pedidos LIMIT 3

SELECT
	pedido_id,
	ROUND(AVG(valor_total),2) AS "Ticket Médio",
	EXTRACT(MONTH FROM data_pedido) AS "Mês do pedido",
	EXTRACT(YEAR FROM data_pedido) AS "Ano do pedido"
FROM
	pedidos
GROUP BY
	pedido_id,
	EXTRACT(MONTH FROM data_pedido),
	EXTRACT(YEAR FROM data_pedido) 
ORDER BY
	"Ano do pedido" ASC,
	"Mês do pedido" ASC
```

---

## 📌 Aula 41

🔹 Desafio 1 - Mostrar apenas categorias que tenham mais de 5 produtos
```sql
SELECT * FROM produtos LIMIT 5

SELECT
	categoria_id,
	COUNT(nome) AS "Qtd de produtos"
FROM 
	produtos
GROUP BY
	categoria_id
HAVING 
	COUNT(nome) > 5
ORDER BY
	categoria_id
```

🔹 Desafio 2 - Mostrar apenas clientes que fizeram mais de 2 pedidos
```sql
SELECT * FROM pedidos LIMIT 3

SELECT	
	cliente_id,
	COUNT(pedido_id) AS "Qtd de pedidos"
FROM
	pedidos
GROUP BY
	cliente_id
HAVING
	COUNT(pedido_id) >2
ORDER BY
	cliente_id
```

---

## 🧠 Desafios Finais
---

## 🏁 Desafio Final 1 - Relatório de Vendas por Status
  - Mostre a quantidade de pedidos e o valor total para cada status
  - Ordene pelo valor total (maior primeiro)
```sql
SELECT
	status,
	COUNT(pedido_id) AS "Qtd. de Pedidos",
	SUM(valor_total) AS "Valor Total (R$)"
FROM
	pedidos
GROUP BY 
	status
ORDER BY
	"Valor Total (R$)" DESC
```

## 🏁 Desafio Final 2 - Análise de Clientes por Região
  - Conte quantos clientes existem em cada combinação de estado e cidade
  - Mostre apenas combinações com mais de 2 clientes
  - Ordene por estado e depois por quantidade (maior primeiro)
```sql
SELECT * FROM clientes LIMIT 2

SELECT
	cidade,
	estado,
	COUNT(cliente_id) AS "Qtd de clientes"
FROM
	clientes
GROUP BY
	cidade, 
	estado
HAVING 
	COUNT(cliente_id) >=2
ORDER BY
	estado, 
	"Qtd de clientes" desc
```

## 🏁 Desafio Final 3 - Performance de Marcas
  - Para cada marca, mostre:
    - Quantidade de produtos
    - Preço médio (arredondado para 2 casas)
    - Estoque total
  - Filtre apenas marcas com mais de 3 produtos
  - Ordene por quantidade de produtos (maior primeiro)
```sql
SELECT * FROM produtos LIMIT 3

SELECT
	marca,
	COUNT(produto_id) AS "Qtd de Produtos",
	ROUND(AVG(preco),2) AS "Preço médio",
	SUM(estoque) AS "Estoque Total"
FROM 
	produtos
GROUP BY
	marca
HAVING
	COUNT(produto_id) >=3
ORDER BY
	"Qtd de Produtos" DESC
```

## 🏁 Desafio Final 4 - Análise de Pagamentos
  - Agrupe pagamentos por método e status
  - Mostre a quantidade e o valor total de cada grupo
  - Filtre grupos com valor total superior a R$ 1000
```sql
SELECT * FROM pagamentos LIMIT 3	

SELECT
	metodo,
	status,
	COUNT(pagamento_id) AS "Qtd de pagamentos",
	SUM(valor) AS "Valor total"
FROM
	pagamentos
GROUP BY
	metodo,
	status
HAVING 
	SUM(valor) > 1000
ORDER BY
	metodo
```

## 🏁 Desafio Final 5 - Relatório Mensal de Vendas (Desafio Avançado)
  - Crie um relatório com:
    - Ano e mês do pedido
    - Quantidade de pedidos
    - Valor total
    - Ticket médio (valor médio por pedido)
  - Considere apenas pedidos com status diferente de "cancelado"
  - Filtre meses com mais de 10 pedidos
  - Ordene por ano e mês
```sql
SELECT * FROM pedidos LIMIT 3

SELECT
	EXTRACT(YEAR FROM data_pedido) AS "Ano",
	EXTRACT(MONTH FROM data_pedido) AS "Mês",
	COUNT(pedido_id) AS "Qtd de pedidos",
	SUM(valor_total) AS "Valor total",
	ROUND(AVG(valor_total),2) AS "Ticket Médio"
FROM
	pedidos
WHERE
	status <> 'cancelado'
GROUP BY
	"Ano",
	"Mês"
ORDER BY
	"Ano",
	"Mês"
```

## 🏁 Desafio Final 6 - Dashboard Completo (Boss Final!)
  - Crie uma consulta que mostre para cada categoria:
    - ID da categoria
    - Total de produtos
    - Preço mínimo, médio e máximo
    - Estoque total
    - Valor total em estoque (soma de preco * estoque)
  - Filtre categorias onde:
    - Existam mais de 5 produtos
    - O valor em estoque seja maior que R$ 5000
  - Ordene pelo valor em estoque (maior primeiro)
```sql
SELECT * FROM produtos LIMIT 5

SELECT
	categoria_id AS "ID da Categoria",
	COUNT(nome) AS "Total de produtos",
	MAX(preco) AS "Preço máximo",
	ROUND(AVG(preco),2) AS "Preço médio",
	MIN(preco) AS "Preço mínimo",
	SUM(estoque) AS "Qtd total em estoque",
	SUM(preco * estoque) AS "Valor total em estoque"
FROM
	produtos
GROUP BY 
	categoria_id
HAVING
	COUNT(nome) >5 AND 
	SUM(preco * estoque) > 5000
ORDER BY
	"Valor total em estoque" DESC
```
