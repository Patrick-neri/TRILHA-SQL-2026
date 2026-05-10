# 🚀 Desafios SQL - Módulo 11 - UNION, EXCEPT e INTERSECT
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Operações%20de%20Conjunto-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este conjunto de desafios aborda operações de conjunto em SQL, utilizando UNION, UNION ALL, EXCEPT e INTERSECT para combinar, comparar e analisar dados de múltiplas tabelas.

💡 Nesses desafios trabalhei com:

- UNION para unificar resultados  
- UNION ALL para manter duplicidades  
- EXCEPT para identificar diferenças entre conjuntos  
- INTERSECT para encontrar dados em comum  
- Construção de timelines e históricos  
- Relatórios consolidados  
- Operações analíticas com múltiplas tabelas  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL avançadas  
✔️ Manipulação de conjuntos de dados  
✔️ Consolidação de informações  
✔️ Construção de relatórios analíticos  
✔️ Pensamento analítico aplicado a SQL  

---

## 📌 Aula 48

🔹 Desafio 1 - Lista unificada de marcas e categorias

  - Combine marcas de produtos + nomes de categorias em uma única lista
  - Adicione coluna "origem" identificando se é 'produtos' ou 'categorias'
  - Exclua marcas nulas e garanta que não haja duplicatas

```sql
SELECT
	nome, 'produtos' AS origem FROM produtos
UNION
SELECT
	nome, 'categorias' AS origem FROM categorias
```
🔹 Desafio 2 - Lista unificada de todos os status do sistema     

   - Combine status de pedidos + status de pagamentos em uma única lista
   - Adicione coluna "origem" para identificar de qual tabela vem cada status
   - Liste apenas valores únicos (sem duplicatas)
```sql
SELECT
	status, 'pedidos' AS origem FROM pedidos
UNION
SELECT
	status, 'pagamentos' AS origem FROM pagamentos
```
## 📌 Aula 49

🔹 Desafio 1 - Clientes que compraram mas nunca avaliaram

  - Encontre clientes que fizeram pedidos MAS NUNCA avaliaram produtos
```sql
SELECT
	cliente_id FROM clientes
EXCEPT 
SELECT 
	cliente_id FROM avaliacoes
ORDER BY cliente_id ASC 
```
🔹 Desafio 2 - Criar histórico completo de ações

  - Combine pedidos e pagamentos em uma linha do tempo
  - Inclua: data, valor, tipo de ação ('Pedido realizado' ou 'Pagamento registrado')
```sql
SELECT
	data_pedido AS data, valor_total, 'Pedido realizado' AS acao FROM pedidos
UNION ALL
SELECT
	data_pagamento AS data, valor, 'Pagamento registrado' AS acao FROM pagamentos
ORDER BY
	data DESC;
```
🔹 Desafio 3 - Clientes engajados (que compraram E avaliaram)

  - Encontre clientes que fizeram pedidos E também avaliaram produtos
  - Use INTERSECT para encontrar IDs que aparecem em ambas as tabelas
```sql
SELECT
	cliente_id FROM clientes
INTERSECT 
SELECT 
	cliente_id FROM avaliacoes
ORDER BY cliente_id ASC
```
----
## 🧠 Desafios Finais
---
## 🏁 Desafio Final 1 - Lista Unificada de Contatos

  - Crie uma lista única com:
    - nome
    - tipo ('Cliente' ou 'Outro')
    - De clientes. Não deve haver nomes duplicados.
```sql
SELECT DISTINCT
    nome,
    'Cliente' AS tipo
FROM clientes
ORDER BY nome;
```
## 🏁 Desafio Final 2 - Histórico Completo de Transações

  - Combine pedidos e pagamentos em uma timeline:
    - data da transação
    - valor
    - tipo ('Pedido' ou 'Pagamento')
    - identificador (pedido_id ou pagamento_id)
  - Ordene por data
```sql
SELECT
	data_pagamento AS data_transacao,
	valor AS valor,
	'Pagamento' AS tipo,
	'pagamento_id' AS identificador
FROM pagamentos

UNION ALL

SELECT
	data_pedido AS data_transacao,
	valor_total AS valor,
	'Pedido' AS tipo,
	'pedido_id' AS identificador
FROM pedidos

ORDER BY
	data_transacao DESC;
```
## 🏁 Desafio Final 3 - Relatório de Totais

   - Mostre um resumo com:
    - Total de produtos
    - Total de clientes
    - Total de pedidos
    - Total de pagamentos
   - Cada linha deve ter: tipo e quantidade
```sql
SELECT
	COUNT(*) AS quantidade,
	'Produtos' AS tipo
FROM produtos

UNION ALL 

SELECT
	COUNT(*) AS quantidade,
	'Pedidos' AS tipo
FROM pedidos

UNION ALL 

SELECT
	COUNT(*) AS quantidade,
	'Clientes' AS tipo
FROM clientes

UNION ALL 

SELECT
	COUNT(*) AS quantidade,
	'Pagamentos' AS tipo
FROM pagamentos

ORDER BY
	quantidade DESC;

```
## 🏁 Desafio Final 4 (Boss Final!) - Análise de Sobreposição

   - Descubra:
      - a) Quantas marcas diferentes existem nos produtos
      - b) Quantas categorias diferentes existem
      - c) Crie uma lista combinada de todas as marcas e nomes de categorias (UNION)
      - d) Se houvesse uma tabela de marcas preferidas do cliente,
        - como você encontraria marcas que existem em produtos
        - E na lista de preferidas? (use INTERSECT conceitualmente)

🔹 a) e b):

```sql
SELECT
	COUNT(DISTINCT marca) AS quantidade, 
	'Marcas' AS tipo_diferente
FROM
	produtos
WHERE marca IS NOT NULL 
UNION ALL
SELECT
	COUNT(DISTINCT nome) AS quantidade,
	'Categorias' AS tiop_diferente
FROM 
	categorias;
```
🔹 c):
```sql
SELECT
	marca AS nome,
	'Marcas' AS tipo
FROM
	produtos
UNION ALL
SELECT
	nome AS nome,
	'Categorias' AS tipo
FROM
	categorias;
```
🔹 d):
```sql
SELECT
	marca
FROM 
	produto
INTERSECT
SELECT
	marca
FROM
	lista_preferida;
```
🔹 Demonstração com dados existentes - marcas que têm produtos em mais de uma categoria
```sql
SELECT
	marca
FROM
	produtos
WHERE marca IS NOT NULL
GROUP BY
	marca
HAVING COUNT(DISTINCT categoria_id) > 1

```
