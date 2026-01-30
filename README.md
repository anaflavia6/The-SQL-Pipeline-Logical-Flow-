

#  The SQL Pipeline: Logical Flow & Patterns

Repositório dedicado à documentação de padrões de consulta em **MySQL**, consolidado através da resolução de desafios técnicos (HackerRank). O foco aqui é a transição da sintaxe básica para a lógica de processamento e manipulação de dados.

---

##  Execution Hierarchy (Hierarquia de Execução)

Diferente de linguagens imperativas, a ordem em que escrevemos o SQL não é a ordem em que o **SGBD** (Sistema Gerenciador de Banco de Dados) o processa. Compreender este fluxo é vital para a otimização de queries.

|**Order**|**Clause**|**Purpose (Propósito)**|**Ação Prática**|
|---|---|---|---|
|1º|**FROM**|**Data Source**|Define a tabela de origem (abre a "pasta").|
|2º|**WHERE**|**Filtering**|Filtra as linhas (registros) que atendem aos critérios.|
|3º|**GROUP BY**|**Grouping**|Agrupa os dados para cálculos agregados.|
|4º|**HAVING**|**Group Filtering**|Filtro aplicado especificamente após o agrupamento.|
|5º|**SELECT**|**Selection/Projection**|Seleciona as colunas e executa funções (`AVG`, `COUNT`).|
|6º|**DISTINCT**|**Deduplication**|Remove resultados duplicados.|
|7º|**ORDER BY**|**Sorting**|Organiza os dados (Crescente/Decrescente).|
|8º|**LIMIT**|**Constraint**|Restringe a quantidade de linhas entregues.|
![b430d590-3a23-4e5b-ab6d-7f0ca2abab0a](https://github.com/user-attachments/assets/6c6abfea-1045-4c45-9d46-fb7d566ba00e)

---

## Resolution Patterns (Padrões de Resolução)

### 1. String Analysis & Extreme Values

Padrão utilizado para identificar registros com comprimentos (length) específicos, utilizando técnicas de desempate.

MySQL

```
/* Identificando a cidade com o nome mais curto (Shortest) */
SELECT CITY, LENGTH(CITY)
FROM STATION
ORDER BY LENGTH(CITY) ASC, CITY ASC
LIMIT 1;
```

- **`LENGTH`**: Função escalar que contabiliza o número de caracteres na string.
    
- **`ORDER BY LENGTH(CITY) ASC`**: Ordenação crescente pelo tamanho do dado.
    
- **`CITY ASC` (Tie-break)**: O "filtro do filtro". Garante que, em caso de empate no tamanho, a ordem alfabética defina o resultado.
    
- **`LIMIT 1`**: Restrição de output para retornar apenas o registro de topo.
    

### 2. Data Aggregation (Funções de Agregação)

Operações que processam uma coluna inteira para retornar um valor estatístico único.

SQL

```
/* Contagem de registros com múltiplos filtros */
SELECT COUNT(*) AS total_alunos
FROM aluno
WHERE curso = 'Logica' AND nota_entrada >= 8;

/* Cálculo de média aritmética */
SELECT AVG(nota_entrada) AS media_geral
FROM aluno;
```

- **`COUNT(*)`**: Contabiliza o total de entradas que passaram pelo filtro do `WHERE`.
    
- **`AVG()`**: Calcula a média aritmética dos valores numéricos.
    
- **`AS` (Alias)**: Define um "apelido" para a coluna, garantindo que o resultado tenha um rótulo legível e profissional.
    
---
### 3. Pattern Matching with REGEXP (Expressões Regulares)

Diferente do operador `LIKE` (que é mais limitado), o `REGEXP` permite buscas complexas dentro de strings usando metacaracteres. Este padrão é essencial para validação de formatos e filtragem refinada.

|**Metacaractere**|**Função (Purpose)**|**Exemplo Prático**|
|---|---|---|
|**`^`**|**Beginning of string**|`^A` (Inicia com a letra A)|
|**`$`**|**End of string**|`Z$` (Termina com a letra Z)|
|**`[abc]`**|**Character Set**|`^[aeiou]` (Inicia com qualquer vogal)|
|**`[^abc]`**|**Negated Set**|`^[^aeiou]` (Não inicia com vogal)|
|**`|`**|**OR (Alternation)**|
|**`.*`**|**Wildcard Sequence**|Qualquer sequência de caracteres entre dois pontos|

#### Exemplos de Aplicação (STATION Dataset):

SQL

```
/* 1. Cidades que iniciam e terminam com vogais */
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[aeiou].*[aeiou]$';

/* 2. Cidades que NÃO iniciam com vogais */
SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT REGEXP '^[aeiou]';
```

**Análise Técnica:**

- **`^[aeiou]`**: O símbolo `^` ancora a busca no início da string, e os colchetes definem o grupo de caracteres permitidos.
    
- **`.*`**: O ponto `.` representa qualquer caractere, e o asterisco `*` indica que ele pode se repetir várias vezes. É o que "conecta" o início ao fim da palavra.
    
- **`[aeiou]$`**: O símbolo `$` ancora a busca no final da string.
    

---
![d896b987-c57c-438e-9dd4-4362457a7921](https://github.com/user-attachments/assets/02c0b619-0ec8-46cd-8751-2bbe3142bcc7)

###  Checklist (REGEXP):

- [ ] **Anchor Focus:** Lembrar que `^` no início do colchete (`^[abc]`) significa "começa com", mas dentro do colchete (`[^abc]`) significa "negação".
    
- [ ] **Case Sensitivity:** No MySQL, o `REGEXP` geralmente é _case-insensitive_ por padrão, mas isso pode variar conforme a _Collation_ configurada no servidor.
    

---
##  Review Checklist (Anti-Error)

- **Literal Typing**: Strings exigem aspas simples (`'JPN'`), enquanto valores numéricos e matemáticos são escritos de forma direta.
    
- **Statement Termination**: Uso obrigatório do `;` para delimitar o fim da instrução.
    
- **Case Sensitivity**: Atenção à _Collation_ do banco, que pode diferenciar 'brasil' de 'BRASIL'.
    
- **Alias Usage**: Sempre utilizar `AS` em funções de agregação para clareza da projeção final.
    
