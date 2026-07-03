# Wazuh Blue Team Lab

Laboratório prático de Blue Team criado para estudo de monitoramento, análise de logs, investigação de alertas e mapeamento MITRE ATT&CK utilizando Wazuh, Sysmon e Windows Endpoint.

## Objetivo

Este projeto tem como objetivo simular atividades de um ambiente SOC, incluindo:

- Coleta e análise de logs do Windows
- Monitoramento com Wazuh
- Detecção de eventos com Sysmon
- Investigação de alertas
- Análise de falhas de autenticação
- Identificação de atividades de discovery
- Mapeamento com MITRE ATT&CK
- Documentação de casos de investigação

## Arquitetura do laboratório

- VirtualBox
- Wazuh OVA
- Windows Endpoint
- Wazuh Agent
- Sysmon
- Rede Host-only
- PowerShell
- Windows Event Logs

## Casos documentados

| Caso | Descrição | Técnica/Evento |
|---|---|---|
| Caso 001 | PowerShell criando arquivo temporário detectado pelo Sysmon | T1105 - Ingress Tool Transfer |
| Caso 002 | Account Discovery usando net1 user | T1087 - Account Discovery |
| Caso 003 | Falha de autenticação no Windows | Event ID 4625 |

## Estrutura do repositório

```text
Wazuh-Blue-Team-Lab
│
├── Casos
│   ├── Caso-001-PowerShell-Sysmon-T1105.md
│   ├── Caso-002-Account-Discovery-Net-User.md
│   └── Caso-003-Windows-Logon-Failure.md
│
├── Evidencias
│   └── Prints
│
└── Consultas
    ├── Wazuh-DQL.md
    ├── Windows-EventIDs.md
    ├── Sysmon-Queries.md
    ├── Authentication-Queries.md
    └── MITRE-Mapping.md
