# nginx

## nginx 代理forward配置 携带host信息

```text
proxy_set_header  X-Real-IP  $remote_addr;
proxy_set_header Host $host;
proxy_pass http://47.104.222.160:8080
```

## nginx无后缀文件 拒绝下载

```text
location ~ /[^\.^/]+$ {
    deny all;
    return 404;
}
```

