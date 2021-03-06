# travis

> 要求ruby 环境 gitbook示例 建议直接使用docker ruby-cli \
>  docker run -it --rm --name ruby ruby bash

> 登陆travis-cli.com 激活github项目 \
> 上传并加密部署私钥到travis 得到encrypted\_key 向量参数

```text

$ gem install travis
$ travis login --com --github-token XXX
$ travis encrypt-file id_rsa -r pnparadise/gitbook --com
```

> 项目根目录配置.travis.yml

```text
sudo: required
language: node_js

branches:
  only:
    - master

services:
  - docker

before_install:
  - openssl aes-256-cbc -K $encrypted_26b4962af0e7_key -iv $encrypted_26b4962af0e7_iv -in id_rsa.enc -out id_rsa -d
  - git config --global user.email "609888703@qq.com"
  - git config --global user.name "zhiqiang.huang"

install:
  - docker run --rm -v "$PWD":/gitbook billryan/gitbook gitbook install
  - docker run --rm -v "$PWD":/gitbook billryan/gitbook gitbook build

before_script:
  - echo -e "Host *\n\tStrictHostKeyChecking no\n" >> ~/.ssh/config

script: true

after_success:
- eval "$(ssh-agent -s)"
- chmod 600 id_rsa
- ssh-add id_rsa
- git fetch --unshallow
- git checkout -b built
- git add -A
- git commit -m 'built version'
- git remote add deploy ssh://root@wiki.sudoso.com:29029/root/gitbook.git
- git push deploy built --force
```

> 目标服务器 wiki.sudoso.com 配置
>
> 第一步 ~/.ssh/authorized\_keys 末尾追加部署公钥
>
> 第二步 建立空仓库 用于接收built代码 [https://gist.github.com/noelboss/3fe13927025b89757f8fb12e9066f2fa](https://gist.github.com/noelboss/3fe13927025b89757f8fb12e9066f2fa)

```text
git init --bare ~/gitbook.git
```

> 新建 hooks/post-receive 脚本 chmod u+x

```text
#!/bin/sh
#
# An example hook script to prepare a packed repository for use over
# dumb transports.
#
# To enable this hook, rename this file to "post-update".
while read oldrev newrev ref
do
    #only checking out the master (or whatever branch you would like to deploy)
    if [[ $ref =~ .*/built$ ]];
    then
        echo "built ref received.  Deploying built branch to production..."
        git --work-tree=/root/gitbook/ --git-dir=/root/gitbook.git/ fetch --all
        git --work-tree=/root/gitbook/ --git-dir=/root/gitbook.git/ reset --hard built
        docker restart nginx
    else
        echo "Ref $ref successfully received.  Doing nothing: only the master branch may be deployed on this server."
    fi
done
```

> 附赠 docker nginx 配置

```text
nginx vhost.conf

server {
    listen       80;
    server_name  wiki.sudoso.com;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /wwwroot;
        index  index.html ;
    }
}



$ docker run -d --name nginx -v /conf/nginx/nginx.conf:/etc/nginx/nginx.conf \
    -v /conf/nginx/conf.d:/etc/nginx/conf.d -v /root/gitbook/_book:/wwwroot \
    --restart always -p 80:80 -p 443:443 nginx
```

> 总结思路: gitbook客户端编辑完 提交到github 触发travis 自动部署 travis使用docker gitbook 构建gitbook网站 新建并得到built分支 push到目标服务器的仓库中 目标git仓库收到push后执行post-receive 脚本 checkout 最新built网站文件 重启nginx 服务 部署完成
>


