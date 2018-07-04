
### add id_rsa 

> mv id_rsa ~/.ssh
> eval $(ssh-agent -s)
> chmod 700 id_rsa
> ssh-add id_rsa

### docker git clone

```
$ docker run -ti --rm -v ${HOME}:/root -v $(pwd):/git alpine/git clone git@github.com:pnparadise/gitbook.git
```

### docker gitbook cmd
```
$ docker run --rm -v "$PWD:/gitbook" billryan/gitbook gitbook install
$ docker run --rm -v "$PWD":/gitbook billryan/gitbook gitbook build
```

> https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

### git base

> git config --global user.email "your\_email@example.com"
> git init
> git remote add origin git@github.com:yourname/yourrepository.git
> git add .
> git commit -m "First commit"
> git push -u origin master

#### 

### git 命令提交 推送

> http://lepidllama.net/blog/how-to-push-an-existing-cloud9-project-to-github/



