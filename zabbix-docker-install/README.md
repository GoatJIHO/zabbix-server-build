1. 필요한 이미지 Pull

- mysql:8.0-oracle
- zabbix/zabbix-java-gateway:6.4-alpine-latest
- zabbix/zabbix-server-mysql:6.4-alpine-latest
- zabbix/zabbix-web-nginx-mysql:6.4-alpine-latest
- zabbix/zabbix-agent:6.4-alpine-latest

```
docker pull mysql:8.0-oracle
docker pull zabbix/zabbix-java-gateway:6.4-alpine-latest
docker pull zabbix/zabbix-server-mysql:6.4-alpine-latest
docker pull zabbix/zabbix-web-nginx-mysql:6.4-alpine-latest
docker pull zabbix/zabbix-agent:6.4-alpine-latest
```

- 도커 네트워크 구축
```
docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 zabbix-net
docker network ls # 생성된 네트워크 확인
```


- mysql 서버
```
docker run --name mysql-server -t \
      -e MYSQL_DATABASE="zabbix" \
      -e MYSQL_USER="zabbix" \
      -e MYSQL_PASSWORD="zabbix_pwd" \
      -e MYSQL_ROOT_PASSWORD="root_pwd" \
      --network=zabbix-net \
      --restart unless-stopped \
      -d mysql:8.0-oracle \
      --character-set-server=utf8 --collation-server=utf8_bin \
      --default-authentication-plugin=mysql_native_password

docker ps # 생성된 컨테이너 확인
```

- Zabbix java gateway 인스턴스
```
docker run --name zabbix-java-gateway -t \
      --network=zabbix-net \
      --restart unless-stopped \
      -d zabbix/zabbix-java-gateway:6.4-alpine-latest
```


- zabbix 서버 인스턴스
```
docker run --name zabbix-server-mysql -t \
      -e DB_SERVER_HOST="mysql-server" \
      -e MYSQL_DATABASE="zabbix" \
      -e MYSQL_USER="zabbix" \
      -e MYSQL_PASSWORD="zabbix_pwd" \
      -e MYSQL_ROOT_PASSWORD="root_pwd" \
      -e ZBX_JAVAGATEWAY="zabbix-java-gateway" \
      --network=zabbix-net \
      -p 10051:10051 \
      --restart unless-stopped \
      -d zabbix/zabbix-server-mysql:6.4-alpine-latest
```

- zabbix 웹 인터페이스 시작
```
docker run --name zabbix-web-nginx-mysql -t \
      -e ZBX_SERVER_HOST="zabbix-server-mysql" \
      -e DB_SERVER_HOST="mysql-server" \
      -e MYSQL_DATABASE="zabbix" \
      -e MYSQL_USER="zabbix" \
      -e MYSQL_PASSWORD="zabbix_pwd" \
      -e MYSQL_ROOT_PASSWORD="root_pwd" \
      --network=zabbix-net \
      -p 80:8080 \
      --restart unless-stopped \
      -d zabbix/zabbix-web-nginx-mysql:6.4-alpine-latest
```
