# 🚀 Desafios SQL - Módulo 2  

<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Iniciante%20→%20Intermediário-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Análise%20de%20Dados-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este repositório contém a resolução dos desafios do **Módulo 2 da Trilha SQL - Instituto NTA**.  

💡 O objetivo foi evoluir desde consultas básicas até análises mais próximas do mundo real, trabalhando com:

- Seleção de dados  
- Filtros e condições  
- Ordenação  
- Limitação de resultados  
- Tratamento de dados para análise  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL  
✔️ Pensamento analítico  
✔️ Simulação de cenários reais  
✔️ Boas práticas com aliases e organização  
✔️ Leitura e interpretação de dados  

---

# 📌 Desafios por Aula  

## ☑️ Aula 1 
🔹 Desafio 1: Visualizar todos os dados da tabela produtos

```sql
SELECT * FROM produtos LIMIT 3;
```
🔹 Desafio 2: Visualizar todos os dados da tabela clientes
```sql
SELECT * FROM clientes;
```
##  ☑️ Aula 2 
🔹 Desafio 1: Mostrar apenas nome e preço dos produtos
```sql
SELECT nome, preco FROM produtos p
```
🔹 Desafio 2: Mostrar apenas nome, email e cidade dos clientes
```sql
SELECT nome, email, cidade FROM clientes c 
```

##  ☑️ Aula 3
🔹 Desafio 1: Renomear colunas para nomes mais amigáveis
 - Selecione nome, preco e estoque com aliases "Nome do Produto", "Preço (R$)" e "Quantidade em Estoque"
```sql
SELECT nome AS "Nome do Produto", preco AS "Preço (R$)", estoque AS "Quantidade em Estoque" FROM produtos p 
```
🔹 Desafio 2: Criar um relatório de pedidos
```sql
SELECT data_pedido AS "Data da Compra", valor_total AS "Valor Total (R$)", status AS "Status do Pedido" FROM pedidos p 
```
##  ☑️ Aula 4
🔹 Desafio 1: Listar todas as cidades únicas dos clientes
```sql
SELECT DISTINCT cidade FROM clientes c 
```
🔹 Desafio 2: Listar todas as marcas únicas de produtos
```sql
SELECT DISTINCT marca FROM produtos p 
```
##  ☑️ Aula 5
🔹 Desafio 1: Mostrar uma amostra de 5 produtos (todas as colunas)
```sql
SELECT * FROM produtos p LIMIT 5
```
🔹 Desafio 2: Mostrar apenas 3 clientes, exibindo nome e email
```sql
SELECT nome AS "Nome do Cliente", email AS "E-mail"
FROM clientes c
LIMIT 3;
```
##  ☑️ Aula 6
🔹 Desafio 1: Listar produtos ordenados por preço do mais barato ao mais caro
```sql
SELECT nome AS "Nome do Produto", preco AS "Preço do Produto (R$)"
FROM produtos p
ORDER BY "Preço do Produto (R$)" ASC;
```
🔹 Desafio 2: Listar os 10 últimos pagamentos
```sql
SELECT * FROM pagamentos p LIMIT 3;

SELECT valor AS "Pagamento (R$)", data_pagamento AS "Data Pagamento"
FROM pagamentos p
ORDER BY "Data Pagamento" DESC
LIMIT 10;
```
##  ☑️ Aula 7
🔹 Desafio 1: Mostrar apenas pedidos em separação
```sql
SELECT * FROM pedidos p LIMIT 3

SELECT p2.nome AS "Nome do Produto", p.status AS "Status do Pedido"
FROM pedidos p, produtos p2
WHERE p.status = 'em_separacao'
AND p.pedido_id = p2.produto_id;
```
🔹 Desafio 2: Mostrar as últimas 5 avaliações nota 1
```sql
SELECT * FROM avaliacoes a LIMIT 3

SELECT nota AS "Nota da Avaliação"
FROM avaliacoes a
WHERE nota = 1;
```
##  ☑️ Aula 8
🔹 Desafio 1: Produtos com preço maior que R$ 500
```sql
SELECT * FROM produtos p LIMIT 1

SELECT nome AS "Nome do Produto", preco AS "Preço (R$)"
FROM produtos
WHERE preco > 500
ORDER BY "Preço (R$)" ASC;
```
🔹 Desafio 2: Produtos com estoque menor que 20 unidades
```sql
SELECT nome AS "Nome do Produto", estoque AS "Estoque de Produtos"
FROM produtos
WHERE estoque < 20
ORDER BY "Estoque de Produtos" DESC ;
```
##  ☑️ Aula 9
🔹 Desafio 1: Pedidos com status diferente de "Entregue"
```sql
SELECT * FROM pedidos LIMIT 10

SELECT status AS "Status do Pedido"
FROM pedidos
WHERE status <> 'Entregue'
```
🔹 Desafio 2: Listar as avaliações sem comentários
```sql
SELECT * FROM avaliacoes LIMIT 1

SELECT avaliacao_id, comentario AS "Comentários do Cliente"
FROM avaliacoes
WHERE comentario = NULL
```
##  ☑️ Aula 10
🔹 Desafio 1: Produtos da marca "Samsung" com preço maior que R$ 1000
```sql
SELECT *
FROM produtos
WHERE marca = 'Samsung'
AND preco > 1000
```
🔹 Desafio 2: Pedidos entregues que possuem data de entrega registrada
```sql
SELECT *
FROM pedidos
WHERE status = 'entregue'
AND data_entrega_realizada IS NOT NULL
```
##  ☑️ Aula 11
🔹 Desafio 1: Produtos premium de marcas específicas
	- Encontre produtos que sejam: marca "Samsung" OU marca "Sony" E preço maior que 2000
```sql
SELECT * FROM produtos LIMIT 15

SELECT nome AS "Nome do Produto", marca AS "Marca do Produto", preco AS "Preço do Produto (R$)"
FROM produtos
WHERE(marca = 'Samsung'
	OR marca = 'LG')
AND preco >= 2000
ORDER BY "Preço do Produto (R$)"ASC;
```
🔹 Desafio 2: Pagamentos problemáticos
	- Encontre pagamentos que sejam Pix ou boleto e não tenham sido aprovados
```sql
SELECT * FROM pagamentos p LIMIT 10

SELECT metodo AS "Forma de Pagamento", valor, status AS "Status do Pagamento"
FROM pagamentos
WHERE(metodo = 'pix'
	OR metodo = 'boleto')
AND status = 'recusado'
```
# 🧠 Desafios Finais
## 🏁 Desafio Final 1: Catálogo de Produtos Premium
 - Liste nome, marca e preço dos produtos com preço maior que R$ 2000
 - Ordene do mais caro para o mais barato
 - Renomeie as colunas para "Produto", "Fabricante" e "Valor (R$)"
```sql
SELECT * FROM produtos LIMIT 3

SELECT nome AS "Produto", marca AS "Fabricante", preco AS "Valor (R$)"
FROM produtos
WHERE preco > 2000
ORDER BY "Valor (R$)" ASC;
```
## 🏁 Desafio Final 2: Análise de Estoque Crítico
 - Encontre produtos com estoque menor que 50 unidades OU preço menor que R$ 200
 - Mostre nome, estoque e preço, ordenados por estoque (menor primeiro)
 - Limite a 10 resultados
```sql
SELECT * FROM produtos LIMIT 3

SELECT nome AS "Produto", preco AS "Valor (R$)", estoque AS "Qtde. em Estoque"
FROM produtos
WHERE estoque < 50
OR preco < 200
ORDER BY "Qtde. em Estoque" ASC;
```
## 🏁 Desafio Final 3: Clientes por Região
 - Liste todos os estados únicos onde há clientes cadastrados
 - Ordene em ordem alfabética
```sql
SELECT * FROM clientes LIMIT 3

SELECT DISTINCT estado FROM clientes c 
ORDER BY estado ASC;
```
## 🏁 Desafio Final 4: Relatório de Pedidos
 - Mostre os 15 pedidos mais recentes com status "entregue"
 - Exiba data_pedido (como "Data"), valor_total (como "Total") e status
 - Ordene pela data mais recente primeiro
```sql
SELECT * FROM pedidos LIMIT 2

SELECT data_pedido AS "Data", valor_total AS "Total", status
FROM pedidos
WHERE status = 'entregue'
ORDER BY "Data" DESC
LIMIT 15;
```
## 🏁 Desafio Final 5: Produtos em Destaque (Desafio Avançado)
 - Encontre produtos que sejam:
 - (marca "Samsung" E preço > 1000) OU (marca "Sony" E estoque > 100)
 - Mostre nome, marca, preço e estoque
 - Ordene por preço decrescente
```sql
SELECT * FROM produtos p LIMIT 3

SELECT nome AS "Produto", marca AS "Fabricante", preco AS "Valor (R$)", estoque
FROM produtos
WHERE(marca = 'Samsung'
	AND preco > 1000)
OR (marca = 'Sony'
	AND estoque > 100 )
ORDER BY "Valor (R$)" DESC;
```
## 🏁 Desafio Final 6: Análise de Avaliações
 - Liste as 10 piores avaliações (nota = 1 ou nota = 2)
 - Mostre nota (como "Estrelas") e comentario (como "Feedback")
 - Ordene pela nota (menor primeiro)
```sql
SELECT * FROM avaliacoes LIMIT 3

SELECT nota AS "Estrelas", comentario AS "Feedback"
FROM avaliacoes
WHERE(nota = 1
	OR nota = 2)
AND comentario IS NOT NULL
ORDER BY "Estrelas" ASC
LIMIT 10;
```
## 🏁 Desafio Final 7: Pagamentos Pendentes
 - Encontre pagamentos com status diferente de "aprovado"
 - Mostre metodo (como "Forma de Pagamento"), valor e status
 - Ordene pelo valor (maior primeiro), limitado a 20 resultados
```sql
SELECT * FROM pagamentos LIMIT 3

SELECT metodo AS "Forma de Pagamento", valor AS "Valor", status AS "Status"
FROM pagamentos
WHERE status <> 'aprovado'
ORDER BY "Valor" DESC
LIMIT 20;
```
## 🏁 Desafio Final 8: Relatório Completo (Boss Final!)
 - Crie uma consulta que mostre:
    - Nome do produto (como "Produto")
    - Marca (como "Fabricante")
    - Preço (como "Preço (R$)")
    - Estoque (como "Qtd Disponível")
 - Filtros: marca = "Samsung" OU marca = "LG" OU marca = "Sony"
    - E preço entre 1000 e 5000 (use >= e <=)
    - E estoque > 0
 - Ordenado por marca (A-Z), depois por preço (menor para maior)
 - Limitado aos 20 primeiros resultados
```sql
SELECT * FROM produtos LIMIT 3

SELECT nome AS "Nome do Produto", marca AS "Fabricante", preco AS "Preço (R$)", estoque AS "Qtd Disponível"
FROM produtos
WHERE(marca = 'Samsung'
	OR marca = 'LG'
	OR marca = 'Sony')
AND preco >= 1000
AND preco <= 5000
AND estoque > 0
ORDER BY "Fabricante" ASC, "Preço (R$)" ASC
LIMIT 20;
```

