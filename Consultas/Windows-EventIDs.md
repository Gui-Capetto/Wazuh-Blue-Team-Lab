# Windows Event IDs

Lista de Event IDs importantes para investigação SOC em ambiente Windows.

---

## Eventos de autenticação

| Event ID | Descrição | Uso em SOC |
|---|---|---|
| 4624 | Logon bem-sucedido | Identificar acessos válidos |
| 4625 | Falha de logon | Investigar senha incorreta, brute force ou password spraying |
| 4634 | Logoff realizado | Verificar encerramento de sessão |
| 4648 | Logon usando credenciais explícitas | Investigar uso de credenciais alternativas |
| 4672 | Privilégios especiais atribuídos ao novo logon | Identificar logon com privilégios administrativos |

---

## Eventos de contas de usuário

| Event ID | Descrição | Uso em SOC |
|---|---|---|
| 4720 | Conta de usuário criada | Identificar criação suspeita de usuário |
| 4722 | Conta de usuário habilitada | Verificar reativação de conta |
| 4723 | Tentativa de alteração de senha | Investigar mudanças de senha |
| 4724 | Tentativa de redefinição de senha | Investigar reset de senha |
| 4725 | Conta de usuário desabilitada | Verificar bloqueio/desativação de conta |
| 4726 | Conta de usuário excluída | Investigar remoção de conta |
| 4738 | Conta de usuário alterada | Identificar mudanças em propriedades de conta |
| 4740 | Conta de usuário bloqueada | Investigar possível brute force ou senha incorreta repetida |

---

## Eventos de grupos

| Event ID | Descrição | Uso em SOC |
|---|---|---|
| 4732 | Usuário adicionado a grupo local | Possível escalação de privilégio |
| 4733 | Usuário removido de grupo local | Alteração de permissões |
| 4728 | Usuário adicionado a grupo global | Investigar alteração de acesso |
| 4729 | Usuário removido de grupo global | Investigar remoção de acesso |
| 4756 | Usuário adicionado a grupo universal | Investigar alteração de privilégios |
| 4757 | Usuário removido de grupo universal | Investigar remoção de privilégios |

---

## Eventos de serviços e persistência

| Event ID | Descrição | Uso em SOC |
|---|---|---|
| 7045 | Serviço instalado | Possível técnica de persistência |
| 4697 | Serviço instalado no sistema | Investigar criação de serviço suspeito |
| 7036 | Serviço iniciado ou parado | Verificar alteração no estado de serviços |

---

## Eventos de processos e comandos

| Event ID | Descrição | Uso em SOC |
|---|---|---|
| 4688 | Processo criado | Investigar execução de comandos e binários |
| 4689 | Processo finalizado | Verificar encerramento de processos |

---

## Eventos de política e auditoria

| Event ID | Descrição | Uso em SOC |
|---|---|---|
| 4719 | Política de auditoria alterada | Investigar tentativa de reduzir visibilidade |
| 1102 | Log de auditoria limpo | Possível tentativa de apagar rastros |
| 4902 | Política de auditoria por usuário criada | Investigar alterações de auditoria |
| 4904 | Fonte de evento registrada | Investigar mudanças em logging |
| 4905 | Fonte de evento removida | Investigar remoção de logging |

---

## Eventos usados nos casos do laboratório

| Caso | Event ID | Descrição |
|---|---|---|
| Caso 001 | Sysmon Event ID 11 | Criação de arquivo pelo PowerShell em diretório temporário |
| Caso 002 | Sysmon Event ID 1 | Criação de processo para comando de discovery |
| Caso 003 | Windows Event ID 4625 | Falha de autenticação com usuário ou senha incorretos |

---

## Observações

O Event ID sozinho não confirma um incidente. A análise deve considerar:

- Usuário envolvido
- Processo executado
- Linha de comando
- Horário do evento
- Host afetado
- Frequência do evento
- Origem da conexão
- Relação com outros eventos próximos
