# Consultas de Autenticação

Consultas usadas para investigar eventos de autenticação no Windows dentro do laboratório Blue Team com Wazuh.

---

## Falha de login

```dql
data.win.system.eventID: 4625
```

Uso:
Investigar tentativas de login malsucedidas no Windows.

---

## Login bem-sucedido

```dql
data.win.system.eventID: 4624
```

Uso:
Identificar autenticações bem-sucedidas no endpoint.

---

## Falha de login para usuário específico

```dql
data.win.system.eventID: 4625 and data.win.eventdata.targetUserName: "labuser"
```

Uso:
Investigar tentativas de autenticação falhas envolvendo o usuário `labuser`.

---

## Eventos classificados como falha de autenticação pelo Wazuh

```dql
rule.groups: authentication_failed
```

Uso:
Localizar eventos que o Wazuh classificou como falha de autenticação.

---

## Buscar alerta de Logon Failure do Wazuh

```dql
rule.id: 60122
```

Uso:
Localizar alertas relacionados a falha de login com usuário desconhecido ou senha incorreta.

---

## Buscar usuário alvo

```dql
data.win.eventdata.targetUserName: "labuser"
```

Uso:
Investigar eventos onde o usuário `labuser` foi alvo de autenticação ou alteração.

---

## Buscar usuário que executou a ação

```dql
data.win.eventdata.subjectUserName: "vboxuser"
```

Uso:
Investigar ações executadas pelo usuário `vboxuser`.

---

## Buscar logon interativo

```dql
data.win.eventdata.logonType: 2
```

Uso:
Identificar tentativas de logon local/interativo.

---

## Buscar falhas com senha incorreta

```dql
data.win.eventdata.subStatus: "0xc000006a"
```

Uso:
Investigar falhas de autenticação relacionadas a senha incorreta.

---

## Buscar falhas genéricas de autenticação

```dql
data.win.eventdata.status: "0xc000006d"
```

Uso:
Identificar falhas genéricas de autenticação no Windows.

---

## Buscar eventos originados da própria máquina

```dql
data.win.eventdata.ipAddress: "::1"
```

Uso:
Investigar autenticações originadas localmente no próprio endpoint.

---

## Campos importantes em eventos de autenticação

```text
data.win.system.eventID
data.win.eventdata.targetUserName
data.win.eventdata.subjectUserName
data.win.eventdata.failureReason
data.win.eventdata.logonType
data.win.eventdata.status
data.win.eventdata.subStatus
data.win.eventdata.ipAddress
data.win.eventdata.workstationName
data.win.eventdata.authenticationPackageName
rule.description
rule.level
rule.id
```

---

## Interpretação de Logon Type

| Logon Type | Significado | Uso em SOC |
|---|---|---|
| 2 | Interativo/local | Login feito diretamente na máquina |
| 3 | Rede | Acesso via rede, compartilhamento ou serviço |
| 4 | Batch | Execução por tarefa agendada |
| 5 | Serviço | Logon realizado por serviço |
| 7 | Unlock | Desbloqueio de sessão |
| 10 | RemoteInteractive | Acesso remoto, como RDP |
| 11 | CachedInteractive | Logon com credenciais em cache |

---

## Interpretação de Status e SubStatus

| Código | Significado |
|---|---|
| 0xc000006d | Falha genérica de autenticação |
| 0xc000006a | Senha incorreta |
| 0xc0000064 | Usuário não existe |
| 0xc0000234 | Conta bloqueada |
| 0xc0000072 | Conta desabilitada |
| 0xc0000193 | Conta expirada |
| 0xc0000071 | Senha expirada |

---

## Caso do laboratório

No Caso 003, foi observado:

| Campo | Valor |
|---|---|
| Event ID | 4625 |
| Rule Description | Logon Failure - Unknown user or bad password |
| Rule ID | 60122 |
| Rule Level | 5 |
| Target User | labuser |
| Subject User | vboxuser |
| Logon Type | 2 |
| Status | 0xc000006d |
| SubStatus | 0xc000006a |
| Source IP | ::1 |
| Workstation | ENDPOINT |

---

## Observações de investigação

Falhas de autenticação devem ser analisadas considerando:

- Quantidade de tentativas
- Frequência dos eventos
- Usuário alvo
- Origem da tentativa
- Tipo de logon
- Horário do evento
- Se houve login bem-sucedido após várias falhas
- Se o usuário alvo é privilegiado
- Se a origem é local ou remota

Em ambiente real, múltiplas falhas podem indicar brute force, password spraying, uso indevido de credenciais ou erro legítimo de usuário.
