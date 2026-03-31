## 6. SEGURANÇA DE REDE

### O que é

Protege comunicação entre aplicação, usuários e SQL Server.

##

### 6.1 Protocolos

* TCP/IP
* Named Pipes
* Shared Memory

**Boa prática**
Desabilitar protocolos não usados.

##

### 6.2 TLS

**O que é**

Criptografa tráfego na rede.

**Como conferir**

```sql
SELECT session_id, client_net_address, encrypt_option, auth_scheme
FROM sys.dm_exec_connections;
```

##

### 6.3 Portas

* 1433 SQL padrão
* 1434 SQL Browser
* portas dinâmicas em instância nomeada

##

### 6.4 Hardening

* firewall liberando somente app server
* VPN
* subnet dedicada
* bastion host
* sem exposição pública

##

**Resumo (Rede)**

* TLS obrigatório
* reduzir superfície
* segmentação
* portas mínimas
