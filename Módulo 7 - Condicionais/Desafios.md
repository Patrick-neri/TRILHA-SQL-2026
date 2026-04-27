# 🚀 Desafios SQL - Módulo 7 - Estruturas Condicionais (CASE WHEN)
<p align="center">
  <img src="https://img.shields.io/badge/SQL-PostgreSQL-blue?style=for-the-badge&logo=postgresql"/>
  <img src="https://img.shields.io/badge/Nível-Intermediário-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Foco-Lógica%20Condicional-orange?style=for-the-badge"/>
</p>

---

## 📌 Aula 31

🔹 Desafio 1: Classificar produtos como:
  - "Barato" (< R$250),
  - "Medio" (R$250-1000),
  - "Caro" (> R$1000)
```sql
SELECT * FROM produtos LIMIT 4

SELECT 
	nome, 
CASE 
	WHEN preco < 250 THEN 'Barato'
	WHEN preco >= 250 AND preco < 1000 THEN 'Medio'
	ELSE 'Caro'
END AS classificacao
FROM 
	produtos
```

🔹 Desafio 2: Classificação de clientes por faixa etária
   - Utilize a data de nascimento para calcular a idade dos clientes
      e classifique cada um em uma faixa etária, conforme as regras abaixo:
       - Jovem   → idade menor que 30 anos
       - Adulto  → idade entre 30 e 50 anos
       - Senior  → idade maior que 50 anos
   - Exiba: nome, data_nascimento, idade calculada e faixa_etaria.
```sql
SELECT EXTRACT(YEAR FROM age(data_nascimento)) FROM clientes 

SELECT 
	nome,
CASE
	WHEN EXTRACT(YEAR FROM age(data_nascimento)) < 30 THEN 'Jovem'
	WHEN EXTRACT(YEAR FROM age(data_nascimento)) >= 30 AND EXTRACT(YEAR FROM age(data_nascimento)) < 50 THEN 'Adulto'
	ELSE 'Senior'	
END AS "Faixa Etária"
FROM 
	clientes
ORDER BY 
	nome ASC
```

---

## 📌 Aula 32

🔹 Desafio 1: Definir status de produtos (Liquidação ou Reposição)
   - Regras:
      - estoque >= 100 e preco >= 2000 → Liquidação
      - caso contrário → Reposição
```sql
SELECT 
	produto_id AS "ID do Produto",
	nome AS "Nome do Produto",
	preco AS "Valor (R$)",
	estoque AS "Estoque",
CASE 
	WHEN estoque >= 100 AND preco >= 2000 THEN 'Liquidação'
	ELSE 'Reposição'
END AS "Statuso do Produto"
FROM
	produtos
```

🔹 Desafio 2: Verificando o status de entrega dos pedidos
   - Regras:
     - cancelado → "Cancelado"
     - em andamento + sem data → "Não entregue"
     - entregue ou com data → "Entregue"
```sql
SELECT * FROM pedidos LIMIT 3

SELECT
	pedido_id AS "ID do Pedido",
	status AS "Status do Pedido",
	data_entrega_prevista AS "Data de Entrega Prevista",
	data_entrega_realizada AS "Data de Entrega Realizada",
CASE 
	WHEN status = 'cancelado' THEN 'Cancelado'
	WHEN status = 'entregue' OR data_entrega_realizada IS NOT NULL THEN 'Entregue'
	ELSE 'Não entregue'
END AS "Status de Entrega"
FROM 
	pedidos p 
```

---

## 🧠 Desafios Finais
---

## 🏁 Desafio Final 1  
   - Crie um relatório de produtos com as seguintes classificações:
      - Preço: "Econômico" (< R$100), "Padrão" (R$100-500), "Premium" (> R$500)
      - Estoque: "Crítico" (< 10), "Baixo" (10-29), "Normal" (30-99), "Alto" (>= 100) 
```sql
SELECT 
	produto_id,
	nome AS "Nome do Produto",
	preco AS "Preço (R$)",
CASE
	WHEN preco < 100 THEN 'Econômico'
	WHEN preco >= 100 AND preco < 500 THEN 'Padrão'
	ELSE 'Premium'
END AS "Classificação do Preço",
CASE
	WHEN estoque < 10 THEN 'Crítico'
	WHEN estoque >= 10 AND estoque < 29 THEN 'Baixo'
	WHEN estoque >= 30 AND estoque < 99 THEN 'Normal'
	ELSE 'Alto'
END AS "Classificação do Estoque"
FROM
	produtos
```

## 🏁 Desafio Final 2  
  - Crie um relatório de produtos com classificação combinada:
     - Se o produto é de "Alta Prioridade" (preço > R$300 E estoque < 20)
     - Se é "Promoção Possível" (preço > R$200 E estoque > 50)
     - Se está em categoria específica (categoria_id IN (1, 2, 3))
  - Adicione uma observação combinando essas condições usando AND/OR 
```sql
SELECT * FROM produtos LIMIT 3

SELECT 
	produto_id,
	nome AS "Nome do Produto",
	preco AS "Preço (R$)",
CASE
	WHEN preco > 300 AND estoque < 20 THEN 'Alta Prioridade'
	WHEN preco > 200 AND estoque > 50 THEN 'Promoção Possível'
	ELSE 'Preço normal'
END AS "Classificação do Preço",
CASE 
	WHEN categoria_id in(1,2,3) THEN 'Categoria Principal'
	WHEN categoria_id IN(4,5,6) THEN 'Categoria Secundária'
	ELSE 'Categoria Normal'
END AS "Categoria do Produto",
CASE
	WHEN (preco > 300 AND estoque < 20) OR (categoria_id IN(1,2,3)) THEN 'Grande oportunidade'
	WHEN (preco > 200 AND estoque > 50) OR (categoria_id IN(4,5,6)) THEN 'Baixar Preço'
	ELSE 'Venda normal'
END AS "Observação"
FROM
	produtos	
ORDER BY 
	"Classificação do Preço" ASC
```

## 🏁 Desafio Final 3  
  - Classifique os clientes em:
      - "Novo" - cadastrado há menos de 6 meses
      - "Regular" - cadastrado entre 6 meses e 2 anos
      - "Veterano" - cadastrado há mais de 2 anos
```sql
SELECT * FROM clientes c  LIMIT 4

SELECT 
	nome,
CASE 
	WHEN data_cadastro <= now() - INTERVAL '6 month' THEN 'Novo'
	WHEN data_cadastro >  now() - INTERVAL '6 month' AND data_cadastro < now() - INTERVAL '2 year' THEN 'Regular'
	ELSE 'Veterano'
END AS "Cadastro dos clientes"
FROM 
	clientes
ORDER BY
	nome ASC
```

## 🏁 Desafio Final 4  
  - Crie um relatório de análise de custos e descontos dos pedidos:
  - Classificação de Frete: "Frete Grátis" (0), "Frete Econômico" (R$0.01-10.00), "Frete Padrão" (R$10.01-25.00), "Frete Premium" (R$25.01-49.96), "Frete Especial" (>R$49.96)
  - Classificação de Desconto: "Sem Desconto" (0), "Desconto Básico" (R$0.24-20.00), "Desconto Bom" (R$20.01-50.00), "Desconto Excelente" (R$50.01-99.85), "Desconto Excepcional" (>R$99.85)
  - Use CASE WHEN com BETWEEN para criar as classificações
```sql
SELECT * FROM pedidos LIMIT 2

SELECT
	pedido_id,
	valor_total,
	frete,
	desconto,
CASE 
	WHEN frete = 0 THEN 'Frete Grátis'
	WHEN frete BETWEEN 0.01 AND 10 THEN 'Frete Econômico'
	WHEN frete BETWEEN 10.01 AND 25 THEN 'Frete Padrão'
	WHEN frete BETWEEN 25.01 AND 49.96 THEN 'Frete Premium'
	ELSE 'Frete especial'
END AS "Classificação de frete",
CASE 
	WHEN desconto = 0 THEN 'Sem desconto'
	WHEN desconto BETWEEN 0.24 AND 20 THEN 'Desconto Básico'
	WHEN desconto BETWEEN 20.01 AND 50 THEN 'Desconto Bom'
	WHEN desconto BETWEEN 50.01 AND 99.85 THEN 'Desconto Excelente'
	ELSE 'Desconto Excepcional'
END AS "Classificação de desconto"
FROM 
	pedidos
```

## 🏁 Desafio Final 5 (Boss Final!)  
  - Crie um relatório de análise temporal de pedidos combinando múltiplos conceitos:
  - Extraia informações da data usando EXTRACT: ano, mês e dia da semana
  - Classifique o semestre: "1º Semestre" (meses 1-6), "2º Semestre" (meses 7-12)
  - Identifique o tipo de dia: "Final de Semana" (sábado/domingo), "Dia de Semana" (segunda-sexta)
  - Crie análise de prioridade: "CRÍTICO" (cancelado E valor > 500), "ATENÇÃO" (pendente E valor > 300), "SUCESSO" (entregue E valor > 400), "NORMAL" (demais casos)
  - Use EXTRACT, CASE WHEN, IN, AND/OR e BETWEEN para resolver
  - Status disponíveis: cancelado, confirmado, em_separacao, entregue, enviado, pendente
```sql
SELECT 
	pedido_id,
	valor_total,
	status,
	extract(YEAR FROM data_pedido) AS "Ano",
	extract(MONTH FROM data_pedido) AS "Mês",
	extract(DOW FROM data_pedido) AS "Dia da Semana",
CASE
	WHEN extract(MONTH FROM data_pedido) >= 1 AND extract(MONTH FROM data_pedido) <= 6 THEN '1º Semestre'
	WHEN extract(MONTH FROM data_pedido) > 6 AND extract(MONTH FROM data_pedido) <= 12 THEN '2º Semestre'
END AS "Semestre",
CASE 
	WHEN extract(DOW FROM data_pedido) BETWEEN 0 AND 5 THEN 'Dia de Semana'
	WHEN extract(DOW FROM data_pedido) BETWEEN 6 AND 7 THEN 'Final de Semana'
END AS "Tipo de dia",
CASE
	WHEN status = 'cancelado' AND valor_total > 500 THEN 'CRÍTICO'
	WHEN status = 'pendente' AND valor_total > 300 THEN 'ATENÇÃO'
	WHEN status = 'entregue' AND valor_total > 400 THEN 'SUCESSO'
	ELSE 'NORMAL'
END AS "Prioridade"
FROM 
	pedidos
```
