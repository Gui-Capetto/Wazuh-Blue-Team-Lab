\# Caso 001 - PowerShell criando arquivo temporário



\## Data

03/07/2026



\## Ferramenta

Wazuh + Sysmon



\## Host

Windows-Endpoint



\## Alerta

Executable file dropped in folder commonly used by malware



\## MITRE

T1105 - Ingress Tool Transfer



\## Event ID

11



\## Processo

powershell.exe



\## Arquivo



C:\\Users\\vboxuser\\AppData\\Local\\Temp\\\_\_PSScriptPolicyTest\_qaonpzij.1gz.ps1



\## Investigação



O evento foi gerado durante testes do laboratório.



O processo responsável foi o PowerShell.



O arquivo foi criado na pasta Temp.



Nenhum indício de comprometimento foi encontrado.



\## Classificação



Verdadeiro positivo benigno.



\## Conclusão



Nenhuma ação necessária.

