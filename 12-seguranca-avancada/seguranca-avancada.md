## 12. SEGURANÇA AVANÇADA

### 12.1 Microsoft Defender for SQL

**O que é**

Detecção de ameaças e comportamento anômalo.

**Detecta**

* brute force
* SQL injection
* privilege escalation
* acesso incomum

### 12.2 Vulnerability Assessment

**O que é**

Scan automatizado de vulnerabilidades.

**Verifica**

* SA habilitado
* TDE ausente
* permissões excessivas
* porta exposta
* contas inseguras

### 12.3 Script útil de validação

```sql
SELECT name, create_date
FROM sys.server_principals
WHERE type_desc = 'SQL_LOGIN';
```

✔️ **Resumo (Avançado)**

* monitoramento proativo
* baseline
* recomendações
* integração com SOC/SIEM
