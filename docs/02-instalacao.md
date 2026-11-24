# 02 - Instalação

## 1. Splunk Enterprise (VM Ubuntu)

```bash
# Baixe o .deb em splunk.com (conta gratuita) e instale
sudo dpkg -i splunk-*.deb
sudo /opt/splunk/bin/splunk start --accept-license
sudo /opt/splunk/bin/splunk enable boot-start
```

Interface web: `http://192.168.56.10:8000`

Crie os índices em **Settings > Indexes**: `sysmon`, `wineventlog`, `powershell`.

Habilite o recebimento de dados em **Settings > Forwarding and receiving > Configure receiving > New Receiving Port: 9997**.

> A licença de avaliação permite ingestão limitada por dia, suficiente para o laboratório.

## 2. Sysmon (VM Windows, PowerShell como Administrador)

```powershell
# Baixe Sysmon (Sysinternals) e a configuração da SwiftOnSecurity
.\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

Verifique: `Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5`

## 3. Splunk Universal Forwarder (VM Windows)

1. Instale o UF e aponte o receiving indexer para `192.168.56.10:9997`.
2. Copie [`configs/inputs.conf`](../configs/inputs.conf) para `C:\Program Files\SplunkUniversalForwarder\etc\system\local\`.
3. Reinicie o serviço `SplunkForwarder`.

## 4. Add-on para Sysmon

Instale o **Splunk Add-on for Sysmon** (Splunkbase) no servidor Splunk para extrair campos como `Image`, `CommandLine`, `ParentImage`.

## 5. Auditoria adicional (Windows)

Habilite a auditoria de criação de processos com linha de comando e o log de scripts do PowerShell:

```powershell
auditpol /set /subcategory:"Process Creation" /success:enable
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Audit" /v ProcessCreationIncludeCmdLine_Enabled /t REG_DWORD /d 1 /f
```

## 6. Validação

No Splunk, rode:

```spl
index=sysmon | stats count by EventCode
```

Se aparecerem eventos, a coleta está funcionando.
