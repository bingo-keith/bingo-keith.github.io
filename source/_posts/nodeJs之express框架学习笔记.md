---

title: nodeJs之express框架学习笔记
date: 2018/7/31 22:16:26 
tags: node nodejs express

---

先介绍下express：

> Express 是一个基于 Node.js 平台的极简、灵活的 web 应用开发框架，它提供一系列强大的特性，帮助你创建各种 Web 和移动设备应用。

放上[Express中文官网](http://www.expressjs.com.cn/)，虽然是中文官网，但对于详细的API翻译并不完整，英文好的同学可以去阅读一下。至于为什么选择Express而不是koa，因为这两个框架来自同一团队，在绝大多数的用法上其实是很相似的，koa有更多的ECMAscript6的语法，而Express更适合初学者学习。

<!-- more -->

# 首先需要安装Nodejs

Nodejs官网点击下一步、下一步，这里不在赘述，如果成功安装，点击开始=>运行=>输入"cmd"=>输入node -v就可看到安装在电脑上的nodeJs的版本号。

# NPM安装Express

npm会和nodeJs一起安装，是一个包管理工具。

```
npm install express
```

# 新建一个app.js

```
var express = require('express');
var app = express();
app.get('/',function(req,res){
    res.send('<h1>Hello world!</h1>');
});
app.listen(1234);
```

这样一个简单的例子就写好了，在浏览器中输入http://localhost:1234，就可看到网页上的Hello world!

# app.get方法

app.get(path,function(req, res));req为请求信息，res为响应信息。

```
var express = require('express');
var app = express();
 
app.get('/', function(req, res) {
       res.send('首页');
});
app.get('/about', function(req, res) {
       res.send('关于');
});

app.listen(1234);
```

localhost:1234 会看到首页，localhost:1234/about 会看到关于。
上面的get请求返回的是文字，以下代码返回文件

```
var express = require('express');
var app = express();
app.get('/test',function(req,res){
    res.sendfile('test.html');//返回文件
})
```

若需访问其他静态文件则需以下设置

```
//根目录作为静态文件访问目录 （可访问根目录内所有文件）
app.use(express.static(path.join(__dirname, '/')));

//根目录下/src作为静态文件访问目录 （指定可访问的文件目录）访问地址为localhost:1234/main.css
app.use(express.static('src'));

//我们还可以添加'伪访问目录'，这样访问的地址就是localhost:1234/src/main.css
app.use('/src',express.static('src'));
```

# 中间件

中间件是用来处理HTTP请求的函数（所有的请求都会走中间件），可以用来完成各种特定的任务，比如检查用户是否登录、分析数据、以及其他在需要最终将数据发送给用户之前完成的任务。 它最大的特点就是，一个中间件处理完，可以把相应数据再传递给下一个中间件。

```
app.use(function(request, response, next){
    console.log("第一！");
    next();//执行next(),进入下一个中间件
});

app.use(function(request, response,next){
    console.log("第二！");
});
```

若在next()中写入任何参数next('error')，则代表抛出错误，程序不再执行下去。app.use()也可以根据不同的访问路径返回不同的内容。

```
var express = require("express");
var app = express();
 
app.use(function(req, res, next) {
    if(req.url == "/") {
        res.send("首页");
    }else {
        next();
    }
});
 
app.use(function(req, res, next) {
    if(req.url == "/about") {
        res.send("关于");
    }else {
        next();
    }
});

app.use(function(req, res) {
    res.send("404!");
});
app.listen(1234);
```

# app.all()

app.all()函数可以过滤所有路径的请求
格式：app.all(path,function(req, res));

```
app.all("*", function(req, res, next) {
    console.log('所有的路径请求都会走我这里')
    res.writeHead(200, { "Content-Type": "text/html;charset=utf-8" }); //设置响应头属性值
    next();
});
```

# query获取GET请求所带参数

假定请求地址为localhost:1234?name=myname

```
var express = require('express');
var app = express();
 
app.get("/", function(req, res) {
    console.log(req.query.name);//myname
});

app.listen(1234);
```

# param也可获取参数

```
//req.param("name")同样可以获得参数
console.log(req.param("name")); //myname

//不同的是 ‘ param可以获取不同的路由规则参数 ’
app.get("/user/:name/", function(req, res) {
    console.log(req.param("name")); //myname
    res.send("使用req.param属性获取具有路由规则的参数对象值!");
});
```

# params

和param相似，但params是一个可以解析包含着有复杂命名路由规则的请求对象的属性。

```
app.get("/user/:name/:id", function(req, res) {
    console.log(req.params.name); //"myname"
    console.log(req.params.id); //"123"
});
```

模版引擎这块就不写了，建议前后端分离。

# POST请求

格式：app.post(path,function(req, res));
前端代码：
```
<script>
var da={data:'testData'};
$.ajax({
    url:'/api',
    type:'POST',
    dataType:'json',
    data:da,
    success:function(data){
        console.log(data);
    }
})
</script>
```

要想使用body属性解析post请求参数值，我们需要先安装和引用body-parser中间件
安装：

```
npm install body-parser
```

使用：
```
var bodyParser = require('body-parser');
....
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

node代码：
```
var express=require('express');
var bodyParser = require('body-parser');
var app=express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

//post 请求
app.post('/api',function(req,res){
    res.send( { 'a ' : req.body.data } );
})

app.listen(1234);
```

请求结果{a,'testData'}

# 链式路由

可使用 app.route()创建路由路径的链式路由句柄。由于路径在一个地方指定，这样做有助于创建模块化的路由，而且减少了代码冗余和拼写错误。

```
app.route('/book')
    .get(function(req, res) { 
        res.send('Get a random book'); 
    }) 
    .post(function(req, res) { 
        res.send('Add a book'); 
    }) 
    .put(function(req, res) { 
        res.send('Update the book'); 
    });
```

# express.Router模块化路由

路由文件 route.js

```
var express = require('express');
var router = express.Router();

// 该路由使用的中间件
router.use(function timeLog(req, res, next) {
    console.log('Time: ', Date.now());
    next();
});
// 定义网站主页的路由
router.get('/', function(req, res) {
    res.send('home page');
});
// 定义 about 页面的路由
router.get('/about', function(req, res) {
    res.send('About birds');
});
//导出
module.exports = router;
```

app.js 引入

```
var express = require('express');
var birds = require('./birds');
...
app.use('/birds', birds);
//应用即可处理发自 /birds和 /birds/about 的请求，并且调用为该路由指定的 timeLog中间件。
```

# 重定向

```
//网址重定向 例1：使用一个完整的url跳转到一个完全不同的域名。
res.redirect("http://www.hubwiz.com");

//例2：跳转指定页面，比如登陆页，如下：
res.redirect("login");
```

# express-session

```
var session = require('express-session');
...
app.use(session({
 secret:'secret',
 resave:true,
 saveUninitialized:false,
 cookie:{
 maxAge:1000*60*10 //过期时间设置(单位毫秒)
 }
}));
```