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
