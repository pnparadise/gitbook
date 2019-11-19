<h1 align="center"><a href="https://pnparadise.github.io" target="_blank">Markdown Book</a></h1>

> 

<p align="center">
<a href="#"><img alt="JDK" src="https://img.shields.io/badge/node-6-yellow.svg?style=flat-square"/></a>
<a href="https://travis-ci.org/pnparadise/markdown-editor"><img alt="Travis CI" src="https://travis-ci.com/pnparadise/markdown-editor.svg?token=GvFziCpGdzmjVWu9pS6a&branch=master"/></a>
</p>

------------------------------

## 简介

轻快，简洁，易用， Node + Vuejs + CodeMirror 开发的Github Flavored Markdown Editor + 微博客预览。

基于github api 数据接口，markdown文档直接同步到repository 实时预览+实时更新 打造 `serverless` 轻应用 

考虑到数据安全 github授权api拒绝cros请求。若需要在线文档编辑功能，授权服务器搭建必不可少  

基本配置
github.config.js
```javascript
module.exports = {
    owner: "pnparadise", //github账号
    defaultRepo: "gitbook", // 公开仓库 登录可提交issue comment
    personalRepo: "secretbook", //optional 私人仓库 仅管理员可读写 普通用户不可见
    publicToken : "040ab84ddddee4c947c9f0f23a546464045f01ad", //readonly 无登录用户只读public仓库
    oauthAppsClientId: "225031944a6bf76a419c", //管理员授权 读写所有仓库
    githubAppsClientId: "Iv1.fb87a658b1a64609", //普通用户授权 可读、可提交issue comment 至install的仓库
    oauthUrl: "https://api.ggga.ga/github.php" //授权服务地址 需要支持oauth 和 githubapp 成功后重定向至redirectUrl
};

```

授权服务器示例
```php
<?php
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, 'https://github.com/login/oauth/access_token');
        curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
        //管理员写入需要用到一个oauth应用
	if(!empty($_GET['client_id']) && $_GET['client_id'] == '<github oauth client_id >'){
        	$post = [
    			'client_id' =>  '<github oauth client_id >',
    			'client_secret' => '<github oauth client_secret>',
    			'code'   => $_GET["code"] ?? '',
			'redirect_uri' => "<回调此接口的完整路径>",
			'state' => $_GET["state"] ?? '' 
		];
	}
	else{
    	  //普通用户读取公开仓库需要一个githubapp应用 并install 该数据仓库
		$post = [
                        'client_id' => '<githubapp client_id >',
                        'client_secret' => '<githubapp client_secret >',
                        'code'   => $_GET["code"] ?? '',
                        'redirect_uri' => "<回调此接口的完整路径>",
                        'state' => $_GET["state"] ?? ''
                ];

	}
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
 
        $result = curl_exec($ch);
        $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
	if (curl_errno($ch)) {
	     echo 'Error: ' . curl_error($ch);
	     exit;
	}
	$http_status = curl_getinfo($ch, CURLINFO_HTTP_CODE);
	curl_close($ch);

	if(!empty($_GET["redirect_uri"])){
		$url = $_GET["redirect_uri"] . "?" . $result;
	}
	
	header("Location: $url");

```


