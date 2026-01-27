# The-SQL-Pipeline-Logical-Flow-
Repositório dedicado à documentação de padrões avançados de consulta SQL, focando em filtragem de nivel mais alto, expressões regulares (Regex) e agregação de dados. O conteúdo foi consolidado através da resolução de desafios técnicos (HackerRank), cobrindo manipulação de strings e lógica de ordenação.

# SQL — Advanced Selection & Filtering

> Guia de referência para estruturação de queries intermediárias, consolidado a partir de desafios práticos (Datasets: STATION e ALUNO).

## 📊 The SQL Pipeline (Logical Flow)

O diagrama abaixo ilustra a hierarquia de execução e decisão do banco de dados.

1. **DATA SOURCE (FROM):** Define a tabela de origem.
2. **FILTER (WHERE):** Filtra linhas (Ex: REGEXP).
3. **GROUPING:** Agrupa os dados para cálculos.
4. **HAVING:** Filtro aplicado após o agrupamento.
5. **SELECTION (SELECT):** Projeção de colunas e funções como `AVG()` e `COUNT()`.
6. **DISTINCT:** Remoção de resultados duplicados.
7. **SORTING (ORDER BY):** Organiza os dados (Ex: por tamanho ou ordem alfabética).
8. **LIMIT:** Restringe a quantidade de linhas entregues.
9. **RESULT:** Entrega final da query processada.
    
![b430d590-3a23-4e5b-ab6d-7f0ca2abab0a](https://github.com/user-attachments/assets/6c6abfea-1045-4c45-9d46-fb7d566ba00e)


---

## 📝 Resolution Patterns

### 1. String Length & Extremes

Padrão para identificar registros com comprimentos mínimos ou máximos, resolvendo empates por ordem alfabética.

```sql
/* Cidade mais curta (Shortest) */
SELECT CITY, LENGTH(CITY)
FROM STATION
ORDER BY LENGTH(CITY) ASC, CITY ASC
LIMIT 1;

/* Cidade mais longa (Longest) */
SELECT CITY, LENGTH(CITY)
FROM STATION
ORDER BY LENGTH(CITY) DESC, CITY ASC
LIMIT 1;

```

### 2. REGEXP Filters (Pattern Matching)

Filtros avançados para busca de padrões específicos em strings.

* **Inicia com vogal:** `WHERE CITY REGEXP '^[aeiou]'`
* **Inicia e termina com vogal:** `WHERE CITY REGEXP '^[aeiou].*[aeiou]$'`
* **Não inicia com vogal:** `WHERE CITY NOT REGEXP '^[aeiou]'`

### 3. Numeric Aggregation

Cálculos e estatísticas básicas com apelidos (`AS`) para legibilidade.

```sql
/* Contagem de alunos específicos */
SELECT COUNT(*) AS total_alunos
FROM aluno
WHERE curso = 'Logica de programação'
  AND nota_entrada >= 8;

/* Média geral da turma */
SELECT AVG(nota_entrada) AS media_geral
FROM aluno;

```

---

## Review Checklist (Anti-Error)

* **Literal Typing:** Strings entre aspas simples (`'texto'`), números sem aspas.
* **Statement Termination:** Uso obrigatório do ponto e vírgula (`;`).
* **Regex Symbols:** `^` (início), `$` (fim) e `.*` (sequência intermediária).
* **Alias:** Use `AS` para deixar os nomes das colunas de resultado claros.

---
![d896b987-c57c-438e-9dd4-4362457a7921](https://github.com/user-attachments/assets/02c0b619-0ec8-46cd-8751-2bbe3142bcc7)


