# Splunk SOC Home Lab: Detecção de Ameaças com Sysmon e MITRE ATT&CK

Laboratório pessoal de SIEM construído com **Splunk Enterprise**, **Sysmon** e **Atomic Red Team**, com o objetivo de praticar o ciclo completo de um analista SOC: coletar logs, simular ataques, escrever detecções e investigar incidentes.

> ⚠️ **Projeto de estudo / laboratório pessoal.** Todo o ambiente roda em VMs isoladas, sem conexão com redes de produção.

---

## Objetivos

- Implantar um SIEM funcional (Splunk Enterprise) em ambiente virtualizado
- Coletar logs do Windows (Security, Sysmon, PowerShell) com o Universal Forwarder
- Simular técnicas reais de ataque com Atomic Red Team
- Criar detecções em **SPL** mapeadas ao **MITRE ATT&CK**
- Documentar a investigação de incidentes no formato de um relatório de SOC

## Arquitetura

```
┌──────────────────────┐        TCP 9997        ┌──────────────────────┐
│  VM Windows 10/11    │ ─────────────────────▶ │  VM Ubuntu Server    │
│  - Sysmon            │     (logs encaminhados) │  - Splunk Enterprise │
│  - Splunk UF         │                        │  - Índice: sysmon    │
│  - Atomic Red Team   │                        │  - Índice: wineventlog│
└──────────────────────┘                        └──────────────────────┘
        Rede host-only / NAT isolada (sem acesso à rede corporativa)
```

Detalhes em [`docs/01-arquitetura.md`](docs/01-arquitetura.md).

## Stack

| Componente | Função |
|---|---|
| Splunk Enterprise | SIEM (busca, alertas, dashboards) |
| Splunk Universal Forwarder | Envio dos logs do endpoint |
| Sysmon + config SwiftOnSecurity | Telemetria detalhada de processos, rede e registro |
| Atomic Red Team | Simulação de técnicas MITRE ATT&CK |
| Splunk Add-on for Sysmon | Extração de campos dos eventos Sysmon |

## Estrutura do repositório

```
splunk-soc-homelab/
├── README.md
├── docs/
│   ├── 01-arquitetura.md
│   ├── 02-instalacao.md
│   └── 03-simulacao-ataques.md
├── configs/
│   └── inputs.conf
├── detections/
│   ├── README.md              # tabela das detecções + mapeamento ATT&CK
│   └── *.spl                  # uma consulta por arquivo
├── investigations/
│   └── template-incidente.md  # modelo de relatório de incidente
└── screenshots/               # SUAS capturas de tela (veja abaixo)
```

## Detecções criadas

| # | Detecção | Tática | Técnica ATT&CK |
|---|---|---|---|
| 1 | PowerShell com comando codificado | Execution | [T1059.001](https://attack.mitre.org/techniques/T1059/001/) |
| 2 | Acesso suspeito ao processo LSASS | Credential Access | [T1003.001](https://attack.mitre.org/techniques/T1003/001/) |
| 3 | Criação de tarefa agendada via schtasks | Persistence | [T1053.005](https://attack.mitre.org/techniques/T1053/005/) |
| 4 | Persistência via chave Run do registro | Persistence | [T1547.001](https://attack.mitre.org/techniques/T1547/001/) |
| 5 | Múltiplas falhas de logon (força bruta) | Credential Access | [T1110](https://attack.mitre.org/techniques/T1110/) |
| 6 | Criação de conta local | Persistence | [T1136.001](https://attack.mitre.org/techniques/T1136/001/) |
| 7 | Download via certutil (LOLBin) | Command and Control | [T1105](https://attack.mitre.org/techniques/T1105/) |
| 8 | Limpeza de logs de eventos | Defense Evasion | [T1070.001](https://attack.mitre.org/techniques/T1070/001/) |

Consultas completas em [`detections/`](detections/).

## Como reproduzir

1. Siga [`docs/02-instalacao.md`](docs/02-instalacao.md) para montar o ambiente
2. Execute os testes de [`docs/03-simulacao-ataques.md`](docs/03-simulacao-ataques.md)
3. Rode as consultas de `detections/` e confira se cada ataque gerou evidência
4. Registre o resultado usando [`investigations/template-incidente.md`](investigations/template-incidente.md)

## Resultados

> Preencha esta seção depois de executar o laboratório.

- [ ] Detecções validadas: _X de 8_
- [ ] Falsos positivos encontrados e como foram tratados: _descreva_
- [ ] Principais aprendizados: _descreva_

### Capturas de tela

Adicione na pasta `screenshots/` e referencie aqui:

```markdown
![Dashboard](screenshots/dashboard.png)
![Alerta disparado](screenshots/alerta-lsass.png)
```

## Competências demonstradas

Splunk (SPL, alertas, dashboards) · Sysmon · MITRE ATT&CK · Análise de logs · Triagem de incidentes · Engenharia de detecção

## Referências

- [MITRE ATT&CK](https://attack.mitre.org/)
- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
- [Sysmon (Microsoft Sysinternals)](https://learn.microsoft.com/sysinternals/downloads/sysmon)
- [SwiftOnSecurity sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)
- [Splunk Attack Range](https://github.com/splunk/attack_range)

## Autor

**Seu Nome**: [LinkedIn](https://linkedin.com/in/seu-perfil) · [GitHub](https://github.com/seu-usuario)

## Licença

MIT
