```
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu18.04_all.deb

sudo dpkg -i zabbix-release_6.4-1+ubuntu18.04_all.deb

sudo apt update

sudo apt install zabbix-agent2 zabbix-agent2-plugin-*
```

/etc/zabbix/zabbix_agent2.conf 파일에 server, serverAcitve, hostname 변수 수정
```
/etc/zabbix/zabbix_agent2.conf
```
<br/>
zabbix agent 버전에 따라 설치

https://www.zabbix.com/download?zabbix=6.4&os_distribution=alma_linux&os_version=9&components=agent_2&db=&ws=


GPU monitoring

```
#/etc/zabbix/zabbix_agentd/nvidia_gpus.conf 생성
UserParameter=gpu.discovery,nvidia-smi --query-gpu=gpu_bus_id,name,driver_version --format=csv,noheader,nounits | sed -e 's/, /,/g'
UserParameter=gpu.card[*],nvidia-smi --query-gpu=temperature.gpu,memory.total,memory.used,fan.speed,utilization.gpu,power.draw --format=csv,noheader,nounits -i $1 | sed -e 's/, /,/g'
```
https://github.com/zabbix/community-templates/tree/main/Server_Hardware/Other/template_nvidia-smi_multigpu/6.4
