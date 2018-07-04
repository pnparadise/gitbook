### nginx 代理forward配置 携带host信息 

```
proxy_set_header  X-Real-IP  $remote_addr;
proxy_set_header Host $host;
proxy_pass http://47.104.222.160:8080
```



