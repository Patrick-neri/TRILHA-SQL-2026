# 🚀 Desafios SQL - Módulo 13 - Window Functions
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário%20→%20Avançado-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Window%20Functions-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este conjunto de desafios aborda **Window Functions em SQL**, utilizando funções analíticas para rankings, comparações, análises temporais e dashboards de performance.

💡 Nesses desafios trabalhei com:

- ROW_NUMBER  
- RANK e DENSE_RANK  
- PARTITION BY  
- LEAD e LAG  
- FIRST_VALUE  
- Rankings analíticos  
- Comparações temporais  
- Dashboards analíticos avançados  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL avançadas  
✔️ Construção de rankings e análises  
✔️ Comparação de dados temporais  
✔️ Pensamento analítico aplicado a SQL  
✔️ Desenvolvimento de dashboards analíticos  

---

## 📌 Aula 54

🔹 Desafio 1: Numerar produtos ordenados por preço (do mais caro ao mais barato)

  - Exiba: número, nome e preço

```sql
SELECT
	ROW_NUMBER() OVER(ORDER BY preco DESC) AS numero,
	nome,
	preco
FROM
	produtos 
```
🔹 Desafio 2: Numerar pedidos de cada cliente por data

  - Exiba: cliente_id, pedido_id, data_pedido e número do pedido
```sql
SELECT
	cliente_id,
	pedido_id,
	data_pedido,
 	ROW_NUMBER() OVER(ORDER BY data_pedido ASC) AS numero_pedido
FROM
	pedidos
```

## 📌 Aula 55

🔹 Desafio 1: Ranking de produtos mais caros

  - Crie um ranking dos produtos ordenados do mais caro para o mais barato.
  - Exiba: posição no ranking, nome do produto e preço

```sql
SELECT
	RANK() OVER(ORDER BY preco DESC) AS ranking,
	nome,
	preco
FROM
	produtos;
```

🔹 Desafio 2: Ranking de clientes por valor total de compras

  - Identifique quais clientes gastam mais na loja criando um ranking.
  - Calcule o total gasto por cada cliente somando o valor_total de todos os seus pedidos.
  - Exiba: nome do cliente, total gasto e ranking
```sql

SELECT
	c.nome,
	SUM(p.valor_total) AS total_gasto,
	RANK() OVER(ORDER BY p.valor_total DESC) AS ranking
FROM
	clientes c
JOIN
	pedidos p
ON
	c.cliente_id = p.cliente_id
GROUP BY
	c.nome,
	p.valor_total;
```
## 📌 Aula 56

🔹 Desafio 1: Rankear produtos por avaliação média (sem pular números)

  - Use DENSE_RANK com AVG(nota)
```sql

SELECT
	p.nome,
	AVG(av.nota) AS media_avaliacao,
	DENSE_RANK() OVER(ORDER BY AVG(av.nota) DESC) AS ranking
FROM
	produtos p
JOIN 
	avaliacoes av
ON 
	p.produto_id = av.produto_id
GROUP BY
	p.nome;
```
🔹 Desafio 2: Rankear categorias por número de produtos

  - Conte produtos por categoria e aplique DENSE_RANK

```sql
SELECT
	cat.nome AS nome_categoria,
	count(p.nome) AS qtd_de_produtos,
	DENSE_RANK() OVER(ORDER BY COUNT(*) desc) AS ranking_de_produtos
FROM
	produtos p
JOIN
	categorias cat
ON
	p.categoria_id = cat.categoria_id
GROUP BY
	cat.nome, cat.categoria_id;
```
## 📌 Aula 57

🔹 Desafio 1: Numerar produtos dentro de cada categoria

  - Ordenar por preço dentro de cada categoria
```sql

SELECT
	cat.nome AS categoria,
	p.nome,
	p.preco,
	ROW_NUMBER() OVER(PARTITION BY cat.nome ORDER BY p.preco DESC) AS numero
FROM
	produtos p
JOIN
	categorias cat
ON 
	p.categoria_id = cat.categoria_id
```
🔹 Desafio 2: Rankear vendas por mês

  - Mostre o ranking de pedidos por valor em cada mês
```sql

SELECT
	pedido_id,
	valor_total,
	data_pedido,
	RANK() OVER(PARTITION BY EXTRACT(MONTH FROM data_pedido) ORDER BY valor_total) AS ranking
FROM
	pedidos
```
## 📌 Aula 58

🔹 Desafio 1: Comparar preço de cada produto com o próximo produto

  - Ordene por preço e mostre a diferença
```sql

SELECT
	nome,
	preco,
	LEAD(preco,1) OVER(ORDER BY preco) AS proximo_preco,
	preco - LEAD(preco,1) OVER(ORDER BY preco) AS diferenca
FROM
	produtos
```
🔹 Desafio 2: Calcular diferença de valor entre pedidos consecutivos de cada cliente

  - Use PARTITION BY cliente_id
```sql

SELECT
	c.nome,
	p.valor_total,
	LAG(p.valor_total) OVER(PARTITION BY p.cliente_id ORDER BY p.cliente_id) AS pedido_anterior,
	p.valor_total - LAG(p.valor_total) OVER(PARTITION BY p.cliente_id ORDER BY p.cliente_id) AS diferenca
FROM
	clientes c
JOIN
	pedidos p
ON
	c.cliente_id = p.cliente_id;
```
---
## 🧠 Desafios Finais
---
## 🏁 Desafio Final 1: Ranking Completo de Produtos

  - Para cada produto, mostre:
      - nome, preco, categoria
      - ROW_NUMBER, RANK e DENSE_RANK por preço
      - Compare os resultados
```sql

SELECT
	p.nome, 
	p.preco,
	c.nome AS categoria,
	ROW_NUMBER() OVER(ORDER BY p.preco DESC) AS numeracao,
	RANK() OVER(ORDER BY p.preco DESC) AS ranking,
	DENSE_RANK() OVER(ORDER BY p.preco desc) AS DENSE_RANK
FROM
	produtos p
JOIN
	categorias c
ON
	p.categoria_id = c.categoria_id;
```
## 🏁 Desafio Final 2: Top 2 Produtos por Categoria

  - Liste os 2 produtos mais caros de cada categoria
  - Use PARTITION BY e ROW_NUMBER
```sql

SELECT * FROM 
	(SELECT 
		p.nome,
		c.nome AS categoria,
		p.preco,
		ROW_NUMBER() OVER(PARTITION BY p.categoria_id ORDER BY preco desc) AS posicao 
	FROM
		produtos p
	JOIN
		categorias c
	ON 
		p.categoria_id = c.categoria_id) AS ranking
WHERE 
	posicao <= 2;
```
## 🏁 Desafio Final 3: Análise de Pedidos do Cliente

  - Para cada pedido, mostre:
      - cliente_id, pedido_id, data_pedido, valor_total
      - valor do pedido anterior do mesmo cliente
      - diferença entre pedidos
      - número do pedido do cliente (1º, 2º, 3º...)
```sql

SELECT
	cliente_id,
	pedido_id,
	data_pedido,
	valor_total,
	LAG(valor_total) OVER(PARTITION BY cliente_id ORDER BY cliente_id) AS valor_pedido_anterior,
	valor_total - LAG(valor_total) OVER(PARTITION BY cliente_id ORDER BY cliente_id) AS diferenca_entre_pedidos,
	ROW_NUMBER() OVER(PARTITION BY cliente_id ORDER BY cliente_id) AS numero_pedido
FROM
	pedidos;
```
## 🏁 Desafio Final 4: Variação de Vendas
  
  - Calcule a variação percentual de vendas diárias
  - Mostre: data, total do dia, total do dia anterior, variação %
```sql

SELECT
	CAST(data_pedido AS date) AS data,
	valor_total,
	LAG(valor_total) OVER(ORDER BY data_pedido) AS valor_pedido_anterior,
	ROUND( 100.0 * (valor_total - LAG(valor_total) OVER (ORDER BY data_pedido))
        / NULLIF(LAG(valor_total) OVER (ORDER BY data_pedido asc), 0),
        2) AS varicao
FROM
	pedidos
ORDER BY
	data_pedido asc;
```
## 🏁 Desafio Final 5 (Boss Final!): Dashboard de Performance

  - Crie um relatório que mostre para cada produto:
      - nome, categoria, preco, estoque
      - Ranking geral por preço
      - Ranking dentro da categoria
      - Se é o mais caro da categoria (sim/não)
      - Diferença de preço para o produto mais caro da mesma categoria

```sql
SELECT
	p.nome,
	c.nome AS categoria,
	p.preco,
	p.estoque,
	RANK() OVER(ORDER BY p.preco DESC) AS rank_de_preco,
	RANK() OVER(PARTITION BY p.categoria_id ORDER BY p.preco DESC) AS rank_da_categoria,
	CASE
		WHEN RANK() OVER(PARTITION BY p.categoria_id ORDER BY p.preco DESC) = 1 THEN 'Sim'
		ELSE 'Não'
	END AS mais_caro_categoria,
	FIRST_VALUE(p.preco) OVER(PARTITION BY p.categoria_id ORDER BY p.preco DESC) - p.preco AS diferenca_valor
FROM
	produtos p
JOIN
	categorias c
ON
	p.categoria_id = c.categoria_id
ORDER BY
	categoria ASC, p.preco DESC
```
