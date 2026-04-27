# 🚀 Desafios SQL - Módulo 6 - Conversão e Tratamento de Dados
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Iniciante%20→%20Intermediário-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Tratamento%20de%20Dados-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este conjunto de desafios aborda **conversão de tipos de dados e tratamento de valores nulos**, aplicados a cenários comuns de análise.

💡 Nesses desafios trabalhei com:

- CAST para conversão de tipos  
- Manipulação de dados com COALESCE  
- Uso de NULLIF  
- Padronização de valores nulos  
- Construção de relatórios com dados tratados  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL  
✔️ Tratamento de dados inconsistentes  
✔️ Conversão de tipos  
✔️ Pensamento analítico  
✔️ Construção de relatórios  

---

## 📌 Aula 29

🔹 Desafio 1: Converter preço para inteiro (sem centavos)  
   - Exiba nome, preco original e preco convertido para inteiro
```sql
SELECT 
    nome, 
    preco AS "Preço Original",
    CAST(preco AS INTEGER) AS "Preço Convertido"
FROM 
    produtos 
```
🔹 Desafio 2: Converter data_pedido para texto
   - Exiba pedido_id e data_pedido como texto
```sql
SELECT * FROM pedidos LIMIT 4

SELECT 
    pedido_id,
    CAST(data_pedido AS varchar) AS "Data do Pedido em texto"
FROM
    pedidos
```
## 📌 Aula 30

🔹 Desafio 1: Substituir comentários nulos por "Sem comentarios"
   - Exiba o ID da avaliação, o ID do cliente e o comentário tratado
```sql
SELECT * FROM avaliacoes a  LIMIT 2

SELECT 
    avaliacao_id, 
    cliente_id,
    COALESCE(comentario, 'Sem comentário') AS "Comentário Tratado"
FROM 
    avaliacoes
```
🔹 Desafio 2: Simular correção de pedido com erro de sistema
   - Exiba o pedido 7 com status "cancelado" e data de entrega realizada ajustada para NULL
```sql
SELECT * FROM pedidos LIMIT 7

SELECT
    pedido_id, 
    cliente_id,
    'cancelado' AS status,
    NULLIF(data_entrega_realizada, data_entrega_realizada) AS ajuste_data
FROM 
    pedidos 
WHERE
    pedido_id = 7
```
## 🧠 Desafios Finais
---

## 🏁 Desafio Final 1: Ficha de Clientes Completa
   - Crie um relatório com:
      - cliente_id convertido para texto com prefixo "CLI-"
      - nome
      - telefone (ou "Não informado" se nulo)
      - email (ou "Não informado" se nulo)
```sql
SELECT 
    CONCAT('CLI', CAST(cliente_id AS VARCHAR)) AS "Prefixo Cliente",
    nome AS "Nome do Cliente",
    COALESCE(telefone, 'Não Informado') AS "Telefone",
    COALESCE(email, 'Não Informado') AS "E-mail"
FROM
    clientes
ORDER BY "Prefixo Cliente"
```
## 🏁 Desafio Final 2: Relatório de Produtos com Preços
   - Mostre:
      - nome do produto
      - preco original
      - preco como inteiro (sem centavos)
      - "R$ " concatenado com o preço (como texto)
```sql
SELECT 
    nome AS "Nome do Produto",
    preco AS "Preço Original",
    CAST(preco AS INTEGER) AS "Preço como inteiro",
    CONCAT('R$ ', CAST(preco AS VARCHAR)) AS "Preço Formatado"
FROM
    produtos
```
## 🏁 Desafio Final 3: Pedidos com Valor Final
   - Calcule o valor final dos pedidos considerando:
       - valor_total
       - frete (pode ser NULL, tratar como 0)
       - desconto (pode ser NULL, tratar como 0)
       - valor_final = valor_total + frete - desconto
```sql
SELECT * FROM pedidos LIMIT 3

SELECT
    pedido_id,
    valor_total,
    COALESCE(frete, '0') AS frete,
    COALESCE(desconto, '0') AS desconto,
    CONCAT('R$ ', valor_total + COALESCE(frete, 0) - COALESCE(desconto, 0)) AS valor_final
FROM
    pedidos p
```
