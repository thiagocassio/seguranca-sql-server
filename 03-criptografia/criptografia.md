# 03. CRIPTOGRAFIA (Encryption)

## O que é

Protege dados contra acesso indevido.

##

## 3.1 TDE (Criptografia em repouso)

### O que protege

* MDF
* LDF
* backups

### Como implementar

```sql
USE master;
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'SenhaForte!123';

CREATE CERTIFICATE Cert_TDE
WITH SUBJECT = 'Certificado TDE';

USE BDTeste;
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256
ENCRYPTION BY SERVER CERTIFICATE Cert_TDE;

ALTER DATABASE BDTeste SET ENCRYPTION ON;
```

### Como conferir

```sql
SELECT
    DB_NAME(database_id) AS banco,
    encryption_state
FROM sys.dm_database_encryption_keys;
```

**Resultado**

* `0` = No encryption
* `1` = Unencrypted
* `2` = In progress
* `3` = Encrypted

### Backup CRÍTICO

```sql
BACKUP CERTIFICATE Cert_TDE
TO FILE = 'C:\certificado.cer'
WITH PRIVATE KEY (
    FILE = 'C:\certificado.pvk',
    ENCRYPTION BY PASSWORD = 'SenhaForte!123'
);

BACKUP MASTER KEY
TO FILE = 'C:\masterkey.bak'
ENCRYPTION BY PASSWORD = 'SenhaForte!123';
```

##

## 3.2 Criptografia em trânsito (TLS)

### O que é

Protege dados na rede.

### Como conferir

```sql
SELECT session_id, encrypt_option
FROM sys.dm_exec_connections;
```

**Resultado**

* `TRUE` = criptografado

### Connection String

```text
Encrypt=True; TrustServerCertificate=False;
```

##

## 3.3 Criptografia de coluna

### Column Encryption (manual)

```sql
USE BDTeste;

CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'SenhaForte!123';

CREATE CERTIFICATE Cert_Dados
WITH SUBJECT = 'Protecao Dados';

CREATE SYMMETRIC KEY ChaveDados
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE Cert_Dados;

OPEN SYMMETRIC KEY ChaveDados
DECRYPTION BY CERTIFICATE Cert_Dados;

INSERT INTO Cliente (Nome)
VALUES (EncryptByKey(Key_GUID('ChaveDados'), 'Thiago'));

SELECT CONVERT(VARCHAR, DecryptByKey(Nome))
FROM Cliente;
```

### Always Encrypted

* criptografia no cliente
* SQL Server não vê o dado
* deterministic = permite busca
* randomized = mais seguro

### Como conferir

```sql
SELECT name, encryption_type_desc
FROM sys.columns
WHERE encryption_type IS NOT NULL;
```

##

## 3.4 Hierarquia de chaves

Service Master Key → Database Master Key → Certificados → Chaves → Dados

### Como conferir

```sql
SELECT *
FROM sys.symmetric_keys
WHERE name = '##MS_DatabaseMasterKey##';

SELECT *
FROM sys.certificates;
```

##

## Resumo

* TDE protege arquivos
* TLS protege rede
* Always Encrypted protege dados
* sempre fazer backup das chaves
