# 🚀 Desafios SQL - Módulo 10 - JOINs e Relatórios
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-JOINs%20e%20Relatórios-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este conjunto de desafios aborda o uso de JOINs em SQL, permitindo combinar dados de múltiplas tabelas para gerar análises completas e relatórios estratégicos.

💡 Nesses desafios trabalhei com:

- INNER JOIN para relacionar tabelas  
- LEFT JOIN e RIGHT JOIN para manter dados não relacionados  
- FULL OUTER JOIN para análises completas  
- SELF JOIN para comparação entre registros  
- Combinação de múltiplas tabelas  
- Construção de relatórios complexos  
- Aplicação de regras de negócio em consultas  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL com múltiplos JOINs  
✔️ Modelagem lógica de relacionamentos  
✔️ Construção de relatórios analíticos completos  
✔️ Pensamento analítico aplicado a dados  
✔️ Integração de múltiplas fontes de dados  

---

## 📌 Aula 42

🔹 Desafio 1 - Listar todos os produtos com o nome de suas categorias                                                                            
                  
  - Mostre: nome do produto, preço, nome da categoria
```sql
SELECT * FROM produtos LIMIT 4

SELECT * FROM categorias LIMIT 4

SELECT
	p.nome AS "Nome do Produto",
	p.preco AS "Preço (R$)",
	c.nome AS "Categoria"
FROM
	produtos p
JOIN 
	categorias c ON p.categoria_id = c.categoria_id
```

🔹 Desafio 2 - Listar todos os pedidos com o nome dos clientes 
      
  - Mostre: pedido_id, data_pedido, valor_total, nome do cliente, email do cliente
```sql
SELECT
	p.pedido_id AS "Id do Pedido",
	CAST(p.data_pedido AS DATE) AS "Data do Pedido",
	p.valor_total AS "Valor total do Pedido",
	c.nome AS "Nome do cliente",
	c.email AS "E-mail do cliente"
FROM 
	pedidos p
JOIN 
	clientes c 
ON 
	p.cliente_id = c.cliente_id
```

---

## 📌 Aula 43

🔹 Desafio 1 - Listar todos os produtos, incluindo os que nunca foram vendidos 
     
  - Mostre: nome do produto, preço, quantidade vendida (ou NULL se nunca vendido)
  - Dica: Use a tabela itens_pedido para verificar vendas
```sql
SELECT
	p.nome AS "Nome do Produto",
	p.preco AS "Preço (R$)",
	i.quantidade AS "Quantidade Vendida"
FROM
	produtos p
LEFT JOIN
	itens_pedido i
ON
	p.produto_id = i.produto_id
ORDER BY
	i.quantidade DESC
```

🔹 Desafio 2 - Listar todos os clientes, incluindo os que nunca fizeram pedidos

  - Mostre: nome do cliente, email, quantidade de pedidos (0 se nunca fez)
  - Dica: Use COUNT() que conta 0 para NULL
```sql
SELECT
	c.nome,
	c.email,
	COUNT(p.pedido_id) AS "Qtd de Pedidos"
FROM
	clientes c
LEFT JOIN
	pedidos p
ON
	c.cliente_id = p.cliente_id
GROUP BY
	c.nome,
	c.email
ORDER BY
	c.nome ASC
```

---

## 📌 Aula 44

🔹 Desafio 1 - Listar todos os pedidos e seus pagamentos (mantendo pedidos sem pagamento)
```sql
SELECT
	p.pedido_id,
	p.valor_total,
	pag.metodo,
	pag.valor AS valor_pagamento 
FROM
	pagamentos pag
RIGHT JOIN
	pedidos p
ON
	p.pedido_id = pag.pedido_id
```

🔹 Desafio 2 - Reescreva o desafio anterior usando LEFT JOIN
```sql
SELECT
	p.pedido_id,
	p.valor_total,
	pag.metodo,
	pag.valor AS valor_pagamento 
FROM
	pedidos p
RIGHT JOIN
	pagamentos pag
ON
	p.pedido_id = pag.pedido_id
```

---

## 📌 Aula 45

🔹 Desafio 1 - Listar todos os produtos e todas as avaliações
      
  - Mostre produtos sem avaliação E avaliações (se houver alguma órfã)
  - Mostre: produto_id, nome do produto, avaliacao_id, nota 
```sql
SELECT
	p.produto_id,
	p.nome,
	a.avaliacao_id,
	a.nota
FROM
	produtos p
FULL OUTER JOIN
	avaliacoes a
ON
	p.produto_id = a.produto_id
ORDER BY
	p.produto_id ASC
```

🔹 Desafio 2 - Produtos com baixo engajamento de avaliações

  - Liste produtos com poucas ou nenhuma avaliação (menos de 3 avaliações)
  - Use FULL OUTER JOIN para garantir que todos os produtos sejam incluídos 
```sql
SELECT
	p.nome,
	COUNT(p.nome) AS qtd_avaliacoes
FROM
	produtos p
FULL OUTER JOIN
	avaliacoes a
ON
	p.produto_id = a.produto_id
GROUP BY
	p.nome
HAVING
	COUNT(p.nome) <3
ORDER BY
	qtd_avaliacoes ASC
```

---

## 📌 Aula 46

🔹 Desafio 1-  Encontrar clientes que moram na mesma cidade
    
  - Mostre: nome do cliente 1, nome do cliente 2, cidade
  - Evite duplicatas e auto-comparação 
```sql
SELECT
	c1.nome AS cliente1,
	c2.nome AS cliente2,
	c1.cidade
FROM
	clientes c1
JOIN
	clientes c2
ON 
	c1.cliente_id = c2.cliente_id
AND
	c1.cliente_id < c2.cliente_id
ORDER BY
	c1.nome
```

🔹 Desafio 2 - Encontrar produtos da mesma categoria com preços similares

  - Considere "similar" uma diferença menor que R$ 100
  - Mostre: produto1, preco1, produto2, preco2, categoria_id, diferença de preço
```sql
SELECT
	p1.nome AS produto1,
	p1.preco AS preco1,
	p2.nome AS produto2,
	p2.preco AS preco2,
	p1.categoria_id,
	ABS(p1.preco - p2.preco) AS diferenca_preco
FROM
	produtos p1
JOIN
	produtos p2
ON
	p1.produto_id < p2.produto_id
AND
	ABS(p1.preco - p2.preco) < 100
```

---

## 📌 Aula 47

🔹 Desafio 1 -  Listar pedidos com nome do cliente, produtos comprados e categoria

  - Mostre: pedido_id, data_pedido, nome do cliente, nome do produto, categoria, quantidade
  - Ordene por pedido_id
```sql
SELECT
	p.pedido_id,
	p.data_pedido,
	c.nome AS nome_cliente,
	prod.nome AS nome_produto,
	cat.nome AS categoria,
	i.quantidade 
FROM
	clientes c
JOIN pedidos p ON p.cliente_id = c.cliente_id
JOIN itens_pedido i ON p.pedido_id = i.pedido_id
JOIN produtos prod ON i.produto_id = prod.produto_id
JOIN categorias cat ON prod.categoria_id = cat.categoria_id
ORDER BY
	p.pedido_id
```

🔹 Desafio 2 - Criar relatório completo de vendas

  - Mostre: pedido_id, cliente, cidade do cliente, produto, categoria,
  - quantidade, valor unitário, subtotal (qtd * valor), forma de pagamento
  - Apenas pedidos com status 'entregue'
  - Ordene por data do pedido (mais recente primeiro) 
```sql
SELECT
	p.pedido_id,
	p.data_pedido,
	c.nome AS nome_cliente,
	c.cidade AS cidade_cliente,
	prod.nome AS produto,
	cat.nome AS categoria,
	i.quantidade,
	i.preco_unitario,
	SUM(i.quantidade * i.preco_unitario) AS subtotal,
	pag.metodo
FROM
	clientes c
JOIN pedidos p ON c.cliente_id = p.cliente_id
JOIN itens_pedido i ON i.pedido_id = p.pedido_id
JOIN produtos prod ON prod.produto_id = i.produto_id
JOIN categorias cat ON cat.categoria_id = prod.categoria_id
JOIN pagamentos pag ON pag.pedido_id = p.pedido_id
WHERE
	p.status = 'entregue'
GROUP BY
	p.pedido_id,
	p.data_pedido,
	c.nome,
	c.cidade,
	prod.nome,
	cat.nome,
	i.quantidade,
	i.preco_unitario,
	pag.metodo
ORDER BY
	p.data_pedido DESC
```

---

## 🧠 Desafios Finais
---

## 🏁 Desafio Final 1 -Catálogo Completo
  - Liste todos os produtos com suas categorias
    - Mostre: nome do produto, preço, estoque, marca, nome da categoria
    - Inclua produtos mesmo que a categoria esteja inativa
    - Ordene por categoria e depois por nome do produto
```sql
SELECT
	p.nome AS nome_produto,
	p.preco,
	p.estoque,
	p.marca,
	c.nome AS nome_categoria
FROM
	produtos p
JOIN
	categorias c
ON
	p.categoria_id = c.categoria_id
ORDER BY
	c.nome,
	p.nome

```

## 🏁 Desafio Final 2: Clientes sem Compras
  - Encontre todos os clientes que nunca fizeram pedidos
  - Mostre: nome, email, cidade, estado, data de cadastro
  - Ordene por data de cadastro (mais antigos primeiro)

```sql
SELECT
	c.cliente_id,
	c.nome AS nome_cliente,
	c.email,
	c.cidade,
	c.estado,
	c.data_cadastro
FROM
	clientes c
LEFT JOIN
	pedidos p
ON 
	c.cliente_id = p.cliente_id
WHERE 
	p.cliente_id IS null
ORDER BY
	c.data_cadastro ASC
```
	

## 🏁 Desafio Final 3: Análise de Pagamentos
  - Liste todos os pedidos com informações de pagamento
  - Mostre: pedido_id, nome do cliente, valor do pedido, método de pagamento, status do pagamento
  - Inclua pedidos que ainda não têm pagamento registrado
  - Ordene por valor do pedido (maior primeiro)
```sql
SELECT
	p.pedido_id,
	c.nome AS nome_cliente,
	p.valor_total,
	pag.metodo AS metodo_pagamento,
	pag.status AS status_pagamento
FROM
	clientes c
JOIN
	pedidos p
ON
	c.cliente_id = p.cliente_id
JOIN
	pagamentos pag
ON
	p.pedido_id = pag.pedido_id
ORDER BY
	p.valor_total DESC;
```

	
## 🏁 Desafio Final 4: Produtos Mais Avaliados
  - Liste produtos que têm avaliações, com estatísticas
  - Mostre: nome do produto, categoria, quantidade de avaliações, nota média
    - Apenas produtos com mais de 1 avaliação
  - Ordene por nota média (maior primeiro)
```sql
SELECT
	p.nome AS nome_produto,
	c.nome AS categotia,
	COUNT(a.comentario) AS qtd_avaliacoes,
	ROUND(AVG(a.nota),2) AS nota_media
FROM
	produtos p
JOIN
	categorias c
ON
	p.categoria_id = c.categoria_id
JOIN
	avaliacoes a
ON
	p.produto_id = a.produto_id
GROUP BY
	p.nome,
	c.nome
HAVING
	COUNT(a.comentario)>1
ORDER BY
	nota_media DESC
```



## 🏁 Desafio Final 5: Relatório de Vendas por Cliente (Desafio Avançado)
  - Para cada cliente que fez pedidos, mostre:
    - Nome do cliente, cidade, estado, total de pedidos, valor total gasto, ticket médio
    - Apenas clientes com mais de 1 pedido
  - Ordene por valor total gasto (maior primeiro)
```sql
SELECT
	c.nome AS nome_cliente,
	c.cidade,
	c.estado,
	COUNT(p.pedido_id) AS total_pedidos,
	SUM(p.valor_total) AS total_gasto,
	ROUND(AVG(p.valor_total),2) AS ticket_medio
FROM
	clientes c
JOIN
	pedidos p
ON
	c.cliente_id = p.cliente_id
GROUP BY
	c.nome,
	c.cidade,
	c.estado
HAVING
	COUNT(p.pedido_id)>1
ORDER BY
	total_gasto DESC
```
## 🏁 Desafio Final 6: Dashboard Completo (Boss Final!)
  - Crie um relatório detalhado de vendas que mostre:
     - pedido_id
     - data do pedido
     - nome do cliente
     - cidade e estado do cliente
     - nome do produto
     - categoria do produto
     - quantidade comprada
     - preço unitário
     - subtotal (quantidade * preço unitário)
     - método de pagamento
  - Filtros:
     - Apenas pedidos com status 'entregue' ou 'enviado'
     - Apenas produtos de categorias ativas
   Ordene por data do pedido (mais recente primeiro), depois por pedido_id

```sql
	
SELECT
	p.pedido_id,
	p.data_pedido,
	c.nome AS nome_cliente,
	CONCAT(c.cidade, ' - ',c.estado) AS cidade_estado,
	pd.nome AS nome_produto,
	cat.nome AS categoria_produto,
	ip.quantidade AS quantidade_comprada,
	ip.preco_unitario,
	(ip.quantidade * ip.preco_unitario) AS subtotal,
	pag.metodo AS metodo_pagamento
FROM
	clientes c
JOIN pedidos p ON c.cliente_id = p.cliente_id
JOIN pagamentos pag ON p.pedido_id = pag.pedido_id
JOIN itens_pedido ip ON p.pedido_id = ip.pedido_id
JOIN produtos pd ON pd.produto_id = ip.produto_id
JOIN categorias cat ON pd.categoria_id = cat.categoria_id
WHERE p.status = 'entregue' OR p.status = 'enviado'
AND	cat.ativo = TRUE
ORDER BY
	p.data_pedido DESC,
	p.pedido_id;
```
