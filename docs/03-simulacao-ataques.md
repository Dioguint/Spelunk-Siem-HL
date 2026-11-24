# 03 - Simulação de ataques (Atomic Red Team)

> Execute **somente** na VM de laboratório isolada. Desative temporariamente o Defender nessa VM se ele bloquear os testes.

## Instalação

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing)
Install-AtomicRedTeam -getAtomics
```

## Testes por detecção

| # | Técnica | Comando |
|---|---|---|
| 1 | T1059.001 | `Invoke-AtomicTest T1059.001 -TestNumbers 1` |
| 2 | T1003.001 | `Invoke-AtomicTest T1003.001 -ShowDetailsBrief` (escolha um teste compatível) |
| 3 | T1053.005 | `Invoke-AtomicTest T1053.005 -TestNumbers 1` |
| 4 | T1547.001 | `Invoke-AtomicTest T1547.001 -TestNumbers 1` |
| 5 | T1110 | Digite senha errada repetidamente no logon local ou via RDP de teste |
| 6 | T1136.001 | `Invoke-AtomicTest T1136.001 -TestNumbers 3` |
| 7 | T1105 | `Invoke-AtomicTest T1105 -ShowDetailsBrief` (teste com certutil) |
| 8 | T1070.001 | `Invoke-AtomicTest T1070.001 -TestNumbers 1` |

Confira os detalhes de cada teste com `-ShowDetailsBrief` antes de executar. Os números podem mudar entre versões.

## Limpeza

```powershell
Invoke-AtomicTest T1053.005 -TestNumbers 1 -Cleanup
```

## Registro

Para cada teste, anote: horário de execução, se a detecção disparou, tempo até o alerta e falsos positivos. Use o [template de incidente](../investigations/template-incidente.md).
