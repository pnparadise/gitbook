## 免服务器 让github pages支持vue-router history模式
#### history模式需要nginx服务层支持，github pages又不支持spa feature
#### 尽管这样，还是有 `serverless` 方案可用
>最简单的解决方式：复制index.html 为404.html \
>最无奈的解决方式：改用hash \
>最复杂的解决方式：自定义域名 cdn + rule 重定向

 404.html方式存在的问题 会响应404错误码 虽不影响使用，但会导致seo无法收录 \
 经研究还是有 `serverless` 改进方案：
 >自有域名cname + cloudflare worker 将404页面错误码改为200返回 \
 也就是和第三种方式结合使用
 
  worker mathes 填写自有域名如: www.xxx.com/*
  代码如下：
```javascript
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

/**
 * Fetch and log a request
 * @param {Request} request
 */
async function handleRequest(request) {
  let resp = await fetch(request);
  if(resp.status === 404){
    return new Response(resp.body, {status: 200, statusText: "OK", headers: resp.headers});
  }
  return resp;
}
```

ps. 免费版流量次数有限 且用且珍惜
