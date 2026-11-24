# Detecções

Cada arquivo `.spl` contém uma consulta pronta para colar no Splunk (ou salvar como alerta). Ajuste `index` e nomes de campo conforme o seu add-on.

| # | Arquivo | Detecção | Tática | Técnica | Fonte |
|---|---|---|---|---|---|
| 1 | `01-powershell-encoded.spl` | PowerShell com comando codificado | Execution | T1059.001 | Sysmon EID 1 |
| 2 | `02-lsass-access.spl` | Acesso ao LSASS | Credential Access | T1003.001 | Sysmon EID 10 |
| 3 | `03-schtasks-create.spl` | Tarefa agendada via schtasks | Persistence | T1053.005 | Sysmon EID 1 |
| 4 | `04-registry-run-key.spl` | Chave Run do registro | Persistence | T1547.001 | Sysmon EID 13 |
| 5 | `05-brute-force.spl` | Falhas de logon repetidas | Credential Access | T1110 | Security 4625 |
| 6 | `06-local-account-created.spl` | Criação de conta local | Persistence | T1136.001 | Security 4720 |
| 7 | `07-certutil-download.spl` | Download via certutil | Command and Control | T1105 | Sysmon EID 1 |
| 8 | `08-log-clearing.spl` | Limpeza de logs | Defense Evasion | T1070.001 | Security 1102 / System 104 |

## Notas de ajuste (falsos positivos)

- **LSASS**: processos legítimos (antivírus, `svchost`, `csrss`, `wininit`) acessam o LSASS. Mantenha a lista de exclusão atualizada.
- **Tarefas agendadas**: softwares de atualização criam tarefas legítimas. Filtre por conta e caminho.
- **Força bruta**: ajuste o limiar (`>= 5` em 5 minutos) ao seu ambiente.
