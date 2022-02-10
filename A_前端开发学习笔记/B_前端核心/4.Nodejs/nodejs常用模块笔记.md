# Node.js常用模块笔记

## Node.js能干什么

Nodejs 的单线程 非阻塞 I/O 事件驱动 

在 Java、PHP 或者.net 等服务器端语言中，会为每一个客户端连接创建一个新的线程。 而每个线程需要耗费大约 2MB 内存。也就是说，理论上，一个 8GB 内存的服务器可以同时 连接的最大用户数为 4000 个左右。要让Web应用程序支持更多的用户，就需要增加服务器 的数量，而 Web 应用程序的硬件成本当然就上升了。 Node.js 不为每个客户连接创建一个新的线程，而仅仅使用一个线程。当有用户连接了， 就触发一个内部事件，通过非阻塞 I/O、事件驱动机制，让 Node.js 程序宏观上也是并行的。 使用 Node.js，一个 8GB 内存的服务器，可以同时处理超过4 万用户的连接。

**它和浏览器的区别?**

1. 没有图形界面

2. 由于浏览器处于对电脑的保护

   1).同源策略

   2).不能编写本地文件

3. nodejs可以编写读写文件 (I/O);

## 模块的标识

模块的标识就是模块的名字或路径
node就是通过模块的标识来寻找模块的
对于内置模块和核心模块(npm中下载的模块)，直接使用模块的名字对其进行引入

```
var fs = require ( "fs");
var express = require ("express");
```

对于自定义的文件模块，需要通过文件的路径来对模块进行引入
路径可以是绝对路径,如果是相对路径必须以./或../开头
var router= require ( " . /router" );

通过npm下载的包都放到node_modules文件夹中
	我们通过npm下载的包，直接通过包名引入即可

node在使用模块名字来引入模块时，它会首先在当前目录的node_modules中寻找是否含有该模块
	如果有则直接使用，如果没有则去上一级目录的node_modules中寻找
	如果有则直接使用，如果没有则再去上一级目录寻找，直到找到为止
	直到找到磁盘的根目录，如果依然没有，则报错

## Buffer（缓冲区）

- Buffer的结构和数组很像，操作的方法也和数组类似

- 数组中不能存储二进制的文件，而buffer就是专门用来存储二进制数据

- 使用buffer不需要引入模块，直接使用即可

- 在buffer中存储的都是二进制数据，但是在显示时都是以16进制的形式显示
	buffer中每一个元素的范围是从00 - ff   0 - 255
	00000000 - 11111111

	计算机 一个0 或一个1 我们称为1位（bit）

	8bit = 1byte（字节）
	1024byte = 1kb
	1024kb = 1mb
	1024mb = 1gb
	1024gb = 1tb

	buffer中的一个元素，占用内存的一个字节

	而一个字符占一个字节，一个中文占三个字节
	
- Buffer的大小一旦确定，则不能修改，Buffer实际上是对底层内存的直接操作

```javascript
//将一个字符串保存到buffer中
var buf = Buffer.from(str);
console.log(buf.length); //占用内存的大小
console.log(str.length);//字符串的长度
console.log(buf);//显示16进制的数据

//创建一个指定大小的buffer
var buf2 = new Buffer(10);//10个字节的buffer 构造函数都是不推荐使用的
console.log(buf2.length);//10

//创建一个10个字节的buffer
var buf2 = Buffer.alloc(10);

//通过索引，来操作buf中的元素
buf2[0] = 88;
buf2[1] = 255; //只能存8位的二进制 大于8位取后八位
buf2[2] = 0xaa;    //能存16进制
buf2[3] = 255;

//只要数字在控制台或页面中输出一定是10进制
console.log(buf2[2].toString(16));

Buffer.allocUnsafe(size) //创建一个指定大小的buffer，
//但是buffer中可能含有敏感数据  可能分配到被使用过未被清除的内存
var buf3 = Buffer.allocUnsafe(10);
console.log(buf3);
buf.toString() 将缓冲区中的数据转换为字符串
```

## fs(File System) 文件系统

文件系统简单来说就是通过Node来操作系统中的文件

使用文件系统，需要先引入fs模块，fs是核心模块，直接引入不需要下载

### 文件的写入

#### 同步文件的写入

```
	- 手动操作的步骤
		1.打开文件
 			fs.openSync(path, flags[, mode])
 				- path 要打开文件的路径
 				- flags 打开文件要做的操作的类型
 					r 只读的
 					w 可写的
 				- mode 设置文件的操作权限，一般不传
			 返回值：
			 - 该方法会返回一个文件的描述符作为结果，我们可以通过该描述符来对文件进行各种操作

		2.向文件中写入内容
 			fs.writeSync(fd, string[, position[, encoding]])
 				- fd 文件的描述符，需要传递要写入的文件的描述符
 				- string 要写入的内容
 				- position 写入的起始位置
 				- encoding 写入的编码，默认utf-8

		3.保存并关闭文件
 			fs.closeSync(fd)
 				- fd 要关闭的文件的描述符异步
```

#### 异步文件写入

异步方法不可能有返回值，有返回值的话一定是同步的，异步方法的返回值只能是undefined

```javascript
fs.open(path, flags[, mode], callback)
- 用来打开一个文件
- 异步调用的方法，结果都是通过回调函数的参数返回的
- 回调函数两个参数：
 		err 错误对象，如果没有错误则为null   //体现了nodejs的错误优先原则 出错了先处理
 		fd  文件的描述符
fs.write(fd, string[, position[, encoding]], callback)
 	- 用来异步写入一个文件
fs.close(fd, callback)
 	- 用来关闭文件
```

异步的好处就是不会阻塞线程，一个地方出错不会影响后面的程序运行
同步的好处是更符合人类的一贯思维

#### 简单文件写入

```javascript
fs.writeFile(file, data[, options], callback)
fs.writeFileSync(file, data[, options])
- file 要操作的文件的路径
- data 要写入的数据
- options 选项，可以对写入进行一些设置
- callback 当写入完成以后执行的函数
- flag
	r 只读
	w 可写
	a 追加
```

#### 流式文件写入

```javascript
//创建一个可写流
/*
	fs.createWriteStream(path[, options])
		- 可以用来创建一个可写流
		- path，文件路径
		- options 配置的参数
 */
//可以通过监听流的open和close事件来监听流的打开和关闭
/*
	on(事件字符串,回调函数)
		- 可以为对象绑定一个事件

	once(事件字符串,回调函数)
		- 可以为对象绑定一个一次性的事件，该事件将会在触发一次以后自动失效

*/
ws.once("open", function () {
    console.log("流打开了~~~")
})
ws.once("close", function () {
    console.log("流关闭了~~~")
})
//通过ws向文件中输出内容
ws.write("通过可写流写入文件的内容")
ws.write("今天天气真不错")
ws.write("锄禾日当午")
ws.write("红掌拨清清")
ws.write("清清真漂亮")
//关闭流
ws.end()
```

### 文件的读取

1.同步文件读取 

2.异步文件读取 

3.简单文件读取 

​	fs.readFile(path[, options], callback) 

​	fs.readFileSync(path[, options]) 

​	- path 要读取的文件的路径 

​	- options 读取的选项 

​	- callback回调函数，通过回调函数将读取到内容返回(err , data) 

​		err 错误对象 

​		data 读取到的数据，会返回一个Buffer

```javascript
fs.readFile("an.jpg", function (err, data) {
	if (!err) {
		console.log(data)
		// 将data写入到文件中
		fs.writeFile("hello.jpg", data, function (err) {
			if (!err) {
				console.log("文件写入成功")
			}
		})
	}
})
```

4.流式文件读取

流式文件读取也适用于一些比较大的文件，可以分多次将文件读取到内存中

### 常用方法

1. fs.stat  检测是文件还是目录 

```js
    const fs = require ('fs')
    
    fs .stat('hello.js', (error, stats) =>{ 
      if(error){ 
        console .log(error) 
      } else { 
        console .log(stats) 
        console .log(`文件：${stats.isFile()}`) 
        console .log(`目录：${stats.isDirectory()}`) 
      }
    }) 
```

2. fs.mkdir  创建目录 

```js
    const fs = require ('fs') 
    
    fs .mkdir('logs', (error) => { 
      if(error){ 
        console .log(error) 
      } else { 
        console .log('成功创建目录：logs') 
      } 
    }) 
```

    3. fs.writeFile  创建写入文件 

```js
    fs .writeFile('logs/hello.log', '您好 ~ \n', (error) => { 
      if(error) { 
        console .log(error) 
      } else { 
        console .log('成功写入文件') 
      } 
    }) 
```

4. fs.appendFile 追加文件 

```js
    fs.appendFile('logs/hello.log', 'hello ~ \n', (error) => { 
      if(error) { 
        console .log(error) 
      } else { 
        console .log('成功写入文件') 
      } }) 
```

5.fs.readFile 读取文件 

```js
    const fs = require ('fs') 
     
    fs.readFile('logs/hello.log', 'utf8', (error, data) =>{ 
      if (error) { 
        console .log(error) 
      } else { 
        console .log(data) 
      } 
    }) 
```

6.fs.readdir 读取目录 

```js
    const fs = require ('fs') 
 
    fs .readdir('logs', (error, files) => { 
        if (error) { 
        console .log(error)  
        } else {
        console .log(files) 
        }
    }) 
```



7.fs.rename 重命名 

```js
    const fs = require ('fs') 
 
    fs .rename('js/hello.log', 'js/greeting.log', (error) =>{ 
        if (error) {   
        console .log(error) 
        } else {   
        console .log('重命名成功')
        }
    })  
```

8. fs.rmdir  删除目录 

```js 
fs.rmdir('logs', (error) =>{   
    if (error) { 
    console .log(error) 
    } else {  
    console .log('成功的删除了目录：logs')
    }
}) 
```

9. fs.unlink 删除文件 

```js
fs .unlink(`logs/${file}`, (error) => {   
    if (error) {     
    console .log(error)  
    } else {    
    console .log(`成功的删除了文件: ${file}`)
    } 
}) 
```

 

10. fs.createReadStream 从文件流中读取数 据 

```js
    const fs = require ('fs') 
    
    var fileReadStream = fs .createReadStream('data.json') 
    let count =0; 
    var str =''; 
     
    fileReadStream.on('data', (chunk) => { 
       console .log(`${ ++ count } 接收到：${chunk.length}`); 
       str +=chunk 
     }) 
     
    fileReadStream.on('end', () => { 
       console .log('--- 结束 ---'); 
       console .log( count ); 
       console .log( str );
    }) 
     
    fileReadStream.on('error', (error) => { 
       console .log(error) 
     }) 
 
```

11. fs.createWriteStream  写入文件 

 ```js
     var fs = require("fs"); 
     var data = '我是从数据库获取的数据，我要保存起来'; 
    // 创建一个可以写入的流，写入到文件 output.txt 中 
     var writerStr eam = fs.createWriteStream('output.txt'); 
    // 使用 utf8 编码写入数据 
    
    writerStream.write(data,'UTF8'); 
    // 标记文件末尾 
    writerStream.end(); 
    // 处理流事件 -- > finish 事件 
    writerStream.on('finish', function() {   
        /*finish - 所有数据已被写入到底层系统时触发。 */ 
      console .log("写入完成。"); 
    }); 
    writerStream.on('error', function(err){ 
      console .log(err.stack); 
    }); 
    console.log("程序执行完毕"); 
 ```

12. 管道流 

管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传 递到另外一个流中。 

我们把文件比作装水的桶，而水就是文件里的内容，我们用一根管子(pipe)连接两个桶使得水从一个
桶流入另一个桶，这样就慢慢的实现了大文件的复制过程。 
以下实例我们通过读取一个文件内容并将内容写入到另外一个文件中。 

 ```js
    var fs = require("fs"); 
    // 创建一个可读流 
    var readerStream = fs.createReadStream('input.txt'); 
    // 创建一个可写流 
    var writerStream = fs.createWriteStream('output.txt'); 

    // 管道读写操作 
    // 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中 
    readerStream.pipe(writerStream ); 
    console.log("程序执行完毕"); 
 ```

------

##  url模块

URL模块用于解析和处理URL字符串，提供了三个方法：

```javascript
parse
format
resolve
```

### parse方法

将URL解析成一下几部分：

```javascript
href：原始url
protocal：url协议
host：主机
host中又包含以下信息：
auth：用户认证
port：端口
hostname：主机名
pathname：跟在host之后的整个文件路径
search：url中HTTP GET信息，包含了？
query：跟search类似，不包含？
hash：片段部分，也就是URL#之后的部分
```

示例：

>    var url = require("url")
>    var myurl="http://www.nodejs.org/some/url/?with=query&param=that#about"
>    parsedUrl=url.parse(myurl)
>
>结果如下：

```javascript
{ protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'www.nodejs.org',
  port: null,
  hostname: 'www.nodejs.org',
  hash: '#about',
  search: '?with=query&param=that',
  query: 'with=query&param=that',
  pathname: '/some/url/',
  path: '/some/url/?with=query&param=that',
  href: 'http://www.nodejs.org/some/url/?with=query&param=that#about' 
}
```

parse方法有两个参数：url字符串与一个可选的布尔值。布尔值用来确定queryString是否要用querystring模块来解析，默认为false。
如果为true，上面的结果如下：

> parsedUrl=url.parse(myurl,true)

```javascript
{ protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'www.nodejs.org',
  port: null,
  hostname: 'www.nodejs.org',
  hash: '#about',
  search: '?with=query&param=that',
  query: { with: 'query', param: 'that' },
  pathname: '/some/url/',
  path: '/some/url/?with=query&param=that',
  href: 'http://www.nodejs.org/some/url/?with=query&param=that#about' }
```

### format方法

format方法与parse方法相反，它用于根据某个对象生成URL字符串。
例如：

```javascript
        var urlObj={
           protocol: 'http:',
           slashes: true,
           auth: null,
           host: 'www.nodejs.org',
           port: null,
           hostname: 'www.nodejs.org',
           hash: '#about',
           search: '?with=query&param=that',
           query: { with: 'query', param: 'that' },
           pathname: '/some/url/',
           path: '/some/url/?with=query&param=that',
           href: 'http://www.nodejs.org/some/url/?with=query&param=that#about' }

    output=url.format(urlObj)
```

    'http://www.nodejs.org/some/url/?with=query&param=that#about'

### resolve方法

resolve(from, to)方法用于拼接URL，它根据相对URL拼接成新的URL。
例如：

```javascript
url.resolve('/one/two/three', 'four')         // '/one/two/four'
url.resolve('http://example.com/', '/one')    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two') // 'http://example.com/two'
```

------

## querystring模块

querystring从字面上的意思就是查询字符串，一般是对http请求所带的数据进行解析。querystring模块只提供4个方法，在我看来，这4个方法是相对应的。

___

这4个方法分别是

###  parse

parse这个方法是将一个字符串反序列化为一个对象。

参数：str指需要反序列化的字符串;
separator（可省）指用于分割str这个字符串的字符或字符串，默认值为"&";

```javascript
    querystring.parse("name=whitemu&sex=man&sex=women");
    /*{ name: 'whitemu', sex: [ 'man', 'women' ] }*/
                
    querystring.parse("name=whitemu#sex=man#sex=women","#",null,{maxKeys:2});
    /*{ name: 'whitemu', sex: 'man' }*/
```

###   stringify

```javascript
stringify这个方法是将一个对象序列化成一个字符串，与querystring.parse相对。
```

参数：obj指需要序列化的对象
    separator（可省）用于连接键值对的字符或字符串，默认值为"&";
    eq（可省）用于连接键和值的字符或字符串，默认值为"=";
    options（可省）传入一个对象，该对象可设置encodeURIComponent这个属性：
    encodeURIComponent:值的类型为function，可以将一个不安全的url字符串转换成百分比的形式,默认值为querystring.escape();

```javascript
    querystring.stringify({name: 'whitemu', sex: [ 'man', 'women' ] });
    /* 'name=whitemu&sex=man&sex=women'*/
    querystring.stringify({name: 'whitemu', sex: [ 'man', 'women' ] },"*","$");
    /*'name$whitemu*sex$man*sex$women'*/
```

###  escape

```javascript
escape可使传入的字符串进行编码
```

```javascript
    querystring.escape("name=老罗");
    //  name%3D%E8%80%81%E7%BD%97' 
```

###  unescape

```javascript
unescape方法可将含有%的字符串进行解码
```

```javascript
    querystring.unescape('name%3D%E8%80%81%E7%BD%97');
    //  'name=慕白' 
```

------

##  http模块

#### HTTP模块的使用

```javascript
//引用模块 var http = require("http"); 
 
//创建一个服务器，回调函数表示接收到请求之后做的事情
var server = http.createServer(function(req,res){  
//req 参数表示请求，res 表示响应  
console.log("服务器接收到了请求" + req.url);  
res.end();   
// End 方法使 Web 服务器停止处理脚本并返回当前结果 }); 
//监听端口 
server.listen(3000,"127.0.0.1"); 
```

设置一个响应头： 

```javascript
res.writeHead(200,{"Content-Type":"text/html;charset=UTF8"});   
```

我们现在来看一下 req 里面能够使用的东西。 

最关键的就是 req.url 属性，表示用户的请求 URL 地址。所有的路由设计，都是通过 req.url 来实现的。 我们比较关心的不是拿到 URL，而是识别这个 URL。 

识别 URL，用到了下面的 url 模块  

2.2、URL 模块的使用 

```js
    url.parse()  //解析 URL
    url.format(urlObject)   //操作的逆向操作 
    url.resolve(from, to)   //添加或者替换地址
```

------

## cookie-parse

 cookie是协议解决无状态的机制，session是解决无状态的常用实施方案，session是基于cookie的机制来完成；JWT是针对API请求进行关键信息对称加密的验证方案。三者之间的本质属性根本不同，虽然都用在了解决无状态连接下的会话认证场景。 

#### cookie会随着请求携带服务器

1).接受前端的cookie,就必须要使用 cookie-parser才能接受到

2).服务器发送cookie到浏览器,不需要配置 cookie-parser

#### 带签名的cookie

##### 1) 发送带有签名的cookie

```js
res.cookie("myname","abc666",{
    maxAge:1000*60*60*50, //毫秒
	signed:true
});
```

##### 2) 接受有签名的cookie  

```js
req.signedCookies 
```

#### 普通cookie

##### 1) 发送普通的cookie

```
res.cookie("myname","abc456",{
	maxAge: 有效时间
})
```

##### 2) 接受普通cookie

```
req.cookies
```

### 实例

```js
//接受前端发送过来的cookie
server.get("/aaa", (req, res, next) => {
    //接受普通cookie
    //    console.log( req.cookies);

    //接受签名的cookie
    console.log(req.signedCookies);
    res.end("aaa");
})

//发送cookei到前端
server.get("/bbb", (req, res, next) => {
    //发送普通cookie
    // res.cookie("myname","abc456",{
    //     maxAge:
    // })
    
    //发送签名cookie到浏览器
    res.cookie("myname", "abc666", {
        maxAge: 1000 * 60 * 60 * 50, //毫秒
        signed: true
    });
    res.end("bbb");
})
```

------

## cookie-session

session在服务器端,会话时的一种服务存储技术, session依赖cookie
cookie  在浏览器端,会话时的一种本地存储技术

配置中间件

```js
//配置中间件
server.use(cookieParser());
server.use(cookieSession({
    keys:[uuid.v1()],
    name: uuid.v1(),
    maxAge: 1000 * 60 * 2,
    overwrite:true
}))
//设置响应头
server.all("*", function (req, res, next) {
    res.setHeader("Content-Type", "text/html;charset=utf-8");
    next()
})
```

------

## uuid 唯一身份码

- 安全-强加密随机值

  使用方法：

  ```js
  const uuid = require('uuid');
  
  uuid.v4()
  
  ```

  

------

## express-session 

用于在服务端存储session及设置session的属性

```js
    const experss = require("express");
    const cookie = require("cookie-parser");
    const session = require("cookie-session");
    let server = experss();
    server.listen(8081);
    server.use(cookie("sdfaj2212"));
    server.use(session({
        name : "sesA",
        keys : ['a'],//签名秘钥
        maxAge : 1000 * 60 * 20
        resave: false,
        saveUninitialized: true
        //cookie的有效时间
    }));
    
    server.use("/", function(request, response){
    
        if(request.session["key"] == null){
            request.session["key"] = 1;
        } else {
            request.session["key"]++;
        }
    
        response.end(request.session["key"].toString());
    });
//续租
server.use(function (req, res, next) {
    req.session._garbage = Date();
    req.session.touch();
    next();
});
```

------

## morgan 访问日志

```js
var express = require('express')
var fs = require('fs')
var morgan = require('morgan')
var path = require('path')
 
var app = express()
 
// create a write stream (in append mode)
var accessLogStream = fs.createWriteStream(path.join(__dirname, 'access.log'), { flags: 'a' })
 
// setup the logger
app.use(morgan('combined', { stream: accessLogStream }))
 
app.get('/', function (req, res) {
  res.send('hello, world!')
})
```



------

## log4js 错误日志

```js
const log4js = require("log4js");
log4js.configure({
  appenders: { cheese: { type: "file", filename: "cheese.log" } },
  categories: { default: { appenders: ["cheese"], level: "error" } }
});
 
const logger = log4js.getLogger("cheese");
logger.trace("Entering cheese testing");
logger.debug("Got cheese.");
logger.info("Cheese is Comté.");
logger.warn("Cheese is quite smelly.");
logger.error("Cheese is too ripe!");
logger.fatal("Cheese was breeding ground for listeria.");
```



------

## morgan 

用于记录访问日志 包含接口、请求方式、ip地址等等

```js
const morgan = require('morgan')

// create a write stream (in append mode)
var accessLogStream = fs.createWriteStream(path.join(__dirname, `./logs/access-${moment().format("YYYY-MM-DD")}.log`), { flags: 'a' })

// setup the logger
server.use(morgan('combined', { stream: accessLogStream }))
```

------

## log4js

记录错误日志，用于跟踪和维护

**封装一个utils工具类**

```js
const log4js = require("log4js");
log4js.configure({
    appenders: { cheese: { type: "file", filename: "cheese.log" } },
    categories: { default: { appenders: ["cheese"], level: "error" } }
});

const logger = log4js.getLogger("cheese");

module.exports = {
    logger
}
```

**在controller中引入**

```js
const { logger } = require("../utils/logger")

class apiController {
    async login (req, res, next) {
        try {
            res.send({ msg: "login" })
        } catch (error) {
            logger.trace("Entering cheese testing");
            logger.debug("Got cheese.");
            logger.info("Cheese is Comté.");
            logger.warn("Cheese is quite smelly.");
            logger.error(error);
            logger.fatal("Cheese was breeding ground for listeria.");
        }
    }
}

```

## jwt-simple 

token模块 用于生成token & 解析token

token

整个token 分为三部分：头部（header)、载荷（payload, )、签证（signature)，上方截图中的token字符串是由此三部分经过base64加密得到的

```js
const tokenExpiresTime = 1000 * 60 * 60 * 24 * 7

//秘钥
const jstSecret = 'jstSecret'

//需要加密的对象
const payload = {
    user:'wang',
    environment:'web',
    expires: Date.now() + tokenExpiresTime
}

//encode
var token = jwt.encode(payload, jstSecret)

console.log('token: ', token)

//decoded
var decoded = jwt.decode(token, jstSecret)

console.log('decoded: ', decoded)
```

 algorithm 默认的为`HS256`，源码中可以看到其他可选 

```js
/**
 * support algorithm mapping
 */
var algorithmMap = {
  HS256: 'sha256',
  HS384: 'sha384',
  HS512: 'sha512',
  RS256: 'RSA-SHA256'
};
```

- **Payload**
   token 的核心，一般主要包括：
   iss(Issuer): token 签发者
   sub(Subject):  jwt所面向的用户
   aud(Audience):  token 收件人
   exp(Expiration Time): token 的过期时间，一般当前时间加期限
   nbf(Not Before):  验证该时间之前token 无效
   iat(Issued At):  token 签发时间
   jti(JWT ID):  jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击

- **signature**
  签名创建，使用编码后的header 和payload，通过指定的算法加秘钥得到的，秘钥必须保存在服务器端。例如：

  ```js
  HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
  ```

**token携带**
token可以作为请求链接的参数、请求主体（body）的参数，或者请求头（Header）的参数

**token获取**
可以通过规定携带方式，也可以同时通过多种方式获取

------

## svg-captcha

验证码插件

**配置svg-captcha,需要注意是svgCaptcha.create（{}），不是svgCaptcha（{}）**

```js
router.get('/code', async (ctx) => {
    const captcha = svgCaptcha.create({
        size: 4, //验证码长度
        fontSize: 45, //验证码字号
        noise: 1, //干扰线条数目
        width: 120, //宽度
        height: 36, //高度
        color: true, //验证码字符是否有颜色，默认是没有，但是如果设置了背景颜色，那么默认就是有字符颜色
        background: '#ccc' //beijing
    })

    ctx.session.code = captcha.text
    ctx.response.type = 'image/svg+xml'
    ctx.body = captcha.data
})
```

前端使用

```csharp
<div class="layui-input-block">
                        <input class="captchaInp" type="text" name="">
                        <img src="{{_HOST_}}/admin/login/code" alt="看不清？点击刷新" class="captchaPic"
                            onclick="javascript:this.src='{{_HOST_}}/admin/login/code?mt='+Math.random()">
 </div>
```

创建算数式验证码

```javascript
const cap = svgCaptcha.createMathExpr(options)
```

------

## json-server模块 

主要的作用就是搭建本地的数据接口，创建json文件，便于调试调用 
```markdown
1. 安装json-server  
	npm i json-server -g   

2. 定义数据  

3. 通过命名执行接口   
	json-server --watch data.json   
	json-server --watch --host 10.41.156.11 --port 8888 data.json   

	--watch 监听文件的变化 
    --host 设置ip 默认127.0.0.1
    --port 端口号 默认3000

json-server 常用接口 
	restful  什么接口对应做什么事情
    get
    1. 获取所有资源  
    	http://127.0.0.1:3000/users
    2. 根据id获取资源   
    	http://127.0.0.1:3000/users/1
    3. 分页查找  
    	http://127.0.0.1:3000/users?_page=1&_limit=2
    4. 模糊查询
    	http://127.0.0.1:3000/users?q=zs

    post   
    	http://127.0.0.1:3000/users
 
 	put 更新所有的资源
 		http://127.0.0.1:3000/users/1
 	patch 更新指定的资源 
 		http://127.0.0.1:3000/users/1

	delete 
		http://127.0.0.1:3000/users/1
```





