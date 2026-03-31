## 4. PROTEÇÃO DE DADOS

### O que é

Conjunto de recursos para proteger dados sensíveis em nível de visualização, linha, coluna e classificação.

##

### 4.1 Dynamic Data Masking (DDM)

**O que é**

Mascara visualmente os dados sem alterar o valor armazenado.

**Quando usar**

* CPF
* cartão
* e-mail
* telefone
* ambientes de suporte

**Como fazer**

```sql
ALTER TABLE dbo.Cliente
ALTER COLUMN CPF VARCHAR(11)
MASKED WITH (FUNCTION = 'partial(0,"***",4)');
```

**Como conferir**

```sql
SELECT name, is_masked, masking_function
FROM sys.columns
WHERE is_masked = 1;
```

**Limitação importante**
Usuários com UNMASK enxergam o valor real.

##

### 4.2 Row-Level Security (RLS)

**O que é**

Restringe quais linhas cada usuário pode visualizar.

**Uso real**

* multiempresa
* multi-filial
* multi-tenant
* loja por região

**Como fazer**

```sql
CREATE FUNCTION dbo.fn_rls_loja(@LojaID INT)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN
SELECT 1
WHERE @LojaID = CAST(SESSION_CONTEXT(N'LojaID') AS INT);
```

```sql
CREATE SECURITY POLICY dbo.Policy_Cliente_Loja
ADD FILTER PREDICATE dbo.fn_rls_loja(LojaID)
ON dbo.Cliente
WITH (STATE = ON);
```

**Como conferir**

```sql
SELECT *
FROM sys.security_policies;
```

##

### 4.3 Data Classification

**O que é**

Classifica dados sensíveis para LGPD, GDPR e auditoria.

**Como fazer**

```sql
ADD SENSITIVITY CLASSIFICATION TO dbo.Cliente.CPF
WITH (
    LABEL = 'Confidencial',
    INFORMATION_TYPE = 'Documento'
);
```

**Como conferir**

```sql
SELECT *
FROM sys.sensitivity_classifications;
```

##

**Resumo (Proteção de Dados)**

* DDM protege visualização
* RLS protege linhas
* Classification ajuda compliance
* combinar com TDE e Audit
