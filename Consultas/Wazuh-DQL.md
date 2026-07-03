# Consultas Wazuh DQL

Consultas utilizadas no Wazuh Threat Hunting para investigação de eventos do laboratório Blue Team.

---

## Filtrar agente Windows

```dql
agent.name: "Windows-Endpoint"
```

Uso: filtrar somente eventos do endpoint Windows monitorado.

---

## Filtrar agente por ID

```dql
agent.id: 001
```

Uso: filtrar eventos do agente cadastrado no Wazuh.

---

## Eventos do Sysmon

```dql
data.win.system.providerName: "Microsoft-Windows-Sysmon"
```

Uso: visualizar somente eventos coletados pelo Sysmon.

---

## Sysmon Event ID 1 - Process Create

```dql
data.win.system.eventID: 1
```

Uso: investigar criação de processos, como PowerShell, CMD, net.exe e outros comandos.

---

## Sysmon Event ID 11 - File Create

```dql
data.win.system.eventID: 11
```

Uso: investigar criação de arquivos no endpoint.

---

## Windows Logon Failure

```dql
data.win.system.eventID: 4625
```

Uso: investigar falhas de autenticação no Windows.

---

## Buscar PowerShell

```dql
data.win.eventdata.image: *powershell*
```

Uso: localizar execuções do PowerShell.

---

## Buscar commandLine com ExecutionPolicy

```dql
data.win.eventdata.commandLine: *ExecutionPolicy*
```

Uso: localizar comandos PowerShell que utilizam alteração ou bypass de política de execução.

---

## Buscar usuário específico

```dql
data.win.eventdata.targetUserName: "labuser"
```

Uso: investigar eventos relacionados ao usuário `labuser`.

---

## Buscar eventos de autenticação falha

```dql
rule.groups: authentication_failed
```

Uso: localizar eventos classificados pelo Wazuh como falha de autenticação.

---

## Buscar alerta por Rule ID

```dql
rule.id: 60122
```

Uso: localizar alertas específicos do Wazuh, como falhas de logon.

---

## Buscar eventos por técnica MITRE

```dql
rule.mitre.id: "T1105"
```

Uso: localizar eventos associados a uma técnica MITRE ATT&CK específica.

---

## Buscar arquivos criados em pasta Temp

```dql
data.win.eventdata.targetFilename: *Temp*
```

Uso: investigar criação de arquivos em diretórios temporários.

---

## Buscar scripts PowerShell

```dql
data.win.eventdata.targetFilename: *.ps1
```

Uso: localizar criação ou manipulação de arquivos de script PowerShell.

---

## Buscar eventos do Windows Security

```dql
data.win.system.providerName: "Microsoft-Windows-Security-Auditing"
```

Uso: visualizar eventos de auditoria de segurança do Windows.

---

## Buscar eventos por usuário de origem

```dql
data.win.eventdata.subjectUserName: "vboxuser"
```

Uso: investigar ações realizadas por um usuário específico.

---

## Buscar eventos por usuário alvo

```dql
data.win.eventdata.targetUserName: "labuser"
```

Uso: investigar tentativas de autenticação ou alterações envolvendo um usuário específico.
