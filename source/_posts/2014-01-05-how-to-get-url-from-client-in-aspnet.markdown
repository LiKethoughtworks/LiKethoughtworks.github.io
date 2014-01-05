---
layout: post
title: "ASP.NET 中如何在客户端获得正确的 URL"
date: 2014-01-05 15:06:11 +0800
comments: true
categories: aspnet url
---

## 可以使用 window.location

`window.location` 的定义是这样的：

* host：host 的名称。包含端口号，不带 Scheme(`http://`, `https://`)。例如 `localhost:8080`，`www.taobao.com`
* hostname：host 的名称，不包含端口号。例如 `localhost`
* port：端口号，如果为 '' 则为 `80`
* href：定义上说需要返回完整的 URL，但是 [W3School](http://www.w3schools.com/jsref/obj_location.asp) 上又提到可能是相对路径，因此慎用。
* protocol：采用的协议，例如 `http:`，`https:`

其他属性省略。因此使用 `window.location` 我们可以得到网站的根路径：

``` javascript
location.protocol + '//' + location.host
```

那么这个 Web 应用的相对地址就可以使用以下方法获得：

``` javascript
function getBaseUrl(relativePath) {
    return window.location.protocol + '//' + window.location.host + '/' + relativePath;
}
```

这种做法对于将应用程序部署到网站根域下的情况是比较适合的。

## 使用服务端和前端结合的方法

在通用的 Layout 页面上插入一段 Javascript，利用服务端后台生成正确的 Application Root URL。例如，在 ASP.NET MVC 的 _Layout.cshtml 中插入：

``` javascript
function getBaseUrl(relativePath) {
    var baseUrl = window.location.protocol + '//' + window.location.host + '@Url.Content("~")';
    return relativePath ? baseUrl + relativePath : baseUrl;
}
```

这样在所有后续页面上我们就可以稳妥的使用 `getBaseUrl()` 函数来获得正确的绝对 URL。这和部署是不相关的。