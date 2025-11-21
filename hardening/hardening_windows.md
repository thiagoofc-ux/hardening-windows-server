# Hardening de Windows Server – Guia Completo

Este documento descreve todas as etapas realizadas durante o hardening do Windows Server, incluindo comandos, justificativas técnicas e evidências coletadas.

---

# 1. Criação e Configuração Inicial

###  Criar usuário administrador dedicado
- Usuário: `admin.security`
- Apenas para administração
- Senha forte inicial

###  Remover/Desativar contas default inseguras
- Desabilitar `Guest`
- Renomear `Administrator`

---

# 2. Política de Senhas (Password Policy)

Local:  
**GPMC → Domain Policy (ou Local Security Policy)**  
`Computer Configuration > Windows Settings > Security Settings > Account Policies > Password Policy`

Aplicado:

| Configuração | Valor |
|-------------|--------|
| Minimum password length | 12 caracteres |
| Maximum password age | 30 dias |
| Minimum password age | 1 dia |
| Password history | 24 senhas |
| Complexity | Habilitado |
| Store password using reversible encryption | Desabilitado |

Motivo: reduzir ataques de força bruta e preenchimento de credenciais.

---

# 3. Hardening do Firewall

Local:  
**Windows Defender Firewall with Advanced Security**

## 3.1 Regras Essenciais Criadas

### Bloquear portas sensíveis
- SMB/445 – Bloqueado (Inbound)
- Telnet/23 – Bloqueado (Inbound)
- PowerShell Remoting (5985/5986) – Bloqueado

###  RDP Restrito por Sub-rede
Criada regra:

**ALLOW_RDP_LOCAL**  
- Porta: 3389  
- Inbound  
- Remote IP: `sua_subrede/24`

**BLOCK_RDP_EXTERNAL**  
- Porta: 3389  
- Inbound  
- Remote IP: ANY  

---

# 4. Serviços Desnecessários Desabilitados

Via *services.msc* ou GPO:

- Xbox Services
- Remote Registry
- SSDP Discovery
- UPnP Device Host
- Fax
- Routing and Remote Access
- Print Spooler (se não for usar)

---

# 5. Auditoria de Segurança

Local:  
`Computer Configuration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration`

Aplicado:

###  Logon/Logoff
- Audit Logon – Success/Failure  
- Audit Account Logon – Success/Failure

###  Diretórios e Objetos
- Audit Object Access – Success/Failure  

###  Alterações de Conta
- User Account Management – Success/Failure  
- Security Group Management – Success/Failure

###  Eventos do Sistema
- System Events – Success/Failure  

---

# 6. Testes e Evidências

Gerados:

- Tentativas de login incorreto  
- Tentativa de alterar senha  
- Criação de novos usuários  
- Log de ativação de políticas  
- Bloqueio de portas pelo firewall  

Local das evidências:  
`./evidencias/prints.png`

---

# 7. Conclusão

O servidor foi endurecido com políticas de senha fortes, firewall restritivo, serviços minimizados e auditoria avançada.  
Este procedimento segue práticas reconhecidas de segurança (CIS Benchmarks, Microsoft Baselines).

