## 9. SEGURANÇA DE INSTÂNCIA E SISTEMA

### O que é

Hardening do serviço SQL Server e do Windows.

##

### 9.1 Surface Area

```sql
EXEC sp_configure 'xp_cmdshell', 0;
RECONFIGURE;
```

##

### 9.2 Como conferir

```sql
SELECT name, value_in_use
FROM sys.configurations;
```

##

### 9.3 Boas práticas

* conta dedicada
* gMSA
* patching
* sem conta local admin
* desabilitar CLR/OLE/xp_cmdshell

##

**Resumo (Instância)**

* menor superfície
* patching contínuo
* serviço dedicado
* configs revisadas
