# 01 - Arquitetura do laboratório

## Máquinas virtuais

| VM | SO | Recursos sugeridos | Função |
|---|---|---|---|
| `splunk-srv` | Ubuntu Server 22.04 | 2 vCPU, 4-8 GB RAM, 40 GB disco | Splunk Enterprise |
| `win-victim` | Windows 10/11 | 2 vCPU, 4 GB RAM, 50 GB disco | Endpoint monitorado e alvo das simulações |

Hipervisor: VirtualBox, VMware Workstation Player ou Hyper-V.

## Rede

- Use rede **Host-only** (ou NAT Network) para que as duas VMs se enxerguem.
- Evite modo *bridge* nas VMs de ataque: o laboratório deve ficar isolado.
- Exemplo de endereçamento:
  - `splunk-srv`: 192.168.56.10
  - `win-victim`: 192.168.56.20

## Fluxo de dados

1. Eventos são gerados no Windows (Security, Sysmon, PowerShell).
2. O **Universal Forwarder** lê os canais de log configurados em `inputs.conf`.
3. Os eventos são enviados à porta **9997** do Splunk.
4. O Splunk indexa nos índices `sysmon` e `wineventlog`.
5. Consultas e alertas em SPL consomem esses índices.

## Índices utilizados

| Índice | Conteúdo |
|---|---|
| `sysmon` | Microsoft-Windows-Sysmon/Operational |
| `wineventlog` | Security, System, Application |
| `powershell` | Microsoft-Windows-PowerShell/Operational |
