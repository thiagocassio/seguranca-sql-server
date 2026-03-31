## 10. ALTA DISPONIBILIDADE E SEGURANÇA

### 10.1 O que é

Garantir disponibilidade sem comprometer autenticação, criptografia e permissões.

##

### 10.2 Always On

```sql
SELECT name, role_desc, connection_auth_desc, encryption_algorithm_desc
FROM sys.database_mirroring_endpoints;
```

```sql
SELECT *
FROM sys.dm_hadr_availability_replica_states;
```

##

### 10.3 Replicação

```sql
SELECT name
FROM msdb.dbo.sysjobs
WHERE name LIKE '%Replication%';
```

##

### 10.4 Cuidados

* endpoint protegido
* listener interno
* contas de serviço
* permissões snapshot

##

**Resumo (HA)**

* proteger endpoint
* validar criptografia
* revisar jobs
