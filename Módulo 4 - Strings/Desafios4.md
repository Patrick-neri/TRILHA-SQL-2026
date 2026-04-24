# 🚀 Desafios SQL - Módulo 4 - Strings
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Iniciante%20→%20Intermediário-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Análise%20de%20Dados-orange?style=for-the-badge"/>
</p>

---

## 📚 Sobre o Projeto  

Este repositório contém a resolução dos desafios do **Módulo 4 da Trilha SQL - Instituto NTA**.  

💡 Nesses desafios trabalhei com:

- Manipulação de Strings 
- CONCATE 
- UPPER e LOWER
- SUBSTRING e TRIM
- Uso da função POSITION
- Combinação de várias funções de String
- Tratamento de dados para análise  

---

## 🧠 Habilidades Desenvolvidas  

✔️ Escrita de queries SQL  
✔️ Pensamento analítico  
✔️ Simulação de cenários reais  
✔️ Boas práticas com aliases e organização  
✔️ Leitura e interpretação de dados  

---


## 📌 Aula 18
🔹 Desafio 1: Crie uma coluna com "Nome - Marca" para produtos
  - Exiba o nome do produto, a marca e a nova coluna concatenada.
```sql
SELECT nome, marca, concat(nome, ' - ', marca) AS Nome_Marca FROM produtos
```
🔹 Desafio 2: Crie o endereço completo dos clientes
  - Concatene cidade, estado e CEP em uma única coluna chamada "Endereço Completo".
  - Ex: "São Paulo - SP, 01000-000"
```sql
SELECT * FROM clientes LIMIT 1

SELECT nome, concat(cidade, ' - ', estado, ', ', cep) AS "Endereço Completo" FROM clientes 
```
## 📌 Aula 19
🔹 Desafio 1: Mostrar todos os nomes de produtos em maiúsculas
  - Exiba o nome original e o nome em maiúsculas para comparação.
```sql
SELECT * FROM produtos LIMIT 3

SELECT nome, UPPER(nome) AS nomes_maisculos
FROM produtos
```
🔹 Desafio 2: Mostrar nomes de clientes em maiúsculas
  - Crie um relatório com os nomes de todos os clientes em caixa alta.
```sql
SELECT * FROM clientes LIMIT 1

SELECT UPPER(nome) AS nomes_clientes_maisculos
FROM clientes 
```
## 📌 Aula 20
🔹 Desafio 1: Mostrar todos os emails dos clientes em minúsculas
  - Ideal para criar uma lista de marketing padronizada.
```sql
SELECT * FROM clientes LIMIT 1

SELECT nome, LOWER(email)
FROM clientes
```
🔹 Desafio 2: Padronizar nomes de categorias em minúsculas
  - Exiba os nomes das categorias em sua forma original e em minúsculas.
```sql
SELECT * FROM categorias 

SELECT LOWER(nome) AS nome_categorias
FROM categorias
```
## 📌 Aula 21
🔹 Desafio 1: Extrair apenas o primeiro nome dos clientes
  - Dica: Use a função POSITION(' ' IN nome) para encontrar o primeiro espaço.
```sql
SELECT * FROM clientes LIMIT 10

SELECT nome AS nome_completo, substring(nome FROM 1 FOR POSITION(' ' IN nome)) AS primeiro_nome
FROM clientes

SELECT nome AS nome_completo, SUBSTRING(nome FROM 1 FOR POSITION(' ' IN nome) - 1) AS primeiro_nome
FROM clientes;
```
🔹 Desafio 2: Extrair o DDD dos telefones dos clientes
  - Considere formatos como '(XX) YYYYY-ZZZZ' ou 'XX YYYYY ZZZZ'.
```sql
SELECT substring(telefone FROM 1 FOR POSITION(' ' IN telefone))
FROM clientes

SELECT nome, telefone, SUBSTRING(telefone FROM 2 FOR 3) AS ddd
FROM clientes
WHERE telefone IS NOT NULL
AND telefone LIKE '(%)%';
```
## 📌 Aula 22
🔹 Desafio 1: Limpar espaços extras nos nomes de produtos
  - Mostre o nome original e o nome limpo.
```sql
SELECT * FROM produtos LIMIT 1

SELECT nome AS "Nome Original", TRIM(nome)
FROM produtos 
```
🔹 Desafio 2: Remover espaços de emails antes de comparar
  - Encontre os clientes cujo email, após remoção de espaços, comece com 'c' e termine com '.com'.
```sql
SELECT * FROM clientes LIMIT 5

SELECT TRIM(email) AS "E-mails" FROM clientes
WHERE email LIKE 'c%'
AND email LIKE '%.com'
```
## 📌 Aula 23
🔹 Desafio 1: Mostrar produtos cujo nome tenha mais de 20 caracteres
  - Exiba o nome e o comprimento.
```sql
SELECT nome AS "Nome do Produto", length(nome) AS "Comprimento do Nome"
FROM produtos
WHERE length(nome) BETWEEN 20 AND 100
ORDER BY "Comprimento do Nome" ASC

🔹 Desafio 2: Validar CPFs com tamanho inválido
  - Liste os clientes com CPFs que não tenham exatamente 14 caracteres.
```sql
SELECT * FROM clientes LIMIT 3

SELECT nome AS "Nome do Cliente", cpf AS "Cpf Inválido"
FROM clientes
WHERE length(cpf) <> 14
```
## 🧠 Desafios Finais

## 🏁 Desafio Final 1: Relatório de Contato Padronizado
```sql
SELECT * FROM clientes LIMIT 5

SELECT UPPER(nome) AS "Nome do Cliente", LOWER(email), CONCAT(cidade, ' (', estado, ')') AS "Localização"
FROM clientes
ORDER BY "Nome do Cliente" ASC
```
## 🏁 Desafio Final 2: Análise de Marcas
```sql
SELECT DISTINCT UPPER(marca) AS "Fabricante", LENGTH(marca) AS "Número de Caracteres"
FROM produtos
ORDER BY "Fabricante"
```
## 🏁 Desafio Final 3: Nomes Curtos de Produtos
```sql
SELECT TRIM(nome) AS "Nomes dos Produtos", LENGTH(nome) AS "Comprimento do Nome"
FROM produtos
WHERE LENGTH(nome) BETWEEN 0 AND 10
```
## 🏁 Desafio Final 4: Gerador de Nome de Usuário (Avançado)
 - Regras:
   - 3 primeiras letras do primeiro nome (minúsculas)
   - comprimento do nome completo
   - Exemplo: "Ana Clara" → ana8
```sql
SELECT * FROM clientes LIMIT 5

SELECT nome AS "Nome Original", CONCAT(SUBSTRING(lower(nome) FROM 1 FOR POSITION(' ' IN nome)-1), LENGTH(nome))
FROM clientes
ORDER BY "Nome Original"
```
