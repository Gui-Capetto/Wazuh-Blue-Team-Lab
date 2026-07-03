# Consultas Sysmon

Consultas e eventos importantes do Sysmon utilizados no laboratório Blue Team com Wazuh.

---

## O que é o Sysmon

O Sysmon é uma ferramenta da Microsoft Sysinternals utilizada para registrar eventos detalhados do Windows, como criação de processos, conexões de rede, criação de arquivos, alterações no registro e outras atividades relevantes para investigação Blue Team.

No laboratório, o Sysmon está integrado ao Wazuh Agent no Windows Endpoint.

---

## Sysmon Event ID 1 - Process Create

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon" and data.win.system.eventID: 1
```

Uso:
Investigar criação de processos no endpoint.

Exemplos de processos importantes:

```text
powershell.exe
cmd.exe
net.exe
net1.exe
whoami.exe
ipconfig.exe
hostname.exe
secedit.exe
```

Campos importantes:

```text
data.win.eventdata.image
data.win.eventdata.commandLine
data.win.eventdata.parentImage
data.win.eventdata.user
data.win.eventdata.processId
data.win.eventdata.parentProcessId
data.win.eventdata.hashes
```

---

## Sysmon Event ID 3 - Network Connection

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon" and data.win.system.eventID: 3
```

Uso:
Investigar conexões de rede realizadas por processos.

Campos importantes:

```text
data.win.eventdata.image
data.win.eventdata.destinationIp
data.win.eventdata.destinationPort
data.win.eventdata.sourceIp
data.win.eventdata.sourcePort
data.win.eventdata.protocol
data.win.eventdata.user
```

---

## Sysmon Event ID 11 - File Create

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon" and data.win.system.eventID: 11
```

Uso:
Investigar criação de arquivos no endpoint.

Campos importantes:

```text
data.win.eventdata.image
data.win.eventdata.targetFilename
data.win.eventdata.creationUtcTime
data.win.eventdata.user
```

---

## Buscar criação de arquivos em diretório Temp

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon" and data.win.eventdata.targetFilename: *Temp*
```

Uso:
Identificar arquivos criados em diretórios temporários, que são frequentemente utilizados por malwares, scripts e ferramentas ofensivas.

---

## Buscar scripts PowerShell

```dql
data.win.eventdata.targetFilename: *.ps1
```

Uso:
Localizar criação ou manipulação de arquivos de script PowerShell.

---

## Buscar execução de PowerShell

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon" and data.win.eventdata.image: *powershell*
```

Uso:
Investigar execuções do PowerShell no endpoint.

---

## Buscar PowerShell com linha de comando suspeita

```dql
data.win.eventdata.commandLine: *ExecutionPolicy*
```

Uso:
Identificar comandos PowerShell que usam alteração ou bypass de política de execução.

---

## Buscar comandos de Discovery

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon" and data.win.system.eventID: 1 and data.win.eventdata.commandLine: *user*
```

Uso:
Localizar comandos relacionados à enumeração de usuários, como `net user` ou `net1 user`.

---

## Buscar net.exe ou net1.exe

```dql
data.win.eventdata.image: *net*
```

Uso:
Investigar uso de comandos administrativos e de discovery no Windows.

---

## Buscar cmd.exe

```dql
data.win.eventdata.image: *cmd.exe*
```

Uso:
Investigar execuções do Prompt de Comando.

---

## Buscar processos iniciados por PowerShell

```dql
data.win.eventdata.parentImage: *powershell*
```

Uso:
Identificar processos filhos criados pelo PowerShell.

---

## Eventos Sysmon usados nos casos do laboratório

| Caso | Event ID | Descrição |
|---|---|---|
| Caso 001 | 11 | PowerShell criou arquivo temporário em AppData Local Temp |
| Caso 002 | 1 | Execução de comando de discovery `net1 user` |
| Caso 003 | Não aplicável diretamente | Caso baseado em Windows Security Event ID 4625 |

---

## Observações de investigação

Durante uma investigação SOC, eventos do Sysmon devem ser analisados junto com:

- Linha de comando executada
- Processo pai
- Usuário responsável
- Horário do evento
- Hash do arquivo/processo
- Arquivo criado ou modificado
- Conexões de rede associadas
- Outros eventos próximos no mesmo host

Um evento isolado pode ser benigno. A análise contextual define se o alerta representa atividade suspeita, falso positivo ou atividade autorizada.
