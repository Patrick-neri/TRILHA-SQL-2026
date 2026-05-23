# 🚀 Desafios SQL - Módulo 14 - CTEs e CTEs Recursivas
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Avançado-red?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-CTEs%20e%20Análises%20Avançadas-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este módulo foi focado em **CTEs (Common Table Expressions)** e **CTEs Recursivas**, aplicando SQL em cenários analíticos mais próximos do mundo real.

💡 Nesses desafios trabalhei com:

- CTEs simples  
- Múltiplas CTEs encadeadas  
- CTEs Recursivas  
- Relatórios analíticos  
- Geração de calendários  
- Comparações estatísticas  
- Dashboards executivos  
- Otimização de consultas  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Estruturação de queries complexas  
✔️ Escrita de SQL modular e legível  
✔️ Análises estatísticas e comparativas  
✔️ Construção de dashboards analíticos  
✔️ Pensamento analítico aplicado a negócio  

---

## 📌 Aula 59

🔹 Desafio 1: Ranking de Produtos Mais Vendidos

  - Objetivo: Identificar os 10 produtos campeões de vendas utilizando CTE
  - Estratégia aplicada:
      - Criar uma CTE para consolidar quantidade vendida por produto
      - Utilizar agregações com SUM para cálculo de vendas
      - Realizar JOIN entre produtos e itens de pedido
      - Aplicar GROUP BY para consolidar os produtos
      - Ordenar os produtos pela maior quantidade vendida
      - Aplicar LIMIT para retornar apenas o TOP 10
      - Comparar abordagem tradicional e abordagem otimizada para performance

#### Menos performática

```sql
WITH mais_vendidos AS
	( SELECT 
		p.nome AS nome_produto, 
		SUM(ip.quantidade) AS total_vendido 
	FROM 
		produtos p 
	JOIN 
		itens_pedido ip 
	ON 
		p.produto_id = ip.produto_id 
	GROUP BY 
		p.nome)
SELECT 
	nome_produto, 
	total_vendido 
FROM 
	mais_vendidos 
ORDER BY
	total_vendido 
DESC LIMIT 10;
```
#### Mais performática — Agrupar antes do JOIN
```sql
WITH produtos_vendidos AS 
	( SELECT 
		produto_id, 
		SUM(quantidade) AS total_vendido 
		FROM 
			itens_pedido 
		GROUP BY
			produto_id ) 
SELECT 
	p.nome AS produto, 
	pv.total_vendido 
FROM 
	produtos p 
INNER JOIN 
	produtos_vendidos pv 
ON 
	p.produto_id = pv.produto_id 
ORDER BY 
	pv.total_vendido 
DESC LIMIT 10;
```
🔹 Desafio 2: Identificando Clientes VIP

  - Objetivo: Identificar clientes que gastaram acima da média geral da loja
  - Estratégia aplicada:
      - Criar uma CTE para consolidar métricas dos pedidos por cliente
      - Utilizar COUNT e SUM para cálculo de pedidos e gastos
      - Criar uma segunda CTE para cálculo da média geral de gastos
      - Utilizar CROSS JOIN para replicar a média geral na consulta principal
      - Comparar o total gasto de cada cliente com a média geral
      - Calcular diferença entre gasto individual e média da empresa
      - Ordenar clientes pelo maior valor gasto

```sql
WITH total_pedidos AS
	(SELECT 
		cliente_id,
		COUNT(pedido_id) AS qtd_pedidos,
		SUM(valor_total) AS total_gasto
	FROM
		pedidos
	GROUP BY
		cliente_id),
media_geral AS
	(SELECT
		AVG(total_gasto) AS media_geral
	FROM
		total_pedidos
	)	
SELECT
	c.nome,
	tp.qtd_pedidos,
	tp.total_gasto,
	ROUND(mg.media_geral,2) AS media_geral,
	ROUND(tp.total_gasto - mg.media_geral,2) AS diferenca
FROM
	clientes c
JOIN
	total_pedidos tp
ON
	c.cliente_id = tp.cliente_id
CROSS JOIN
	media_geral mg
WHERE
	tp.total_gasto > mg.media_geral
ORDER BY
	total_gasto DESC
```

## 📌 Aula 60

🔹 Desafio 1: Gerador de Sequências

  - Objetivo: Gerar uma sequência numérica de 1 a 100 utilizando CTE recursiva
  - Estratégia aplicada:
      - Criar uma CTE RECURSIVE com valor inicial
      - Aplicar UNION ALL para repetição da sequência
      - Incrementar os números de forma recursiva
      - Definir condição de parada para evitar loop infinito
      - Demonstrar funcionamento de recursividade em SQL

```sql
WITH RECURSIVE sequencia AS
	(SELECT 1 AS n
	 UNION ALL
	 SELECT n + 1 
	 FROM
	 	sequencia
	 WHERE
	 	n < 100)
SELECT * FROM sequencia
```
🔹 Desafio 2: Relatório de Vendas Diárias Completo

  - Objetivo: Criar um relatório diário contendo todos os dias do mês, incluindo dias sem vendas
  - Estratégia aplicada:
      - Criar uma CTE recursiva para geração de calendário
      - Gerar sequência de datas do mês completo
      - Consolidar vendas diárias em uma segunda CTE
      - Utilizar SUM e COUNT para cálculo de métricas
      - Aplicar LEFT JOIN entre calendário e vendas
      - Utilizar COALESCE para tratar dias sem movimentação
      - Exibir relatório completo com datas, dia da semana e indicadores de vendas
  
```sql
WITH RECURSIVE
datas AS	(
	 SELECT DATE '2026-01-01' AS dias
	
     UNION ALL
     
     SELECT CAST(dias + INTERVAL '1 day' AS DATE)
     FROM datas
     WHERE dias < '2026-01-31'
),
relatorio AS 	(
	SELECT
 		CAST(data_pedido AS date) AS data,
 		SUM(valor_total) AS total_vendas,
 		COUNT(*) AS qtd_pedidos
 	FROM pedidos
 	WHERE data_pedido >= '2026-01-01' 
 	AND	data_pedido < '2026-02-01'
 	GROUP BY CAST(data_pedido AS date) 		
 )
SELECT 
	d.dias AS data,
	TO_CHAR(d.dias, 'Day') AS dia_da_semana,
	COALESCE(r.total_vendas, 0) AS total_vendas,
	COALESCE(r.qtd_pedidos, 0) AS qtd_pedidos
FROM datas d
LEFT JOIN relatorio r
ON	d.dias = r.data
ORDER BY d.dias;
```
---
## 🧠 Desafios Finais
---
## 🏁 Desafio Final 1: CTE com Análise Completa de Clientes

  - Objetivo: Identificar os 10 clientes mais valiosos da empresa
  - Estratégia aplicada:
      - Criar uma CTE para consolidar métricas dos clientes
      - Realizar agregações com COUNT, SUM e AVG
      - Utilizar JOIN entre clientes e pedidos
      - Calcular ticket médio dos pedidos
      - Select principal exibindo os clientes com maior valor gasto
      - Ordenação decrescente e limitação para TOP 10 clientes

```sql
WITH 
resumo_clientes AS	(
	SELECT 
		c.cliente_id,
		c.nome,
		COUNT(p.pedido_id) AS total_pedidos,
		SUM(p.valor_total) AS valor_total_gasto,
		ROUND(AVG(p.valor_total),2) AS ticket_medio
	FROM clientes c
	JOIN pedidos p
	ON c.cliente_id = p.cliente_id
	GROUP BY c.cliente_id, c.nome
)
SELECT
	r.nome AS cliente,
	total_pedidos,
	valor_total_gasto,
	ticket_medio
FROM
	resumo_clientes r
ORDER BY
	valor_total_gasto DESC
LIMIT 10
```

## 🏁 Desafio Final 2: CTE Recursiva de Datas

  - Criar um relatório de vendas diárias completo, incluindo dias sem vendas
  - Estratégia Aplicada:
    - Definir os meses de referência
    - Separar os meses para cálculo
    - Calcular as vendas diárias nos meses seprados
    - Select principal com LEFT JOIN mostrando o relatório
    - Uso de CROSS JOIN e LEFT JOIN  

```sql
WITH RECURSIVE 
mes_referencia AS (
	 SELECT
        DATE '2025-12-01' AS primeiro_dia,
        DATE '2025-12-31' AS ultimo_dia
),
data_mes AS (
	SELECT primeiro_dia AS DATA
	FROM mes_referencia
	UNION ALL
	SELECT DATA + 1
	FROM data_mes
	CROSS JOIN mes_referencia
	WHERE DATA < mes_referencia.ultimo_dia
),
vendas_diarias AS (
	SELECT 
		CAST(data_pedido AS DATE) AS DATA,
		SUM(valor_total) AS total_vendas,
		COUNT(*) AS qtd_pedidos
	FROM
		pedidos
	WHERE 
		data_pedido > '2025-12-01' 
	AND data_pedido < '2026-01-01'
	GROUP BY DATA
	ORDER BY DATA asc
)	
SELECT
	d.DATA,
	COALESCE(v.total_vendas, 0) AS total_vendas,
	COALESCE(v.qtd_pedidos, 0) AS qtd_pedidos
FROM data_mes d
LEFT JOIN vendas_diarias v
ON d.DATA = v.DATA
ORDER BY d.data
```

## 🏁 Desafio Final 3: Múltiplas CTEs Encadeadas

- Objetivo: Identificar produtos com desempenho acima da média usando CTEs encadeadas
- Estratégia aplicada:
  - Uma CTE para agregar vendas por produtos
  - Uma CTE para calcular média de vendas
  - Comparar produtos com a média
  - Gerar percentuais de desempenho
  - Select mostrando produtos que performaram acima da média e seus dados.


```sql
WITH 
vendas_por_produto AS (
	SELECT
		prod.produto_id,
		prod.nome,
		SUM(ip.quantidade) AS total_vendido,
		SUM(ip.quantidade * ip.preco_unitario) AS receita_total
	FROM
		produtos prod
	JOIN
		itens_pedido ip
	ON prod.produto_id = ip.produto_id
	GROUP BY prod.produto_id, prod.nome
),
media_vendas AS (
	SELECT
		ROUND(AVG(total_vendido),2) AS media_qtd,
		ROUND(AVG(receita_total),2) AS media_receita
	FROM
		vendas_por_produto
),
produtos_acima_media AS (
	SELECT
		vp.produto_id,
		vp.nome,
		vp.total_vendido,
		vp.receita_total,
		mv.media_qtd,
		mv.media_receita,
		ROUND(((vp.total_vendido - mv.media_qtd) / mv.media_qtd * 100),2) AS perc_acima_media_qtd,
		ROUND(((vp.receita_total - mv.media_receita) / mv.media_receita * 100),2) AS perc_acima_media_receita
	FROM
		vendas_por_produto vp
	CROSS JOIN 
		media_vendas mv
	WHERE vp.total_vendido > mv.media_qtd OR vp.receita_total > mv.media_receita
)
SELECT
	nome,
	total_vendido,
	receita_total,
	media_qtd,
	media_receita,
	perc_acima_media_qtd,
	perc_acima_media_receita
FROM
	produtos_acima_media
ORDER BY
	receita_total DESC;
```

## 🏁 Desafio Final 4 (Boss Final!): Dashboard Executivo

  - Construção de um dashboard executivo utilizando múltiplas CTEs encadeadas
  - Estratégia aplicada:
      - Criar CTE para identificar o mês mais recente
      - Consolidar métricas mensais de vendas
      - Aplicar Window Functions com LAG para comparação mensal
      - Calcular taxa de crescimento percentual
      - Criar ranking dos produtos mais vendidos no período
      - Criar ranking dos clientes com maior volume de compras
      - Combinar múltiplos relatórios utilizando UNION ALL
      - Utilizar CROSS JOIN, agregações e funções analíticas
      - Aplicar otimização de performance utilizando EXPLAIN ANALYSE
      - Construção de um dashboard consolidado com múltiplas análises em uma única consulta SQL

```sql

-- Query foi otimizada com a ajuda de IA para melhor performance, após essa query poderá ver a orignal, que eu fiz.
--EXPLAIN ANALYSE : Planning Time: 0.580 ms - Execution Time: 0.654 ms
--EXPLAIN analyse

WITH
mes_referencia AS (
	SELECT 
		CAST(DATE_TRUNC('month', MAX(data_pedido)) AS date) AS ultimo_mes
	FROM
		pedidos
),
vendas_mensais AS (
	SELECT
		DATE_TRUNC('month', p.data_pedido) AS mes,
		COUNT(p.pedido_id) AS total_pedidos,
		SUM(p.valor_total) AS total_vendas,
		ROUND(AVG(p.valor_total),2) AS ticket_medio
	FROM pedidos p
	CROSS JOIN mes_referencia mr
	WHERE p.data_pedido >= mr.ultimo_mes
	AND p.data_pedido < mr.ultimo_mes + INTERVAL '1 month'
	GROUP BY mes
),
comparacao_mensal AS (
	SELECT
		mes,
		total_pedidos,
		total_vendas,
		ticket_medio,
		LAG(total_vendas) OVER(ORDER BY mes) AS vendas_mes_anterior,
		ROUND(100 * (total_vendas - LAG(total_vendas) OVER(ORDER BY mes))/
		NULLIF(LAG(total_vendas) OVER(ORDER BY mes), 0),2) AS taxa_crescimento
	FROM
		vendas_mensais
),
top_produtos AS (
	SELECT
		prod.produto_id,
		prod.nome AS nome_produto,
		SUM(ip.quantidade) AS total_vendido,
		SUM(ip.quantidade*ip.preco_unitario) AS receita
	FROM produtos prod
	JOIN itens_pedido ip
	ON prod.produto_id = ip.produto_id
	JOIN pedidos p
	ON ip.pedido_id = p.pedido_id
	CROSS JOIN mes_referencia mr
	WHERE p.data_pedido >= mr.ultimo_mes
	AND p.data_pedido < mr.ultimo_mes + INTERVAL '1 month'
	GROUP BY prod.produto_id, prod.nome
	ORDER BY total_vendido desc
	LIMIT 5	
),
top_clientes AS (
	SELECT
		c.cliente_id,
		c.nome AS nome_cliente,
		COUNT(p.pedido_id) AS qtd_pedidos,
		SUM(p.valor_total) AS total_gasto
	FROM clientes c
	JOIN pedidos p
	ON c.cliente_id = p.cliente_id
	CROSS JOIN mes_referencia mr
	WHERE p.data_pedido >= mr.ultimo_mes
	AND p.data_pedido < mr.ultimo_mes + INTERVAL '1 month'
	GROUP BY c.cliente_id, c.nome
	ORDER BY total_gasto desc
	LIMIT 5
)
SELECT 
	'Resumo Mensal' AS secao,
	TO_CHAR(mes, 'YYYY - MM' ) AS mes_produto, 
	CAST(total_pedidos AS TEXT) AS total_vendido_pedidos, 
	CAST(total_vendas AS TEXT), 
	CAST(ticket_medio AS TEXT), 
	CAST(taxa_crescimento AS TEXT)
FROM comparacao_mensal 
WHERE mes = (SELECT ultimo_mes FROM mes_referencia)
UNION ALL
SELECT
	'Top Produtos' AS secao,
	CAST(nome_produto AS TEXT), 
	CAST(total_vendido AS TEXT),
	CAST(receita AS TEXT), 
	NULL AS ticket_medio, 
	NULL AS taxa_crescimento
FROM top_produtos
UNION ALL
SELECT
	'Top Clientes' AS secao,
	CAST(nome_cliente AS TEXT), 
	CAST(qtd_pedidos AS TEXT),
	CAST(total_gasto AS TEXT),
	NULL AS ticket_medio, 
	NULL AS taxa_crescimento
FROM top_clientes


--EXPLAIN ANALYSE : Planning Time: 0.531 ms - Execution Time: 0.951 ms
WITH
mes_referencia AS (
	SELECT 
		CAST(DATE_TRUNC('month', MAX(data_pedido)) AS date) AS ultimo_mes
	FROM pedidos
),
vendas_mensais AS (
	SELECT
		DATE_TRUNC('month', p.data_pedido) AS mes,
		COUNT(p.pedido_id) AS total_pedidos,
		SUM(p.valor_total) AS total_vendas,
		ROUND(AVG(p.valor_total),2) AS ticket_medio
	FROM pedidos p
	WHERE CAST(DATE_TRUNC('month', p.data_pedido) AS DATE) = (SELECT ultimo_mes FROM mes_referencia)
	GROUP BY mes
),
comparacao_mensal AS (
	SELECT
		mes,
		total_pedidos,
		total_vendas,
		ticket_medio,
		LAG(total_vendas) OVER(ORDER BY mes) AS vendas_mes_anterior,
		ROUND(100 * (total_vendas - LAG(total_vendas) OVER(ORDER BY mes))/
		NULLIF(LAG(total_vendas) OVER(ORDER BY mes), 0),2) AS taxa_crescimento
	FROM vendas_mensais
),
top_produtos AS (
	SELECT
		prod.nome AS nome_produto,
		SUM(ip.quantidade) AS total_vendido,
		SUM(ip.quantidade*ip.preco_unitario) AS receita
	FROM produtos prod
	JOIN itens_pedido ip
	ON prod.produto_id = ip.produto_id
	JOIN pedidos p
	ON ip.pedido_id = p.pedido_id
	WHERE CAST(DATE_TRUNC('month', p.data_pedido) AS DATE) = (SELECT ultimo_mes FROM mes_referencia)
	GROUP BY prod.nome
	ORDER BY total_vendido desc
	LIMIT 5
),
top_clientes AS (
	SELECT
		c.nome AS nome_cliente,
		COUNT(p.pedido_id) AS qtd_pedidos,
		SUM(p.valor_total) AS total_gasto
	FROM clientes c
	JOIN pedidos p
	ON c.cliente_id = p.cliente_id
	WHERE CAST(DATE_TRUNC('month', p.data_pedido) AS DATE) = (SELECT ultimo_mes FROM mes_referencia)
	GROUP BY c.nome
	ORDER BY total_gasto desc
	LIMIT 5
)
	SELECT 
		'Resumo Mensal' AS secao,
		TO_CHAR(mes, 'YYYY - MM' ) AS mes_produto, 
		CAST(total_pedidos AS TEXT) AS total_vendido_pedidos, 
		CAST(total_vendas AS TEXT), 
		CAST(ticket_medio AS TEXT), 
		CAST(taxa_crescimento AS TEXT)
	FROM comparacao_mensal 
	WHERE mes = (SELECT ultimo_mes FROM mes_referencia)
	UNION ALL
	SELECT
		'Top Produtos' AS secao,
		CAST(nome_produto AS TEXT), 
		CAST(total_vendido AS TEXT),
		CAST(receita AS TEXT), 
		NULL AS ticket_medio, 
		NULL AS taxa_crescimento
	FROM top_produtos
	UNION ALL
	SELECT
		'Top Clientes' AS secao,
		CAST(nome_cliente AS TEXT), 
		CAST(qtd_pedidos AS TEXT),
		CAST(total_gasto AS TEXT),
		NULL AS ticket_medio, 
		NULL AS taxa_crescimento
	FROM top_clientes
```
