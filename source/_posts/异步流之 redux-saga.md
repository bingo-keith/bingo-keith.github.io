---

title: 异步流之 redux-saga
date: 2018/6/10 13:44:34  
tags: redux saga 异步 generator react

---

# 介绍

这里包含了两个信息：

首先，redux-saga 是一个 redux 的中间件，而中间件的作用是为 redux 提供额外的功能。

其次，我们都知道，在 reducers 中的所有操作都是同步的并且是纯粹的，即 reducer 都是纯函数，纯函数是指一个函数的返回结果只依赖于它的参数，并且在执行过程中不会对外部产生副作用，即给它传什么，就吐出什么。但是在实际的应用开发中，我们希望做一些异步的（如Ajax请求）且不纯粹的操作（如改变外部的状态），这些在函数式编程范式中被称为“副作用”。

Redux 的作者将这些副作用的处理通过提供中间件的方式让开发者自行选择进行实现。

> redux-saga 就是用来处理上述副作用（异步任务）的一个中间件。它是一个接收事件，并可能触发新事件的过程管理者，为你的应用管理复杂的流程。

<!-- more -->

![](https://i.imgur.com/5L5bsUy.png)

# 先说一说 redux-thunk

redux-thunk 和 redux-saga 是 redux 应用中最常用的两种异步流处理方式。

> 从异步的角度，Thunk 是指一切都就绪的会返回某些值的函数。你不用传任何参数，你只需调用它，它便会返回相应的值。—— Rethinking Asynchronous Javascript

redux-thunk 的任务执行方式是从 UI 组件直接触发任务。

**举个栗子：**

假如当每次 Button 被点击的时候，我们想要从给定的 url 中获取数据，采用 redux-thunk， 我们会这样写：

```
// fetchUrl 返回一个 thunk
function fetchUrl(url) {
  return (dispatch) => {
    dispatch({
      type: 'FETCH_REQUEST'
    });

    fetch(url).then(data => dispatch({
      type: 'FETCH_SUCCESS',
      data
    }));
  }
}

// 如果 thunk 中间件正在运行的话，我们可以 dispatch 上述函数如下：
dispatch(
  fetchUrl(url)
):
```

redux-thunk 的主要思想是扩展 action，使得 action 从一个对象变成一个函数。

**另一个较完整的栗子：**

```
// redux-thunk example
import {applyMiddleware, createStore} from 'redux';
import axios from 'axios';
import thunk from 'redux-thunk';

const initialState = { fetching: false, fetched: false, users: [], error: null }
const reducer = (state = initialState, action) => {
    switch(action.type) {
        case 'FETCH_USERS_START': {
            return {...state, fetching: true} 
            break;
        }
        case 'FETCH_USERS_ERROR': {
            return {...state, fetching: false, error: action.payload} 
            break;
        }
        case 'RECEIVE_USERS': {
            return {...state, fetching: false, fetched: true, users: action.payload} 
            break;
        }
    }
    return state;
}
const middleware = applyMiddleware(thunk);

// store.dispatch({type: 'FOO'});
// redux-thunk 的作用即是将 action 从一个对象变成一个函数
store.dispatch((dispatch) => {
    dispatch({type: 'FETCH_USERS_START'});
    // do something async
    axios.get('http://rest.learncode.academy/api/wstern/users')
        .then((response) => {
            dispatch({type: 'RECEIVE_USERS', payload: response.data})
        })
        .catch((err) => {
            dispatch({type: 'FECTH_USERS_ERROR', payload: err})
        })
});
```

redux-thunk 的缺点：
（1）action 虽然扩展了，但因此变得复杂，后期可维护性降低；
（2）thunks 内部测试逻辑比较困难，需要mock所有的触发函数；
（3）协调并发任务比较困难，当自己的 action 调用了别人的 action，别人的 action 发生改动，则需要自己主动修改；
（4）业务逻辑会散布在不同的地方：启动的模块，组件以及thunks内部。

# redux-saga 是如何工作的？

sages 采用 Generator 函数来 yield Effects（包含指令的文本对象）。Generator 函数的作用是可以暂停执行，再次执行的时候从上次暂停的地方继续执行。Effect 是一个简单的对象，该对象包含了一些给 middleware 解释执行的信息。你可以通过使用 effects API 如 fork，call，take，put，cancel 等来创建 Effect。（ redux-saga API 参考）

如 yield call(fetch, '/products') 即 yield 了下面的对象，call 创建了一条描述结果的信息，然后，redux-saga middleware 将确保执行这些指令并将指令的结果返回给 Generator：

```
// Effect -> 调用 fetch 函数并传递 `./products` 作为参数
{
  type: CALL,
  function: fetch,
  args: ['./products']
}
```

与 redux-thunk 不同的是，在 redux-saga 中，UI 组件自身从来不会触发任务，它们总是会 dispatch 一个 action 来通知在 UI 中哪些地方发生了改变，而不需要对 action 进行修改。redux-saga 将异步任务进行了集中处理，且方便测试。

```
dispacth({ type: 'FETCH_REQUEST', url: /* ... */} );
```

所有的东西都必须被封装在 sagas 中。sagas 包含3个部分，用于联合执行任务：

1. worker saga
做所有的工作，如调用 API，进行异步请求，并且获得返回结果
2. watcher saga
监听被 dispatch 的 actions，当接收到 action 或者知道其被触发时，调用 worker saga 执行任务
3. root saga
立即启动 sagas 的唯一入口

## ☀ 如何使用？

首先，我们得在文件入口中加入 saga 中间件，并且启动它，它会一直运行：

```
//...
import { createStore, applyMiddleware} from 'redux';
import createSagaMiddleware from 'redux-saga';
import appReducer from './reducers';
//...

const sagaMiddleware = createSagaMiddleware();
const middlewares = [sagaMiddleware];

const store = createStore(appReducer, applyMiddleware(...middlewares));
sagaMiddleware.run(rootSaga);

render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('app')
);
```

然后，就可以在 sagas 文件夹中集中写 saga 文件了：

```
// example 1
import { take, fork, call, put } from 'redux-saga/effects';

// The worker: perform the requested task
function* fetchUrl(url) {
  const data = yield call(fetch, url);  // 指示中间件调用 fetch 异步任务
  yield put({ type: 'FETCH_SUCCESS', data });  // 指示中间件发起一个 action 到 Store
}

// The watcher: watch actions and coordinate worker tasks
function* watchFetchRequests() {
  while(true) {
    const action = yield take('FETCH_REQUEST');  // 指示中间件等待 Store 上指定的 action，即监听 action
    yield fork(fetchUrl, action.url);  // 指示中间件以无阻塞调用方式执行 fetchUrl
  }
}
```

在 redux-saga 中的基本概念就是：sagas 自身不真正执行副作用（如函数 call），但是会构造一个需要执行作用的描述。中间件会执行该副作用并把结果返回给 generator 函数

**对上述例子的说明：**

（1）引入的 redux-saga/effects 都是纯函数，每个函数构造一个特殊的对象，其中包含着中间件需要执行的指令，如：call(fetchUrl, url) 返回一个类似于 {type: CALL, function: fetchUrl, args: [url]} 的对象。

（2）在 watcher saga watchFetchRequests中：

首先 yield take('FETCH_REQUEST') 来告诉中间件我们正在等待一个类型为 FETCH_REQUEST 的 action，然后中间件会暂停执行 wacthFetchRequests generator 函数，直到  FETCH_REQUEST action 被 dispatch。一旦我们获得了匹配的 action，中间件就会恢复执行 generator 函数。

下一条指令 fork(fetchUrl, action.url) 告诉中间件去无阻塞调用一个新的 fetchUrl 任务，action.url 作为 fetchUrl 函数的参数传递。中间件会触发 fetchUrl generator 并且不会阻塞 watchFetchRequests。当fetchUrl 开始执行的时候，watchFetchRequests 会继续监听其它的 watchFetchRequests actions。当然，JavaScript 是单线程的，redux-saga 让事情看起来是同时进行的。

（3）在 worker saga fetchUrl 中，call(fetch,url) 指示中间件去调用 fetch 函数，同时，会阻塞fetchUrl 的执行，中间件会停止 generator 函数，直到 fetch 返回的 Promise 被 resolved（或 rejected），然后才恢复执行 generator 函数。

**另一个栗子：**

```
// example 2
import { takeEvery } from 'redux-saga';
import { call, put } from 'redux-saga/effects';
import axios from 'axios';

// 1. our worker saga
export function* createLessonAsync(action) {
    try {
        // effects(call, put): 
        // trigger off the code that we want to call that is asynchronous 
        // and also dispatched the result from that asynchrous code.
        const response = yield call(axios.post, 'http://jsonplaceholder.typicode.com/posts', {section_id: action.sectionId});
        yield put({type: 'lunchbox/lessons/CREATE_SUCCEEDED', response: response.data});
    } catch(e) {
        console.log(e);
    }
}

// 2. our watcher saga: spawn a new task on each ACTION
export function* watchCreateLesson() {
    // takeEvery: 
    // listen for certain actions that are going to be dispatched and take them and run through our worker saga.
    yield takeEvery('lunchbox/lessons/CREATE', createLessonAsync);
}


// 3. our root saga: single entry point to start our sagas at once
export default function* rootSaga() {
    // combine all of our sagas that we create
    // and we want to provide all our Watchers sagas
    yield watchCreateLesson()
}
```

最后，总结一下 redux-saga 的优点：

（1）声明式 Effects：所有的操作以JavaScript对象的方式被 yield，并被 middleware 执行。使得在 saga 内部测试变得更加容易，可以通过简单地遍历 Generator 并在 yield 后的成功值上面做一个 deepEqual 测试。
（2）高级的异步控制流以及并发管理：可以使用简单的同步方式描述异步流，并通过 fork 实现并发任务。
（3）架构上的优势：将所有的异步流程控制都移入到了 sagas，UI 组件不用执行业务逻辑，只需 dispatch action 就行，增强组件复用性。

# 附上测试 demo

[redux-async-demo](https://github.com/RukiQ/redux-async-demo)

# 参考

[redux-saga - Saga Middleware for Redux to Handle Side Effects - Interview with Yassine Elouafi](https://survivejs.com/blog/redux-saga-interview/)

[redux-saga 基本概念](https://redux-saga-in-chinese.js.org/docs/basics/index.html)

[Redux: Thunk vs. Saga](http://blog.jakegardner.me/redux-thunk-vs-saga/)

[从redux-thunk到redux-saga实践](https://github.com/Pines-Cheng/blog/issues/9)

[React项目小结系列：项目中redux异步流的选择](https://mingjiezhang.github.io/2017/02/06/React%E9%A1%B9%E7%9B%AE%E5%B0%8F%E7%BB%93%E7%B3%BB%E5%88%97%EF%BC%9A%E9%A1%B9%E7%9B%AE%E4%B8%ADredux%E5%BC%82%E6%AD%A5%E6%B5%81%E7%9A%84%E9%80%89%E6%8B%A9/)