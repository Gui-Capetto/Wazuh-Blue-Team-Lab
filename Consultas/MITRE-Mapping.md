# MITRE ATT&CK Mapping

Mapeamento dos casos do laboratório Blue Team com técnicas, táticas e observações relacionadas ao MITRE ATT&CK.

---

## Objetivo

Este arquivo tem como objetivo relacionar os alertas investigados no laboratório com técnicas MITRE ATT&CK, facilitando a documentação, análise e entendimento do comportamento observado.

Nem todo alerta terá um mapeamento perfeito. A análise contextual do evento deve sempre ser considerada pelo analista SOC.

---

## Casos documentados

| Caso | Técnica | ID | Tática | Observação |
|---|---|---|---|---|
| Caso 001 | Ingress Tool Transfer | T1105 | Command and Control | Criação de arquivo temporário via PowerShell em diretório comumente usado por malware |
| Caso 002 | Account Discovery | T1087 / T1087.001 | Discovery | Execução de `net1 user` para listar usuários locais |
| Caso 003 | Falha de autenticação | Event ID 4625 | Credential Access / Defense Monitoring | Tentativas de login inválidas com `runas` |

---

## Caso 001 - PowerShell criando arquivo temporário

### Alerta observado

```text
Executable file dropped in folder commonly used by malware
```

### Técnica MITRE associada pelo Wazuh

```text
T1105 - Ingress Tool Transfer
```

### Evidência principal

```text
Provider: Microsoft-Windows-Sysmon
Event ID: 11
Processo: powershell.exe
Arquivo criado: C:\Users\vboxuser\AppData\Local\Temp\__PSScriptPolicyTest_qaonpzij.1gz.ps1
```

### Interpretação

O evento representa criação de arquivo em diretório temporário por meio do PowerShell. Diretórios como `Temp` e `AppData` são frequentemente utilizados por malwares para armazenar scripts, payloads ou ferramentas baixadas.

No laboratório, o evento foi classificado como verdadeiro positivo benigno, pois foi gerado durante atividade autorizada.

---

## Caso 002 - Account Discovery usando net1 user

### Comando observado

```text
C:\Windows\system32\net1 user
```

### Técnica MITRE relacionada

```text
T1087 - Account Discovery
T1087.001 - Local Account
```

### Evidência principal

```text
Provider: Microsoft-Windows-Sysmon
Event ID: 1
Tipo: Process Create
Comando: net1 user
```

### Interpretação

O comando `net1 user` pode ser utilizado para listar usuários locais do sistema. Em um ambiente real, esse comportamento pode indicar tentativa de reconhecimento, enumeração de contas ou preparação para movimentação lateral.

No laboratório, o evento foi classificado como verdadeiro positivo benigno.

---

## Caso 003 - Falha de autenticação no Windows

### Alerta observado

```text
Logon Failure - Unknown user or bad password
```

### Evento Windows

```text
Event ID: 4625
Provider: Microsoft-Windows-Security-Auditing
```

### Campos observados

```text
Target User: labuser
Subject User: vboxuser
Logon Type: 2
Status: 0xc000006d
SubStatus: 0xc000006a
Source IP: ::1
```

### Interpretação

O evento indica falha de autenticação no Windows. O Logon Type 2 indica tentativa local/interativa, e o SubStatus `0xc000006a` está relacionado a senha incorreta.

Em um ambiente real, múltiplas ocorrências desse tipo podem indicar brute force, password spraying ou tentativa indevida de autenticação.

No laboratório, o evento foi classificado como verdadeiro positivo benigno.

---

## Técnicas úteis para próximos exercícios

| Técnica | ID | Possível exercício |
|---|---|---|
| Account Discovery | T1087 | Executar `net user`, `net localgroup`, `whoami` |
| System Information Discovery | T1082 | Executar `systeminfo`, `hostname`, `ipconfig` |
| Command and Scripting Interpreter: PowerShell | T1059.001 | Executar PowerShell com parâmetros suspeitos |
| Ingress Tool Transfer | T1105 | Simular download controlado de arquivo |
| Create or Modify System Process: Windows Service | T1543.003 | Criar serviço de teste no Windows |
| Valid Accounts | T1078 | Simular login com usuário válido |
| Brute Force | T1110 | Gerar múltiplas falhas de autenticação |
| Scheduled Task/Job | T1053 | Criar tarefa agendada de teste |
| Account Manipulation | T1098 | Criar usuário ou alterar grupo local |

---

## Observações importantes

O MITRE ATT&CK deve ser usado como apoio para entender o comportamento observado, mas a decisão final depende do contexto.

Um mesmo evento pode ter interpretações diferentes dependendo de:

- Ambiente
- Usuário envolvido
- Processo executado
- Linha de comando
- Frequência
- Horário
- Origem da conexão
- Relação com outros eventos
- Histórico do host

No laboratório, os eventos são classificados como verdadeiros positivos benignos, pois foram gerados de forma autorizada para estudo.
