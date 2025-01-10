# Teste de SQL - Matheus Cintra

## Tabela de Produtos

**Estrutura da Tabela:**

| Campo          | Tipo de Campo    | Chave | Comentário                |
|-----------------|------------------|-------|---------------------------|
| cod_prod       | Integer (8)      | X     | Código do Produto         |
| loj_prod       | Integer (8)      | X     | Código da Loja            |
| desc_prod      | Char (40)        |       | Descrição do Produto      |
| dt_inclu_prod  | Data (dd/mm/yyyy)|       | Data de Inclusão do Produto |
| preco_prod     | Decimal (8,3)    |       | Preço do Produto          |

### Inserção de Registro
```sql
INSERT INTO produtos (cod_prod, loj_prod, desc_prod, dt_inclu_prod, preco_prod) 
VALUES (170, 2, 'LEITE CONDESADO MOCOCA', '30/12/2010', 45.400);
```

### Alteração do Preço do Produto
```sql
UPDATE produtos 
SET preco_prod = 95.400 
WHERE cod_prod = 170 AND loj_prod = 2;
```

### Selecionar Registros das Lojas 1 e 2
```sql
SELECT * 
FROM produtos 
WHERE loj_prod IN (1, 2);
```

### Maior e Menor Data de Inclusão

```sql
Copiar código
SELECT MIN(dt_inclu_prod) AS data_minima, MAX(dt_inclu_prod) AS data_maxima 
FROM produtos;
```

### Quantidade Total de Registros
```sql
SELECT COUNT(*) AS total_registros 
FROM produtos;
```

### Produtos que Começam com a Letra "L"
```sql
SELECT * 
FROM produtos 
WHERE desc_prod LIKE 'L%';
```
### Soma dos Preços dos Produtos por Loja
```sql
SELECT loj_prod, SUM(preco_prod) AS soma_precos 
FROM produtos 
GROUP BY loj_prod;
```

### Soma dos Preços Totalizados por Loja com Valor Maior que R$100.000
```sql
SELECT loj_prod, SUM(preco_prod) AS soma_precos 
FROM produtos 
GROUP BY loj_prod 
HAVING SUM(preco_prod) > 100000;
```

## Estrutura das Tabelas

### Tabela de Produtos

| Campo        | Tipo de Campo    | Chave | Comentário                        |
|--------------|------------------|-------|-----------------------------------|
| cod_prod     | Integer (8)      | X     | Código do Produto                 |
| loj_prod     | Integer (8)      | X     | Código da Loja                    |
| desc_prod    | Char (40)        |       | Descrição do Produto              |
| dt_inclu_prod| Data (dd/mm/yyyy)|       | Data de Inclusão do Produto       |
| preco_prod   | Decimal (8,3)    |       | Preço do Produto                  |


### Tabela de Estoque

| Campo    | Tipo de Campo  | Chave | Comentário                          |
|----------|----------------|-------|-------------------------------------|
| cod_prod | Integer (8)    | X     | Código do Produto                   |
| loj_prod | Integer (8)    | X     | Código da Loja                      |
| qtd_prod | Decimal(15,3)  |       | Quantidade em Estoque do Produto   |

### Tabela de Lojas

| Campo   | Tipo de Campo | Chave | Comentário        |
|---------|---------------|-------|-------------------|
| loj_prod| Integer (8)   | X     | Código da Loja    |
| desc_loj| Char (40)     |       | Descrição da Loja |

### Consultas

#### A) Trazer Dados de Produtos, Lojas e Estoque para uma Loja Específica

```sql
SELECT p.loj_prod, l.desc_loj, p.cod_prod, p.desc_prod, p.preco_prod, e.qtd_prod 
FROM produtos p 
JOIN lojas l ON p.loj_prod = l.loj_prod 
JOIN estoque e ON p.cod_prod = e.cod_prod AND p.loj_prod = e.loj_prod 
WHERE p.loj_prod = 1;
```

#### B) Produtos na Tabela de Produtos que Não Existem na Tabela de Estoque

```sql
SELECT p.* 
FROM produtos p 
LEFT JOIN estoque e ON p.cod_prod = e.cod_prod AND p.loj_prod = e.loj_prod 
WHERE e.cod_prod IS NULL;
```

#### C) Produtos na Tabela de Estoque que Não Existem na Tabela de Produtos

```sql
Copiar código
SELECT e.* 
FROM estoque e 
LEFT JOIN produtos p ON e.cod_prod = p.cod_prod AND e.loj_prod = p.loj_prod 
WHERE p.cod_prod IS NULL;
```
