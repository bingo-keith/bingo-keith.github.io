---

title: Redux源码深度解析
date: 2017/10/25 23:31:55 
tags: redux react 柯里化 纯函数 状态 框架

---

> 一般情况下我们用js设置元素对象的样式会使用这样的形式：

<!-- more -->

## 1、什么是redux，这里就不做介绍，如想了解可以移步 ReadMe.redux，整体redux的代码只有800行，src下面分为一下几个部分。

- applyMiddleware.js

- bindActionCreators.js

- combineReducers.js

- compose.js

- createStore.js

- index.js

首先我们来看index.js主js的内容，很简单，就是引入和模块和抛出模块，这里有一句提醒内容，如果是production生产环境并且js已经被压缩，会输出warning信息

```
function isCrushed() {}

if (
  process.env.NODE_ENV !== 'production' &&
  typeof isCrushed.name === 'string' &&
  isCrushed.name !== 'isCrushed'
) {
  warning(
    'You are currently using minified code outside of NODE_ENV === \'production\'. ' +
    'This means that you are running a slower development build of Redux. ' +
    'You can use loose-envify (https://github.com/zertosh/loose-envify) for browserify ' +
    'or DefinePlugin for webpack (http://stackoverflow.com/questions/30030031) ' +
    'to ensure you have the correct code for your production build.'
  )
}
```

注: isCrushed.name 函数名.name是一个es6的属性，返回函数的名称。 
除了index.js外，我们接下去从redux实现的接口，来深度分析一下Redux的源码。

## 2、compose.js

我们首先从compose.js入手，首先redux贯穿始终的是函数式变成的思想，个人对于函数式编程的理解为：

–首先是纯函数（相同的输入产生相同的输出）

–在范畴论理，状态或者输出表示点，函数表示边，从点到点的转移可以看成运算符，函数也是一种运算符，因为运算符是纯净的，因此函数式编程中的函数也是纯净的

–函数式编程中的函数，与变量等价，可以作为参数传递或者成为其他函数函数体里的一部分

–因为是函数式编程，便于函数的组合，这里有一个curry和compose的组合过程

基础了解函数式编程之后，下面我们来看compose.js的源码：

```
export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```

这个compose其实很简单，传入的参数为函数数组，返回的为reduce从左到右合并后的新的函数。是一个类似于链式调用的过程。

来看：

```
funcs.reduce((a, b) => (...args) => a(b(...args)))
```

这句特别重要，组合函数的这部非常重要，我们发现…args参数会依次的从右到左执行，比如将b（…args）的执行结果，传入a中作为参数继续执行。

## 3、applyMiddleware.js

applyMiddleware.js其实是基于compose.js来实现的

```
export default function applyMiddleware(...middlewares) {
  return (createStore) => (reducer, preloadedState, enhancer) => {
    const store = createStore(reducer, preloadedState, enhancer)
    let dispatch = store.dispatch
    let chain = []

    const middlewareAPI = {
      getState: store.getState,
      dispatch: (action) => dispatch(action)
    }
    chain = middlewares.map(middleware => middleware(middlewareAPI))
    dispatch = compose(...chain)(store.dispatch)

    return {
      ...store,
      dispatch
    }
  }
}
```

上述代码如果applyMiddleware(…Middleware)(createStore)这样调用，会生成一个新的createStore函数，用于创建新的createStore，新在哪里呢？就是链式的调用了所有的middleware：

```
let chain = []

const middlewareAPI = {
   getState: store.getState,
   dispatch: (action) => dispatch(action)
}
chain = middlewares.map(middleware => middleware(middlewareAPI))
dispatch = compose(...chain)(store.dispatch)
```

来看上述的代码，chain是一个函数数组，是middleware({})执行后的返回函数的数组，compose(…chain)是链式的组合函数，这里的…args是初始时候的store.dispatch，当最右边的函数以store.dispatch为参数，执行后生成一个新的store.dispatch，又向外传递，因此middleware是从右到左执行的。

从上述的描述中，我们知道了middle的书写形式，如果以纯函数的形式，首先第一个参数应该是{getState:”,dispatch:”}，第二个参数是store.dispatch，第三个参数应该是action，因此最基本形式的middleware应该是：

return ({ dispatch, getState }) => next => action => {

}

我们以redux-thunk为例，redux-thunk中间件是严格按照上述的形式，

代码只有13行：

```
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

这个中间件的功能其实很简单，也就是action如果是一个函数就选择执行这个函数，并且action函数执行的时候，会传入dispatch和getState.

## 4、createStore.js

createStore相对而言会较为的复杂，我们还是从接口出发。

（1）首先看,createStore()函数的返回值store有哪些接口：

–getState()：返回当前的state树

–dispatch(action)：分发action，是改变state的唯一方法

–subscribe(listener)：添加一个监听器，当state变化的时候，执行监听器里面的函数。

–unsubscribe(listener)：subscribe的返回值，用于移除监听器

–replaceReducer(nextReducer)：替换store中当前的reducer

（2）下面根据代码，依次来看，各个接口的实现情况。

首先明确createStore的形参，形参有3个，分别是reducer(处理函数)，preloadedState（初始化state），enhancer（一个高阶函数，可以改变store的接口）。

```
export default function createStore(reducer, preloadedState, enhancer) {

}
```

- getState函数：

```
let currentState = preloadedState
function getState() {
	return currentState
}
```

getState函数比较简单，类似于一个get的方法，返回currentState的值

- dispatch函数：

```
function dispatch(action) {
    if (!isPlainObject(action)) {
      throw new Error(
        'Actions must be plain objects. ' +
        'Use custom middleware for async actions.'
      )
    }

    if (typeof action.type === 'undefined') {
      throw new Error(
        'Actions may not have an undefined "type" property. ' +
        'Have you misspelled a constant?'
      )
    }

    if (isDispatching) {
      throw new Error('Reducers may not dispatch actions.')
    }

    try {
      isDispatching = true
      currentState = currentReducer(currentState, action)
    } finally {
      isDispatching = false
    }

    const listeners = currentListeners = nextListeners
    for (let i = 0; i < listeners.length; i++) {
      const listener = listeners[i]
      listener()
    }

    return action
}
```

dispatch也不复杂，去掉判断类型（因为action必须是对象）的部分，其实只有2步：

```
 try {
  isDispatching = true
  currentState = currentReducer(currentState, action)
} finally {
  isDispatching = false
}

const listeners = currentListeners = nextListeners
for (let i = 0; i < listeners.length; i++) {
  const listener = listeners[i]
  listener()
}
```

这就很显然易见了，就是执行currentReducer()传入当前的currentState和action，返回新的state，并且执行监听函数数组里面的所有函数。

- subscribe：

```
function subscribe(listener) {
  if (typeof listener !== 'function') {
    throw new Error('Expected listener to be a function.')
  }

  let isSubscribed = true

  ensureCanMutateNextListeners()
  nextListeners.push(listener)

  return function unsubscribe() {
    if (!isSubscribed) {
      return
    }

    isSubscribed = false

    ensureCanMutateNextListeners()
    const index = nextListeners.indexOf(listener)
    nextListeners.splice(index, 1)
  }
}
```

监听函数也挺简单，就是一个简单的移入和移出，这是一个底层 API。多数情况下，你不会直接使用它，会使用一些 React（或其它库）的绑定。比如react-redux中的容器组件中的props改变会自动的更新（也算一个监听过程）。

- replaceReducer：

```
function replaceReducer(nextReducer) {
    if (typeof nextReducer !== 'function') {
      throw new Error('Expected the nextReducer to be a function.')
    }

    currentReducer = nextReducer
    dispatch({ type: ActionTypes.INIT })
}
```

这个函数就更加的简单了，replaceReducer是为了改变当前的reducer，因此只要将currentReducer赋值为形参即可。

## 5、combineReducers.js

最复杂的部分就是combineReducer.js了

- combineReducer（reducer）接受一个reducer对象,recuder是key表示属性名，value是一个小的reduce函数：

```
export default function combineReducers(reducers) {

}
```

- 遍历reducer中的对象，取出其中的value值（reduce函数），生成一个新的对象：

```
const reducerKeys = Object.keys(reducers)
const finalReducers = {}
for (let i = 0; i < reducerKeys.length; i++) {
  const key = reducerKeys[i]
  finalReducers[key] = reducers[key]
}
```

- 因为reduce函数有一个初始执行过程，即会自动执行一次 type: ActionTypes.INIT,因此有以下过程：

```
assertReducerShape(finalReducers);
function assertReducerShape(reducers) {
  Object.keys(reducers).forEach(key => {
    const reducer = reducers[key]
    const initialState = reducer(undefined, { type: ActionTypes.INIT })
  }
}
```

- 最后就是最终要的返回的combine函数：

其原理也很简单，state的属性名和reducer对象的key是相对的，因此也就是在所有的小的reduce函数中，传入相对的state[key]，action,依次执行后得到一个新的newState，然后与state做比较，选择性返回。

```
return function combination(state = {}, action) {

    let hasChanged = false
    const nextState = {}
    for (let i = 0; i < finalReducerKeys.length; i++) {
      const key = finalReducerKeys[i]
      const reducer = finalReducers[key]
      const previousStateForKey = state[key]
      const nextStateForKey = reducer(previousStateForKey, action)
      if (typeof nextStateForKey === 'undefined') {
        const errorMessage = getUndefinedStateErrorMessage(key, action)
        throw new Error(errorMessage)
      }
      nextState[key] = nextStateForKey
      hasChanged = hasChanged || nextStateForKey !== previousStateForKey
    }
    return hasChanged ? nextState : state
  }
}
```

## 6、bindActionCreators.js

这个一般比较少用，这里就不分析源码了，只简单的阐述功能，

bindActionCreators（）把 action creators 转成拥有同名 keys 的对象，但使用 dispatch 把每个 action creator 包围起来，这样可以直接调用它们。