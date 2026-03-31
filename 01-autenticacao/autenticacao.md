# 01. AUTENTICAÇÃO (Authentication)

## O que é

Define como o usuário prova identidade ao conectar no SQL Server.


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
- S = SQL Login (Login criado dentro do próprio SQL Server)

  Ex.: app_user, etl_login, sa

- U = Windows Login (Usuário individual do AD/Windows)

  Ex.: DOMINIO\thiago

- G = Windows Group (Grupo do Active Directory) 

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

## 1.5 Auditoria

### Como ver acessos atuais
Se o login estiver conectado agora, você consegue ver por:

```sql
SELECT 
    login_name,
    login_time,
    host_name,
    program_name,
    session_id
FROM sys.dm_exec_sessions
WHERE is_user_process = 1;
```
Aqui:

- login_time = hora em que a sessão atual foi aberta
- só mostra sessões ativas
- não serve como histórico permanente



### Como descobrir “último acesso”
Error Log (mais simples)
```sql
EXEC xp_readerrorlog 0, 1, N'Login succeeded';
```

### Tentativas falhas
```sql
EXEC xp_readerrorlog 0, 1, N'Login failed';
```
Detecta:
- brute force
- senha expirada
- app com senha antiga
- login órfão
- ataque extero

### Detectar contas órfãs
```sql
SELECT 
    dp.name AS usuario_orfao,
    dp.type_desc,
    dp.sid
FROM sys.database_principals dp
LEFT JOIN sys.server_principals sp
    ON dp.sid = sp.sid
WHERE sp.sid IS NULL
  AND dp.authentication_type_desc = 'INSTANCE'
  AND dp.name NOT IN ('dbo','guest','INFORMATION_SCHEMA','sys');
```

##

### Extended Events (melhor prática)
Capturar evento de login e salvar em arquivo.

Ideal para:
- logins sem uso há 90 dias
- contas órfãs
- detectar contas de aplicação esquecidas
- auditoria ISO 27001
- revisão trimestral de acessos

##

### SQL Server Audit

Melhor para compliance e evidência formal.

##

## Resumo

* preferir Windows Authentication
* usar grupos AD
* evitar SQL Login quando possível
* sempre mapear LOGIN → USER
