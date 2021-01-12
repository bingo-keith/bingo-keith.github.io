---

title: 手写一个简单版本的Promise
date: 2019/2/14 20:59:11  
tags: promise 异步 es6

---

# 什么是promise

> Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。

> Promise对象代表一个异步操作，有三种状态：pending（进行中）、resolved（已成功）和rejected（已失败）。一旦成功就不允许失败，一旦失败就不允许成功。

<!-- more -->

# 先把一个空架子搭出来

```js
const PENDING = 'pending'; 
const RESOLVED = 'resolved';
const REJECTED = 'rejected';
function MyPromise(fn){
    /* 初始化 */
    const self = this; // 实例对象
    self.value = null; // 默认值为null
    self.status = PENDING; // 初始状态为pending等待状态
    self.resolvedCallbacks = []; // 成功回调列表
    self.rejectedCallbacks = []; // 失败回调列表
    function resolve() {}; // 成功的方法
    function rejecte() {}; // 拒绝的方法
    /* 初始化 */
    try {
        fn(resolve, reject); // 调用传入的函数，并传入resolve和reject方法为入参
    } catch(e) {
        reject(self.value); // 异常场景直接调用reject拒绝方法
    }
}
MyPromise.prototype.then(function(onFulfilled, onRejected) {})
```

# 因为new返回的是promise对象，初始化一个对象，完善该对象的属性和方法

```js
const PENDING = 'pending'; 
const RESOLVED = 'resolved';
const REJECTED = 'rejected';
function MyPromise(fn){
    /* 初始化 */
    const self = this; // 实例对象
    self.value = null; // 默认值为null
    self.status = PENDING; // 初始状态为pending等待状态
    self.resolvedCallbacks = []; // 成功回调列表
    self.rejectedCallbacks = []; // 拒绝回调列表
    function resolve(value) {
        self.status = RESOLVED; // 把状态改为resolved
        self.value = value;
        self.resolvedCallbacks.map(cb => cb(self.value)); // 分别执行成功回调列表里的方法，并传入value
    }; // 成功的方法
    function reject(value) {
        self.status = REJECTED; // 把状态改为rejected
        self.value = value;
        self.rejectedCallbacks.map(cb => cb(self.value)); //分别执行拒绝回调列表里的方法，并传入value
    }; // 拒绝的方法
    /* 初始化 */
    try {
        fn(resolve, reject); // 调用传入的函数，并传入resolve和reject方法为入参
    } catch(e) {
        reject(self.value); // 异常场景直接调用reject拒绝方法
    }
}
MyPromise.prototype.then(function(onFulfilled, onRejected) {})
```

# 返回的promise具有then方法，并能获取到初始化数据

```js
MyPromise.prototype.then = function(onFulfilled, onRejected) {
    const self = this;
    if(self.status === PENDING) { // 初始化时为pending状态，此时把成功和拒绝的方法push进指定列表里
        self.resolvedCallbacks.push(onFulfilled);
        self.rejectedCallbacks.push(onRejected);
    }
    // 初始化后再调用then方法根据初始化的状态分别执行对应的方法
    if(self.status === RESOLVED) {
        onFulfilled(self.value);
    }
    if(self.status === REJECTED) {
        onRejected(self.value);
    }
    return self; // 链式调用
}
```

# 测试一下

```js
new MyPromise(function(resolve, reject) {
    setTimeout(function(){
        resolve(' test ')
    }, 2000);
}).then(function(value) {
    console.log('resolve' + value);
}, function(value) {
    console.log('reject' + value);
})
// 结果: resolve test 
```

# 完整代码

```js
const PENDING = 'pending'; 
const RESOLVED = 'resolved';
const REJECTED = 'rejected';
function MyPromise(fn){
    /* 初始化 */
    const self = this; // 实例对象
    self.value = null; // 默认值为null
    self.status = PENDING; // 初始状态为pending等待状态
    self.resolvedCallbacks = []; // 成功回调列表
    self.rejectedCallbacks = []; // 拒绝回调列表
    function resolve(value) {
        self.status = RESOLVED; // 把状态改为resolved
        self.value = value;
        self.resolvedCallbacks.map(cb => cb(self.value)); // 分别执行成功回调列表里的方法，并传入value
    }; // 成功的方法
    function reject(value) {
        self.status = REJECTED; // 把状态改为rejected
        self.value = value;
        self.rejectedCallbacks.map(cb => cb(self.value)); //分别执行拒绝回调列表里的方法，并传入value
    }; // 拒绝的方法
    /* 初始化 */
    try {
        fn(resolve, reject); // 调用传入的函数，并传入resolve和reject方法为入参
    } catch(e) {
        reject(self.value); // 异常场景直接调用reject拒绝方法
    }
}
MyPromise.prototype.then = function(onFulfilled, onRejected) {
    const self = this;
    if(self.status === PENDING) { // 初始化时为pending状态，此时把成功和拒绝的方法push进指定列表里
        self.resolvedCallbacks.push(onFulfilled);
        self.rejectedCallbacks.push(onRejected);
    }
    // 初始化后再调用then方法根据初始化的状态分别执行对应的方法
    if(self.status === RESOLVED) {
        onFulfilled(self.value);
    }
    if(self.status === REJECTED) {
        onRejected(self.value);
    }
    return self; // 链式调用
}
new MyPromise(function(resolve, reject) {
    setTimeout(function(){
        resolve(' test ')
    }, 2000);
}).then(function(value) {
    console.log('resolve' + value);
}, function(value) {
    console.log('reject' + value);
})
```

# 总结

> 首先是利用构造函数模式初始化一个promise对象，在原型上绑定实例方法，注意初始化调用then方法的时候此时状态为pending，在执行传入的方法改变状态后再调用then方法会根据修改后的状态执行不同的逻辑

> 我觉得promise对象利用了构造函数设计模式解决了js异步回调嵌套的问题，完美解决了返回数据嵌套依赖的问题，也就是俗称的回调地狱。现在算是把长久以来关于promise的疑惑搞明白了，over！！！