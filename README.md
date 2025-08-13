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
---

## 🖥️ Painel Inicial do Wazuh

Abaixo está o print do painel inicial do Wazuh após a configuração:

![Painel Inicial Wazuh](docs/wazuh_painel_inicial.png)

### Agentes Conectados
Após a instalação e configuração, os agentes Windows e Kali aparecem como ativos no Wazuh:

![Agentes ativos](docs/agents_ativos.png)
> 🔒 Alguns endereços IP foram ocultados propositalmente nos prints por questões de privacidade, mantendo apenas as informações relevantes para demonstração do projeto.

### Primeiros eventos coletados no Kali (SSH)

Após a configuração do agente no Kali para monitorar `/var/log/auth.log`, o Wazuh passou a registrar eventos de:
- Tentativas de brute force via SSH.
- Tentativas de login com usuário inexistente.
- Falhas de autenticação.
- Uso de privilégios administrativos (`sudo`).

**Exemplo de eventos capturados no Kali:**
![Logs de SSH no Kali](docs/wazuh_kali_ssh_logs.png)

## Simulações
- [Cenário 01 — Falhas de login SSH no Kali](docs/03-simulacoes/01-ssh-falhas-kali.md)
- [Cenário 02 — Falhas de login no Windows](docs/03-simulacoes/02-windows-falhas-login.md)

---

1. Configuração das VMs no VirtualBox.
2. Instalação do Ubuntu Server e configuração de rede.
3. Instalação do Wazuh e serviços associados.
4. Configuração de agentes no Windows.
5. Simulação de incidentes usando Kali Linux.
6. Análise de alertas no SIEM.
7. Documentação e publicação no GitHub.

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
- Finalizar instalação do **Ubuntu Server 22.04 LTS**.
- Instalar e configurar o **Wazuh**.
- Adicionar os endpoints e iniciar testes de segurança.

---

## ✍️ Autor
**Luciendel Alves**  
Estudante de Cibersegurança | Foco em SOC e Blue Team  

