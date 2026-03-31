## 5. AUDITORIA E MONITORAMENTO

### O que é

Permite rastrear acessos, alterações, falhas de login e ações críticas.

##

### 5.1 SQL Server Audit

**Como fazer**

```sql
CREATE SERVER AUDIT Audit_Seguranca
TO FILE (FILEPATH = 'C:\\SQLAudit\\');

ALTER SERVER AUDIT Audit_Seguranca
WITH (STATE = ON);
```

```sql
CREATE SERVER AUDIT SPECIFICATION Audit_Login
FOR SERVER AUDIT Audit_Seguranca
ADD (FAILED_LOGIN_GROUP),
ADD (SUCCESSFUL_LOGIN_GROUP);

ALTER SERVER AUDIT SPECIFICATION Audit_Login
WITH (STATE = ON);
```

**Como conferir**

```sql
SELECT *
FROM sys.server_audits;
```

**Leitura do audit**

```sql
SELECT *
FROM sys.fn_get_audit_file('C:\\SQLAudit\\*', DEFAULT, DEFAULT);
```

##

### 5.2 Logs

```sql
EXEC xp_readerrorlog;
EXEC xp_readerrorlog 0, 1, 'Login failed';
```

##

### 5.3 Sessões e conexões

```sql
SELECT login_name, host_name, program_name
FROM sys.dm_exec_sessions
WHERE is_user_process = 1;
```

##

✔️ **Resumo (Auditoria)**

* auditar login
* DDL
* acesso sensível
* falhas
* retenção de evidências
