# CSRF攻击

## 定义

CSRF（Cross-site request forgery）跨站请求伪造：攻击者诱导受害者进入第三方网站，在第三方网站中，向被攻击网站发送跨站请求。利用受害者在被攻击网站已经获取的注册凭证，绕过后台的用户验证，达到冒充用户对被攻击的网站执行某项操作的目的。

一个典型的CSRF攻击有着如下的流程：

-   受害者登录a.com，并保留了登录凭证（Cookie）。
-   攻击者引诱受害者访问了b.com。
-   b.com 向 a.com 发送了一个请求：a.com/act=xx。浏览器会默认携带a.com的Cookie。
-   a.com接收到请求后，对请求进行验证，并确认是受害者的凭证，误以为是受害者自己发送的请求。
-   a.com以受害者的名义执行了act=xx。
-   攻击完成，攻击者在受害者不知情的情况下，冒充受害者，让a.com执行了自己定义的操作。

## CSRF攻击类型

### GET类型的CSRF

​	通过一个http请求即可完成攻击，通常使用自动发送的http请求完成，如下：

```html
<img src="http://bank.example/withdraw?amount=10000&for=hacker" >
```

### POST类型的CSRF

​	通常是结合一个自动提交的表单完成攻击，用户访问页面后，页面自动提交一个表单，相当于用户自己发起一起post请求。

### 连接类型的CSRF

​	链接类型的CSRF并不常见，比起其他两种用户打开页面就中招的情况，这种需要用户点击链接才会触发。这种类型通常是在论坛中发布的图片中嵌入恶意链接，或者以广告的形式诱导用户中招，攻击者通常会以比较夸张的词语诱骗用户点击，如下：

```html
<a href="http://test.com/csrf/withdraw.php?amount=1000&for=hacker" taget="_blank">重磅消息！！<a/>
```

## CSRF攻击特点

CSRF通常是跨域的，因为外域通常更容易被攻击者掌控。但是如果本域下有容易被利用的功能，比如可以发图和链接的论坛和评论区，攻击可以直接在本域下进行，而且这种攻击更加危险。

-   攻击一般发起在第三方网站，而不是被攻击的网站。被攻击的网站无法防止攻击发生。
-   攻击利用受害者在被攻击网站的登录凭证，冒充受害者提交操作；而不是直接窃取数据。
-   整个过程攻击者并不能获取到受害者的登录凭证，仅仅是“冒用”。
-   跨站请求可以用各种方式：图片URL、超链接、CORS、Form提交等等。部分请求方式可以直接嵌入在第三方论坛、文章中，难以进行追踪。

### 防护策略

根据CSRF的特点有两种专门的防护方法

**阻止不明外域的访问**

-   同源检测
-   Samesite Cookie

**提交时要求附加本域才能获取的信息**

-   CSRF Token
-   双重Cookie验证

### 同源检测

在HTTP协议中，每一个异步请求都会携带两个Header，用于标记来源域名：

-   Origin Header
-   Referer Header

这两个Header在浏览器发起请求时，大多数情况会自动带上，并且不能由前端自定义内容。
服务器可以通过解析这两个Header中的域名，确定请求的来源域。

### 使用Origin Header确定来源域名

在部分与CSRF有关的请求中，请求的Header中会携带Origin字段。字段内包含请求的域名（不包含path及query）。

如果Origin存在，那么直接使用Origin中的字段确认来源域名就可以。

但是Origin在以下两种情况下并不存在：

-   **IE11同源策略：** IE 11 不会在跨站CORS请求上添加Origin标头，Referer头将仍然是唯一的标识。最根本原因是因为IE 11对同源的定义和其他浏览器有不同，有两个主要的区别，可以参考[MDN Same-origin_policy#IE_Exceptions](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy#IE_Exceptions)
-   **302重定向：** 在302重定向之后Origin不包含在重定向的请求中，因为Origin可能会被认为是其他来源的敏感信息。对于302重定向的情况来说都是定向到新的服务器上的URL，因此浏览器不想将Origin泄漏到新的服务器上。

https://tech.meituan.com/fe_security_csrf.html