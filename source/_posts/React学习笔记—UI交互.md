---

title: React学习笔记—UI交互
date: 2017/8/11 13:54:41
tags: react js reactjs

---

## props
> 先看个简单的例子：

```
var HelloWorld = React.createClass({
    render: function () {
        return (
            <div data-title={this.props.title}>{this.props.content}</div>
        )
    }
});

React.render(
    <HelloWorld title="this is title" content="this is content"/>,
    document.body
);
```

<!-- more -->

> 看代码就很容易理解：通过this.props我们可以拿到组件使用时的属性。稍微改变下代码，我们打印出this.props瞅瞅：

```
var HelloWorld = React.createClass({
    render: function () {
        return (
            <div data-title={this.props.title}>{JSON.stringify(this.props)}</div>
        )
    }
});

React.render(
    <HelloWorld title="this is title" content="this is content"/>,
    document.body
);
```

> 浏览器页面效果：

![](https://segmentfault.com/img/bVlgHD)

> 可以看出this.props就是组件的属性集合，稍微改下代码，再来看看：

```
var HelloWorld = React.createClass({
    render: function () {
        return (
            <div>{JSON.stringify(this.props)}</div>
        )
    }
});

React.render(
    <HelloWorld title="this is title">
        <span>1</span>
        <span>2</span>
    </HelloWorld>,
    document.body
);
```

> 浏览器页面效果：

![](https://segmentfault.com/img/bVlgHT)

> 这个时候多了一个children的属性，React将组件的子节点封装到了children属性中，如果想获取到子节点的内容，可以这么写：

```
var HelloWorld = React.createClass({
    render: function () {
        return (
            <div>
            {
                this.props.children.map(function (child) {
                        return child;
                })}
            </div>
        )
    }
});

React.render(
    <HelloWorld title="this is title">
        <span>1</span>
        <span>2</span>
    </HelloWorld>,
    document.body
);
```

> 当子节点只有一个的时候直接通过this.props.children获取子节点。当子节点的个数大于1，this.props.children是一个数组。

> 综上我们可以看出，React将节点属性和子节点都封装到props当中，我们可以通过this.props获取到。在React的设定中，props是不可变的，当props属性确定后，我们不应该再去手动修改它。

## 一个陷阱

> 看下这个代码：

```
var Hello = React.createClass({
    render: function () {
        return (
            <div></div>
        )
    }
});

React.render(
    <Hello>
        <p>content</p>
    </Hello>,
    document.body
);
```

> 咋一看是不是觉得页面渲染出来应该是这个效果：

```
<div>
    <p>content</p>
</div>
```

> 实际上渲染出来是这样的：

```
<div></div>
```

> 看页面的渲染效果:我们要找到根节点，关注它的render方法的返回值。至于使用时的嵌套结构，看完props应该明白，这些都是组件属性，想要使用的话，请通过this.props.children.

## state

> state是同UI交互最重要的属性，看个简单的例子，点击按钮，切换按钮的颜色：

```
var ColorButton = React.createClass({
    getInitialState: function () {
        return {bColor: 'green'};
    },
    render: function () {
        return (
            <button onClick={this.handleClick} style={{backgroundColor: this.state.bColor}}>click</button>
        )
    },
    handleClick: function (event) {
        this.setState({bColor: this.state.bColor === 'green' ? 'red' : 'green'});
    }
});

React.render(
    <ColorButton />,
    document.body
);
```

> getInitialState是用来初始化state，handleClick是用来处理我们点击事件的。

### 事件机制

> React实现了自己的一套事件代理和处理机制，这套机制是符合W3C标准的。通过这套机制，React有两个很重要的特点：

#### 自动绑定

> 所有的事件处理函数当中的this指向组件的实例。如果想要拿到当前操作的DOM，通过参数event获取。

```
var ColorButton = React.createClass({
    getInitialState: function () {
        return {name: 'button'};
    },
    render: function () {
        return (
            <button onClick={this.handleClick}>click</button>
        )
    },
    handleClick: function (event) {
        console.log(this.state);
        console.log(event.target);
    }
});

React.render(
    <ColorButton />,
    document.body
);
```

#### 事件代理

> React并没有将事件处理添加到相应的每个节点上。当React启动时，在根节点上监听所有的事件，并管理事件到相应节点的映射。当组件mounted或者unmounted时，事件将会被添加到映射关系或者被删除。我感觉，有点jQuery的事件代理的意思：

```
#糟糕的写法
$('li').on('click', function () {
    //todo
});

//好点的写法
$('ul').on('click', 'li', function () {
    //todo
});
```

### 状态机制

> React将UI简单的看作状态机。看UI看作各种各样的状态，并在各种状态间切换，很容易保持UI的一致性。在React中，你只要改变组件的状态，就会重新渲染UI，React会在最有效的方式下更新DOM。

### state工作原理

> 通过调用setState(data, callback)方法，改变状态，就会触发React更新UI。大部分情况下，我们不需要提供callback函数。React会自动的帮我们更新UI。

> 后面在好好看看这个callback的功能和调用时机。

### 什么样的组件该有state

> 大部分的组件应该从props属性中获取数据并渲染。但有的时候组件得相应用户输入，同服务器交互，这些情况下会用到state。React的官方说法是：尽可能的保持你的组件无状态化。为了实现这个目标，得保持你的状态同业务逻辑分离，并减少冗余信息，尽可能保持组件的单一职责。

> React官方推荐的一种模式就是：构建几个无状态的组件用来渲染数据，在这些之上构建一个有状态的组件同用户和服务交互，数据通过props传递给无状态的组件。我的理解大概就是这样：

```
var RenderComponent = React.createClass({
    render: function () {
        return (
            <ul>
                {
                    this.props['data-list'].map(function (item) {
                        return (<li>{item}</li>)
                    })
                }
            </ul>
        )
    }
});


var StateComponent = React.createClass({
    getInitialState: function () {
        return {list: ['xxx', 'yyy']};
    },
    render: function () {
        return (
            <div>
                <button onClick={this.handleClick}>click</button>
                <RenderComponent data-list={this.state.list}/>
            </div>

        )
    },
    handleClick: function () {
        this.setState({list: [1, 2, 3]});
    }
});

React.render(
    <StateComponent />,
    document.body
);
```

### state应该包含什么样的数据

> UI交互会导致改变的数据。

### state不应包含什么样的数据

- 计算过的数据
- 组件
- 从props复制的数据

> state应保含最原始的数据，比如说时间，格式化应该交给展现层去做。

> 组件应在render方法里控制。