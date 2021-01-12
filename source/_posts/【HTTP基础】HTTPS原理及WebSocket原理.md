---

title: 【HTTP基础】HTTPS原理及WebSocket原理
date: 2021/1/11 10:30:14 
tags: HTTP WebSoket 前端 网络 协议 聊天
---



由于HTTP没有加密机制，其传输的内容很容易泄漏，并且HTTP协议没法确认通信方，也无法保证接收到的报文在传输过程中是否被篡改，因此HTTPS是在HTTP协议的基础上提供了加密、认证和完整性保护的功能。HTTPS并非是应用层的一种新协议，只是HTTP通信接口部分用SSL和TLS协议代替而已，通常HTTP直接和传输层的TCP协议通信，当使用了SSL后，HTTP先和SSL协议通信，SSL再和TCP协议通信。

<!-- more -->

## 加密方法

近代的加密方法中加密算法是公开的，而密钥却是保密的，加密和解密都需要用到密钥，没有密钥就没法对加密内容进行解密，通过这种方式得以保持加密方法的安全性。加密和解密同用一个密钥的方式陈为共享密钥加密，也被称为`对称密钥加密`。以共享密钥方式的加密必须将密钥也发给对方，在网络上发送密钥很容易被攻击者获取。并且服务器如果对所有客户端都使用同样的共享密钥，无异于没有加密，所以HTTPS采用生成的`随机数`来作为共享加密算法的密钥。

公开密钥加密使用一对`非对称的密钥`，一把叫做私有密钥，一把叫做公有密钥，发送密文的一方使用对方的公开密钥进行加密处理，对方收到被加密的信息后，再使用自己的私有密钥进行解密，这样就不用担心密钥被攻击者获取。

HTTPS采用共享密钥加密和公开密钥加密两者并用的混合加密机制，如果仅仅保证密钥的安全性，使用公开密钥加密的方式就可以实现了，但是公开密钥加密方式比共享密钥加密，其处理速度要慢。所以`HTTPS在交换密钥环节采用公开密钥加密方式，之后建立通信交换报文阶段采用共享密钥加密方式`。

公开密钥加密方式也不能保证公开密钥本身的真实性，比如，在与某台服务器建立连接时，无法保证接收到的公开密钥就是需要连接的那个服务器的密钥，这个时候可以采用数字证书认证机构和其相关机关颁发的公开密钥证书。

## HTTPS的握手过程

1. 首先，客户端会发送一个https的请求，把自身支持的一系列密钥算法组件(Cipher Suite)发送给服务器。
2. 服务器接收到客户端所有的Cipher后与自身支持的对比，如果不支持则连接断开，反之则会从中选择一种加密算法和HASH算法以证书的形式返回给客户端，证书中包含了加密公钥，颁证机构，网站地址，失效日期等等。
3. 客户端收到服务器端的响应后会做以下几件事：
   1：验证证书的合法性，颁发证书的机构是否合法与是否过期，证书中包含的网站地址是否与正在访问的地址一致等，证书验证通过后，在浏览器的地址栏会加上一把小锁。
   2：证书验证通过后，生成随机密码，用证书中的公钥加密。
   3：使用约定好的HASH计算握手消息，并使用生产的随机数对消息进行加密，最后将之前生成的所有消息发送给网站。
4. 服务器接收到客户端传来的密文，用自己的私钥来解密取出随机数密码，然后用随机数密码解密浏览器发送过来的握手消息，并验证HASH是否是与浏览器发来的一致，然后使用密码加密一段握手消息，发送给浏览器。
5. 客户端用随机数解密并计算出握手消息的HASH，如果与服务器端发来的HASH一致，此时握手过程结束。

之后所有的通信数据将由之前浏览器生成的随机密码并利用对称加密算法进行加密。因为这串密码只有客户端和服务器知道，所有即使中间请求被拦截也是没法解密数据的，以此保证了通信的安全。
其中非对称加密算法用于在握手消息过程中加密生成的随机数密码，对称加密算法用于对真正传输的数据进行加密，而HASH算法用于验证数据的完整性，TLS握手过程中如果有任何错误，都会使加密连接断开，从而阻止了隐私信息的传输，由于HTTPS非常的安全，攻击者无法从中找到下手的地方，于是更多的是采用了假证书的手法来欺骗客户端，从而获取明文的信息。

## HTTPS攻击手段

浏览器在对证书进行验证时，以下几种情况会导致验证失败：

1. SSL证书不是由受信任的CA机构颁发的
2. 证书过期
3. 访问的网站域名与证书绑定的域名不一致

对HTTPS最常见的攻击手段就是SSL证书欺骗或者叫SSL劫持，是一种典型的中间人攻击，不过SSL劫持并非只是用于攻击目的，在一些特殊情况下利用SSL劫持可以更顺畅的访问网络。SSL劫持需要将攻击者接入到浏览器与目标网站之间，在传输数据的过程中，替换目标网站发给浏览器的证书，之后解密传输的数据，中间人攻击最好的环境是在局域网中，因为局域网中所有的计算机需要通过网关来接入互联网，因此攻击者只需要实施一次中间人攻击就可以顺利的截获所有计算机与网关之间传输的数据。一般SSL劫持，浏览器会给出证书错误的提示，如果继续访问，所有加密的数据都可以被攻击者解密。

SSLStrip攻击也需要将攻击者设置为中间人，之后将HTTPS访问替换为HTTP返回给浏览器，由于HTTP协议传输的数据都是未加密的，从而截获用户访问的数据。对于登录账号密码等关键信息，可以在发送之前用javaScript进行一次加密处理，这种方法对SSLScrip和SSL劫持都是有效的。

HTTPS也存在一些问题，那就是处理速度会变慢，一是由于存在HTTPS握手环节，导致通信变慢，二是由于存在传输信息加密处理，消耗CPU及内存资源，这对于高并发的服务器而言，更是一种性能瓶颈，所以服务器只是对含有敏感信息的数据进行加密。

# WebSoket原理

HTTP协议的通信都是浏览器发出一个请求，服务器接收请求后进行处理并返回结果，浏览器再将接收到的信息进行渲染，这种机制对于实时性要求高的应用场景显得捉襟见肘，传统请求-响应模式的web开发通常采用轮询方案，就是浏览器以一定时间间隔频繁的向服务器请求数据，来保持客户端数据的实时更新，但是服务器数据可能并没有更新，会带来很多无谓的请求，浪费带宽，效率低下。

webSocket是HTML5下的一种新的协议，基于TCP传输协议，本身属于应用层协议，并且复用了HTTP握手通道。它实现了浏览器与服务器的全双工通信，能更好的节省服务器资源和带宽并达到实时通讯的目的，webSocket与HTTP长连接的区别：HTTP长连接在建立TCP连接后，在每个请求中任需要单独发送请求头，数据传输的效率低，并且服务器不能主动给浏览器推送数据。

## WebSocket建立连接

webSocket借用HTTP的协议来完成握手，首先通过HTTP发起请求报文，报文如下：

```
GET / HTTP/1,1
Host: localhost:8080
Origin: http://127.0.0.1:3000
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-key:w4v7O6xFTi36lq3RNcgctw==
Sec-WebSocket-Protocol:chat, superchat
Sec-WebSocket-Version:13
```

在上面的HTTP请求头中，Connection和Upgrade字段表示请求服务器升级协议为webSocket，其中Sec-WebSocket-key的值是随机生成的Base64编码的字符串，Sec-WebSocket-Protocol和Sec-WebSocket-Version字段指定子协议和版本。服务器响应头如下：

```
HTTP/1.1 101 Switching Protocols
Connection:Upgrade
Upgrade: websocket
Sec-WebSocket-Accept: Oy4NRAQ13jhfONC7bP8dTKb4PTU=
```

状态码101表示协议切换，到此完成协议升级，后续的数据交互都按照新的协议来。Sec-WebSocket-Accept是根据请求字段Sec-WebSocket-key计算出来的，其计算过程为：

1. 将Sec-WebSocket-Key跟258EAFA5-E914-47DA-95CA-C5AB0DC85B11字符串进行拼接，形成新的字符串w4v7O6xFTi36lq3RNcgctw==258EAFA5-E914-47DA-95CA-C5AB0DC85B11；
2. 通过sha1算法计算出结果，并转换成base64字符串。

浏览器会校验Sec-WebSocket-Accept的值，如果成功，webSocket的握手成功将开始接下来的数据传输。

webSocket传输数据的基本单位是帧，一般webScoket传输的一条消息会被切割成多个帧，数据帧有一个标志位FIN，FIN=1表示这是消息的最后一帧，接收端接收到帧数据后，如果该帧的FIN标识为1，就会将已经接收到的数据帧组装成一个完整的消息。

## WebSocket实战

### HTML5中的WebSoket

```
var Socket = new WebSocket(url,[protocol])
```

URL字符串必须以“ws”或“wss”(加密通信时)文字作为开头，protocol是Web应用能够使用的协议，可以是字符串，也可以是数组，如果一下代码中proto1和proto2是定义明确、可能已注册且标准化的协议名称，它们能够同时为客户端和服务器所理解，服务器会从列表中选择首选协议。webSocket支持onopen、onmessage、onclose、onerror四个异步事件。

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .kuang{text-align: center;margin-top:200px;}
        #mess{text-align: center}
        .value{width: 200px;height:200px;border:1px solid;text-align: center;line-height: 200px;display: inline-block;}
    </style>
</head>
<body>
<div id="mess">正在连接...</div>
<div id="content">

</div>

<div>
    <input type="text" id="mytext"><input type="button" id="dosend" value="发送">
</div>

<script>

    var mess = document.getElementById("mess");
    var content = document.getElementById("content");
    if(window.WebSocket){
        var ws = new WebSocket('ws://127.0.0.1:8001');

        ws.onopen = function(e){
            console.log("连接服务器成功");
            ws.send("game1");
        }
        ws.onclose = function(e){
            console.log("服务器关闭");
        }
        ws.onerror = function(){
            console.log("连接出错");
        }

        ws.onmessage = function(e){
            if(e.data==="success"){
                mess.innerHTML = "连接成功";
            }
            else{
                content.innerHTML += e.data+"<br>";
            }
        }
        document.querySelector("#dosend").onclick = function(e){
            ws.send("a:"+document.querySelector("#mytext").value);
            document.querySelector("#mytext").value = "";
        }
    }
</script>
</body>
</html>
```

webSocket.bufferAmount属性表示已在WebSocket上发送但尚未写入网络的字节数，一般用于调节发送速率。WebSocket API支持以二进制发送Blob和ArrayBuffer实例。当然也可以设置接收Blob和ArrayBuffer对象。

### Nodejs中的webSocket

webSocket的数据帧格式为以下部分：

![图片描述](https://i.loli.net/2021/01/11/2AvJO8FgMWn3qYz.png)
FIN：1位，表示是否结束, 1:结束
RSV[1-3]：用于协商扩展
opcode: 4位，0，1，2属于数据帧，8，9，10属于控制帧
mask:掩码，0表示不使用亚掩码，一般服务器回消息不会使用掩码，接收消息需要掩码
payloadLen 0-125 直接表示 数据长度

```
        126，后面16位对应数据长度
        127，后面64位对应数据长度
```

masking-key：如果mask为1，后面32位作为masking-key，mask为0，则缺省
payload Data: 负载的数据
其连接的代码为：

```javascript
var ws = require("nodejs-websocket");
console.log("开始建立连接...")

var game1 = null,game2 = null , game1Ready = false , game2Ready = false;
var server = ws.createServer(function(conn){
  conn.on("text", function (str) {
    console.log("收到的信息为:"+str)
    if(str==="game1"){ // game1已连接
      game1 = conn;
      game1Ready = true;
      //conn.sendText("success");
    }
    if(str==="game2"){ // game2已连接
      game2 = conn;
      game2Ready = true;
      //conn.sendText("success");
    }

    if(game1Ready && game2Ready) { // 两个同时连接
        // console.log(game1Ready, game2Ready)
        // console.log(typeof str, 'str')
        // console.log(str, 'str')
      if(str==="game1" || str==="game2"){ // 连接成功的状态
        // console.log(str==="game2", 'str1')
        game1.sendText("success");
        game2.sendText("success");
      }
      else{ // 发送消息
        // console.log(str==="game2", 'str2')
        game1.sendText(str);
        game2.sendText(str);
      }

    }

    //conn.sendText(str);
  })
  conn.on("close", function (code, reason) {
    console.log("关闭连接")
  });
  conn.on("error", function (code, reason) {
    console.log("异常关闭")
  });
}).listen(8001)
console.log("WebSocket建立完毕")
```