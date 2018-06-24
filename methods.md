### docker centos install

```
sudo yum remove docker \
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

```
sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
```

```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```
sudo yum install docker-ce
```

```
sudo systemctl start docker
```

### docker nginx

```
docker run --net nat --ip 172.168.0.3 -d --name nginx \
    -v /conf/nginx/nginx.conf:/etc/nginx/nginx.conf \
    -v /conf/nginx/conf.d:/etc/nginx/conf.d --restart always -p 80:80 -p 443:443 nginx
```

### docker gradle & spring boot

```
docker run --net nat --ip 172.168.0.2 -u root \
    -v /data/wwwroot:/data/wwwroot -w /data/wwwroot --restart always \
    --name app -d -p 8080:8080 gradle gradle bootRun
```

### docker shadowsocksr

```
docker run -d -p 443:51348 --restart=always -e PASSWORD=breakwall \
        -e METHOD=aes-256-cfb -e PROTOCOL=auth_sha1_v4 -e OBFS=tls1.2_ticket_auth \
        --name shadowsocksr breakwa11/shadowsocksr
```



