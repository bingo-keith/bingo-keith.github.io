---

title: useEffect引起的React Hooks深入了解
date: 2021/1/12 16:50:51 
tags: JavaScript react hooks 函数式

---

## 前言

> 参考文章 **react源码**，[useEffect 完整指南](https://overreacted.io/zh-hans/a-complete-guide-to-useeffect/)，[呕心沥血，一文看懂 react hooks](https://juejin.im/post/5d985deae51d4577f9285c2f)，[react官网](https://react.docschina.org/docs/hooks-intro.html)

在进入正式阅读之前，最好先思考一下下面的问题：

1. React Hooks真的有生命周期吗？
2. React Hooks的函数里面定义的函数或者变量会被缓存吗，这样下次再调用组件的时候就可以不用重新声明了。
3. 为什么我的useEffect有时候拿到了之前的值。

<!-- more -->

## React Hooks函数式渲染

> 我非常喜欢参考文章里面说的，如果你想要学好React Hooks，那么你摒弃掉之前组件的想法可能会更加好

为什么我会这么讲？

React Hooks会璞归真，其实就是将我们之前封装好的组件对象重新变回来了我们原始的代码模式。让你只要考虑执行过程中的栈，堆和队列

### 函数式渲染与生命周期的关系

在React Hooks里面，当我们声明一个组件时

```javascript
import React from 'react';

function App() {
  return (
    <div className="App">
      我是React Hooks
    </div>
  );
}

export default App;
```

我们可以很明显的看到，这就是一个函数嘛，只是加入JSX的写法，返回了组件而已。是的，你没有理解错，这应该也是React Hooks的创始人的想法，不比Vue，虽然轻量，但是封装好了一切，导致可能前端就是学习框架去了~

但是，React不也是组件化的框架吗？是的，当代码增多

```javascript
import React, { useState, useEffect } from 'react';

function App() {
  const [name, setName] = useState('hello');

  useEffect(() => {
    console.log(name)
  })

  return (
    <div className="App" onClick={() => {setName(name + 'world')}}>
      我是{name}
    </div>
  );
}

export default App;
```

让我们来大胆的猜想一下，当这段代码运行完了之后，页面会发生什么？

```javascript
// 第一次渲染
-----函数开始-----
useState定义了一个为'name'的state
useEffect定义了一个函数，他没有任何的依赖项
返回一个JSX显示到我们页面上
页面加载成功，发现我们有个useEffect，发现他不依赖任何属性，他就要运行，于是他从此次函数中拿到name-->打印hello
-----函数结束-----

// 当点击我们的文本
点击事件回调修改我们的state，告诉我们相应的组件，你需要更新了
-----函数开始-----
从我们的state中拿出修改后的'name'，这时候name为helloworld
第二次渲染了，useEffect已经被声明过了，不理他了
返回一个JSX显示到我们页面上
页面加载成功，发现我们有个useEffect，发现他不依赖任何属性，他就要运行，于是他从此次函数中拿到name-->打印helloworld
-----函数结束-----
```

想必，看完了这段例子之后，你对React Hooks也有了一定的理解了。其实就是使用useState去存储我们需要存储的数据，当他更新的时候刷新页面，当页面刷新的时候，我们再使用useEffect来进行我们需要的操作。

**这样看的话，useEffect岂不就是相当于我们之前的componentDidMount + componentDidUpdate**

我可以很负责任的告诉你，不是的，useEffect是我们更新页面的副作用，当我们对他加上了依赖项之后，他就会在页面加载完了之后，检查依赖项是否有变化来进行决定是否要运行自己，例如：

```javascript
import React, { useState, useEffect } from 'react';

function App() {
  const [name, setName] = useState('hello');
  const [myName, setMyName] = useState('my Hello')

  useEffect(() => {
    console.log('我是第一个副作用' + name)
  })

  useEffect(() => {
    console.log('我是第二个副作用' + name)
  }, [])
  
  useEffect(() => {
    console.log('我是第三个副作用' + name)
  }, [myName])

  useEffect(() => {
    console.log('我是第四个副作用' + name)
  }, [name])

  return (
    <div className="App" onClick={() => {setName(name + 'world')}}>
      我是{name}
    </div>
  );
}

export default App;
```

这时候我们执行完了之后和点击文本之后会发生什么呢？

```javascript
// 第一次渲染
-----函数开始-----
...（省略相同步骤）
页面加载完毕
发现没有依赖项，打印：我是第一个副作用hello
发现是首次渲染，打印：我是第二个副作用hello
发现是首次渲染，打印：我是第三个副作用hello
发现是首次渲染，打印：我是第四个副作用hello
-----函数开始-----

// 点击我们的文本
更新state
-----函数开始-----
...（省略相同步骤）
发现没有依赖项，打印：我是第一个副作用helloworld
发现依赖项还是空，与上次相同，不打印
发现myName没有更新，与上次相同，不打印
发现name更新，打印：我是第四个副作用helloworld
-----函数结束-----
```

详情可参考例子：[https://codesandbox.io/s/reac...](https://codesandbox.io/s/react-effect-demo1-ceb61)

### 函数式渲染的特点

上面的例子可能还没能够理解为什么是函数式渲染，接下来这一次你可能就能意会到，而且是开发中可能经常出现的问题，看下面的例子

```javascript
function Counter() {
  const [count, setCount] = useState(0);

  function handleAlertClick() {
    setTimeout(() => {
      alert('You clicked on: ' + count);
    }, 3000);
  }

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
      <button onClick={handleAlertClick}>
        Show alert
      </button>
    </div>
  );
}
```

在该例子中，我们点击了Show alert之后，再去点击Click me，发现弹出来的是了Show alert时候的count，详情可参考例子：[https://codesandbox.io/s/pric...](https://codesandbox.io/s/priceless-tereshkova-ucvpe?file=/src/App.js)

> 为什么会出现这样的情况呢

因为Counter是一个函数，在点击Show alert，此时的count为当前值，在我们每次点击Click me的时候Counter函数都会运行一次。这样是不是就能理解了呢

### 附录：

1. 一般来说，在函数退出后变量就会”消失”，而 state 中的变量会被 React 保留，所以定义的函数会被清除
2. React hook更新 state 变量总是替换它而不是合并它。跟class不一样
3. useEffect只有一个参数的时候=componentDidMount+componentDidUpdate
4. useEffect它已经保存在函数作用域中。Hook 使用了 JavaScript 的闭包机制
5. return一个函数，运行就会清理，因为 useEffect 默认就会处理。它会在调用一个新的 effect 之前对前一个 effect 进行清理
6. 在条件语句违反Hook的规则原因如下

```javascript
// 🔴 在条件语句中使用 Hook 违反第一条规则
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }
  
// 第二次调用
useState('Mary')           // 1. 读取变量名为 name 的 state（参数被忽略）
// useEffect(persistForm)  // 🔴 此 Hook 被忽略！
useState('Poppins')        // 🔴 2 （之前为 3）。读取变量名为 surname 的 state 失败
useEffect(updateTitle)     // 🔴 3 （之前为 4）。替换更新标题的 effect 失败
```