### docker-ce centos install

```
$ docker centos install
```

```
$ sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-selinux \
    docker-engine-selinux \
    docker-engine
```

```bash
$ sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
```

```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```
$ sudo yum install -y docker-ce
```

```
$ sudo systemctl start docker
```

### docker container 时区修改

```
$ docker exec -it [container:name] bash
$ cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
$ dpkg-reconfigure -f noninteractive tzdata
```

### docker nginx

```
$ docker run -d --name nginx \
    -v /conf/nginx/nginx.conf:/etc/nginx/nginx.conf \
    -v /conf/nginx/conf.d:/etc/nginx/conf.d --restart always -p 80:80 -p 443:443 nginx
```

配置文件\[[https://github.com/nginxinc/docker-nginx/tree/master/stable/alpine](https://github.com/nginxinc/docker-nginx/tree/master/stable/alpine)\]

### docker gradle & spring boot

```
$ docker run --net nat --ip 172.168.0.88 -u root --add-host=mysql.texustek.com:172.168.0.33 \
    --add-host=redis.texustek.com:172.168.0.79 \
    -v /data/wwwroot/m.texustek.com:/data/wwwroot -w /data/wwwroot --restart always \
    --name app-prod -d -p 8888:8888 gradle gradle bootRun -PjvmArgs="-Dspring.profiles.active=prod"
```

### docker mysql

```
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -v /data/mysql:/var/lib/mysql -v /conf/mysql:/etc/mysql/conf.d \
    -p 3306:3306 --net nat --ip 172.168.0.33 -d mysql:5.7 --character-set-server=utf8mb4 \
    --collation-server=utf8mb4_unicode_ci
```

### docker redis

```
docker run --name redis -d --net nat --ip 172.168.0.79 -v /data/redis:/data redis redis-server --appendonly yes
```

### docker shadowsocksr

```
$ docker run -d -p 443:51348 --restart=always -e PASSWORD=breakwall \
    -e METHOD=aes-256-cfb -e PROTOCOL=auth_sha1_v4 -e OBFS=tls1.2_ticket_auth \
    --name shadowsocksr breakwa11/shadowsocksr
```

### docker redis client link

```
$ docker run -it --link redis:redis --rm redis redis-cli -h redis -p 6379
```



