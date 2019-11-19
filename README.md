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

授权服务器示例
```php
<?php
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, 'https://github.com/login/oauth/access_token');
        curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
        //return the transfer as a string
	if(!empty($_GET['client_id']) && $_GET['client_id'] == '225031944a6bf76a419c'){
        	$post = [
    			'client_id' =>  '<github oauth client_id >',
    			'client_secret' => '<github oauth client_secret>',
    			'code'   => $_GET["code"] ?? '',
			'redirect_uri' => "http://nonwa11.xyz:8443/github.php",
			'state' => $_GET["state"] ?? '' 
		];
	}
	else{
		$post = [
                        'client_id' => '<githubapp client_id 公开仓库只读权限即可>',
                        'client_secret' => '<githubapp client_secret 公开仓库只读权限即可>',
                        'code'   => $_GET["code"] ?? '',
                        'redirect_uri' => "http://nonwa11.xyz:8443/github.php",
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
	else{	
		$url = "http://localhost:8080?" . $result;
	}
	
	header("Location: $url");

```


