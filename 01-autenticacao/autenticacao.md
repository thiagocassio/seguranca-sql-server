# 01. AUTENTICAÇÃO (Authentication)

## O que é

Define como o usuário prova identidade ao conectar no SQL Server.

##

## 1.1 Modos de Autenticação

### Windows Authentication (RECOMENDADO)

* usa usuário do Windows / AD
* autenticação via Kerberos/NTLM
* mais seguro (sem senha no banco)

### SQL Server Authentication

* login e senha criados no SQL Server
* usado quando não há AD

### Mixed Mode

* permite Windows + SQL Login

### Como verificar o modo atual

```sql
SELECT SERVERPROPERTY('IsIntegratedSecurityOnly') AS ApenasWindows;
```

**Resultado**

* `1` = Apenas Windows (ideal)
* `0` = Mixed Mode (revisar a necessidade)

##

## 1.2 Gerenciamento de Logins

### O que é

Login = porta de entrada no servidor.

### Criar login Windows

```sql
CREATE LOGIN [DOMINIO\usuario] FROM WINDOWS;
CREATE LOGIN [DOMINIO\DBA] FROM WINDOWS;
```

### Criar login SQL

```sql
CREATE LOGIN usuario_sql
WITH PASSWORD = 'SenhaForte123!';
```

##

## 1.3 Políticas de senha

### O que é

Aplica regras do Windows:

* complexidade
* expiração
* histórico

### Como fazer

```sql
CREATE LOGIN usuario_seguro
WITH PASSWORD = 'SenhaForte123!',
CHECK_POLICY = ON,
CHECK_EXPIRATION = ON;
```

### Como conferir logins

```sql
SELECT name, type_desc, is_disabled
FROM sys.server_principals
WHERE type IN ('S','U','G');
```

### Na coluna is_disabled:
1 = login desabilitado
0 = login habilitado

### Legenda coluna Type:
- S = SQL Login
Login criado dentro do próprio SQL Server
Ex.: app_user, etl_login, sa

- U = Windows Login
Usuário individual do AD/Windows
Ex.: DOMINIO\thiago

- G = Windows Group
Grupo do Active Directory
Ex.: DOMINIO\DBA_SQL

### Testar login

```sql
EXECUTE AS LOGIN = 'usuario_sql';
SELECT SUSER_NAME();
REVERT;
```

##

## 1.4 Mapeamento Login → User (CRÍTICO)

### O que é

O login acessa a instância, mas precisa de USER para acessar o banco.

### Como fazer

```sql
USE BDTeste;
CREATE USER usuario_sql FOR LOGIN usuario_sql;
```

### Como conferir usuários do banco

```sql
SELECT name, type_desc
FROM sys.database_principals;
```

### Erro comum

Login existe, mas não acessa o banco.

**Solução:** criar o USER mapeado ao LOGIN.

##

## Resumo

* preferir Windows Authentication
* usar grupos AD
* evitar SQL Login quando possível
* sempre mapear LOGIN → USER
