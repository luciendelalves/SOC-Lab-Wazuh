# 01 – Instalação do Ambiente SOC com Wazuh

Esta etapa documenta todo o processo de instalação do **Ubuntu Server 22.04 LTS** no VirtualBox e a configuração inicial do **Wazuh SIEM**.

---

## 📌 Resumo da Etapa
- Criar VM Ubuntu Server no VirtualBox.
- Configurar recursos (RAM, CPU, disco e rede).
- Instalar Ubuntu Server 22.04 LTS.
- Instalar Wazuh, Elasticsearch e Kibana via script oficial.
- Confirmar que o painel web do Wazuh está acessível.

---

## 🔹 Configuração da VM
- **Nome da VM:** `SIEM_Wazuh`
- **Sistema:** Linux → Ubuntu (64-bit)
- **RAM:** 4 GB
- **CPU:** 2 núcleos
- **Disco:** 30 GB (VDI, dinamicamente alocado)
- **Rede:** Adaptador 1 = NAT

---

## 🔹 Instalação do Ubuntu Server
1. Idioma: `English`
2. Teclado: `Portuguese (Brazil)`
3. Proxy: vazio
4. Disco: **Use entire disk**
5. Usuário: `socadmin`
6. Senha: `Lab@123`
7. Marcar **Install OpenSSH Server**
8. Não instalar pacotes adicionais

---

## 🔹 Instalação do Wazuh
Após login no Ubuntu Server:
```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
