# Cenário 01 — Falhas de login SSH no Kali (brute force leve)

**Objetivo:** gerar tentativas de login inválidas no SSH do Kali e validar a detecção no Wazuh.

**Ambiente:** Wazuh Server (Ubuntu) + Agente Kali Linux.

## Como reproduzir
1. **Ativar o SSH no Kali**  
   sudo systemctl enable --now ssh

2. **Gerar falhas (no próprio Kali, contra 127.0.0.1)**  
   - **Usuário inexistente (5x):**  
     for i in {1..5}; do ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no noone@127.0.0.1; done
   - **Usuário válido com senha errada (5x):**  
     for i in {1..5}; do ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no <seu_usuario>@127.0.0.1; done

3. **(Opcional) Gerar evento de sudo**  
   sudo -k; sudo id

## Coleta no agente (ossec.conf do Kali)
<localfile>
  <log_format>syslog</log_format>
  <location>journalctl -u ssh.service</location>
</localfile>

## Alertas esperados no Wazuh
- 5712 — sshd: brute force… (level 10)
- 5710 — sshd: Attempt to login using a non-existent user (level 5)
- 2502 — syslog: User missed the password more than one time (level 10)
- 5503 — PAM: User login failed (level 5)
- 5402 — Successful sudo to ROOT executed (level 3)

Filtro rápido (Security events):  
agent.name:"kali" AND (rule.id:5712 OR rule.id:5710 OR rule.id:2502 OR rule.id:5503 OR rule.id:5402)

MITRE ATT&CK: T1110 (Brute Force); opcional: T1548.003 (Sudo).

## Evidências
![Eventos de falha de login no Kali](img/kali_ssh_fail_events.png)

## Ações de resposta sugeridas
- Endurecer sshd_config (ex.: MaxAuthTries 3, PermitRootLogin no)
- Considerar uso de fail2ban
- Abrir ticket de hardening e revisão de acessos
