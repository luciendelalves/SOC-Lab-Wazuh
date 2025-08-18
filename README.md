# SOC Lab com Wazuh SIEM

Este projeto é um laboratório prático de cibersegurança criado para simular o trabalho de um Analista de SOC (Security Operations Center) Júnior.  
O objetivo é configurar um ambiente com SIEM para coleta, análise e resposta a incidentes de segurança, utilizando ferramentas open source.

---

## 🎯 Objetivos do Projeto
- Montar um ambiente SOC funcional utilizando **Wazuh SIEM**.
- Simular ataques controlados e gerar eventos de segurança.
- Analisar alertas e criar relatórios de investigação.
- Documentar todo o processo para portfólio profissional.

---

## 🖥️ Estrutura do Laboratório
O projeto utiliza **máquinas virtuais no VirtualBox** com a seguinte arquitetura:

- **Ubuntu Server 22.04 LTS** → Servidor Wazuh + Elasticsearch + Kibana.
- **Windows 10/11** → Endpoint monitorado (agente Wazuh).
- **Kali Linux** → Máquina atacante para simulação de incidentes.

### Configuração do Nome do Host - Windows 10
O nome do computador foi configurado para `WIN10-LAB` para padronização no ambiente do SOC Lab e facilitar a identificação no Wazuh.

### Ubuntu Server com Wazuh Server
![Ubuntu Server com Wazuh Server](docs/docs_ubuntu_server.png)

### Windows com agente Wazuh
![Nome do host Windows 10](docs/win10_nome_host.png)

### Kali Linux inicial
![Tela inicial do Kali Linux](docs/docs_kali_inicial.png)

---

## 🛠️ Ferramentas Utilizadas
- **VirtualBox** (virtualização)
- **Ubuntu Server** (servidor SIEM)
- **Wazuh** (coleta e análise de logs)
- **Elasticsearch** (armazenamento e busca)
- **Kibana** (visualização de dados)
- **Kali Linux** (ferramentas de teste de invasão)
- **Wireshark** *(opcional)* para análise de pacotes

---

## 📅 Etapas do Projeto
1. Configuração das VMs no VirtualBox.
2. Instalação do Ubuntu Server e configuração de rede.
3. Instalação do Wazuh e serviços associados.
4. Configuração de agentes no Windows.
5. Simulação de incidentes usando Kali Linux.
6. Análise de alertas no SIEM.
7. Documentação e publicação no GitHub.

---

## 🖥️ Painel Inicial do Wazuh

Abaixo está o print do painel inicial do Wazuh após a configuração:

![Painel Inicial Wazuh](docs/wazuh_painel_inicial.png)

### Agentes Conectados
Após a instalação e configuração, os agentes Windows e Kali aparecem como ativos no Wazuh:

![Agentes ativos](docs/agents_ativos.png)  
> 🔒 Alguns endereços IP foram ocultados propositalmente nos prints por questões de privacidade, mantendo apenas as informações relevantes para demonstração do projeto.

---

### Primeiros eventos coletados no Kali (SSH)
Após a configuração do agente no Kali para monitorar `/var/log/auth.log`, o Wazuh passou a registrar eventos de:
- Tentativas de brute force via SSH.
- Tentativas de login com usuário inexistente.
- Falhas de autenticação.
- Uso de privilégios administrativos (`sudo`).

**Exemplo de eventos capturados no Kali:**  
![Logs de SSH no Kali](docs/wazuh_kali_ssh_logs.png)

---

### Primeiros eventos coletados no Windows (RDP/Logon)
Após a configuração do agente no Windows para monitorar eventos de segurança, o Wazuh passou a registrar eventos de:
- Tentativas de login com usuário inexistente.
- Tentativas de login com senha incorreta.
- Falhas de autenticação (Event ID 4625).

**Exemplo de eventos capturados no Windows:**  
![Logs de Logon no Windows](docs/win_4625_events.png)

### Monitoramento de Integridade de Arquivos (FIM)

Após corrigir a configuração do agente no Kali (remoção de seções duplicadas de `<syscheck>`), o Wazuh passou a registrar alterações em arquivos monitorados.

Foi criado o arquivo `/etc/fim_demo` no Kali e, após modificações, o agente enviou os eventos de **File Integrity Monitoring** para o servidor.  

**Exemplo de eventos capturados no Dashboard:**  
![Evento de FIM detectado no Kali](docs/fim_demo_event.png)

**Detalhes dos eventos:**
- **Agente:** Kali  
- **Arquivo monitorado:** `/etc/fim_demo`  
- **Evento:** modified  
- **Descrição:** Integrity checksum changed.  

> ✅ O módulo **Syscheck** do Wazuh está ativo e funcional, registrando mudanças críticas em diretórios sensíveis.

📄 [Veja o passo a passo completo da configuração](docs/03-simulacoes/03-fim-kali.md)

---

## 🎯 Simulações
- [Cenário 01 — Falhas de login SSH no Kali](docs/03-simulacoes/01-ssh-falhas-kali.md)  
- [Cenário 02 — Falhas de login no Windows](docs/03-simulacoes/02-windows-falhas-login.md)

---

## 📂 Status Atual
- [x] Criação do repositório no GitHub.
- [x] Configuração das VMs no VirtualBox.
- [x] Instalação do Ubuntu Server 22.04 LTS.
- [x] Instalação do Wazuh.
- [x] Conexão do agente Windows.
- [x] Simulação de ataques e geração de alertas.
- [x] Documentação final com prints.

---

## 📌 Próximos Passos
- Criar Cenário 03 — Simulação de varredura de portas no Kali e detecção no Wazuh.
- Criar Cenário 04 — Alterações críticas no sistema (ex.: criação de usuário suspeito) e monitoramento.
- Implementar regras adicionais de hardening no Ubuntu Server e no Windows.
- Testar integração do Wazuh com envio de alertas por e-mail e/ou Slack.
- Criar dashboards personalizados no Kibana para visualização dos alertas.

---

## ✍️ Autor
**Luciendel Alves**  
Estudante de Cibersegurança | Foco em SOC e Blue Team  
