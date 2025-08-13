# Cenário 01 — Falhas de login SSH no Kali (brute force leve)

**Objetivo:** gerar tentativas de login inválidas no SSH do Kali e validar a detecção no Wazuh.

**Ambiente:** Wazuh Server (Ubuntu) + Agente Kali Linux.

## Como reproduzir
1. Ativar o SSH no Kali  
   `sudo systemctl enable --now ssh`
2. Gerar falhas (no próprio Kali, contra 127.0.0.1)  
   - Usuário inexistente (5x):  
     `for i in {1..5}; do ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no noone@127.0.0.1; done`
   - Usuário válido com senha errada (5x):  
     `for i in {1..5}; do ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no <seu_usuario>@127.0.0.1; done`
3. (Opcional) Gerar evento de sudo  
   `sudo -k; sudo id`

## Coleta no agente (ossec.conf do Kali)


