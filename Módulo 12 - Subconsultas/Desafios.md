# 🚀 Desafios SQL - Módulo 12 - Subconsultas
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário%20→%20Avançado-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Subqueries%20e%20Análise%20Avançada-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este conjunto de desafios aborda o uso de **subconsultas em SQL**, aplicadas em filtros, tabelas derivadas, análises correlacionadas e relatórios avançados.

💡 Nesses desafios trabalhei com:

- Subconsultas no WHERE  
- Subconsultas no FROM (Derived Tables)  
- Subconsultas correlacionadas  
- EXISTS e NOT EXISTS  
- Relatórios analíticos complexos  
- Comparações com médias e agregações  
- Dashboards analíticos com múltiplas subconsultas  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL avançadas  
✔️ Construção de relatórios analíticos  
✔️ Manipulação de grandes conjuntos de dados  
✔️ Pensamento analítico aplicado a SQL  
✔️ Uso estratégico de subconsultas  

---

## 📌 Aula 50

🔹 Desafio 1: Produtos com preço acima da média geral

  - Use uma subconsulta no WHERE para comparar com a média de preços de todos os produtos
  - Mostre: nome e preço dos produtos
  - Ordene do mais caro para o mais barato

```sql
SELECT
	nome,
	preco
FROM
	produtos
WHERE 
	preco > (SELECT AVG(preco) FROM produtos)
ORDER BY
	preco DESC;
```
🔹 Desafio 2: Clientes que fizeram pedidos de alto valor (acima de R$ 1000)

  - Use uma subconsulta com IN no WHERE para filtrar clientes
  - Mostre: nome e email dos clientes que têm pelo menos um pedido acima de R$ 1000
  - Ordene por nome em ordem alfabética
```sql
SELECT
	nome,
	email
FROM
	clientes
WHERE
	cliente_id IN (SELECT DISTINCT cliente_id FROM pedidos WHERE valor_total > 1000)
ORDER BY
	nome ASC;
```

## 📌 Aula 51

🔹 Desafio 1: Resumo de vendas por produto usando tabela derivada (Derived Table)

  - Crie uma subconsulta no FROM que agrupe as vendas por produto
  - Mostre: nome do produto, quantidade total vendida e valor total arrecadado
  - Ordene do produto que gerou mais receita para o que gerou menos
```sql
SELECT
	nome,
	qtd_vendida,
	valor_arrecadado
FROM
	(SELECT p.nome AS nome, SUM(i.quantidade) AS qtd_vendida, SUM(i.preco_unitario * i.quantidade) AS valor_arrecadado FROM produtos p
		JOIN itens_pedido i ON p.produto_id = i.produto_id GROUP BY p.nome)
ORDER BY
	valor_arrecadado DESC;
```
🔹 Desafio 2: Calcular médias gerais a partir de dados agregados por cliente

  - Use uma subconsulta no FROM que agrupe pedidos por cliente (total de pedidos e valor total)
  - Mostre: média de pedidos por cliente e média de valor gasto por cliente
  - Arredonde ambos os valores para 2 casas decimais
```sql
SELECT
	cliente_id,
	ROUND(AVG(qtd_pedido),2) AS media_pedidos,
	ROUND(AVG(valor_total),2) AS media_de_gasto
FROM
	(SELECT cliente_id AS cliente_id, COUNT(pedido_id) AS qtd_pedido, SUM(valor_total) AS valor_total FROM pedidos GROUP BY cliente_id)
GROUP BY 
	cliente_id
ORDER BY
	cliente_id;
```
## 📌 Aula 52

🔹 Desafio 1: Relatório de produtos com quantidade total vendida

  - Mostre: nome, preço, estoque atual e total de unidades vendidas de cada produto
  - Inclua produtos que nunca foram vendidos (devem aparecer com 0 unidades)
  - Ordene do produto mais vendido para o menos vendido
```sql
SELECT
	p.nome AS nome_produto,
	p.preco,
	p.estoque AS estoque_atual,
	(SELECT COALESCE(SUM(ip.quantidade),0) FROM itens_pedido ip WHERE ip.produto_id = p.produto_id GROUP BY produto_id) AS total_vendido
FROM
	produtos p
ORDER BY
	total_vendido DESC;

SELECT p.nome, p.preco, p.estoque,(
        SELECT COALESCE(SUM(ip.quantidade), 0)
FROM itens_pedido ip
WHERE ip.produto_id = p.produto_id
    ) AS total_vendido
FROM produtos p
ORDER BY total_vendido DESC;
```
🔹 Desafio 2: Relatório de clientes com valor total gasto individualmente

  - Mostre: ID, nome, email, cidade e total gasto por cada cliente
  - Inclua clientes que nunca fizeram pedidos (devem aparecer com R$ 0,00)
  - Ordene do cliente que mais gastou para o que menos gastou
```sql
EXPLAIN ANALYSE 
SELECT
	c.cliente_id,
	c.nome,
	c.email,
	c.cidade,
	(SELECT COALESCE(SUM(p.valor_total),0) FROM pedidos p WHERE c.cliente_id = p.cliente_id)  AS total_gasto_cliente
FROM
	clientes c
ORDER BY
	total_gasto_cliente DESC;

EXPLAIN ANALYSE 
SELECT c.cliente_id, c.nome, c.email, c.cidade,(
        SELECT COALESCE(SUM(ped.valor_total), 0)
FROM pedidos ped
WHERE ped.cliente_id = c.cliente_id
    ) AS total_gasto
FROM clientes c
ORDER BY total_gasto DESC;
```
## 📌 Aula 53

🔹 Desafio 1: Produtos que já foram vendidos pelo menos uma vez

  - Mostre: nome, preço e estoque atual dos produtos
  - Ordene por nome do produto em ordem alfabética
```sql
SELECT
	p.nome,
	p.preco,
	p.estoque
FROM
	produtos p
WHERE EXISTS 
	(SELECT 1 FROM itens_pedido ip WHERE ip.produto_id = p.produto_id)
ORDER BY
	p.nome ASC;
```
🔹 Desafio 2: Clientes que nunca avaliaram produtos

  - Mostre: nome e email dos clientes
  - Ordene por nome em ordem alfabética
```sql
EXPLAIN 
SELECT
	c.nome,
	c.email
FROM
	clientes c
WHERE NOT EXISTS 
	(SELECT 1 FROM avaliacoes a WHERE a.cliente_id = c.cliente_id)
ORDER BY
	c.nome ASC;
```
---
## 🧠 Desafios Finais
---
## 🏁 Desafio Final 1: Produtos com preço acima da média da sua categoria
  - Use subconsultas no WHERE (para filtrar) e no SELECT (para calcular a média)
  - Mostre: nome do produto, preço, nome da categoria e preço médio da categoria
  - Ordene por categoria e depois por preço (do mais caro para o mais barato)
```sql
EXPLAIN 
SELECT
	prod.nome,
	prod.preco,
	cat.nome,
	(SELECT ROUND(AVG(prod2.preco),2) FROM produtos prod2 WHERE prod2.categoria_id = prod.categoria_id ) AS media_categoria
FROM
	produtos prod
JOIN
	categorias cat
ON 
	prod.categoria_id = cat.categoria_id
WHERE 
	prod.preco > (SELECT AVG(prod2.preco) FROM produtos prod2 WHERE prod2.categoria_id = cat.categoria_id)
ORDER BY
	cat.nome, prod.preco DESC;
```
## 🏁 Desafio Final 2: Identificar clientes VIP (gastam acima da média)
  - Use subconsulta no FROM para calcular o total gasto por cliente
  - Use outra subconsulta para calcular a média geral de gastos
  - Mostre: nome, email, total gasto e média geral (apenas clientes acima da média)
  - Ordene do cliente que mais gastou para o que menos gasto
```sql

SELECT
	nome,
	email,
	total_gasto_cliente
FROM
	(SELECT c.nome, c.email, SUM(p.valor_total) AS total_gasto_cliente FROM clientes c JOIN pedidos p ON c.cliente_id = p.cliente_id GROUP BY c.nome, c.email, p.cliente_id)
WHERE 
	total_gasto_cliente > (SELECT AVG(p.valor_total) FROM pedidos p)
ORDER BY
	total_gasto_cliente DESC;
```
## 🏁 Desafio Final 3: Contagem de produtos com estoque baixo por categoria

  - Use uma subconsulta escalar no SELECT para contar produtos com estoque < 10
  - Mostre: nome da categoria e quantidade de produtos com estoque baixo
  - Considere apenas categorias ativas
  - Ordene da categoria com mais produtos em estoque baixo para a com menos
```sql
SELECT
	c.nome AS categoria,
	(SELECT COUNT(p.estoque) AS quantidade_estoque FROM produtos p WHERE p.categoria_id = c.categoria_id AND p.estoque < 100) AS qtd_estoque
FROM
	categorias c
WHERE c.ativo = TRUE 
ORDER BY 
	qtd_estoque DESC;
```
## 🏁 Desafio Final 4: Produtos vendidos em pedidos de alto valor (acima de R$ 1000)
  
  - Use EXISTS para verificar se o produto aparece em pedidos grandes
  - Mostre: nome, preço e marca dos produtos (sem duplicatas)
  - Ordene por nome do produto
```sql
EXPLAIN ANALYSE 
SELECT DISTINCT
	nome,
	preco,
	marca	
FROM
(SELECT p.nome AS nome, p.preco AS preco, p.marca AS marca, ip.pedido_id AS id_pedido FROM produtos p JOIN itens_pedido ip ON p.produto_id = ip.produto_id )
WHERE EXISTS
	(SELECT 1 FROM pedidos ped WHERE ped.pedido_id = id_pedido AND ped.valor_total > 1000) 
ORDER BY
	nome;

EXPLAIN ANALYSE
SELECT DISTINCT
    p.nome AS produto,
    p.preco,
    p.marca
FROM produtos p
WHERE EXISTS (
    SELECT 1
    FROM itens_pedido ip
    INNER JOIN pedidos ped ON ip.pedido_id = ped.pedido_id
    WHERE ip.produto_id = p.produto_id
      AND ped.valor_total > 1000
)
ORDER BY p.nome;

```
## 🏁 Desafio Final 5: Análise mensal de desempenho de vendas

  - Use subconsulta no FROM para agrupar vendas por ano/mês
  - Use outra subconsulta para calcular a média mensal de vendas
  - Mostre: ano, mês, total de vendas, quantidade de pedidos e se superou a média (Sim/Não)
  - Exclua pedidos cancelados da análise
  - Ordene por ano e mês cronologicamente
```sql
SELECT 	
	ano,
	mes,
	total_vendas,
	qtd_pedidos,
	CASE 
		WHEN total_vendas > media_mensal THEN 'Sim'
		ELSE 'Não'
	END AS superou_a_media
FROM
	(SELECT 
		EXTRACT(YEAR FROM p.data_pedido) AS ano,
		EXTRACT(MONTH FROM p.data_pedido) AS mes,
		SUM(p.valor_total) AS total_vendas, 
		COUNT(*) AS qtd_pedidos
	FROM 
		pedidos p 
	WHERE p.status <> 'cancelado'	
	GROUP BY 
		ano, mes) AS vendas,
	(SELECT ROUND(AVG(total_mes),2) AS media_mensal
	FROM 
		(SELECT 
			SUM(p1.valor_total) AS total_mes
		FROM 
		 	pedidos p1
		WHERE p1.status <> 'cancelado'
		GROUP BY
			EXTRACT(YEAR FROM p1.data_pedido),
			EXTRACT(MONTH FROM p1.data_pedido)
		) AS total_media
	) AS media_mensal
ORDER BY
	ano ASC, mes ASC;
```
## 🏁 Desafio Final 6: Dashboard executivo completo de produtos (Boss Final!)

  - Combine TODOS os tipos de subconsultas
  - Mostre para cada produto:
    - Nome, categoria, preço, estoque atual
    - Total de unidades vendidas e receita total gerada
    - Nota média das avaliações (arredondada para 1 casa decimal)
    - Se o produto já foi avaliado alguma vez (Sim/Não )
    - Classificação: "Campeão" se vendeu acima da média geral, "Normal" caso contrário
  - Ordene do produto mais vendido para o menos vendido
  - DICA: Use COALESCE para produtos que nunca foram vendidos ou avaliados

```sql
SELECT
	p.nome AS produto,
	(SELECT 
		cat.nome 
	FROM 
		categorias cat 
	WHERE 
		p.categoria_id = cat.categoria_id) AS categoria,
	p.preco,
	p.estoque AS esoque_atual,
	(SELECT 
		COALESCE(SUM(ip.quantidade),0)
	FROM 
		itens_pedido ip 
	WHERE
		p.produto_id = ip.produto_id) AS quantidade_vendida,
	(SELECT 
		COALESCE(SUM(ip.quantidade*ip.preco_unitario),0)
	FROM 
		itens_pedido ip 
	WHERE 
		p.produto_id = ip.produto_id)  AS receita_gerada,
	(SELECT 
		ROUND(AVG(av.nota),1) 
	FROM 
		avaliacoes av 
	WHERE 
		p.produto_id = av.produto_id) AS media_notas,
	CASE
		WHEN EXISTS
			(SELECT 1
			 FROM 
			 	avaliacoes av2 
			 WHERE 
			 	p.produto_id = av2.produto_id)
		THEN 'Sim'
		ELSE 'Não'
	END AS foi_avaliado,
	CASE
		WHEN 
			(SELECT 
				COALESCE(SUM(ip.quantidade),0)
			FROM 
				itens_pedido ip 
			WHERE 
				p.produto_id = ip.produto_id) 
				>
			(SELECT 
				avg(quantidade) 
			FROM 
				itens_pedido) 
		THEN 'Campeão'
		ELSE 'Normal'
	END AS classificacao
FROM
	produtos p
ORDER BY
	quantidade_vendida DESC;
```
