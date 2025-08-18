### Passo a passo — Habilitar e validar o FIM (Syscheck) no Kali

**Pré-requisitos:** agente do Wazuh instalado no Kali e aparecendo como *Active* no servidor.

```
# 1) Verificar se o Syscheck está com uma única seção válida
sudo awk '/<syscheck>/{f=1} f; /<\/syscheck>/{f=0}' /var/ossec/etc/ossec.conf

# Se houver duas seções <syscheck> ... </syscheck>, edite:
sudo nano /var/ossec/etc/ossec.conf
```

Exemplo de configuração mínima (manter apenas UMA seção `<syscheck>`):
```xml
<syscheck>
  <disabled>no</disabled>
  <frequency>3600</frequency>        <!-- (para teste use 60, depois volte a 3600) -->
  <scan_on_start>yes</scan_on_start>
  <directories check_all="yes">/etc</directories>
  <directories>/root</directories>
  <directories>/bin,/sbin,/boot</directories>
  <directories>/usr/bin,/usr/sbin</directories>
  <!-- ignores comuns -->
  <ignore>/etc/mtab</ignore>
  <ignore>/etc/hosts.deny</ignore>
  <ignore>/etc/mail/statistics</ignore>
  <ignore>/etc/random-seed</ignore>
  <ignore>/etc/adjtime</ignore>
  <ignore>/etc/httpd/logs</ignore>
  <ignore type="sregex">.*\.log$|\.swp$</ignore>
</syscheck>
```

```
# 2) (Opcional para teste) Reduzir a frequência do scan para 60s
# Ajuste no <frequency> da mesma seção
<frequency>60</frequency>
```

```
# 3) Reconstruir o estado do Syscheck e reiniciar o agente
sudo systemctl stop wazuh-agent
sudo rm -rf /var/ossec/queue/syscheck
sudo systemctl start wazuh-agent

# Verifique se a pasta voltou a existir
ls -l /var/ossec/queue/
# deve aparecer: fim/  (entre outras pastas)
```

```
# 4) Gerar um evento de integridade no Kali
echo "teste $(date)" | sudo tee -a /etc/fim_demo > /dev/null
sudo chmod 600 /etc/fim_demo && sudo chmod 644 /etc/fim_demo
```

```
# 5) Aguardar 1–2 minutos (se frequency=60) e validar no servidor
# Ver alertas recebidos do agente (ex.: ID 003)
sudo tail -n 200 /var/ossec/logs/alerts/alerts.json | grep '"agent":{"id":"003"'
```

```
# 6) Confirmar no Dashboard (Discover / Security events)
# Buscar eventos do agente Kali relacionados ao Syscheck
agent.id:"003" AND rule.groups:"syscheck"

# Ou filtrar pelo arquivo específico
data.syscheck.path:"/etc/fim_demo"

# Resultado esperado:
# syscheck.event: modified
# rule.description: Integrity checksum changed.
```

```
# 7) (Depois do teste) Voltar a frequência para produção
sudo nano /var/ossec/etc/ossec.conf   # <frequency>3600</frequency>
sudo systemctl restart wazuh-agent
```

#### Observações (troubleshooting usado)
- Se o evento não aparecer no `ossec.log` do servidor, verifique **alerts.json**:
  ```
  /var/ossec/logs/alerts/alerts.json
  ```
- Se o Syscheck iniciar mas não gerar eventos, limpe `/var/ossec/queue/syscheck` e reinicie o agente (passo 3).
- Garanta que **existe apenas uma** seção `<syscheck>` no `ossec.conf` do agente.
