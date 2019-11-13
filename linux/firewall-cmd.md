## firewall-cmd 命令

增加端口
```console
firewall-cmd --zone=public --permanent --add-port=5000/tcp
firewall-cmd --zone=public --permanent --add-port=4990-4999/udp
```

查看端口
```console
firewall-cmd --zone=public --permanent --list-ports
```

生效
```console
firewall-cmd --reload
```