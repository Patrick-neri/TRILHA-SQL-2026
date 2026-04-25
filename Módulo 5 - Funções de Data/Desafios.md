# 🚀 Desafios SQL - Módulo 5 - Funções de Data
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Iniciante%20→%20Intermediário-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Análise%20de%20Dados-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este repositório contém a resolução dos desafios do **Módulo 5 da Trilha SQL - Instituto NTA**.  

💡 Nesses desafios trabalhei com:

- Manipulação de datas e horas 
- Cálculo de tempo entre eventos (ex: dias desde um pedido ou prazo de entrega)
- Extração de partes da data (ano, mês, dia, dia da semana)
- Formatação de datas para relatórios
- Uso de funções como EXTRACT, AGE, DATE_TRUNC, TO_CHAR e INTERVAL
- Combinação de várias funções
- Tratamento de dados para análise  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL  
✔️ Pensamento analítico  
✔️ Simulação de cenários reais  
✔️ Boas práticas com aliases e organização  
✔️ Leitura e interpretação de dados  

---

## 📌 Aula 24
 🔹 Desafio 1: Calcular quantos dias se passaram desde cada pedido 
   - Exiba pedido_id, data_pedido, data atual e dias desde o pedido
```sql
SELECT * FROM pedidos LIMIT 3;

SELECT 
    pedido_id, 
    data_pedido, 
    current_date AS data_atual, 
    current_date - data_pedido AS dias_desde_o_pedido
FROM pedidos;
```
🔹 Desafio 2: Mostrar idade aproximada dos clientes
  - Exiba o nome, data de nascimento e idade em anos
```sql
SELECT * FROM clientes LIMIT 3;

SELECT 
    nome, 
    data_nascimento, 
    EXTRACT(YEAR FROM AGE(data_nascimento))
FROM clientes
WHERE data_nascimento IS NOT NULL;
```
## 📌 Aula 25
 🔹 Desafio 1: Extrair o mês de cada pedido
   - Exiba o pedido_id, data_pedido e o mês
```sql
SELECT * FROM pedidos LIMIT 3;

SELECT 
    pedido_id, 
    data_pedido, 
    EXTRACT(MONTH FROM data_pedido) AS mes_do_pedido
FROM pedidos;
```
🔹 Desafio 2: Extrair ano, mes e dia de cada pedido
  - Exiba pedido_id, data_pedido, ano, mes e dia separadamente
```sql
SELECT 
    pedido_id, 
    data_pedido, 
    EXTRACT(YEAR FROM data_pedido) AS ano_do_pedido,
    EXTRACT(MONTH FROM data_pedido) AS mes_do_pedido,
    EXTRACT(DAYS FROM data_pedido) AS dia_do_pedido
FROM pedidos
ORDER BY ano_do_pedido DESC;
```
## 📌 Aula 26
 🔹 Desafio 1: Calcular prazo de entrega (data_pedido + 7 dias)
    - Exiba pedido_id, data_pedido e prazo_entrega
```sql
SELECT 
    pedido_id, 
    data_pedido, 
    data_pedido + INTERVAL '7days' AS prazo_entrega
FROM pedidos
ORDER BY data_pedido ASC;
```
🔹 Desafio 2: Adicionar 30 dias à data de nascimento dos clientes
  - Exiba nome, data_nascimento e a nova data
```sql
SELECT 
    nome, 
    data_nascimento, 
    data_nascimento + INTERVAL '30 days' AS nova_data
FROM clientes;
```
## 📌 Aula 27
 🔹 Desafio 1: Calcular tempo de entrega em dias (data_entrega - data_pedido)
   - Exiba pedido_id, data_pedido, data_entrega e dias_para_entrega
```sql
SELECT * FROM pedidos LIMIT 3;

SELECT 
    pedido_id, 
    data_pedido, 
    data_entrega_prevista, 
    data_entrega_prevista - data_pedido AS dias_para_entrega
FROM pedidos 
WHERE data_entrega_prevista IS NOT NULL 
ORDER BY dias_para_entrega ASC;
```
 🔹 Desafio 2: Calcular quantos dias após o início do mês o pedido foi feito
   - Exiba pedido_id, data_pedido, início do mês e dias desde o início do mês
```sql
SELECT 
    pedido_id, 
    data_pedido, 
    date_trunc('month', data_pedido) AS inicio_mes, 
    data_pedido - date_trunc('month', data_pedido) AS dias_do_inicio_mes
FROM pedidos;
```
## 📌 Aula 28
   🔹 Desafio 1: Formatar data_pedido como "DD-MM-YYYY"
     - Exiba pedido_id e a data formatada
```sql
SELECT 
    pedido_id, 
    to_char(data_pedido, 'DD-MM-YYYY') AS data_pedido
FROM pedidos;
```
🔹 Desafio 2: Mostrar mês por extenso dos pedidos
   - Exiba pedido_id, data_pedido e o nome do mês
```sql
SELECT 
    pedido_id, 
    data_pedido, 
    to_char(data_pedido, 'TMMonth') AS mes_do_pedido
FROM pedidos;
```
## 🧠 Desafios Finais
---
## 🏁 Desafio Final 1: Relatório de Pedidos com Datas Formatadas
  - Crie um relatório que mostre:
    - pedido_id
    - data_pedido formatada como "DD/MM/YYYY"
    - dia da semana do pedido (número: 0=Dom, 6=Sáb)
    - mês do pedido (número)
```sql
SELECT 
    pedido_id, 
    to_char(data_pedido, 'DD/MM/YYYY') AS data_pedido, 
    EXTRACT(DOW FROM data_pedido) AS dia_semana, 
    EXTRACT(MONTH FROM data_pedido) AS mes
FROM pedidos
ORDER BY data_pedido ASC;
```
## 🏁 Desafio Final 2: Análise de Tempo de Entrega
   - Para pedidos com data_entrega preenchida, mostre:
      - pedido_id
      - data_pedido
      - data_entrega
      - dias para entrega (data_entrega - data_pedido)
  - Dica: Use WHERE data_entrega IS NOT NULL
```sql
SELECT 
    pedido_id, 
    data_pedido, 
    data_entrega_realizada, 
    data_entrega_realizada - data_pedido AS dias_para_entrega
FROM pedidos
WHERE data_entrega_realizada IS NOT NULL
ORDER BY dias_para_entrega ASC;
```
## 🏁 Desafio Final 3: Pedidos Recentes
  -  Mostre os pedidos dos últimos 30 dias:
     - pedido_id
     - data_pedido formatada como "DD/MM/YYYY"
     - data_pedido formatada como "Dia da semana, DD de Mês"
  - Dica: Use WHERE data_pedido >= CURRENT_DATE - INTERVAL '30 days'
```sql
SELECT 
    pedido_id, 
    TO_CHAR(data_pedido, 'DD/MM/YYYY') AS data_pedido, 
    TO_CHAR(data_pedido, 'DD "da semana," DD "de" TMMonth') AS data_pedido_formatado 
FROM pedidos 
WHERE data_pedido >= CURRENT_DATE - INTERVAL '30 days'
ORDER BY data_pedido ASC;
```
## 🏁 Desafio Final 4: Clientes e Idades
   - Crie um relatório de clientes com:
     - nome
     - data_nascimento formatada como "DD/MM/YYYY"
     - idade em anos (use EXTRACT com AGE)
     - Ordene pelos mais velhos primeiro
```sql
SELECT * FROM clientes LIMIT 3;

SELECT 
    nome, 
    TO_CHAR(data_nascimento, 'DD/MM/YYYY'), 
    EXTRACT(YEAR FROM AGE(data_nascimento)) AS idade_em_anos
FROM clientes
ORDER BY idade_em_anos DESC;
```
🔹 Análise adicional
   - Exiba o nome do cliente, a data de nascimento, o mês atual e o mês de aniversário.
   - Em seguida, calcule a diferença em meses entre o mês atual e o mês de nascimento do cliente.
```sql
SELECT 
    nome AS "Nome do Cliente", 
    data_nascimento AS "Data de Nascimento", 
    EXTRACT(MONTH FROM current_date) AS "Mês Atual", 
    EXTRACT(MONTH FROM data_nascimento) AS "Mês de Nascimento", 
    EXTRACT(MONTH FROM data_nascimento) - EXTRACT(MONTH FROM current_date) AS "Diferença dos Meses"
FROM clientes;
```
## 🏁 Desafio Final 5 (Boss Final!): Relatório Completo de Pedidos
   - Crie um relatório detalhado que mostre:
     - pedido_id
     - data_pedido formatada como "Dia da semana, DD de Mês de YYYY"
     - há quantos dias o pedido foi realizado
     - data_entrega formatada (se existir)
```sql
SELECT 
    pedido_id,
    TO_CHAR(data_pedido, 'TMDAY"," DD "de" TMMonth "de" YYYY') AS "Data completa do Pedido",
    EXTRACT(DAY FROM age(current_date, data_pedido)) AS "Pedido realizado a quantos dias",
    TO_CHAR(data_entrega_realizada, 'DD "de" TMMonth "de" YYYY') AS "Data de entrega"
FROM pedidos
WHERE data_entrega_realizada IS NOT NULL 
ORDER BY "Pedido realizado a quantos dias" DESC

```
FROM pedidos
WHERE data_entrega_realizada IS NOT NULL 
ORDER BY "Pedido realizado a quantos dias" DESC;
