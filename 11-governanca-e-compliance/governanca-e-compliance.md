## 11. GOVERNANÇA E COMPLIANCE

### O que é

Segurança voltada para política, norma e evidência.

### 11.1 Policy-Based Management

```sql
SELECT *
FROM msdb.dbo.syspolicy_policies;
```
##

### 11.2 Frameworks

* LGPD
* GDPR
* ISO 27001
* PCI-DSS
* SOX

##

### 11.3 Dados sensíveis

Combinar:

* DDM
* RLS
* TDE
* Always Encrypted
* Audit

```sql
SELECT *
FROM sys.sensitivity_classifications;
```

##

**Resumo (Governança)**

* política
* classificação
* retenção
* evidência
* trilha de auditoria
