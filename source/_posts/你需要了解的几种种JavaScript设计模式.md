---

title: 你需要了解的几种JavaScript设计模式
date: 2019/1/15 21:54:57 
tags: JavaScript 代码 设计模式

---

# 为什么要学习设计模式？

为了使应用程序具有可扩展性，可靠性和易维护性，应该编写符合设计模式的代码。

## 什么是设计模式？

- 设计模式是我们每天编程遇到的问题的可重用解决方案。

- 设计模式主要是为了解决对象的生成和整合问题。

- 换句话说，设计模式可以作为可应用于现实世界编程问题的模板。

**读万卷书，不如行万里路。接下来要贴代码了，从代码中就可以理解常用设计模式的主要概念。**

## 目录

1. 单例模式
2. 观察者模式（发布\订阅模式）
3. 中介者模式
4. 策略模式/状态模式
5. 命令模式
6. 迭代器模式
7. 参与者模式
8. 数据访问对象模式
9. 等待者模式
10. 同步模块模式
11. 异步模块模式
12. 链模式
13. 构造函数模式
14. 职责链模式
15. MVC模式
16. MVP模式
17. MVVM模式
18. 享元模式
19. 状态模式
20. 模板方法模式

<!-- more -->

# 单例模式

```
var Singleton = function() {
    var midseason = getMidseason();
    return {
        midseason: midseason, // 单例模式私有变量 
        sendAjax: function(api, formData, callback) { 
            // 代码...
        },
        submitForm: function(dlg){ 
            // 代码...
        },
        cancelForm: function(dlg){
            // 代码...
        },
        initChosen: function(selector, options){
            // 代码...
        },
        initDataTable: function(selector, options) {
            // 代码...
        },
        editInventoryType: function(data){
            // 代码...
            Singleton.initChosen($('.check-goods-type'));
        },
        changePriceCardStatus: function(data) {
            Singleton.sendAjax('api', {'data': data}, function(data) {
                // 代码...
            })
        }
    }
}()
```

# 观察者模式（发布\订阅模式）

```
var Observer = (function() {
    var _message = {};
    return {
        regist: function(type, fn) {
            if (typeof _message[type] === 'undefined') {
                _message[type] = [fn];
            } else {
                _message[type].push(fn);
            }
        },
        fire: function(type, args) {
            if (!_message[type]) return;
            var events = {
                type: type,
                args: args || {}
            }
            for ( var i = _message[type].length -1 ; i >= 0; i --) {
                _message[type][i].call(this, events);
            }
        },
        remove: function(type, fn) {
            if (_message[type] instanceof Array) {
                for( var i = _message[type]; i>= 0; i -- ) {
                    _message[type][i] === fn && _message[type].splice(i ,1);
                }
            };
        }
    }
})()

Observer.regist('test', function(e) {
    console.log(e.type, e.args.msg);
})

Observer.fire('test', {msg: '传递参数'});
```

# 中介者模式

```
var Mediator = function() {
    var _msg = {};
    return {
        register: function(type, action) {
            if (_msg[type]) {
                _msg[type].push(action);
            } else {
                _msg[type] = [];
                _msg[type].push(action);
            }
            return this;
        },
        send: function(type) {
            if (_msg[type]) {
                for (var i = 0, len = _msg[type].length; i < len; i++) {
                    _msg[type][i] && _msg[type][i]();
                };
            };
            return this;
        }
    }
}();
Mediator.register('demo', function() {
    console.log("first");
}).register('demo', function() {
    console.log("second");
}).send('demo');
```

# 策略模式与状态模式类似，区别是调用指定的方法传参

```
var Pattern = function() {
    var status = {
        return30: function(price) {
            console.log(price+'折扣');
        },
        return50: function(price) {
            console.log(price+'折扣');
        }
    }
    return function(item, price) {
        return status[item] && status[item](price);
    }
}()
Pattern('return30', 300)
```

# 命令模式

```
<canvas id="canvas"></canvas>
<script>
     var CanvasCommand = function() {
     	var canvas = document.getElementById('canvas'),
     		ctx = canvas.getContext('2d');
     	var Action = {
     		fillStyle: function(c) {
     			ctx.fillStyle = c;
	     	},
	     	fillRect: function(x, y, width, height) {
	     		ctx.fillRect(x, y, width, height);
	     	}
     	}
     	return {
     		excute: function(msg) {
     			if (!msg) return;
     			if (msg.length) {
     				for(var i = 0, len = msg.length; i < len; i ++) {
     					arguments.callee(msg[i]);
     				}
     			} else {
     				msg.param = Object.prototype.toString.call(msg.param) === '[object Array]' ? msg.param : [msg.param];
     				Action[msg.command].apply(Action, msg.param);
     			}
     		}
     	}
     }()
     CanvasCommand.excute([
     	{command: 'fillStyle', param: 'red'},
     	{command: 'fillRect', param: [20, 20, 100, 100]}
     ])
</script>
```

# 迭代器模式

```
<ul id="container">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ul>
<script>
    var Iterator = function(items, container) {
        var container = container && document.getElementById('container') || document,
            items = container.getElementsByTagName(items),
            length = items.length,
            index = 0;
        var splice = [].splice;
        return {
            // 获取第一个元素
            first: function() {
                index = 0;
                return items[index];
            },
            // 获取最后一个元素
            last: function() {
                index = length - 1;
                return items[index];
            },
            // 获取前一个元素
            pre: function() {
                if (--index > 0) {
                    return items[index];
                } else {
                    index = 0;
                    return null;
                }
            },
            // 获取后一个元素
            next: function() {
                if (++index < length) {
                    return items[index];
                } else {
                    index = length - 1;
                    return null;
                }
            },
            // 获取某一个元素
            get: function(num) {
                index = num >= 0 ? num % length : num % length + length;
                return items[index];
            },
            // 对每一个元素执行某一个方法
            dealEach: function(fn) {
                var args = splice.call(arguments, 1);
                for (var i = 0; i < length; i++) {
                    fn.apply(items[i], args);
                };
            },
            // 对某一个元素执行某一个方法
            dealItem: function(num, fn) {
                fn.apply(this.get(num), splice.call(arguments, 2));
            },
            // 排他方式处理某一个元素
            exclusive: function(num, allFn, numFn) {
                this.dealEach(allFn);
                if (Object.prototype.toString.call(num) === '[object Array]') {
                    for (var i = 0, len = num.length; i < len; i++) {
                        this.dealItem(num[i], numFn);
                    };
                } else {
                    this.dealItem(num, numFn);
                }
            },
        }
    }
    var demo = new Iterator('li', 'container');
    console.log(demo.first());
    console.log(demo.last());
    console.log(demo.pre());
    console.log(demo.next());
    console.log(demo.get(1));
    demo.dealEach(function(text, color) {
        this.innerHTML = text;
        this.style.background = color;
    }, 'test', 'pink');
    demo.exclusive([2, 3], function() {
        this.innerHTML = '被排除的';
        this.style.background = 'green';
    }, function() {
        this.innerHTML = '被选中的';
        this.style.background = 'red';
    })

</script>
```

# 参与者模式

其实就是把函数绑定和函数柯里化结合起来的设计模式

```
if (Function.prototype.bind === undefined) {
	Function.prototype.bind = function(context) {
		var slice = Array.prototype.slice,
			args = slice.call(arguments, 1),
			that = this;
		return function() {
			var addArgs = slice.call(arguments),
				allArgs = args.concat(addArgs);
			return that.apply(context, allArgs);
		}	
	}
};
```

# 数据访问对象模式

```
<script>
    // 数据访问对象模式可以方便我们对前端存储的管理，并且使用时更方便
    var BaseLocalStorage = function(preId, timeSign) {
        this.preId = preId; // 定义本地存储数据前缀
        this.timeSign = timeSign || '|-|'; // 定义时间戳与存储数据之间的拼接符
    }
    BaseLocalStorage.prototype = {
        status: {
            SUCCESS: 0, // 成功
            FAILURE: 1, // 失败
            OVERFLOW: 2, // 溢出
            TIMEOUT: 3, // 过期
        }, // 操作状态
        storage: localStorage || window.localStorage, // 保存本地存储链接
        getKey: function(key) {
            return this.preId + key;
        }, // 获取本地存储数据库数据真实字段
        /**
         * 添加修改数据
         * @param {string}   key      数据字段标识
         * @param {string}   value    数据值
         * @param {Function} callback 回调函数
         * @param {time}     time     添加时间
         */
        set: function(key, value, callback, time) {
            var status = this.status.SUCCESS,
                key = this.getKey[key];
            try {
                time = new Date(time).getTime() || time.getTime();
            } catch(e) {
                time = Date.now() + 1000 * 60 * 60 * 24 * 31;
            }
            try {
                this.storage.setItem[key, time + this.timeSign + value];
            } catch(e) {
                status = this.status.OVERFLOW;
            }
            callback && callback.call(this, status, key, value);
        },
        /**
         * 获取数据
         * @param  {string}   key      数据字段标识
         * @param  {Function} callback 回调函数
         * @return {[object]}          值和状态
         */
        get: function(key, callback) {
            var status = this.status.SUCCESS,
                key = this.getKey[key],
                value = null,
                timeSignLen = this.timeSign.length,
                that = this,
                index,
                time,
                result;
            try {
                value = that.storage.getItem(key);
            } catch(e) {
                result = {
                    status: that.status.FAILURE,
                    value: null,
                }
                callback && callback.call(this, result.status, result.value);
                return result;
            }
            if (value) {
                index = value.indexOf(that.timeSign);
                time = +value.slice(0, index);
                if (new Date(time).getTime() > Date.now() || time == 0) {
                    value = value.slice(index + timeSignLen);
                } else {
                    value = null,
                    status = that.status.TIMEOUT;
                    that.remove(key);
                }
            } else {
                status = that.status.FAILURE;
            }
            result = {
                status: status,
                value: value
            };
            callback && callback.call(this, result.status, result.value);
            return result;
        },
        /**
         * 删除数据
         * @param  {string}   key      数据字段标识
         * @param  {Function} callback 回调函数
         */
        remove: function(key, callback) {
            var status = this.status.FAILURE,
                key = this.getKey[key],
                value = null;
            try {
                value = this.storage.getItem(key);
            } catch(e) {}
            if (value) {
                try {
                    this.storage.removeItem(key);
                    status = this.status.SUCCESS;
                } catch(e) {}
            };
            callback && callback.call(this, status, status > 0 ? null : value.slice(value.indexOf(this.timeSign) + this.timeSign.length))
        }
    }
    var LS = new BaseLocalStorage('LS__');
    LS.set('a', 'xiao ming', function() {console.log(arguments);})
    LS.remove('a', function() {console.log(arguments);})
    LS.remove('a', function() {console.log(arguments);})
    LS.get('a', function() {console.log(arguments);})
</script>
```

# 等待者模式

```
<script>
var Waiter = function() {
    var dfd = [], // 注册等待对象容器
        doneArr = [], // 成功回调方法容器
        failArr = [], // 失败回调方法容器
        slice = Array.prototype.slice,
        that = this;
    // 监控对象类
    var Primise = function() {
        this.resolved = false;
        this.rejected = false;
    }
    // 监控对象类原型方法
    Primise.prototype = {
        resolve: function() {
            this.resolved = true;
            if (!dfd.length) return;
            for(var i = dfd.length - 1; i >= 0; i--) {
                // 如果有任意一个监控对象没有被解决或者解决失败则返回
                if (dfd[i] && !dfd[i].resolved || dfd[i].rejected) {
                    return;
                };
                // 清除监控对象
                dfd.splice(i, 1);
            }
            // 执行解决成功回调方法
            _exec(doneArr);
        },
        reject: function() {
            this.rejected = true;
            if (!dfd.length) return;
            // 清除所有监控对象
            dfd.splice(0);
            // 执行解决成功回调方法
            _exec(failArr);
        }
    }
    // 创建监控对象
    that.Deferred = function() {
        return new Primise();
    }
    // 回调执行方法
    function _exec(arr) {
        var i = 0,
            len = arr.length;
        for(; i < len; i ++) {
            try {
                arr[i] && arr[i]();
            } catch(e) {}
        }
    };
    // 监控异步方法 参数 监控对象
    that.when = function() {
        dfd = slice.call(arguments);
        var i = dfd.length;
        for( --i; i >= 0; i --) {
            // 如果不存在监控对象，或者监控对象已经解决，或者不是监控对象
            if(!dfd || dfd[i].resolved || dfd[i].rejected || !dfd[i] instanceof Primise) {
                dfd.splice(i, 1);
            }
        }
        return that;
    };
    // 解决成功回调函数添加方法
    that.done = function() {
        doneArr = doneArr.concat(slice.call(arguments));
        return that;
    };
    // 解决失败回调函数添加方法
    that.fail = function() {
        failArr = failArr.concat(slice.call(arguments));
        return that;
    };
}
var waiter = new Waiter();
var first = function() {
    var dtd = waiter.Deferred();
    setTimeout(function(){
        console.log("first finish");
        // 发布解决成功消息
        dtd.resolve();
    }, 500)
    return dtd;
}();
var second = function() {
    var dtd = waiter.Deferred();
    setTimeout(function(){
        console.log("second finish");
        // 发布解决成功消息
        dtd.resolve();
    }, 500)
    return dtd;
}();
waiter
    .when(first, second)
    .done(function() {
        console.log("success");
    }, function() {
        console.log("success again");
    })
    .fail(function() {
        console.log("fail");
    })
// first finish
// second finish
// success
// success again
</script>
```

# 同步模块模式

```
<div id="test">                fsadas         </div>
<script>
(function() {
    var F = F || {};
    /**
     * 定义模块方法
     * @param  {string}   str 模块路由
     * @param  {Function} fn  模块方法
     */
    F.define = function(str, fn) {
        var parts = str.split('.'),
            old = parent = this,
            i = len = 0;
        // 如果第一个模式是模块管理器单体对象，则移除
        if (parts[0] === 'F') {
            parts = parts.slice(1);
        };
        // 屏蔽对define与module模块方法的重写
        if (parts[0] === 'define' || parts[0] === 'module') {
            return;
        };
        for(len = parts.length; i < len; i ++) {
            if (typeof parent[parts[i]] === 'undefined') {
                // 生命当前模块
                parent[parts[i]] = {};
            };
        }
        // 缓存下一层的祖父模块
        old = parent;
        // 缓存下一层级父模块
        parent = parent[parts[i]];
        if (fn) {
            old[parts[-- i]] = fn();
        };
    }
    F.module = function() {
        var args = [].slice.call(arguments),
            // 获取回调执行函数
            fn = args.pop(),
            // 获取依赖模块，如果args[0]是数组，则依赖模块为args[0]，否则依赖模块为arg
            parts = args[0] && args[0] instanceof Array ? args[0] : args,
            // 依赖模块列表
            modules = [],
            // 模块路由
            modIds = '',
            // 依赖模块索引
            i = 0,
            // 依赖模块长度
            ilen = parts.length,
            // 父模块，模块路由层级索引，模块路由层级长度
            parent, j, jlen;
        // 遍历依赖模块
        while(i < ilen) {
            // 如果是路由模块
            if (typeof parts[i] === 'string') {
                // 设置当前模块父对象F
                parent = this;
                // 解析模块路由，并屏蔽掉模块父对象
                modIds = parts[i].replace(/^F\./, '').split('.')
                // 遍历模块路由层级
                for(j = 0, jlen = modIds.length; j < jlen; j ++) {
                    // 重置父模块
                    parent = parent[modIds[j]] || false;
                }
                // 将模块添加到依赖模块列表中
                modules.push(parent);
            } else { // 如果是模块对象
                // 直接加入依赖模块列表中
                modules.push(parts[i]);
            }
            i ++;
        }
        fn.apply(null, modules);
    }

    F.define('string', function() {
        return {
            trim: function(str) {
                return str.replace(/^\s+|\s+$/g, '');
            }
        }
    })
    F.define('dom', function() {
        var $ = function(id) {
            $.dom = document.getElementById(id);
            return $;
        }
        $.html = function(html) {
            if (html) {
                this.dom.innerHTML = html;
                return this;
            } else {
                return this.dom.innerHTML;
            }
        }
        return $;
    });
    console.log(F.dom('test').html());
    console.log(F.string.trim('    范德萨更大噶  '));

    F.module('dom', 'string.trim', function(dom, trim) {
        var html = dom('test').html();
        var str = trim(html);
        console.log('***' + html + '***' + str + '***');
    })
})()
</script>
```

# 异步模块模式

```
<script>
// 向闭包中传入模块管理器对象F，~屏蔽压缩文件时，前面漏写;报错
~(function(F) {
    // 模块缓存器。存储已创建模块
    var moduleCache = {
        /**
         * 设置模块并执行模块构造函数
         * @param {[type]}   moduleName 模块id名称
         * @param {[type]}   params     依赖模块
         * @param {Function} callback   模块构造函数
         */
        setModule: function(moduleName, params, callback) {
            // 模块容器，模块文件加载完成回调函数
            var _module, fn;
            // 如果模块被调用过
            if (moduleCach[moduleName]) {
                // 获取模块
                _module = moduleCache[moduleName];
                // 设置模块已经加载完成
                _module.status = 'loaded';
                // 矫正模块接口
                _module.exports = callback ? callback.apply(_module, params) : null;
                // 执行模块文件加载完成回调函数
                while(fn = _module.onload.shift()) {
                    fn(_module.exports);
                }
            } else {
                // 模块不存在，则直接执行构造函数
                callback && callback.apply(null, params)
            }
        },
        /**
         * 异步加载依赖模块所在文件
         * @param  {[type]} moduleName 模块路径
         * @param  {[Function]} callback  模块加载完成回调函数
         * @return {[type]}            [description]
         */
        loadModule: function(moduleName, callback) {
            // 依赖模块
            var _module;
            // 如果依赖模块被要求加载过
            if (moduleCache[moduleName]) {
                // 获取该模块信息
                _module = moduleCache[moduleName];
                // 如果模块加载完成
                if (_module.status === 'loaded') {
                    // 执行模块加载完成回调函数
                    setTimeout(callback(_module.exports), 0);
                } else {
                    // 缓存该模块所处文件加载完成回调函数
                    _module.onload.push(callback);
                }
            } else { // 模块第一次被依赖引用
                // 缓存该模块初始化信息
                moduleCache[moduleName] = {
                    moduleName: moduleName,
                    status: 'loading', // 模块对应文件加载状态，默认加载中
                    exports: null, // 模块接口
                    onload: [callback] // 模块对应文件加载完成回调函数缓冲器
                }
            };
            // 加载模块对应文件
            loadScript(getUrl[moduleName]);
        }
    };
    // 获取文件路径
    getUrl: function(moduleName) {
        return String(moduleName).replace(/\.js$/g, '') + '.js';
    },
    // 加载脚本文件
    loadScript: function(src) {
        var _script = document.createElement('script');
        _script.type = 'text/JavaScript';
        _script.charset = 'UTF-8';
        _script.async = true;
        _script.src = src;
        document.getElementsByTagName('head')[0].appendChild(_script);
    }
})((function() {
    // 创建模块管理对象F,并保存在全局作用域中
    return window.F = {
        /**
         * 创建或调用模块方法
         * @param  {string} url         参数为模块url
         * @param  {[type]} modDeps     参数为依赖模块
         * @param  {[type]} modCallback 参数为模块主函数
         */
        module: function(url, modDeps, modCallback) {
                // 将参数转化为数组
            var args = [].slice.call(arguments),
                // 获取模块构造函数（参数数组中最后一个参数成员）
                callback = args.pop(),
                // 获取依赖模块（紧邻回调函数参数，且数据类型为数组）
                deps = (args.length && args[args.length - 1] instanceof Array) ? args.pop() : [];
                // 该模块url
                url = args.length ? args.pop() : null,
                // 依赖模块序列
                params = [],
                // 未加载的依赖模块数量统计
                depsCount = 0,
                // 依赖模块序列中索引值
                i = 0,
                // 依赖模块序列长度
                len;
            // 获取依赖模块长度
            if (len = deps.length) {
                // 遍历依赖模块
                while(i < len) {
                    // 闭包保存i
                    (function(i) {
                        // 增加未加载依赖模块数量统计
                        depsCount ++;
                        // 异步加载依赖模块
                        loadModule(deps[i], function(mod) {
                            // 依赖模块序列中添加依赖模块接口引用
                            params[i] = mod;
                            // 依赖模块加载完成，依赖模块数量统计减一
                            depsCount --;
                            // 如果依赖模块全部加载
                            if (depsCount === 0) {
                                // 在模块缓存器中矫正该模块，并执行构造函数
                                setModule(url, param, callback);
                            };
                        })
                    })(i)
                    i ++;
                }
            } else { // 无依赖模块，直接执行回调函数
                // 在模块缓存器中矫正该模块，并执行构造函数
                setModule(url, [], callback);
            }
        }
    };
})());
</script>
```

# 链模式（仿jquery）

```
<div id="demo">123</div>
<script>
    var A = function(selector, context) {
        return new A.fn.init(selector, context);
    }
    A.fn = A.prototype = {
        constructor: A,
        init: function(selector, context) {
            this.length = 0,
            context = context || document;
            if(~selector.indexOf('#')) {
                this[0] = document.getElementById(selector.slice(1));
            } else {
                var domes = context.getElementsByTagName(selector),
                    i = 0,
                    len = doms.length;
                for(; i < len; i ++) {
                    this[i] = doms[i];
                }
                this.lenth = len;
            }
            this.context = context;
            this.selector = selector;
            return this;
        }
    }
    console.log(A('#demo'));
</script>
```

# 构造函数模式

```
<script>
	var obj = {
		'name': 'zs',
		'age': 11,
		'say': function(){
			console.log(this.name);
		}
	}
console.log('-----------------------------------------------------------------------');
	var Singletom = function(name, age){
		this.name = name;
		this.age = age;
		this.instance = null;
		this.say = function(){
			console.log(this.name);
		}
	}
	function getInstance(name, age){
		return this.instance || (this.instance = new Singletom(name, age)); 
	}
	var a = getInstance('zs', 11);
	var b = getInstance('ls', 12);
	a.say();
	b.say();
	console.log(a);
	console.log(b);
	console.log(a.name);
	console.log(a.age);
console.log('-----------------------------------------------------------------------');
	var getInstance2 = function(fn){
		var result;
		return function(){
			return result || (result = fn.call(this, arguments));
		}
	}
	// 创建div
	var createWindow = function(){
	    var div = document.createElement("div");
	    div.innerHTML = "我是弹窗内容";
	    div.style.display = 'none';
	    document.body.appendChild(div);
	    return div;
	};
	var divs = getInstance2(createWindow);
	console.log(divs());
</script>
```

# 职责链模式

```
职责链模式定义了请求的传递方向，通过多个对象对请求的传递，实现一个复杂的逻辑操作。因此职责链模式将负责的需求颗粒化逐一实现每个对象分内的需求，并将请求顺序地传递。对于职责链上的每一个对象来说，他都可能是请求的发起者也可能是请求的接收者。通过这样的方式不仅仅简化原对象的复杂度，而且解决原请求的发起者与元请求的接收者之间的耦合。当然也方便对每个阶段对象进行单元测试。同时对于中途插入的请求，此模式依然使用，并可顺利对请求执行并产出结果。

对于职责链上的每一个对象不一定都能参与请求的传递，有时会造成一丝资源的浪费，并且多个对象参与请求的传递，这在代码调试时增加了调试成本。

有感：职责链模式就是把一个复杂的逻辑处理分解成更细粒度的单元，结合项目中，就是把请求，弹框等功能分解封装好，职责链这种设计模式有利于实现那种某些需求点不确定的需求，把不确定的需求单独抽离出来并且单独封装，其他的功能点或需求点不影响，有利于分别对各个功能点做测试。
```

# MVC模式

```
<script>
// MVC架构模式很好地解决了页面中数据层、视图层、业务逻辑层（控制器）之间的耦合关系，使它们得到显性的区分，这也使得层次之间的耦合度降低。我们在开发中可以不用顾忌所有需求而专注于某一层次开发，降低了开发与维护成本，提升了开发效率。如果页面系统足够复杂，某些视图要共享同一组数据，或者某些需求的实现引用类似视图，此时MVC模式便可提高某些视图与数据的复用率。
$(function() {
    var MVC = MVC || {};
    MVC.model = function() {
        // 内部数据对象
        var M = {};
        // 服务器端获取的数据，通常通过ajax获取并存储
        M.data = {};
        // 配置数据，页面加载时即提供
        M.conf = {};
        // 返回数据模型层对象操作方法
        return {
            // 获取服务器端数据
            getData: function(m) {
                return M.data[m];
            },
            // 获取配置数据
            getConf: function(c) {
                return M.conf[c];
            },
            // 设置服务器端数据
            setData: function(m, v) {
                M.data[m] = v;
                return this;
            },
            // 设置配置数据
            setConf: function(c, v) {
                M.conf[c] = v;
                return this;
            }
        }
    }();
    MVC.view = function() {
        // 模型数据层对象操作方法引用
        var M = MVC.model;
        // 内部视图创建方法对象
        var V = {};
        // 获取视图接口方法
        return function(v) {
            // 根据视图名称返回视图
            V[v]();
        }
    }();
    MVC.controller = function() {
        // 模型数据层对象操作方法引用
        var M = MVC.model;
        // 视图数据层对象操作方法引用
        var V = MVC.view;
        // 控制器创建方法对象
        var C = {};
    }();
})
</script>
```

# MVP模式

```
<script>
~(function(window) {
    var MVP = function() {};

    MVP.model = function() {
        var M = {};
        M.data = [
            {
                text: '新闻头条',
                mode: 'news',
                url: 'http://www.example.com/01'
            },
            {
                text: '最新电影',
                mode: 'movie',
                url: 'http://www.example.com/02'
            }
        ];
        M.conf = {};
        return {
            getData: function(m) {
                return M.data(m);
            },
            /**
             * 设置数据
             * @param {[type]} m 模块名称
             * @param {[type]} v 模块数据
             */
            setData: function(m, v) {
                M.data[m] = v;
                return v;
            },
            getConf: function(c) {
                return M.conf[c]
            },
            /**
             * 设置配置
             * @param {[type]} c 配置项名称
             * @param {[type]} v 配置项值
             */
            setConf: function(c, v) {
                M.conf[c] = v;
                return v;
            }
        }
    }();

    MVP.view = function() {
        // 子元素或者兄弟元素替换模板
        var REPLACEKEY = '__REPLACEKEY__';
        // 获取完整元素模板
        function getHTML(str, type) {
            return str
                    .replace(/^(\w+)()[^\{\}*]?(\{([@\w]+)\})?(.*?)$/, function(match, $1, $2, $3, $4, $5) {
                        $2 = $2 || '';
                        $3 = $3 || '';
                        $4 = $4 || '';
                        $5 = $5.replace(/\{([@\w]+)\}/g, '');
                        return type === 'in' ?
                        '<' + $1 + $2 + $5 + '>' + $4 + REPLACEKEY + '</' + $1 + '>':
                            type === 'add' ?
                            '<' + $1 + $2 + $5 + '>' + $4 + '</' + $1 + '>' + REPLACEKEY
                            :
                            '<' + $1 + $2 + $5 + '>' + $4 + '</' + $1 + '>'
                    })
                    .replace(/#([@\-\w]+)/g, ' id="$1"')
                    .replace(/\.([@\-\s\w]+/g, ' class="$1"')
                    .replace(/\[(.+)\]/g, function(match, key) {
                        var a = key
                                .replace(/'|"/g, '')
                                .split(' '),
                            h = '';
                        for(var j = 0, len = a.length; j < len; j ++) {
                            h += ' ' + a[j].replace(/=(.*)/g, '="$1"');
                        }
                        return h;
                    })
                    .replace(/@(\w+)/g, '{#$1#}');
        }

        function eachArray(arr, fn) {
            for(var i = 0, len = arr.length; i < len; i ++) {
                // 将索引值、索引对应值、数组长度传入回调函数中并执行
                fn(i, arr[i], len);
            }
        }

        /**
         * 替换兄弟元素模板或者子元素模板
         * @param  {[type]} str 原始字符串
         * @param  {[type]} rep 兄弟元素模板或者子元素模板
         */
        function formateItem(str, rep) {
            // 用对应元素字符串替换兄弟元素模板或者子元素模板
            return str.replace(new RegExp(REPLACEKEY, 'g'), rep);
        }

        return function(str) {
            var part = str
                .replace(/^\s+|\s+$/g, '')
                .replace(/^\s+(>)\s+/g, '$1')
                .split('>'),
                html = REPLACEKEY,
                item,
                nodeTpl;
            eachArray(part, function(partIndex, partValue, partLen) {
                item = partValue.split('+');
                nodeTpl = REPLACEKEY;
                eachArray(item, function(itemIndex, itemValue, itemLen) {
                    nodeTpl = formateItem(nodeTpl, getHTML(itemValue, itemIndex === itemLen - 1 ? (partIndex === partLen - 1 ? '' : 'in') : 'add'));
                })
                html = formateItem(html, nodeTpl);
            })
            return html;
        }
    }()

    MVP.presenter = function() {
        var V = MVP.view;
        var M = MVP.model;
        var C = {};
        return {
            init: function() {
                for(var i in C) {
                    C[i] && C[i](M, V, i);
                }
            },
            add: function(modName, pst) {
                C[modName] = pst;
                return this;
            }
        }
    }();

    MVP.init = function() {};

    window.MVP = MVP;
})(window)

var C = {
    nav: function(M, V) {
        var data = M.getData('nav');
        data[0].choose = 'choose';
        data[data.length - 1].last = 'last';
        var tpl = V('li.@mode @choose @last[data-mode=@mode]>a#nax_@mode.nav-@mode[href=@url title=@text]>i.nav-icon-@mode-@mode+span{@text}');
        $
        .create('ul', {
            'class': 'navigator',
            'id': 'nav'
        })
        .html(
            A.formateString(tpl, data)
        )
        .appendTo('#container');
    }
}

window.onload = function() {
    MVP.init();
}
</script>
```

# MVVM模式

```
<div class="first" data-bind="type: 'slider', data: demo1">test1</div>
<div class="second" data-bind="type: 'slider', data: demo2">test2</div>
<div class="third" data-bind="type: 'progressbar', data: demo3">test3</div>
<script>
~(function() {
    var window = this || (0, eval)('this');
    // 获取页面字体大小，作为创建页面UI尺寸参照物
    var FONTSIZE = function() {
        // 获取页面body元素字体大小并转化成整数。
        return parseInt(document.body.currentStyle ? document.body.currentStyle['fontSize'] : getComputedStyle(document.body, false)['fontSize']);
    }();
    // 视图模型对象（策略对象）
    var VM = function() {
        var Method = {
            progressbar: function(dom, data) {
                // 进度条进度完成容器
                var progress = document.createElement('div'),
                    param = data.data;
                // 设置进度完成容器尺寸
                progress.style.width = (param.position || 100) + '%';
                // 为进度条组件添加UI样式
                dom.className += ' ui-progressbar';
                // 进度完成容器元素插入进度条容器中
                dom.appendChild(progress);
            },
            slider: function(dom, data) {
                // 滚动条拨片
                var bar = document.createElement('span'),
                    // 滑动条进度容器
                    progress = document.createElement('div'),
                    // 滑动条容量提示信息
                    totleText = null,
                    // 滑动条拨片提示信息
                    progressText = null,
                    // 数据模型数据，结构{position: 60, totle: 200}
                    param = data.data,
                    // 容器元素宽度
                    width = dom.clientWidth,
                    // 容器元素横坐标值
                    left = dom.offsetLeft,
                    // 拨片位置
                    realWidth = (param.position || 100) * width / 100;
                // 清空滑动条容器，为创建滑动条作准备
                dom.innerHTML = '';
                // 如果模型数据中提供容器总量信息，则创建滚动条提示文案
                if (param.totle) {
                    // 容器总量提示文案
                    text = document.createElement('b');
                    // 拨片位置提示文案
                    progressText = document.createElement('em');
                    // 设置容器总量提示文案
                    text.innerHTML = param.totle;
                    // 将容器总量提示文案元素添加到滑动条组件中
                    dom.appendChild(text);
                    // 将拨片位置提示文案元素添加到滑动条组件中
                    dom.appendChild(progressText);
                };
                // 设置滑动条
                setStyle(realWidth);
                // 为滑动条组件添加UI样式
                dom.className += ' ui-slider';
                // 将进度容器添加到滑动条组件中
                dom.appendChild(progress);
                // 将拨片添加到滑动条组件中
                dom.appendChild(bar);
                // 设置滑动条
                function setStyle(w) {
                    // 设置进度容器宽度
                    progress.style.width = w + 'px';
                    // 设置拨片横坐标
                    bar.style.left = w - FONTSIZE / 2 + 'px';
                    // 如果有拨片提示文案
                    if (progressText) {
                        // 设置拨片提示文案横坐标
                        progressText.style.left = w - FONTSIZE / 2 * 2.4 + 'px';
                        // 设置拨片提示文案内容
                        progressText.innerHTML = parseFloat(w / width * 100).toFixed(2) + '%';
                    };
                }
                //创建组件逻辑 按下鼠标拨片
                bar.onmousedown = function() {
                    // 移动拨片（鼠标光标在页面中滑动，事件绑定给document是为了优化交互体验，使鼠标光标可以在页面中自由滑动）
                    document.onmousemove = function(event) {
                        var e = event || window.event;
                        var w = e.clientX - left;
                        // 设置滑动条
                        setStyle(w < width ? ( w > 0 ? w : 0) : width);
                    }
                    // 阻止页面滑动选取事件
                    document.onselectstart = function() {
                        return false;
                    }
                    // 停止滑动交互
                    document.onmouseup = function() {
                        // 取消文档鼠标光标移动事件
                        document.onmousemove = null;
                        // 取消文档滑动选取事件
                        document.onselectstart = null;
                    }
                }
            }
        }
        /**
         * 获取视图层组件渲染数据的映射信息
         * @param  {[type]} dom 组件元素
         */
        function getBindData(dom) {
            // 获取组件自定义属性data-bind值
            var data = dom.getAttribute('data-bind');
            // 将自定义属性data-bind值转化为对象
            return !!data && (new Function("return ({"+ data +"})"))();
        }
        // 组件实例化方法
        return function() {
            var doms = document.body.getElementsByTagName('*'),
            // 元素自定义数据句柄
                ctx = null;
            // ui处理是会向页面中插入元素，此时doms.length会改变，此时动态获取dom.length
            for(var i = 0; i < doms.length; i ++) {
                // 获取元素自定义数据
                ctx = getBindData(doms[i]);
                // 如果元素是ui组件，则根据自定义属性中组件类型，渲染该组件
                ctx.type && Method[ctx.type] && Method[ctx.type](doms[i], ctx);
            }
        }
    }();
    // 将视图模型对象绑定在Window上，供外部获取
    window.VM = VM;
})()
var demo1 = {
        position: 60,
        totle: 200
    },
    demo2 = {
        position: 20
    },
    demo3 = {position: 50};
window.onload = function() {
    VM();
}
</script>
```

# 享元模式

```
<div id="container"></div>
<script>
// 其实就是js里组合继承，构造函数绑定属性，原型绑定方法
// 每页显示5条数据，不够5条从最前面取
var FlyWeight = function() {
	var created = [];
	function create() {
		var dom = document.createElement('div');
		document.getElementById('container').appendChild(dom);
		created.push(dom);
		return dom;
	}
	return {
		getDiv: function() {
			if (created.length < 5) {
				return create();
			} else {
				var div = created.shift();
				created.push(div);
				return div;
			}
		}
	}
}()
console.log(FlyWeight.getDiv());
for(var i = 0; i < 5; i ++) {
	FlyWeight.getDiv().innerHTML = i;
}
</script>
```

# 状态模式

```
var MarrySate = function() {
	var _currentState = {},
		status = {
			jump: function() { console.log("jump"); },
			move: function() { console.log("move"); },
			shoot: function() { console.log("shoot"); },
			squat: function() { console.log("squat"); }
		};
	var Action = {
		changeState: function() {
			var arg = arguments;
			_currentState = {};
			for(var i = 0, len = arg.length; i < len; i ++) {
				_currentState[arg[i]] = true;
			}
			return this;
		},
		goes: function() {
			console.log("触发了");
			for(var k in _currentState) {
				_currentState[k] && status[k]();
			}
		}
	}
	return {
		change: Action.changeState,
		goes: Action.goes
	}
}
MarrySate()
		.change('jump', 'move')
		.goes()
// 触发了
// jump
// move
```

# 模板方法模式

> 可以利用该模式封装弹层插件
```
<script>
var Alert = function(data) {
	if (!data) return;
	// 设置内容
	this.content = data.content;
	// 创建提示框面板
	this.panel = document.createElement('div');
	// 创建提示内容组件
	this.contentNode = document.createElement('p');
	// 创建确定按钮组件
	this.confirmBtn = document.createElement('span');
	// 创建关闭按钮组件
	this.closeBtn = document.createElement('b');
	// 为提示框面板增加类
	this.panel.className = 'alert';
	// 为关闭按钮增加类
	this.closeBtn.className = 'a-close';
	// 为确定按钮增加类
	this.confirmBtn.className = 'a-confirm';
	// 为关闭按钮增加文案
	this.closeBtn.innerHTML = data.close || '关闭';
	// 为确定按钮增加文案
	this.confirmBtn.innerHTML = data.confirm || '确定';
	// 为提示内容增加文本
	this.contentNode.innerHTML = this.content;
	// 点击确定按钮执行方法 如果data中有succuess方法则为success方法，否则为空函数
	this.success = data.success || function() {};
	// 点击关闭按钮执行方法
	this.fail = data.fail || function() {};
}
// 提示框原型方法
Alert.prototype = {
	init: function() {
		this.panel.appendChild(this.closeBtn);
		this.panel.appendChild(this.contentNode);
		this.panel.appendChild(this.confirmBtn);
		document.body.appendChild(this.panel);
		this.bindEvent();
		this.show();
	},
	bindEvent: function() {
		var self = this;
		this.closeBtn.onclick = function() {
			self.fail();
			self.hide();
		}
		this.confirmBtn.onclick = function() {
			self.success();
			self.hide();
		}
	},
	hide: function() {
		this.panel.style.display = 'none';
	},
	show: function() {
		this.panel.style.display = 'block';
	}
}
new Alert({
	'content': 'test',
	'close': 'close test'
}).init()

var RightAlert = function(data) {
	Alert.call(this, data);
	this.confirmBtn.className = this.confirmBtn.className + ' right';
}
RightAlert.prototype = new Alert();
new RightAlert({
	'content': 'test',
	'close': 'right close test'
}).init()
</script>
```