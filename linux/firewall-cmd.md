## firewall-cmd 命令

增加端口
```sh
firewall-cmd --zone=public --permanent --add-port=5000/tcp
```

查看端口
```sh
firewall-cmd --zone=public --permanent --list-ports
```


更新服务
```sh
firewall-cmd --reload
```