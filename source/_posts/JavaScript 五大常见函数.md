---

title: JavaScript 五大常见函数
date: 2018/3/26 22:40:50
tags: javscript 函数 高阶函数 方法 递归 数组扁平化 柯里化

---

> 　　在 JavaScript 中有一些问题会被拿出来经常讨论，这些问题每个人都有不同的思路，想要理解这些问题，最好的方法就是自己实现一遍，话不多说，开始正题。

<!-- more -->

# 数组扁平化

数组扁平化有很多方法，但最终最好的方法就是递归，实现一个指定深度的扁平化方法，这样基本的套路都会了解。

```
function flattenDepth(array, depth = 0){
    let res = [];
    array.forEach(item => {
        if (Array.isArray(item)) {
            depth ++;
        };
        if (Array.isArray(item) && depth > 0) {
            res.push(...flattenDepth(item, --depth));
        } else {
            res.push(item);
        }
    })
    return res;
}
console.log(flattenDepth([1,[2,21,[1,2,3,[2,3,[5,6],1],4],22,23],3,[4,5,[6,7,8]]]));
//[1, 2, 21, 1, 2, 3, 2, 3, 5, 6, 1, 4, 22, 23, 3, 4, 5, 6, 7, 8]
```

> 递归实现很简洁易懂，就是将每一项遍历，如果某一项为数组，则让该项继续调用，这里指定了 depth 作为扁平化的深度，因为这个参数对数组的每一项都要起作用，故放在循环的里面。

# 柯里化

函数的柯里化都被讲烂了，每个人都有自己的理解和实现方法，一句话解释就是**参数够了就执行，参数不够就返回一个函数，之前的参数存起来，直到够了为止。**

```
function curring(func){
    var len = func.length;
    return function curried(){
        var args = [].slice.apply(arguments);
        if (args.length < len) {
            return function(){
                var argsInner = [].slice.apply(arguments);
                return curried.apply(this, args.concat(argsInner));
            }
        } else {
            return func.apply(this, args);
        }
    }
}
function f(a, b, c, d){
    console.log([a, b, c, d]);
}
var curry = curring(f);
curry(1)(2)(3)(4)//[1, 2, 3, 4]
curry(1,2)(3)(4)//[1, 2, 3, 4]
curry(1,2)(3,4)//[1, 2, 3, 4]
curry(1,2,3)(4)//[1, 2, 3, 4]
curry(1,2,3,4)//[1, 2, 3, 4]
```
> 上面的代码不难看出，每次判断参数的个数，与被柯里化的函数参数个数比较，如果小于就继续返回函数，否则就执行。

# 防抖

防抖按照我的理解就是不管你触发多少次，都等到你最后触发后过一段你指定的时间才触发。按照这个解释，写一个基本版的。

```
function debounce(func, wait) {
  var timer;
  return function() {
    var context = this,
        args = arguments;
    clearTimeout(timer)
    timer = setTimeout(function() {
      func.apply(context, args)
    }, wait)
  }
}
debounce(function(){alert('测试')},2000)()
```

现在有个要求就是刚开始的时候也触发，最后一次也触发，并且可以配置，先写个测试页面方便测试功能，来测试防抖和节流函数。

```
<div id="container"></div>

var container = document.getElementById('container');
document.onkeydown = debounce(getUserAction, 1000, false, true);

function getUserAction(e) {
    if (e.keyCode === 32) { // 空格
        alert('测试');
    }
}

function debounce(func, wait, leading, trailing) {
    var timer, lastCall = 0,
        flag = true
    return function(e) {
        var context = this;
        var args = arguments;
        var now = +new Date();
        if (now - lastCall < wait) {
            flag = false;
            lastCall = now;
        } else {
            flag = true;
        }
        if (leading && flag) {
            lastCall = now;
            return func.apply(context, args);
        }
        if (trailing) {
            clearTimeout(timer);
            timer = setTimeout(function() {
                flag = true;
                func.apply(context, args);
            }, wait)
        }
    }
}
```

> 通过 leading 和 trailing 两个参数来决定开始和结束是否执行，如果 leading 为 true，则每次按空格都会执行alert，如果 trailing 为 true，则每次结束都会将最后一次触发执行。以防抖函数距离，如果两者都为 true，则第一次按空格会alert，然后快速按空格，此时里面的 getUserAction 并不会执行，而是等到松手后再执行，加入 trailing 为 false，则松手后不会执行。

> 解释一下，每次记录上次调用的时间，与现在的时间对比，小于间隔的话，第一次执行后之后就不会执行，大于间隔或在间隔时间后调用了，则重置 flag，可以与上面那个基本版的对比着看。

# 节流

节流就是，**不管怎么触发，都是按照指定的间隔来执行**，同样给个基本版。

```
function throttle(func, wait) {
    var timer;
    return function() {
        var context = this;
        var args = arguments;
        if (!timer) {
            timer = setTimeout(function() {
                timer = null;
                func.apply(context, args);
            }, wait)
        }
    }
}
throttle(function(a){alert(a)},1000)('测试节流');
```

同样和防抖函数一样加上两个参数，也可使用上面的例子来测试，其实两者的代码很类似。

```
function throttle(func, wait, leading, trailing) {
    var timer, lastCall = 0, flag = true;
    return function() {
        var context = this;
        var args = arguments;
        var now = +new Date();
        flag = now - lastCall > wait;
        if (leading && flag) {
            lastCall = now;
            return func.apply(context, args);
        }
        if (!timer && trailing && !(flag && leading)) {
            timer = setTimeout(function() {
                timer = null;
                lastCall = +new Date();
                func.apply(context, args);
            }, wait)
        } else {
            lastCall = now;
        }
    }
}
throttle(function(a){alert(a)},1000,false,true)('测试节流');
```

# 对象拷贝

对象拷贝都知道分为深拷贝和浅拷贝，黑科技手段就是使用`JSON.parse(JSON.stringify(obj))`

还有个方法就是使用递归了

```
function clone(value, isDeep) {
  if (value === null) return null
  if (typeof value !== 'object') return value
  if (Array.isArray(value)) {
    if (isDeep) {
      return value.map(item => clone(item, true))
    }
    return [].concat(value)
  } else {
    if (isDeep) {
      var obj = {}
      Object.keys(value).forEach(item => {
        obj[item] = clone(value[item], true)
      })
      return obj
    }
    return { ...value }
  }
}

var objects = { c: { 'a': 1, e: [1, {f: 2}] }, d: { 'b': 2 } }
var shallow = clone(objects, true)
console.log(shallow.c.e[1]) // { f: 2 }
console.log(shallow.c === objects.c) // false
console.log(shallow.d === objects.d) // false
console.log(shallow === objects) // false
```

> 对于基本类型直接返回，对于引用类型，遍历递归调用 clone 方法。

# 总结

其实对于上面这些方法，总的来说思路就是递归和高阶函数的使用，其中就有关于闭包的使用，前端就爱问这些问题，最好就是自己实现一遍，这样有助于理解。