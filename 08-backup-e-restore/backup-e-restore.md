## 8. SEGURANÇA DE BACKUP E RESTORE

### O que é

Protege backups contra corrupção, acesso indevido e vazamento.

##

### 8.1 Backup FULL

```sql
BACKUP DATABASE BDTeste
TO DISK = 'D:\\Backup\\BDTeste_FULL.bak'
WITH INIT, COMPRESSION, CHECKSUM;
```

##

### 8.2 Backup DIFF

```sql
BACKUP DATABASE BDTeste
TO DISK = 'D:\\Backup\\BDTeste_DIFF.bak'
WITH DIFFERENTIAL, CHECKSUM;
```

##

### 8.3 Backup LOG

```sql
BACKUP LOG BDTeste
TO DISK = 'D:\\Backup\\BDTeste_LOG.trn'
WITH CHECKSUM;
```

##

### 8.4 Backup criptografado

```sql
BACKUP DATABASE BDTeste
TO DISK = 'D:\\Backup\\BDTeste_ENC.bak'
WITH ENCRYPTION (
    ALGORITHM = AES_256,
    SERVER CERTIFICATE Cert_TDE
);
```

##

### 8.5 Restore completo

```sql
RESTORE DATABASE BDTeste
FROM DISK = 'D:\\Backup\\BDTeste_FULL.bak'
WITH NORECOVERY;

RESTORE DATABASE BDTeste
FROM DISK = 'D:\\Backup\\BDTeste_DIFF.bak'
WITH NORECOVERY;

RESTORE LOG BDTeste
FROM DISK = 'D:\\Backup\\BDTeste_LOG.trn'
WITH RECOVERY;
```

##

### 8.6 Point in Time

```sql
RESTORE LOG BDTeste
FROM DISK = 'D:\\Backup\\BDTeste_LOG.trn'
WITH STOPAT = '2026-03-31 10:30:00', RECOVERY;
```

##

### 8.7 Como conferir histórico

```sql
SELECT database_name, backup_start_date, type
FROM msdb.dbo.backupset;
```
##

**Resumo (Backup)**

* usar CHECKSUM
* testar restore
* criptografar backup
* proteger mídia
