## 7. SEGURANÇA DE APLICAÇÃO

###  O que é

Protege acesso via procedures, aplicações e código dinâmico.

##

### 7.1 SQL Injection

**Errado**

```sql
SET @sql = 'SELECT * FROM Cliente WHERE Nome = ''' + @Nome + '''';
```

**Correto**

```sql
EXEC sp_executesql
N'SELECT * FROM dbo.Cliente WHERE Nome = @Nome',
N'@Nome VARCHAR(100)',
@Nome;
```

##

### 7.2 EXECUTE AS

```sql
CREATE PROCEDURE dbo.usp_consulta
WITH EXECUTE AS OWNER
AS
SELECT * FROM dbo.Cliente;
```

##

### 7.3 Module Signing

Uso avançado para elevar privilégio com certificado sem dar permissão direta.

##

**Resumo (Aplicação)**

* sempre parametrizar
* evitar SQL dinâmico inseguro
* usar procedure
* controlar contexto
