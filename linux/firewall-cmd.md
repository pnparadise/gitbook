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

### centos7 默认安装 无法更换ssh 22端口 解决
```console
yum install -y policycoreutils-python \
semanage port -a -t ssh_port_t -p tcp 23
```
