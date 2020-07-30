---

title: SeaJS学习笔记
date: 2018/1/14 20:51:27  
tags: seaJS 模块 CMD

---

今天专题把seaJs模块化开发了解学习了下，算是把seaJS的基本用法弄清楚了，其他的跟着项目慢慢再深入！现在把自己整理出来的资料保存一下，也方便以后遗忘了可以快速地回顾。
```
1. 先引seaJS - demo.html
	<script src="http://apps.bdimg.com/libs/seajs/2.3.0/sea.js"></script>

2.1 定义模块app.js
	define(function(require, exports, module){
		var jquery = require('./jquery.js');
		var demo = {
			'jquery': jquery
		}
		module.exports = demo;
	})

2.2 修改jQuery.js为支持CMD模块(3.2.1版本，10308行)
	if (typeof define === "function" && define.cmd) {
	  // 有 Sea.js 等 CMD 模块加载器存在
	  define(function (require, exports, module) {
	    // 使用 module.exports 向外暴露接口对象
	    module.exports = jQuery;
	  })
	}

3. 启用模块 - demo.html
	seajs.use('./app.js',function(v){
		v.$('html, body').animate({'scrollTop':v.$('input').offset().top},200).find('input').focus();
	})
```

<!-- more -->

# 1 SeaJS 介绍

- 关于 SeaJS
  + SeaJS 是一个适用于浏览器环境的 JavaScript 模块加载器
    * 一个库文件，类似于 jQuery
    * 使用这个库提供的规范的模块化的方式来编写 JavaScript 代码
    * 只关心 JavaScript 文件代码模块如何组织
      - 只关心 JavaScript 文件之间如何相互协议、引用、依赖
  + SeaJS 的作者是阿里巴巴支付宝前端架构师，花名：玉伯，玉伯也叫射雕
    * [Sea.js创始人玉伯的前端开发之路](http://www.csdn.net/article/2013-09-03/2816801-front-end-development-seajs-framework)
  + [SeaJS](http://seajs.org/)
  + [SeaJS -github](https://github.com/seajs/seajs/)

- 为什么学习和使用 SeaJS ？
  + **简单友好的模块定义规范**：SeaJS 遵循 CMD 规范，可以像 Node 一样书写模块代码
  + **自然直观的代码组织方式**：依赖的自动加载、配置简洁清晰，可以让我们更多的享受编码的乐趣
  + SeaJS兼容性非常好，几乎可以运行在任何浏览器引擎上
  + 注1：SeaJS 只是实现模块化开发的一种方式或者说一种工具而已，重在模块化思想的理解
  + 注2：因为 SeaJS 采用的 CMD 模块规范和 Node 中的 CommonJS 模块规范非常一致，所以有利于我们学习 Node 中的模块化编程

- 谁在用？
  + 淘宝网、支付宝、京东、爱奇艺。。。

- SeaJS 适用场景
  + 没有使用任何框架，例如 AngularJS
  + 例如 只写写 原生 JavaScript 或者用了一些第三方库
  + SeaJS 不提供任何功能性 API，只提供了解决 JavaScript 代码的命名污染和文件依赖的问题

# 2 快速上手（Getting Started）

0. 下载 sea.js 库文件
  + [SeaJS - Release](https://github.com/seajs/seajs/releases)
  + `bower install seajs`
  + `npm install seajs`
1. 在页面中引入 sea.js
2. 使用 `define` 函数定义模块
3. 使用 `require` 函数加载模块
4. 使用 `module.exports` 对外暴露接口对象
5. 使用 `seajs.use` 函数启动模块系统

# 3 API 详解

## 3.1 `seajs.use`

加载模块，启动模块系统。

- 加载一个模块 `seajs.use('id')`
- 加载一个模块，在加载完成时，执行回调 `seajs.use('id', callback)`
- 加载多个模块，加载完成时，执行回调 `seajs.use(['id1','id2',...],callback)`

- 注意：
  + 在调用 seajs.use 之前，需要先引入 sea.js 文件
  + seajs.use 与 `DOM ready` 事件没有任何关系。如果某些操作要确保在 `DOM ready` 后执行，需要使用 jquery 等类库来保证
  + seajs.use 理论上只用于加载启动，不应该出现在 `define` 中的模块代码里

## 3.2 `define(factory)`

`define` 是一个全局函数，用来定义模块。

`define` 接受 `factory` 参数，`factory` 可以是一个函数，也可以是一个对象或字符串。

`factory` 为对象、字符串时，表示模块的接口就是该对象、字符串。

- factory 是一个对象时
  + `define({})`

- factory 是一个字符串时
  + `define('hello')`

- factory 是一个函数时
  + `define(function(require, exports, module){})`

## 3.3 `require`

require 用来加载一个 js 文件模块，
require 用来获取指定模块的接口对象 `module.exports`。

require 在加载和执行的时候，js 会按照同步的方式和执行。

使用注意：

- 正确拼写
  + 模块 factory 构造方法的第一个参数 `必须` 命名为 require
- 不要修改
  + 不要重命名 require 函数，或在任何作用域中给 require 重新赋值
- 使用字符串直接量
  + require 的参数值 必须 是字符串直接量

Tips: 把 `require` 看做是语法关键字就好啦

## 3.4 模块标识

模块标识是一个字符串，用来标识模块。

- 模块标识可以不包含文件后缀名，比如 `.js`
  + seajs 推荐不加 .js 文件模块后缀

- 模块标识可以是 **相对** 或 **顶级** 标识

- 相对标识

相对标识以 `.` 开头，永远相对于当前模块所处的路径来解析。

- 顶级标识

顶级标识不以 `.` 或 `/` 开始，会相对模块系统的基础路径（base路径，默认是 sea.js 文件所属的路径）。
可以手动配置 base 路径。

```js
seajs.config({
  base: './js'
})
```

- 普通路径

除了相对和顶级标识之外的标识都是普通路径。
普通路径的解析规则，会相对当前页面解析。

```js
// 假设当前页面是 http://example.com/path/to/page/index.html

// 绝对路径是普通路径：
require.resolve('http://cdn.com/js/a');
  // => http://cdn.com/js/a.js

// 根路径是普通路径：
require.resolve('/js/b');
  // => http://example.com/js/b.js

// use 中的相对路径始终是普通路径：
seajs.use('./c');
  // => 加载的是 http://example.com/path/to/page/c.js

seajs.use('../d');
  // => 加载的是 http://example.com/path/to/d.js
```

**Tips**:

- 顶级标识始终相对 `base` 基础路径解析。
  + 如果不设置，base 路径默认就是 sea.js 库文件所属的路径
  + 可以通过 `seajs.config({ base: '基础路径' })` 来配置基础路径
- 绝对路径和根路径始终相对当前页面解析。
- 相对标识永远相对于当前文件
- `seajs.use` 中的相对路径始终相对当前页面来解析。

## 3.5 module

module 是一个对象，上面存储了与当前模块相关联的一些属性和方法。

- `module.id`
  + 模块的唯一标识，可以通过 `define` 方法的第一个参数来指定，默认为该模块文件的绝对路径
- `module.uri`
  + 模块的绝对路径
- `module.dependencies`
  + dependencies 是一个数组，表示当前模块的依赖
- `module.exports`
  + 当前模块对外提供的接口对象
  + 相当于每个模块内部最终都执行了这么一句话：`return module.exports`
  + 模块与模块之间的通信接口

## 3.6 exports

exports 仅仅是 module.exports 的一个引用。
也就是说修改了 exports 就相当于修改了 module.exports。

但是一旦在 factory 内部给 exports 重新赋值，并不会改变 module.exports 的值。
因此给 exports 赋值是无效的。

# 4 exports 和 module.exports 的区别

- 每个模块内部对外到处的接口对象始终都是 `module.exports`
- 可以通过修改 `module.exports` 或给它赋值改变模块接口对象
- `exports` 是 `module.exports` 的一个引用，就好比在每一个模块定义最开始的地方写了这么一句代码：`var exports = module.exports`

关于这俩哥们儿的区别请分析一下代码：

```js
var module = {
  exports: {}
}

function changeExports (exports, module) {
  // var exports = module.exports
  exports.foo = 'bar'

  // 这里赋值拿不到，不要使用使用
  // exports = function () {}
  return module.exports
}

changeExports(module.exports, module)
```

那为啥要有 `exports` ？

为了开发体验，API更友好，使用 exports 的时候，可以少写一个点儿。

如果你实在分不清楚 `exports` 和 `module.exports` 之间的区别，就只记得 `module.exports` 就可以了。

# 5 如何将一个普通的模块文件改造为兼容 CMD 规范的模块

```js
if (typeof define === "function" && define.cmd) {
  // 有 Sea.js 等 CMD 模块加载器存在
  define(function (require, exports, module) {
    // 使用 module.exports 向外暴露接口对象
	module.exports = jQuery; //以jQuery为例
	// 或return jQuery 或 exports = jQuery （尽量不要用后者）
  })
}
```

# 6 高级配置 `seajs.config(options)`

可以对 Sea.js 进行配置，让模块编写、开发调试更方便。

```js
seajs.config({

  // 别名配置
  alias: {
    'es5-safe': 'gallery/es5-safe/0.9.3/es5-safe',
    'json': 'gallery/json/1.0.2/json',
    'jquery': 'jquery/jquery/1.10.1/jquery'
  },

  // 路径配置
  paths: {
    'gallery': 'https://a.alipayobjects.com/gallery'
  },

  // Sea.js 的基础路径
  base: 'http://example.com/path/to/base/',
});
```