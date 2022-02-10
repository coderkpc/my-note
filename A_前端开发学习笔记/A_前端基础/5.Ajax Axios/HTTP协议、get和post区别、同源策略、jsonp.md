## HTTP
HTTP（hypertext transport protocol）协议『超文本传输协议』，协议详细规定了浏览器和万维网服务器之间互相通信的规则。
约定, 规则<br><br>
服务器软件：IIS微软   tomcat  apache  ngix（负载均衡）<br>
浏览器软件：Chrome 火狐 IE Firefox

### HTTP特点
1. 断开式  被动模式   一次请求-->一次响应 
2. 短连接  降低服务器的压力 用完就断开 不保存长的连接
3. 无状态  http协议对事务处理没有记忆能力，处理请求的速度更快采用(cookie+session)

### 请求报文
重点是格式与参数
```
* 行      POST  /s?ie=utf-8  HTTP/1.1 
          请求方式   路径    协议
* 头      Host: atguigu.com
          主机号↑
*         Cookie: name=guigu
          携带导服务器上↑
*         Content-type: application/x-www-form-urlencoded
          数据类型↑
*         User-Agent: chrome 83
          浏览器操作系统的信息↑
* 空行
* 体      username=admin&password=admin
          数据
```

### 响应报文
```
* 行      HTTP/1.1  200  OK
          协议   状态码 
* 头      Content-Type: text/html;charset=utf-8
          服务器返回的数据类型 告诉浏览器以指定格式解析
          Content-length: 2048
          Content-encoding: gzip
          Date
          Expires
          Server
          
* 空行    
* 体        <html>
                <head>
                </head>
                <body>
                    <h1>尚硅谷</h1>
                </body>
            </html>
```
* 404
* 403
* 401
* 500
* 200
* 
## GET和POST
### get请求：
1.通过url地址传递参数<br>
2.相对post不安全<br>
3.速度要比post快<br>
4.长度有限制（后端）<br>
5.容易产生缓存<br>
6.传输的格式有限制（字符串，ASCII码）<br>
7.适合做查询

### post请求：
1.通过表单体传值<br>
2.传输的体积大（不做限制可以达到2G）<br>
3.传输的格式多（图片、视频、音频、文档、文件都可以）<br>
4.适合做提交：注册、上传<br>
5.相对get请求更安全<br>
6.速度比get请求慢<br>

## 同源策略

同源策略是一种ajax的安全机制,

如果出现 **协议,域名,端口**,三者不统一,就会产生跨域

```
https://www.baidu.com
http://www.baidu.com   //协议不同,跨域

http://www.baidu.com
http://mail.baidu.com    二级域名
http://aaa.bbb.baidu.com 三级域名    域名不同,跨域

http://www.baidu.com:8080
http://www.baidu.com:5500  端口不一致,跨域
```

#### 域名

* 一级域名
  **    baidu.com
  **    baidu.com.cn
* 二级域名
  **    mail.baidu.com
  **    mail.baidu.com.cn

#### 协议 端口

* http 80
* https 443
* mysql 3306
  这三个必须要一致，不一致就会产生跨域cors
  cors跨域的解决方案

#### 如何解决跨域(**CORS**)呢?

目前常见的方案

1. 在后端的**响应头**加上一句 `Access-Control-Allow-Origin:*`,这里的*表示所有请求,都可以访问该服务

2. 采用非官方的跨域方案 ,JSONP, 它算不上真正ajax请求,它只能算get请求,因为它是利用了带`src`属性的

   `script`,不受限制的访问外部资源,再又结合`callback`回调函数获取数据.

   还要和后端配合使用

3. 前端使用webpack模块中的server proxy ,实现服务器端代理,来解决跨域

CORS是一种浏览器的安全机制

## JSONP和JSON的区别 ?

```
jsonp 是一种非官方的跨域解决方案,它是利用带src属性的标签(script,iframe),不受限制的访问外部资源,并结合callback拿到数据,它并不是真正的ajax,它是一个get请求,更加适合做查询.
json  是一种轻量级的数据类型,能跨平台进行网络传输,能做配置文件.
xml   可扩展性标记语言,是一种重量级的数据格式,也能做跨平台进行网络传输和做配置文件.
```