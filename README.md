# Segurança no SQL Server — Guia de Estudos e Consulta para DBA

Este repositório foi criado como um **guia prático de estudos e consulta rápida sobre Segurança no SQL Server**, com foco em:

* revisão para certificações Microsoft
* uso no dia a dia como DBA
* troubleshooting e incidentes
* hardening de instâncias
* auditoria e compliance
* scripts prontos para validação

A proposta é manter o conteúdo **organizado por tópicos**, com **uma pasta para cada tema**, facilitando estudo, busca rápida e evolução contínua.

---

# Estrutura do Repositório

```text
01-autenticacao
02-autorizacao
03-criptografia
04-protecao-de-dados
05-auditoria-e-monitoramento
06-seguranca-de-rede
07-seguranca-de-aplicacao
08-backup-e-restore
09-seguranca-de-instancia-e-sistema
10-alta-disponibilidade-e-seguranca
11-governanca-e-compliance
12-seguranca-avancada
README.md
```

---

# Conteúdo por Tópico

## 01 — Autenticação

* Windows Authentication
* SQL Login
* Mixed Mode
* criação de logins
* políticas de senha
* mapeamento login → user
* validação com `EXECUTE AS`

## 02 — Autorização

* GRANT / DENY / REVOKE
* permissões por escopo
* roles
* roles customizadas
* permissões efetivas
* least privilege

## 03 — Criptografia

* TDE
* TLS
* criptografia de coluna
* Always Encrypted
* hierarquia de chaves
* backup de certificados

## 04 — Proteção de Dados

* Dynamic Data Masking
* Row-Level Security
* Data Classification
* proteção por linha
* LGPD / GDPR

## 05 — Auditoria e Monitoramento

* SQL Server Audit
* failed login
* logs
* sessões
* conexões
* rastreabilidade

## 06 — Segurança de Rede

* TLS
* protocolos
* portas
* firewall
* VPN
* segmentação
* hardening de comunicação

## 07 — Segurança de Aplicação

* SQL Injection
* parametrização
* stored procedures
* `EXECUTE AS`
* module signing

## 08 — Segurança de Backup e Restore

* FULL
* DIFF
* LOG
* backup criptografado
* restore chain
* point in time
* CHECKSUM

## 09 — Segurança de Instância e Sistema

* hardening
* surface area
* `xp\_cmdshell`
* CLR
* contas de serviço
* patching
* gMSA

## 10 — Alta Disponibilidade e Segurança

* Always On
* endpoints
* replicação
* listener
* permissões
* criptografia entre réplicas

## 11 — Governança e Compliance

* Policy-Based Management
* LGPD
* GDPR
* ISO 27001
* PCI-DSS
* classificação de dados
* evidências

## 12 — Segurança Avançada

* Microsoft Defender for SQL
* Vulnerability Assessment
* baseline
* recomendações
* integração SOC/SIEM

---

# Como usar este material

## Estudo

Leia um tópico por vez e execute os scripts se necessário.

## Consulta rápida

Use como referência para:

* troubleshooting
* validação de segurança
* auditorias
* hardening
* incidentes

## Pesquisa

Sempre que esquecer uma DMV, comando ou fluxo de restore, consulte a pasta do tema.

---

# Público-alvo

Este material é ideal para:

* DBAs SQL Server
* Administradores de Infraestrutura
* Analistas de Segurança
* SOC / SIEM Engineers
* profissionais estudando certificação Microsoft

---

# Objetivo do Repositório

O objetivo não é ser um material acadêmico, e sim um **guia direto, prático e reutilizável**, que ajude a:

* reduzir tempo de troubleshooting
* padronizar boas práticas
* lembrar comandos importantes
* apoiar estudos
* servir como fonte de pesquisa rápida

---

# Autor

Material organizado para estudo contínuo, consulta operacional e evolução profissional em Segurança no SQL Server.



