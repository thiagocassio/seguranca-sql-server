# 02. AUTORIZAÇÃO (Authorization)

## O que é
Define o que o usuário pode fazer após autenticar.

##

## 2.1 Princípios

* Least Privilege
* Separation of Duties
* uso de roles
* evitar permissões excessivas

##

## 2.3 Permissões

### GRANT

```sql
GRANT SELECT ON dbo.Cliente TO usuario_sql;
```

### DENY

```sql
DENY SELECT ON dbo.Cliente TO usuario_sql;
```

> `DENY` sobrescreve `GRANT`.

### REVOKE

```sql
REVOKE SELECT ON dbo.Cliente FROM usuario_sql;
```

##

## 2.3 Escopos

### Server-level

```sql
GRANT VIEW SERVER STATE TO usuario_sql;
```

### Database-level

```sql
USE BDTeste;
GRANT CREATE TABLE TO usuario_sql;
```

### Schema-level

```sql
GRANT SELECT ON SCHEMA::dbo TO usuario_sql;
```

### Object-level

```sql
GRANT SELECT, INSERT ON dbo.Pedido TO usuario_sql;
```

##

## 2.4 Roles

### Server Roles

```sql
ALTER SERVER ROLE sysadmin ADD MEMBER usuario_sql;
```

### Database Roles

```sql
USE BDTeste;
ALTER ROLE db_datareader ADD MEMBER usuario_sql;
```

### Criar role customizada

```sql
USE BDTeste;
CREATE ROLE role_leitura;
GRANT SELECT ON SCHEMA::dbo TO role_leitura;
ALTER ROLE role_leitura ADD MEMBER usuario_sql;
```

##

## 2.5 Como conferir permissões

```sql
SELECT
    dp.name,
    dp.type_desc,
    pe.permission_name,
    pe.state_desc
FROM sys.database_permissions pe
JOIN sys.database_principals dp
    ON pe.grantee_principal_id = dp.principal_id
WHERE dp.name = 'usuario_sql';
```

##

## 2.6 Ver roles

```sql
SELECT
    dp.name AS usuario,
    dr.name AS role
FROM sys.database_role_members drm
JOIN sys.database_principals dp
    ON drm.member_principal_id = dp.principal_id
JOIN sys.database_principals dr
    ON drm.role_principal_id = dr.principal_id
WHERE dp.name = 'usuario_sql';
```

##

## 2.7 Testar acesso efetivo

```sql
EXECUTE AS USER = 'usuario_sql';
SELECT USER_NAME();
SELECT HAS_PERMS_BY_NAME('dbo.Cliente', 'OBJECT', 'SELECT');
REVERT;
```

##

## Resumo

* usar ROLE ao invés de permissão direta
* evitar `sysadmin`
* validar sempre com `EXECUTE AS`
