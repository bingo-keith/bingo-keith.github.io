---

title: React从入门到精通系列
date: 2018/4/27 23:35:00 
tags: React 学习 库 框架

---

# 安装React

React是灵活的，可以在各种类型的项目中使用。
你可以使用它创建一个全新的应用程序，也可以逐步将其引入现有的项目中，而不需要重写整个项目。

<!-- more -->

## 创建一个单页面应用

`Create React App`是开始构建新的React单页应用程序的最佳方式。
它可以帮助您快速集成您的开发环境，以便您可以使用最新的JavaScript功能，它提供了一个很好的开发体验，并可以有效优化您的应用程序,提升开发效率。

```
$ npm install -g create-react-app
$ create-react-app hello-world
$ cd hello-world
$ npm run start
```

上面只是创建了一个React应用，不需要关心也需要不处理后端逻辑或数据库; 
它只是一个React的前端环境集成工具,负责创建前端的开发环境，所以你可以使用它与你想要的任何后端进行交互。 它内部使用了webpack，Babel和ESLint，你可以单独配置它们，来达到定制化的效果。

## 在已经开发的项目中使用React

您不需要重新编写应用程序即可开始使用React。

我们建议将React添加到应用程序的一小部分，例如单个小部件，以便您可以看到它是否适合您的用例。

虽然React可以在没有构建工具的情况下使用，但我们建议使用并设置它，以便提高生产力。 现代构建工具通常包括：

- 一个包管理器，例如`npm`

- 一个打包工具，例如`webpack`

- 一个编译工具，例如`Babel`

## 安装React

我们建议使用Yarn或npm来管理React前端模块的依赖。

通过Yarn安装：

`yarn add react react-dom`

通过npm安装：

`npm install --save react react-dom`

## 使用ES6和JSX

我们建议您使用Babel中的React配置让您在JavaScript代码中可以使用ES6和JSX。 ES6是一组现代JavaScript特性，使开发更容易，JSX是对React非常有效的JavaScript语言的扩展。

具体请百度Babel如何在许多不同的构建环境中配置Babel。首先确保你安装了`babel-preset-react`和`babel-preset-es2015`，并已经在`.babelrc`配置中启用它们。

## 使用ES6和JSX写一个HelloWorld的例子

我们建议使用像`webpack`或`Browserify`这样的打包工具，以便您可以编写模块化代码，它们可以将不同的代码模块打包捆绑到一起，用来优化的代码加载时间。

一个简单的React示例如下所示：

这里我使用的是bower来安装react和react-dom。

```javascript
mkdir hello-world & cd hello-world
bower install react babel --save
touch index.html
```

然后在index.html中写入以下内容：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="bower_components/react/react.js"></script>
    <script src="bower_components/react/react-dom.js"></script>
    <script src="bower_components/babel/browser.js"></script>
    <script type="text/babel">
        var doc = document;
        ReactDOM.render(
                <h1>你好，react</h1>,
                doc.getElementById('app')
        )
    </script>
    <title>ReactDOM.render</title>
</head>
<body>
<div id="app"></div>
</body>
</html>
```

# JSX的介绍

首先，请考虑下面的变量声明：

`const element = <h1>hello world</h1>;`

这个有趣的标签语法，因为它既不是`字符串`也不`是HTML`。

它被称为JSX，它是JavaScript的语法扩展。我们建议使用它和React一起使用，以便描述UI应该是什么样子的。JSX或许看上去像是一个模板语言，但是它具有JavaScript的全部能力。

JSX用来生成React元素。我们将在下一节中探讨将它们渲染到DOM。下面，你可以找到JSX的基础知识。

## JSX中的嵌入表达式

你可以通过将表达式包含在一个大括号里，以便用来在JSX中嵌入任何JavaScript表达式。

例如，下面代码中的 `2+2`，`user.name`，还有`formatName(user)`都是可用的表达式：

```javascript
function formatName(user){
    return user.firstName + user.lastName;
}

const user = {
    firstName: 'yatao',
    lastName: 'zhang'
};

const Element = (
    <h1>
        hello, {formatName(user)}
    </h1>
);

ReactDOM.render(
    Element,
    document.getElementById('root')
);
```

我们将JSX拆分为多行以提高可读性。虽然它不是强制性的要求，但当执行此操作时，我们还建议将其括在括号中，以避免自动分号插入而引起不必要的bug。

## JSX也是一个表达式

编译后，JSX表达式会成为常规JavaScript对象。

这意味着您可以在`if`语句和`for`循环中使用JSX，可以将其赋值给变量或者将其作为参数，然后从函数中返回：

```javascript
function getGreeting(user) {
    if (user) {
        return <h1>hello, {formatName(user)}!</h1>;
    }
    return <h1>hello, stranger.</h1>;
}
```

## 使用JSX指定属性

您可以使用引号将字符串文字指定为属性：

`const element = <div tabindex="0"></div>;`

您还可以使用大括号在属性中嵌入JavaScript表达式：

`const element = <img src={user.avatarUrl}></img>;`

## 使用JSX指定子元素

如果标记为空，您可以使用` />` 立即关闭它，例如html：

JSX标签也可以包含子元素：

```javascript
const element = (
    <div>
        <h1>hello!</h1>
        <h2>Good to see you here.<h2>
    </div>
);
```

> **警告**<br/>
> 由于JSX比HTML更接近JavaScript，ReactDOM使用驼峰命名法约定而不是HTML属性名称。
> 例如，**class**在JSX中变为**className**，**tabindex**变为**tabIndex**。

## JSX防止注入攻击

在JSX中嵌入用户输入是安全的：

```javascript
const title = response.potentiallyMaliciousInput;
// 要接收到的可能含有危险内容的字符串放入大括号中，这是比较安全的做法
const element = <h1>{title}</h1>;
```

默认情况下，ReactDOM在渲染它们之前`转义`嵌入在JSX中的任何值。 因此，它确保不能注入那些没有明确写在你的应用程序中的任何东西。 在渲染之前，一切都转换为字符串。 这有助于防止`XSS（跨站点脚本）`攻击。

## JSX表示对象

Babel将JSX编译为`React.createElement()`调用。

这两个例子是相同的：

```javascript
const element = (
    <h1 className="greeting">
        hello world
    </h1>
);
```

```javascript
const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'hello world'
);
```

`React.createElement()`会进行一些检查，以帮助您编写无明显语法错误的代码，本质上，它创建的是一个像这样的对象：

```javascript
// 提示：这是一个简单对象结构
const element = {
   type: 'h1',
   props: {
       className: 'greeting',
       children: 'hello, world'
   }
};
```

这些对象称为“React元素”。你可以把它们想象成你想在屏幕上看到的样子。React读取这些对象，并使用它们构造成为DOM元素同时一直保持其为最新的。

# 渲染元素

React元素是React应用程序的最小构建结构。

React元素描述了您想在屏幕上看到什么：

`const element = <h1>hello world</h1>;`

与浏览器DOM元素不同，React元素是纯对象，创建起来很方便。ReactDOM负责匹配React元素并更新DOM。

> **note**<br/>
> 人们可能将元素与更广为人知的“组件”概念混淆。我们将在下一节中介绍组件。元素是由什么组件组成的，我们建议您在向前跳过之前阅读此部分。

## 渲染元素到DOM中

假设您的HTML文件中有一个`<div>`：

`<div id="root"></div>`

我们将其称为“root”DOM节点，接下来其中的所有内容将由React DOM来管理。

仅使用React构建的应用程序通常具有单个 root DOM节点。 如果你正在将React集成到现有的应用程序中，则可以创建尽可能多单独的 root DOM节点。

要将React元素渲染到 root DOM节点，可以将它们都传递给`ReactDOM.render()`方法:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

const element = <h1>hello world</h1>;
ReactDOM.render(
    element,
    document.getElementById('root')
);
```

## 更新渲染元素

React元素是不可变的。 创建元素后，您不能更改其子元素或属性。 

**React元素就像电影中的单个帧：它表示某个时间点的UI。**

到目前为止，以我们的知识，更新UI的唯一方法是创建一个新的元素，并将其传递给`ReactDOM.render()`。

考虑这个滴答时钟示例：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function tick() {
    const element = (
        <div>
            <h1>hello world</h1>
            <h2>it is {new Date().toLocaleTimeString()}</h2>
        </div>
    );
    ReactDOM.render(
        element,
        document.getElementById('root')
    )
}

setInterval(tick, 1000);
```

它每秒从`setInterval()`的回调函数中调用`ReactDOM.render()`。

> **note**<br/>
> 实际上，大多数React应用程序只调用ReactDOM.render()一次。在接下来的章节中，我们将学习如何将这样的代码封装成有状态的组件。

## 按需更新

React DOM将元素及其子元素的内容与该元素变化之前的内容进行比较，并仅进行DOM更新以使DOM达到所需的状态。您可以通过使用浏览器工具检查最后一个示例来验证。

即使我们在每个tick上创建一个描述整个UI树的元素，只有内容发生改变的文本节点才被React DOM更新。

在我们的经验中，思考UI应该如何更新给定的时间，而不是随着时间的更改去修改整体的内容(DOM比较，按需更新)。

# 组件化和属性(props)

组件允许您将UI拆分为独立的可重用的部分，并单独地考虑每个部分。从概念上讲，组件就像JavaScript函数。 它们接受任意输入（称为“props”），并返回应该出现在屏幕上的React元素。

## 功能性组件(functional)和类组件(class component)

定义组件的最简单的方法是编写JavaScript函数：

```javascript
function Welcome(props) {
    return <h1>hello, {props.name}</h1>
}
```

此函数是个有效的React组件，因为它接收一个带有数据的“props”对象作为参数，并返回一个React元素。 我们把这样的组件称为“功能性组件(functional)”，因为它们是个JavaScript函数。

您还可以使用ES6类来定义组件：

```javascript
class Welcome extends React.Component {
    render() {
        return <h1>hello, {this.props.name}</h1>;
    }
}
```

上述两个组件从React的角度来看是等效的。

类组件有一些额外的功能，我们将在下面的章节中讨论。 在那之前，我们将简单地使用功能组件。

## 渲染一个组件

以前，我们只遇到使用DOM标签的React元素：

`const element = <div />;`

但是，元素也可以表示用户自定义的组件：

`const element = <Welcome name="keith" />;`

当React看到表示用户定义组件的元素时，它将该JSX标签的所以属性放到一个对象中传递给组件。 我们称这个对象为“props”。

例如，此代码在页面上呈现“Hello，keith”：

```javascript
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="keith" />;
ReactDOM.render(
    element,
    document.getElementById('root')
);
```

让我们回顾一下在这个例子中发生了什么：

1. 首先调用`ReactDOM.render()`方法，并处理`<Welcome name =“keith”/>`组件。

2. React使用`{name：'keith'}`作为props调用Welcome组件。

3. 我们的Welcome组件返回一个`<h1> Hello，keith </ h1>`元素作为结果。

4. React DOM有效地根据`<h1> Hello，keith </ h1>`来更新DOM。

> **警告**<br/>
> 组件名称始终使用·首字母大写。例如，`<div/>`表示一个DOM标签，但`<Welcome/>`表示一个组件，并要求Welcome必须和ReactDOM在一个作用域内。

## 编写组件

组件可以在其返回值中引用去其他组件。 这允许我们对任何级别的细节使用相同的组件抽象。 一个按钮，一个表单，一个对话框，一个屏幕：在React应用程序中，所有这些通常表示为组件。

例如，我们可以创建一个App组件，让它渲染多个`Welcome`组件：

```javascript
function Welcome(props) {
   return <h1>Hello, {props.name}</h1>;
}

function App() {
   return (
       <div>
           <Welcome name="keith" />
           <Welcome name="zhengwenbin" />
           <Welcome name="lisa" />
       </div>
   );
}

ReactDOM.render(
   <App />,
   document.getElementById('root')
);
```

这个新的React应用程序在顶部有一个单独的App组件。 

但是，如果您将React集成到现有应用程序中，则可以使用Button这样的小组件自下而上开始逐步向上到达视图层次结构的顶部。

> **警告**<br/>
> 引用多个组件时必须包裹在一个根元素中返回。这就是为什么我们添加了一个`<div>`来包含所有`<Welcome />`元素。

## 抽离组件

永远不要害怕将组件拆分成更小的组件。

例如，考虑这个`Comment`组件：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function formatDate(date) {
    return date.toISOString();
}

function Comment(props) {
    return (
        <div className="Comment">
            <div className="UserInfo">
                <img className="avatar"
                     src={props.author.avatarUrl}
                     alt={props.author.name}
                />
                <div className="UserInfo-name">
                    {props.author.name}
                </div>
            </div>
            <div className="Comment-text">
                {props.text}
            </div>
            <div className="Comment-date">
                {formatDate(props.date)}
            </div>
        </div>
    );
}

ReactDOM.render(
    <Comment author={{
        avatarUrl: 'https://ss0.bdstatic.com/7Ls0a8Sm1A5BphGlnYG/sys/portrait/item/3ae1dc06.jpg',
        name: 'zhangyatao'
    }} text={'我的名字叫郑文斌'} date={new Date()}/>,
    document.getElementById('root')
);
```

它接受`author`（作者），`text`（内容）和`date`（日期）作为`props`，用来描述社交媒体网站上的评论。

这个组件可能很难改变，因为所有的嵌套，它也很难重用其中的单个部分。让我们从中提取几个组件。

首先，我们将提取`avatar`：

```javascript
function Avatar(props) {
    return (
        <img className="Avatar"
            src={props.user.avatarUrl}
            alt={props.user.name}
        />
    );
}
```

`avatar`不需要知道它正在`Comment`中呈现。这就是为什么我们给它的prop一个更通用的名称：`user`而不是`author`。我们建议从组件自己的角度来命名`props`，而不是使用它的上下文。

我们现在可以对`Comment`组件做一点点简化：

```javascript
function Comment(props) {
    return (
        <div className="Comment">
            <div className="UserInfo">
                <Avatar user={props.user} />
                <div className="UserInfo-name">
                    {props.user.name}
                </div>
            </div>
            <div className="Comment-text">
                {props.text}
            </div>
            <div className="Comment-date">
                {formatDate(props.date)}
            </div>
        </div>
    );
}
```

接下来，我们将提取一个`UserInfo`组件，该组件在用户名称旁边呈现一个`avatar`:

```javascript
function UserInfo(props) {
    return (
        <div className="UserInfo">
            <avatar uer={props.user} />
            <div className="UserInfo-name">
                {props.user.name}
            </div>
        </div>
    );
}
```

这使我们可以进一步简化`Comment`组件：

```javascript
function Comment(props) {
    return (
       <div className="Comment">
           <UserInfo user={props.user} />
           <div className="Comment-text">
               {props.text}
           </div>
           <div className="Comment-date">
               {formatDate(props.date)}
           </div>
       </div>
    );
}
```

最终的代码如下：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function formatDate(date) {
    return date.toISOString();
}
function Avatar(props) {
    return (
        <img className="Avatar"
             src={props.user.avatarUrl}
             alt={props.user.name}
        />
    );
}
function UserInfo(props) {
    return (
        <div className="UserInfo">
            <Avatar user={props.user}/>
            <div className="UserInfo-name">
                {props.user.name}
            </div>
        </div>
    );
}
function Comment(props) {
    return (
        <div className="Comment">
            <UserInfo user={props.user}/>
            <div className="Comment-text">
                {props.text}
            </div>
            <div className="Comment-date">
                {formatDate(props.date)}
            </div>
        </div>
    );
}
ReactDOM.render(
    <Comment author={{
        avatarUrl: 'https://ss0.bdstatic.com/7Ls0a8Sm1A5BphGlnYG/sys/portrait/item/3ae1dc06.jpg',
        name: 'zhangyatao'
    }} text={'我的名字叫郑文斌'} date={new Date()}/>,
    document.getElementById('root')
);

```

让一个个可重用组件在大型应用程序中交付使用的过程中，抽离组件起初可能看起来像又脏又累的活儿。 所以有一个好的经验法则：如果UI的一部分被使用了好几次（按钮，面板，头像），或者内部比较复杂的东西（App，FeedStory，评论），一个可重用的组件对它来说可以达到最大的发挥空间。

## Props是只读的

无论是将组件声明为功能组件还是类组件，它都不能修改自己的props。考虑这个计算参数总和的函数：

```
function sum(a, b) {
    return a + b;
}
```

这样的函数被称为“纯函数”，因为它们不会改变它们的参数值，并且对于相同的输入总是返回相同的结果。（作者注：我理解的纯函数归纳为3点：1、同样的输入必定有同样的输出，2、不会产生副作用，3、不依赖任何外部的状态）

相反，这个函数是不纯的，因为它改变自己的参数：

```javascript
function withdraw(account, amount) {
    account.total -= amount;
}
```

React非常灵活，但它有一个严格的规则：

**所有React组件必须像它们的porps的纯函数那样运行。**

当然，应用中的UI大部分是动态的，随时间变化。 在下一节中，我们将介绍一个“state”的新概念。 状态允许React组件响应用户操作，网络响应和其他任何内容，随时间更改其输出，而不违反此规则。

# State和生命周期

考虑前面部分中的滴答时钟示例（第三章）。

到目前为止，我们只学习了一种更新UI的方法。

我们调用`ReactDOM.render()`来改变渲染输出：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function tick() {
    const element = (
        <div>
            <h1>Hell world</h1>
            <h2>It is {new Date().toLocaleTimeString()}</h2>
        </div>
    );
    ReactDOM.render(
        element,
        document.getElementById('root')
    );
}

setInterval(tick, 1000);
```

在本节中，我们将学习如何使·Clock·组件真正可重用和封装。它将设置自己的计时器并每秒更新一次。
我们可以从封装时钟的外观开始：

```javcscript
import React from 'react';
import ReactDOM from 'react-dom';

function Clock(props) {
    return (
        <div>
            <h1>hello world</h1>
            <h2>It is {props.date.toLocaleTimeString()}</h2>
        </div>
    );
}

function tick() {
   ReactDOM.render(
       <Clock date={new Date()} />,
       document.getElementById('root')
   );
}

setInterval(tick, 1000);
```

然而，它缺少了一个关键要求：时钟设置一个定时器和每秒更新UI的事实应该是时钟的实现细节。

理想情况下，我们要写这一次，并由时钟本身来更新时间：

```javascript
ReactDOM.render(
    <Clock />,
    document.getElementById('root')
);
```

要实现这一点，我们需要添加“state”到时钟组件。

**state类似于props，但它是私有的，完全由组件控制。**

我们之前提到，定义为类组件具有一些附加功能。 内部state就是：一个只有类组件可用的功能。

## 将函数形式组件改为类形式组件

您可以通过五个步骤将功能组件（如Clock）转换为类组件 ：

1. 创建一个与扩展React.Component相同名称的ES6类。

2. 为它添加一个单一的空方法render()。

3. 将函数的主体移动到render()方法中。

4. 在render()主体中用this.props替换props。

5. 删除剩余的空函数声明。

```javascript
class Clock extends React.Component {
   render() {
       return (
           <div>
               <h1>hello world</h1>
               <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
           </div>
       )
   };
}
```

`Clock`现在已经重新定义为类组件而不是之前的功能组件了。

这使我们可以使用额外的功能，如内部state和生命周期钩子。

## 向类组件中添加state

我们将分为三个步骤把`date`从`props`移动到`state`：

1）在`render()`方法中将`this.props.date`替换为`this.state.date`：

```javascript
class Clock extends React.Component {
    render() {
        return (
            <div>
                <h1>hello world</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
            </div>
        );
    }
}
```

2）添加一个赋值初始this.state的类构造函数：

```javascript
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    
    render() {
        return (
            <div>
                <h1>hello world</h1>
                <h2>It is {this.state.date.toLocalTimeString()}.</h2>
            </div>
        );
    }
}
```

注意我们如何将props传递给基类的构造函数：

```javascript
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
```

类组件应该总是用props调用基类构造函数。

3）从`<Clock />`元素中删除`date` prop：

我们稍后将定时器代码添加回组件本身。

结果如下所示：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    
    render() {
       return (
           <div>
               <h1>hello world</h1>
               <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
           </div>
       );
    }
}

ReactDOM.render(
    <Clock />,
    document.getElementById('root')
);
```

接下来，我们将使时钟设置自己的定时器，并每秒更新一次。

## 向类中添加声明周期方法

在具有许多组件的应用程序中，释放组件在销毁时占用的资源非常重要。

我们想要在第一次将时钟渲染到DOM时设置一个计时器。 这在React中称为`“安装（mounting）”`。

我们还想清除定时器，当时钟产生的DOM被删除。 这在React中称`为“卸载（unmounting）"`。

我们可以在组件类上声明特殊方法，以便在组件装入和卸载时运行一些代码：

```javascript
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    
    componentDidMount() {
        // 组件已经安装完毕
    }
    
    componentWillUnmount() {
        // 组件将要被卸载
    }
    
    render() {
       return (
           <div>
               <h1>hello world</h1>
               <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
           </div>
       );
    }
}
```

这些方法称为`“生命周期钩子”`。`componentDidMount()`只在组件输出呈现到DOM之后运行。这是设置计时器的好地方：

```javascript
componentDidMount() {
    this.timerID = setInterval(
        () => this.tick(),
        1000
    )
}
```

注意我们如何保存计时器ID就在这。

虽然`this.props`是由React本身设置的，并且`this.state`有一个特殊的含义，如果你需要存储不用于视觉输出的东西，你可以手动地添加额外的字段到类中。

如果你不使用`render()`中的东西，它不应该放置在`state`中。

我们将拆除`componentWillUnmount()`生命周期钩子中的计时器：

```javascript
componentWillUnmount() {
    clearInterval(this.timerID);
}
```

最后，我们将实现每秒运行的`tick()`方法。

它将使用`this.setState()`来调度组件本地state的更新：

```javascript
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    
    componentDidMount() {
        this.timerID = setInterval(
            () => this.tick(),
            1000
        )
    }
    
    componentWillUnmount() {
        clearInterval(this.timerID);
    }

    tick() {
        this.setState({
            date: new Date()
        });
    }
    
    render() {
       return (
           <div>
               <h1>hello world</h1>
               <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
           </div>
       );
    }
}
ReactDOM.render(
    <Clock />,
    document.getElementById('root')
);
```

现在时钟每秒钟都在滴答地走，棒不棒。。。。

让我们快速回顾一下发生了什么以及调用方法的顺序：

- 1）当将`<Clock />`传递给ReactDOM.render()时，React调用Clock组件的构造函数。由于Clock需要显示当前时间，它使用包括当前时间的对象初始化`this.state`。我们稍后将更新此state。

- 2）React然后调用Clock组件的`render()`方法。这是React如何学习应该在屏幕上显示什么。 React然后更新DOM以匹配时钟的渲染输出。

- 3）当时钟输出插入到DOM中时，React调用`componentDidMount()`生命周期钩子。在其中，时钟组件要求浏览器设置一个定时器，每秒调用`tick()`一次。

- 4）每秒钟浏览器调用`tick()`方法。在其中，Clock组件通过调用`setState()`和包含当前时间的对象来调度UI更新。由于`setState()`调用，React知道state已更改，并再次调用`render()`方法来了解屏幕上应该显示的内容。这个时候，`render()`方法中的`this.state.date`将会不同，因此渲染输出将包括更新的时间。 React相应地更新DOM。

- 5）如果时钟组件从DOM中被移除，React将调用`componentWillUnmount()`生命周期钩子，因此定时器停止。

## 正确使用state

关于`setState()`你应该了解三件事情:

### 不要直接修改state

例如，这将不会重新渲染组件：

```javascript
// 这是错误的
this.state.comment = 'hello';
```
应该使用`setState()`代替：

```
// 这是正确的
this.setState({comment: 'hello'});
```

唯一可以分配`this.state`的地方是构造函数。

### state更新可能是异步的

React可以将多个`setState()`用批处理为单个更新以实现较高的性能。

因为`this.props`和`this.state`可能是异步更新的，你不应该依赖它们的值来计算下一个state。

例如，此代码可能无法更新计数器：

```javascript
// 这是错误的
this.setState({
    counter: this.state.counter + this.props.increment,
});
```

要解决它，应该使用回调函数而不是对象来调用`setState()`。回调函数将接收先前的state作为第一个参数，并将应用更新时的props作为第二个参数：

```javascript
// 这是正确的
this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
}));
```

我们使用上面的箭头函数，但它也可以与常规函数一起使用：

```javascript
// 这同样也是正确的，将剪头函数改为普通函数
this.setState(function(prevState, props) {
   return {
       counter: prevState.counter + prps.increment
   }
});
```

state更新是经过合并的

当调用`setState()`时，React会将您提供的对象合并到当前state。例如，您的state可能包含几个独立变量：

```javascript
constructor(props) {
    super(props);
    this.state = {
        posts: [],
        comments: []
    }
}
```

然后，您可以使用单独的`setState()`来独立地更新它们：

```javascript
componentDidMount() {
    fetchPosts().then(response => {
        this.setState({
            posts: response.posts
        });
    });
    
    fetchComments().then(response => {
        this.setState({
            comments: response.comments
        }});
    });
}
```

合并很浅，所以`this.setState({comments})`不会波及`this.state.posts`。仅仅只是完全替换了`this.state.comments`而已。

### 数据是向下流动的

父组件和子组件都不能知道某个组件是有State的还是无State的，并且它们不应该关心它是否为功能组件或类组件。

这就是为什么State通常被设置为局部变量或封装到组件内部。除了拥有和设置它的组件之外的其他任何组件都不能访问它。

组件可以选择将其state作为props传递给其子组件：

`<h2>Is is {this.state.date.toLocaleTimeString()}.</h2>`

这也适用于用户定义的组件：

`<formattedDate date={this.state.data} />`

`FormattedDate`组件将在其`props`中接收`date`，并且不知道它是来自时钟的`state`，`props`还是`手动输入`：

```javascript
function FormattedData(props) {
    return <h2>Is is {props.date.toLocaleTimeString()}.</h2>;
}
```

这通常被称为**自顶向下**或**单向**数据流。任何state总是由一些特定组件拥有，并且从该state派生的任何数据或UI只能影响树中的“下面”组件。

如果你想象一个组件树作为props的瀑布流，每个组件的state就像一个额外的水源，它可以在任意点连接它，但也向下流。

为了显示所有组件都是真正隔离的，我们可以创建一个App组件来渲染三个`<Clock>`：

```javascript
function App() {
    return (
        <div>
            <Clock />
            <Clock />
            <Clock />
        </div>
    );
}

ReactDOM.render(
    <App />,
    document.getElementById('root')
);
```

每个时钟设置自己的定时器并独立更新。

在React应用程序中，组件是有状态还是无状态被视为可能随时间更改的组件的实现细节。您可以在有状态组件内使用无状态组件，反之亦然。

# 事件处理

使用React元素处理事件与处理DOM元素上的事件非常相似。不过有一些语法上的差异：

- React事件使用驼峰命名法，而不是全部小写命名。

- 使用JSX你传递一个函数作为事件处理程序，而不是一个字符串。

例如，HTML：

```javascript
<button onclick="activeLasers()">
    Active Lasers
</button>
```

在React中略有不同：

```
<button onClick={activateLasers}>
    Active Lasers
</button>
```

另一个区别是，你不能返回false来防止React中的默认行为。 您必须显式调用preventDefault。 例如，使用纯HTML，为了防止打开新页面的默认链接行为，您可以写：

```javascript
<a href="#" onclick="console.log('The link was clicked'); return false;">
    Click me
</a>
```

在React中，这可以改为：

```javascript
function ActiveLink() {
    function handleClick(e) {
        e.preventDefault();
        console.log('The link was clicked');
    }

    return (
        <a href='#' onClick={handleClick}>
            click me
        </a>
    );
}
```

这里，e是合成过的event。React根据W3C规范定义这些event，因此你不需要担心浏览器兼容性的问题。

当使用React时，在创建后向DOM元素添加通常不需要调用addEventListener监听器。相反，只是在最初渲染元素时提供事件监听器。

当您使用ES6类定义组件时，常见的模式是将事件处理程序作为类上的公共方法。例如，此Toggle组件呈现一个按钮，让用户在“ON”和“OFF”状态之间切换：

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 这个绑定是必要的，要不然该事件中的this就会是当前实例
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

你必须十分注意JSX中事件函数的意义。 在JavaScript中，类中的方法默认不绑定this。 如果你忘记绑定`this.handleClick`中的this关键字并将它传递给了`onClick`事件，当函数实际被调用时，会报出`undefined`的错误。

这不是React中特定的行为;它是JavaScript中函数正常工作的一部分。一般来说，如果你引用一个方法是后面没有`()`，如`onClick={this.handleClick}`，就会绑定该方法。

如果调用`bind`会使你烦恼，有两种方法可以解决这个问题。 您可以使用属性初始值设定props来正确处理：

```javascript
class LoggingButton extends React.Component {
  // 使用剪头函数绑定this
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

默认情况下，在`Create React App`中启用此语法。

如果不使用属性初始化语法，可以在回调中使用箭头函数：

```
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 给事件传入一个剪头函数也可以绑定this
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

此语法的问题是，每次`LoggingButton`渲染时都会创建不同的回调函数。在大多数情况下，这是要被罚款的。 然而，如果这个回调作为一个prop传递给较低的组件，这些组件可能会做额外的重新渲染。我们一般建议在构造函数中绑定以避免这种性能问题。

# 根据条件选择性渲染元素

在React中，您可以根据所需行为来创建并封装的不同组件。 然后，您可以根据某些条件来仅仅渲染其中的某一些。当然，这具体取决于当前应用程序的状态。

React中的分条件渲染与JavaScript中的分条件工作方式相同。 使用JavaScript运算符（如if或条件运算符）来创建一个表示当前状态的元素，让React匹配它们然后更新UI。

考虑这两个组件：

```javascript
function UserGreeting(props) {
    return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
    return <h1>Please sign up!</h1>;
}
```

我们将创建一个`Greeting`组件，根据用户是否登录显示这些组件中的其中一个：

```javascript
function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;
    if (isLoggedIn) {
        return <UserGreeting />;
    }
    return <GuestGreeting />;
}

ReactDOM.render(
    // 可以尝试将isLoggedIn改为true
    <Greeting isLoggedIn={false} />,
    document.getElementById('root')
)
```

此示例根据`isLoggedIn prop`的值呈现不同的问候语。

当然还有另外一种情况，就是满足某个条件之后显示具体的内容，否则不显示，可以参考一下组件：

```javascript
function DoSomething(props) {
    return <h1>hello everybody!</h1>;
}

function Amazing(props) {
    const isShow = props.isShow;
    if (isShow) {
        return <DoSomething />
    } else {
        // 返回null即可
        return null;
    }
}

ReactDOM.render(
    <Amazing isShow={true} />,
    document.getElementById('root')
)
```

## 元素变量

您可以使用变量来存储React元素。这可以帮助您有条件地渲染组件的一部分，其余输出也不会更改。

考虑这两个代表注销和登录按钮的新组件：

```javascript
function LoginButton(props) {
    return (
        <button onClick={props.onClick}>
            Login
        </button>
    );
}

function LogoutButton(props) {
    return (
        <button onClick={props.onClick}>
            Logout
        </button>
    );
}
```

在下面的示例中，我们将创建一个名为LoginControl的有状态组件。

它将根据其当前state呈现<LoginButton/>或<LogoutButton />。 它还将呈现来自前面示例的<Greeting/>：

```javascript
class LoginControl extends React.Component {
    constructor(props) {
        super(props);
        this.state = {isLogin: false};
        this.loginClick = this.loginClick.bind(this);
        this.logoutClick = this.logoutClick.bind(this);
    }
    
    loginClick() {
        this.setState({isLogin: true});
    }
    
    logoutClick() {
        this.setState({isLogin: false});
    }
    
    render() {
        const isLoggedIn = this.state.isLogin;
        let button = null;
        
        if (isLoggedIn) {
            button = <LogoutButton onClick={this.logoutClick} />;
        } else {
            button = <LoginButton onClick={this.loginClick} />;
        }
        
        return (
            <div>
                <Greeting isLoggedIn={isLoggedIn} />
                {button}
            </div>
        );
    }
}


ReactDOM.render(
    <LoginControl />,
    document.getElementById('root')
)
```

虽然`声明变量`并使用`if`语句是一个有条件地渲染组件的好方法，但有时您可能需要使用较短的语法。 有几种方法来内联JSX中的条件，如下所述。

## 通过逻辑运算符『 &&』内联if判断

您可以在JSX中嵌入任何表达式，将它们括在大括号中。 这包括JavaScript逻辑&&运算符。它可以方便地有条件地包括一个元素：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function Mailbox(props) {
    const unreadMessages = props.unreadMessages;
    return (
        <div>
            <h1>Hello!</h1>
            {unreadMessages.length > 0 &&
            <h2>
                You have {unreadMessages.length} unread messages.
            </h2>
            }
        </div>
    );
}

const messages =['zhangyatao', 'Re: zhangyatao', 'Re:Re: zhangyatao'];
ReactDOM.render(
    <Mailbox unreadMessages={messages} />,
    document.getElementById('root')
);
```

它的工作原理如下：
因为在JavaScript中，`true&&expression`总是返回为`expression`，而`false&&expression`总是返回为false。

因此，如果条件为`true`，紧接在`&&`之后的元素将出现在输出中。如果它为`false`，React将忽略并跳过它。

## 内联if-else进行条件判断操作

根据条件进行内联元素判断的另一种方法是使用JavaScript三元运算符`判断 ? 真 : 假 `。

在下面的例子中，我们使用它来有条件地渲染一小块文本。

```javascript
render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
        <div>
            The user is <b>{isLoggedIn ? '已登录' : '未登录'}</b>
        </div>
    );
}
```

它也可以用于较大的表达式，虽然不太明显发生了什么：

```javascript
render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
        <div>
            {isLoggedIn ? (
                <LogoutButton onClick={this.logoutClick} />
            ) : (
                <LoginButton onClick={this.loginClick} />
            )}
        </div>
    );
}
```

就像在JavaScript中一样，它取决于你和你的团队考虑更多的可读性，选择一个适当的风格。还要记住，当条件变得太复杂时，可能是提取组件的好时机。

## 阻止组件渲染

在极少数情况下，您可能希望由另一个组件呈现的情况下该组件仍然隐藏自身。这样的话，返回null即可。

在下面的示例中，`<WarningBanner/>`根据称为`warn`的props的值来渲染。如果prop的值为false，则组件不渲染到页面中：

```javascript
function WarningBanner(props) {
    if (!props.warn) {
        return null;
    }
    
    return (
        <div className="warning">
            warning!
        </div>
    );
}

class Page extends React.Component {
    constructor(props) {
        super(props);
        this.state = {showWarning: true};
        this.toggleClick = this.toggleClick.bind(this);
    }
    
    toggleClick() {
        this.setState(prevState => ({
            showWarning: !prevState.showWarning
        }));
    }
    
    render() {
        return (
            <div>
                <WarningBanner warn={this.state.showWarning} />
                <button onClick={this.toggleClick}>
                   {this.state.showWarning ? 'Hide' : 'Show'}
                </button>
            </div>
        );
    }
}

ReactDOM.render(
    <Page />,
    document.getElementById('root')
)
```

# Lists和Keys的处理

首先，让我们回顾一下如何在JavaScript中遍历lists。

下面的代码，我们使用`map()`函数获取一个`数字`数组，并将它们的值加倍。我们将`map()`返回的新数组赋给变量`doubled`并记录下来：

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(item => item * 2);
console.log(doubled); //=> [2, 4, 6, 8, 10]
```

上面的代码会在控制台打印`[2, 4, 6, 8, 10]`。

在React中，将数组转换为`元素集合`几乎和上面的代码是一样的。

## 渲染多个组件

您可以自己一个创建元素集合，并使用花括号{}将它们包含在JSX中。

下面，我们使用Javascript `map()`函数循环一个数字数组。我们为每个`item`返回一个`<li>`元素。最后，我们将结果数组的元素赋给`listItems`：

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map(item => <li>{item}</li>);
```

我们将整个`listItems`数组包含在`<ul>`元素中，并将其渲染到DOM：

```javascript
ReactDOM.render(
    <ul>{listItems}</ul>,
    document.getElementById('root')
)
```

此代码将会显示1到5之间的数字。

## 基础的List组件

通常你会将List放在组件中。

我们可以将前面的例子重构为接受数字数组的组件，并输出一个无序的元素列表。

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map(item => <li>{item}</li>);

    return (
        <ul>{listItems}</ul>
    );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
);
```

> 当您运行此代码时，将会收到一条警告，`Each child in an array or iterator should have a unique "key" prop. Check the render method of "NumberList".`

提示指出应该为列表的每一项提供一个属性`key`。 `“key”`是创建元素列表时需要包含的特殊字符串属性。 我们将在下一节讨论为什么它很重要。

让我们给`numbers.map()`的列表项分配一个`key`，并修复那个提示缺少`key`的问题。

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function NumberList(props) {
    const numbers = props.numbers;
-    const listItems = numbers.map(item => <li>{item}</li>);
+    const listItems = numbers.map(item => <li key={item.toString()}>{item}</li>);

    return (
        <ul>{listItems}</ul>
    );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
    <NumberList numbers={numbers}/>,
    document.getElementById('root')
)
```

## Key的用法

`keys`帮助确定哪些`item`已更改，已添加或已删除。 应该给数组中的元素设置上`key`属性，以便给元素一个稳定的身份：

```javascript
const numbers =[1, 2, 3, 4, 5];
const listItems = numbers.map(item => 
    <li key={item.toString()}>
        {item}
    </li>
);
```

最好的方法是使用一个`字符串`来选择`key`，它是其兄弟之间一个列表项的`唯一标识`。通常，您会使用数据中的ID作为key：

```javascript
const todoItems = todos.map(todo =>
    <li key={todo.id}>
        {todo.text}
    </li>
);
```

当您对已渲染的item没有稳定的ID时，您可以将项目index用作关键字：

```javascript
const todoItems = todos.map((todo, index) =>
    // 如果todo没有一个稳定的id，可以使用这种方法
    <li key={index}>
        {todo}
    </li>
);
```

如果项目需要实现重新排序，我们不建议为key使用索引，因为这将很慢。

## 合理提取组件中的Keys

**Keys仅在循环时的上下文中有意义。**

例如，如果您提取了一个`ListItem`组件，则应该将该key保存在数组中的`<ListItem/>`元素上，而不是`ListItem`本身的根`<li>`元素上。

**示例：key的错误用法**

```javascript
function ListItem(props) {
    const value = props.value;
    return (
        <li key={value.toString()}>
            {value}
        </li>
    );
}

function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map(item =>
        // 这是错误的，这里应该设置上key
        <ListItem value={item} />
    );
    return (
        <ul>{listItems}</ul>
    );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
    <NumberList numbers={numbers}> />,
    document.getElementById('root')
)
```

**实例：key的正确用法**

```javascript
function ListItem(props) {
    // 这才是正确的，在这里不需要设置key
    return <li>{props.value}</li>;
}

function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map(item => 
        // 这才是正确的，在这里设置key
        <ListItem key={item.toString()} value={item} />
    );
    return (
        <ul>
            {listItems}
        </ul>
    );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
)
```

**注意：map()中的元素都需要属性key。在哪儿循环就在哪儿设置key。**

## key在兄弟组件中必须是唯一的

数组中使用的`key`在其兄弟组件之间应该是唯一的。 但是，它们不需要是全局唯一的。 当我们生成两个不同的数组时，我们可以使用相同的键：

```javascript
function Blog(props) {
   const sidebar = (
       <ul>
           {props.posts.map(post => 
               <li key={post.id}>
                   {post.title}
               </li>
           )}
       </ul>
   );
   
   const content = props.posts.map(post =>
      <div key={post.id}>
          <h3>{post.title}</h3>
          <p>{post.content}</p>
      </div>
   );
   
   return (
       <div>
           {sidebar}
           <hr />
           {content}
       </div>
   );
}

const posts = [
    {id: 1, title: 'hello zhangyatao', content: 'you\'re so handsome!'},
    {id: 2, title: 'hi jiangyanyun', content: 'you\'re so beautiful!'}
];

ReactDOM.render(
    <Blog posts={posts} />,
    document.getElementById('root')
)
```

key用来作为React的观察点，但它们不会传递给您的组件。 如果你需要在组件中使用相同的值，则使用不同的名称显式地将它作为props传递：

```javascript
const content = posts.map(post =>
    <Post key={post.id} id={post.id} title={post.title} />
);
```

使用上面的示例，Post组件可以读取`props.id`，但不能读取`props.key`。

## 在JSX中嵌入map()

在上面的示例中，我们声明了一个单独的`listItems`变量并将其包含在JSX中：

```javascript
function NumberList(props) {
   const numbers = props.numbers;
   const listItems = numbers.map(item =>
       <ListItem key={item.toString()} value={item} />
   );
   return (
       <ul>{listItems}</ul>
   );
}
```

JSX允许在大括号中嵌入任何表达式，所以我们可以内联`map()`结果：

```javascript
function NumberList(props) {
    const numbers = props.numbers;
    return (
        <ul>
            {numbers.map(item =>
                <ListItem key={item.toString()} value={item} />
            )}
        </ul>
    );
}
```

这会让我们的代码更清晰，这种风格可以被随意使用。 

就像在JavaScript中，它是由你决定是否值得提取一个变量的可读性。

请记住，如果`map()`主体嵌套太多层，那么它是抽出一个组件的好时机。

# 表单处理

HTML表单元素与React中的其他DOM元素有点不同，因为表单元素自然地保留一些内部状态。例如，这种采用纯HTML格式的表单接受单个name：

```javascript
<form>
    <label>
        Name:
        <input type="text" name="name" />
    </label>
    <input type="submit" value="Submit" />
</form>
```

此表单具有在用户提交表单时浏览到新页面的默认HTML表单行为。

如果你想在React中这个行为，它是可以工作的。但在大多数情况下，使用JavaScript函数处理表单的提交并访问用户在表单中输入的数据很方便。实现这一点的标准方法是使用称为“可控组件”的技术。

## 可控组件

在HTML中，诸如`<input>`，`<textarea>`和`<select>`的表单元素通常保持它们自己的状态并且是基于用户输入来更新它的。在React中，可变state通常保存在组件的state属性中，并且仅使用`setState()`更新。

我们可以通过使React state是`“真理的唯一来源”`来结合这两者。 后续用户在该表单进行输入时依然受React组件控制。 其值由React以这种方式控制的可输入表单元素被称为`“可控组件”`。

例如，如果我们想让前面的示例日志在提交时记录下名称，我们可以将表单作为`可控组件`：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class NameForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: ''};
        this.change = this.change.bind(this);
        this.submit = this.submit.bind(this);
    }

    change(e) {
        this.setState({value: e.target.value});
    }

    submit(e) {
        e.preventDefault();
        console.log('A name was submitted: ', this.state.value);
    }

    render() {
        return (
            <form onSubmit={this.submit} >
                <label>
                    Name:
                    <input type="text"
                           value={this.state.value}
                           onChange={this.change}
                    />
                </label>
                <input type="submit" value="Submit" />
            </form>
        )
    };
}
ReactDOM.render(
    <NameForm/>,
    document.getElementById('root')
);
```

因为`value`属性是在`form`元素上设置的，所以显示的值将总是为`this.state.value`，让React `state`成其`value`的为真正来源。由于`change`在每次击键时运行并更新React `state`，因此显示的值将随用户键入而更新。

使用`受控组件`，使得直接修改或验证用户输入的时候，每个`state`变量都将具有关联的处理函数。例如，如果我们想强制执行`name全部用大写字母写`，我们可以写change为：

```javascript
change(e) {
    this.setState({value: e.target.value.toUpperCase()});
}
```

## textarea标签

在HTML中，`<textarea>`元素的子元素就是它的文本内容：

```javascript
<textarea>
    Hello there, my name is zhangyatao!
</textarea>
```

在React中，`<textarea>`改为使用`value`属性。 这样，使用`<textarea>`的表单可以非常类似于使用单行输入的表单：

```javascript
class EssayForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: '请填写一个你喜欢的dom元素'};
        this.change = this.change.bind(this);
        this.submit = this.submit.bind(this);
    }
    change(e) {
        this.setState({value: e.target.value});
    }
    submit(e) {
        e.preventDefault();
        console.log('有东西被提交了：', this.state.value);
    }
    render() {
        return (
            <form onSubmit={this.submit}>
                <label>
                    Name:
                    <textarea value={this.state.value} onChange={this.change} />
                </label>
                <input type="submit" value="Submit" />
            </form>
        );
    }
}
```

请注意，`this.state.value`在构造函数中初始化，因此`textarea`默认显示其中的文本。

## select标签

在HTML中，`<select>`创建一个下拉列表。例如，此HTML创建一个下拉列表：

```
<select>
    <option value="fruit">fruit</option>
    <option value="lime">lime</option>
    <option selected value="coconut">coconut</option>
    <option value="mango">mango</option>
</select>
```

请注意，由于`selected`属性`coconut`选项默认被选中的。

在React中在一般是在根`select`标签上使用`value`属性而不是使用`selected`属性。这在受控组件中很方便，因为您只需要在一个地方更新它。 例如：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class FlavorForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: 'coconut'};
        this.change = this.change.bind(this);
        this.submit = this.submit.bind(this);
    }

    change(e) {
        this.setState({value: e.target.value});
    }

    submit(e) {
        e.preventDefault();
        console.log('你喜欢的是：', this.state.value);
    }

    render() {
        return (
            <form onSubmit={this.submit}>
                <label>
                    请选择一个你喜欢的水果
                    <select value={this.state.value} onChange={this.change}>
                        <option value="fruit">fruit</option>
                        <option value="lime">lime</option>
                        <option value="coconut">coconut</option>
                        <option value="mango">mango</option>
                    </select>
                </label>
                <input type="submit" value="submit"/>
            </form>
        );
    }
}
ReactDOM.render(
    <FlavorForm/>,
    document.getElementById('root')
);
```

总的来说，这使得`<input type =“text”>`，`<textarea>`和`<select>`都非常类似 - 它们都接受一个`value`属性，您可以使用它来实现`可控组件`。

## 可控组件的备选方案

使用受控组件有时可能很乏味，因为您每次都需要为数据更改去编写事件处理程序，并通过React组件管理所有输入状态。 

当您将已经存在的项目转换为React或将React应用程序与非React库集成时，这可能会变得特别烦人。在这些情况下，您可能想要选择那些`不可控组件`(后续会有详细讲解)，一种用于实现输入表单的替代技术。

## 提升state

通常，如果有几个组件需要反映相同的变化数据。 我们建议将共享state提升到层级最近的，并且是共同的父组件上。 让我们看看这是如何工作的。

在本节中，我们将创建一个温度计算器来计算水是否在给定温度下沸腾。

我们将从一个名为`BoilingVerdict`的组件开始。 它接受`celsius(摄氏温度)`作为props，并打印是否足以煮沸水：

```javascript
function BoilingVerdict(props) {
    if (props.celsius >= 100) {
        return <p>水沸腾了</p>
    }
    return <p>水没有沸腾</p>
}
```

接下来，我们创建一个名字叫`Calculator`的组件。它渲染一个`<input>`，让您输入温度，并将其值保存在`this.state.value`中。

此外，它还会根据当前输入值渲染`BoilingVerdict`。

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function BoilingVerdict(props) {
    if (props.celsius >= 100) {
        return <p>水沸腾了</p>
    }
    return <p>水没有沸腾</p>
}

class Calculator extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: ''};
        this.change = this.change.bind(this);
    }

    change(e) {
        this.setState({value: e.target.value});
    }

    render() {
        const value = this.state.value;
        return (
            <fieldset>
                <legend>请输入温度</legend>
                <input
                    value={value}
                    onChange={this.change}/>
                <BoilingVerdict celsius={parseFloat(value)}/>
            </fieldset>
        );
    }
}
ReactDOM.render(
    <Calculator />,
    document.getElementById('root')
);
```

## 添加第二个input

我们的新需求是，除了输入`摄氏温度`，我们提供一个输入`华氏温度`，并保持同步。

我们可以从`Calculator`提取一个`TemperatureInput`组件。 我们将添加一个新的`scale` prop，可以是`“c”`或`“f”`：

```javascript
const scaleNames = {
    c: 'Celsius',
    f: 'Fahrenheit'
}
class TemperatureInput extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: ''};
        this.change = this.change.bind(this)
    }
    change(e) {
        this.setState({value: e.target.value});
    }
    render() {
        const value = this.state.value;
        const scale = this.props.scale;
        return (
            <fieldset>
                 <legend>输入{scaleName[scale]}温度</legend>
                 <input value={value} onChange={this.change} />
            </fieldset>
        );
    }
}
```

我们现在可以更改计算器来渲染两个单独的温度输入：

```javascript
class Calculator extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        return (
            <div>
                <TemperatureInput scale='f' />
                <TemperatureInput scale='c' />
            </div>
        );
    }
}
ReactDOM.render(
    <Calculator />,
    document.getElementById('root')
)
```

我们现在有两个输入框，但是当您在其中一个输入温度时，另一个不更新。 这违反了我们的要求：我们希望保持它们同步。

我们也不能从`Calculator`显示`BoilingVerdict`。 计算器不知道当前温度，因为它隐藏在`TemperatureInput`中。

## 提升state

首先，我们将写两个函数来将摄氏度转换为华氏度，然后返回：

```javvascript
// 将华氏度转换为摄氏度
function toCelsius(f) {
    return (f - 32) * 5 / 9;
}
// 将摄氏度转换为华氏度
function toFahrenheit(c) {
    return (c * 9 / 5) + 32;
}
```

这两个函数转换数字。我们将写另一个函数，它接受一个字符串值和一个转换函数作为参数，并返回一个字符串。 我们将使用它来计算一个输入基于另一个输入的值。

如果传入一个无效的`value`，那么会返回一个空字符串，并且保持输出四舍五入到小数点后第三位：

```javascript
function tryConvert(value, convert) {
    const input = parseFloat(value);
    if (Number.isNaN(input)) {
        return '';
    }
    const output = convert(input);
    const rounded = Math.round(output * 1000) / 1000;
    return String(rounded);
}
```

例如，`tryConvert('abc', toCelsius)`返回一个空字符串，`tryConvert('10.22', toFahrenheit)`返回`50.396`。
接下来，我们会从`TemperatureInput`中移除`state`。同时从props接收一个`value`和一个`onChange`事件来代替。

```javascript
class Temperature extends React.Component {
    constructor(props) {
        super(props);
        this.change = this.change.bind(this);
    }
    
    change(e) {
        this.props.onChange(e.target.value);
    }
    
    render() {
        const value = this.props.value;
        const scale = this.props.scale;
        return (
            <fieldset>
                <legend>请输入{scaleName[scale]}温度</legend>
                <input value={value} onChange={this.change} />
            </fieldset>
        );
    }
}
```

如果几个组件需要访问相同的state，这是一个state应该提升到层级最近的共同父级组件的标志。 在我们的例子中，这是那个`Calculator`组件。 我们将在其state中存储当前`value`和`scale`。

我们可以存储两个输入框的值，但事实证明这是没有必要的。 它足以存储最近更改的输入的`value`及其表示的`scale`。然后，我们可以基于当前`value`和`scale`单独推断其他输入的值。

输入保持同步，因为它们的值是从相同的`state`计算的：

```javascript
class Calculator extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: '', scale: 'c'};
        this.CelsiusChange = this.CelsiusChange.bind(this);
        this.FahrenheitChange = this.FahrenheitChange.bind(this);
    }

    CelsiusChange() {
        this.setState({scale: 'c', value});
    }

    FahrenheitChange() {
        this.setState({scale: 'f', value});
    }

    render() {
        const scale = this.state.scale;
        const value = this.state.value;
        const celsius = scale === 'f' ? tryConvert(value, toCelsius) : value;
        const fahrenheit = scale === 'c' ? tryConvert(value, toFahrenheit) : value;

        return (
            <div>
                <Temperature scale="c" value={celsius} onChange={this.CelsiusChange} />
                <Temperature scale="f" value={fahrenheit} onChange={this.FahrenheitChange} />
                <BoilingVerdict celsius={parseFloat(celsius)} />
            </div>
        );
    }
}
```

最终代码：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

const scaleNames = {
    c: 'Celsius',
    f: 'Fahrenheit'
};

function BoilingVerdict(props) {
    if (props.celsius >= 100) {
        return <p>水沸腾了</p>
    }
    return <p>水没有沸腾</p>
}

function toCelsius(f) {
    return (f - 32) * 5 / 9;
}

function toFahrenheit(c) {
    return (c * 9 / 5) + 32;
}

function tryConvert(value, convert) {
    const input = parseFloat(value);
    if (Number.isNaN(input)) {
        return '';
    }
    const output = convert(input);
    const rounded = Math.round(output * 1000) / 1000;
    return String(rounded);
}
class TemperatureInput extends React.Component {
    constructor(props) {
        super(props);
        this.change = this.change.bind(this);
    }

    change(e) {
        this.props.onChange(e.target.value);
    }

    render() {
        const value = this.props.value;
        const scale = this.props.scale;
        return (
            <fieldset>
                <legend>请输入{scaleNames[scale]}温度</legend>
                <input value={value} onChange={this.change}/>
            </fieldset>
        );
    }
}

class Calculator extends React.Component {
    constructor(props) {
        super(props);
        this.state = {value: '', scale: 'c'};
        this.CelsiusChange = this.CelsiusChange.bind(this);
        this.FahrenheitChange = this.FahrenheitChange.bind(this);
    }

    CelsiusChange(value) {
        this.setState({scale: 'c', value});
    }

    FahrenheitChange(value) {
        this.setState({scale: 'f', value});
    }

    render() {
        const scale = this.state.scale;
        const value = this.state.value;
        const celsius = scale === 'f' ? tryConvert(value, toCelsius) : value;
        const fahrenheit = scale === 'c' ? tryConvert(value, toFahrenheit) : value;

        return (
            <div>
                <TemperatureInput scale="c" value={celsius} onChange={this.CelsiusChange}/>
                <TemperatureInput scale="f" value={fahrenheit} onChange={this.FahrenheitChange}/>
                <BoilingVerdict celsius={parseFloat(celsius)}/>
            </div>
        );
    }
}
ReactDOM.render(
    <Calculator />,
    document.getElementById('root')
);
```

现在，无论您编辑哪个输入框，`Calculator`中的`this.state.value`和`this.state.scale`都会更新。 其中一个输入框获取value为原样，所以任何用户输入都被保留，另一个输入值总是基于它重新计算。

## 经验教训

对于在React应用程序中更改的任何数据，都应该有一个唯一的`“数据来源”`，也就是`state`。通常，首先将state添加到需要渲染的组件。如果其他组件也需要它，你可以将其提升到它们层级最近的共同父级组件中。而不是尝试在不同组件之间去同步状态，总归就一句话：`你应该依赖于自上而下的数据流`。

提升state会涉及编写比双向绑定方法更多的“样板”代码。但这样做有一个好处，就是开发者可以很快就找到错误。由于所有的state都`“保存”`在这些组件中并且只有该组件可以改变它，所以大大减少了错误的出现概率。此外，你可以实现任何自定义逻辑来拒绝或转换用户输入。

如果某些东西可以从prps或state派生，它都不应该再继续呆在state里。例如，不是同时存储`celsiusValue`和`fahrenheitValue`，而是只存储最后一次编辑的`value`和`scale`。另一个输入的值总是可以从`render()`方法中计算出来。这允许我们清除或应用四舍五入到其他字段，而不会丢失用户输入。

当您在UI中看到错误时，可以使用`React Developer Tools`检查props，并向上逐个排查DOM树，直到找到负责更新`state`的组件。这使你可以轻松地跟踪错误来源。

# 组合 VS 继承

React具有强大的组合模式，我们建议使用组合而不是继承来重用组件之间的代码。

在本节中，我们将考虑一些新的React常常遇到的开发继承的问题，并展示如何使用组合来解决它们。

## 有容乃大

一些组件提前不知道它们的子组件是什么的。这对于像`Sidebar`或`Dialog`这样的代表通用`“框”`的组件是特别常见的。

我们建议这些组件使用特殊的`children属性`将子组件元素直接传递到他们中：

```javascript
function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
            {props.children}
        </div>
    );
}
```

这让其他组件通过嵌套JSX传递任意到他们的`children`中：

```javascript
function WelcomeDialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                Welcome
            </h1>
            <p className="Dialog-message">
                感谢参观鹏寰国际大厦
            </p>
        </FancyBorder>
    );
}
```

最终的html结构为：

```javascript
<div id="root">
    <div data-reactroot="" class="FancyBorder FancyBorder-blue">
        <h1 class="Dialog-title">Welcome</h1>
        <p class="Dialog-message">感谢参观鹏寰国际大厦</p>
    </div>
</div>
```

任何在`<FancyBorder>`JSX标签内部的东西都会作为`children props`被传递到`FancyBorder`组件中。 由于`FancyBorder`在`<div>`中渲染`{props.children}`，所以所传递的元素将显示在这个`div`当中。

虽然这不常见，有时你可能需要在组件中有多个`“窟窿”`。 在这种情况下，你可以提出自己的规范，而不是使用`children`：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function Contacts() {
    return <h1>tel:182012322**</h1>
}
function Chat() {
    return <span>chat content</span>
}

function SplitPanel(props) {
    return (
        <div className="SplitPane">
            <div className="SplitPane-left">
                {props.left}
            </div>
            <div className="SplitPane-right">
                {props.right}
            </div>
        </div>
    )
}

function App() {
    return (
        <SplitPanel left={<Contacts />} right={<Chat />}/>
    );
}
ReactDOM.render(
    <App />,
    document.getElementById('root')
);
```

像`<Contacts/>`和`<Chat/>`等React元素只是一个对象，所以你可以像任何其他数据一样把它们当做props去传递。

## 用的专业一点

有时我们认为某个组件是其他组件的`“special cases”`。 例如，我们可能会说`WelcomeDialog`是`Dialog`的一个`“special cases”`。

在React中，这也通过组合实现，其中更`“特殊”`的组件渲染更`“通用”`的组件并用`props`配置它：

```javascript
function Dialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <p className="Dialog-message">
                {props.message}
            </p>
        </FancyBorder>
    );
}

function WelcomeDialog(props) {
    return (
        <Dialog title="Welcome" message="欢迎参观鹏寰国际大厦" />
    );
}
```

组合对于定义为类的组件同样有效：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
            {props.children}
        </div>
    );
}

function Dialog(props) {
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <p className="Dialog-message">
                {props.message}
            </p>
            {props.children}
        </FancyBorder>
    );
}

class SignUpDialog extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.handleSignUp = this.handleSignUp.bind(this);
        this.state = {login: ''};
    }

    render() {
        return (
            <Dialog title="Mars Exploration Program"
                    message="How should we refer to you?">
                <input value={this.state.login}
                       onChange={this.handleChange}/>
                <button onClick={this.handleSignUp}>
                    Sign Me Up!
                </button>
            </Dialog>
        );
    }

    handleChange(e) {
        this.setState({login: e.target.value});
    }

    handleSignUp() {
        alert(`Welcome aboard, ${this.state.login}!`);
    }
}

ReactDOM.render(
    <SignUpDialog />,
    document.getElementById('root')
);
```

## 关于继承

在Facebook，使用了React在数千个组件，他们没有发现一个必须实现组件继承层次结构的用例。

props和组合给你所有的灵活性，你需要以一个明确和安全的方式自定义组件的外观和行为。请记住，组件可以接受任意props，包括原始值，React元素或函数。

如果要在组件之间重用非UI功能，建议您将其提取到单独的JavaScript模块中。组件可以导入它并使用该函数，对象或类，而不扩展它。

# 深入理解JSX

从根本上讲，JSX就是提供了一个`React.createElement`(component,props,...children)`函数的语法糖。就像下面的JSX代码：

```javascript
<MyButton color="blue" shadow={2}>
    Click Me
</MyButton>
```

经过编译后为：

```javascript
React.createElement(
    MyButton,
    {color: 'blue', shadow: 2},
    'Click Me'
)
```

如果一个标签没有子元素的话，你可以使用`/>`来自动闭合。例如：

`<div className="sidebar" />`

经过编译后为：

```javascript
React.createElement(
    'div',
    {className: 'sidebar'},
    null
)
```

如果你想测试一些特定的JSX是如何转换成JavaScript的话，你可以试试在线Babel编译器。

## 指定React元素类型

JSX标记的第一部分决定了React元素的类型。

首字母大写的类型表示JSX标记指的为React组件。 这些标签被编译为对指定变量的直接引用，因此如果使用JSX `<Foo/>`表达式，Foo必须在当前的作用域内。

## React必须在作用域内

由于JSX编译的本质是对`React.createElement`的调用，因此React库也必须始终在JSX代码的作用域中。
例如，虽然`CustomButton`没有直接引用`React`，但是这两个导入的模块在这段代码中也还是很有必要的：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function WarningButton(props) {
    // return React.createElement(CustomButton, {color: 'red'}, null);
    return <CustomButton color="red" />
}
```

如果不使用JavaScript打包工具并将React通过script标签引入，那么它就会作为一个全局变量`React`。

## 对JSX类型使用『点』表示符

您还可以使用JSX中的点表示符来引用React组件。 如果您有一个模块会导出很多React组件的话，使用这种方法就会十分方便。例如，如果`MyComponents.DatePicker`是一个组件，您可以直接从JSX使用它：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

const MyComponents = {
    DatePicker(props) {
        return <div>这里有一个颜色为{props.color}的日期选择器</div>
    }
};

function BlueDataPicker(props) {
    return <MyComponents.DatePicker color="blue" />
}

ReactDOM.render(
    <BlueDataPicker />,
    document.getElementById('root')
);
```

## 用户自定义组件必须是首字母大写

当元素类型以是小写字母开头时，它指向一个内置组件，如`<div>`或`<span>`，并生成一个字符串`'div'`或`'span'`传递给`React.createElement`。以大写字母开头的类型，如<Foo/>编译为`React.createElement(Foo)`，并且在当前作用域内寻找这个名称为Foo的已定义或已导入组件。

我们建议使用`首字母大写`命名组件。如果你有一个以小写字母开头的组件，请在JSX中使用它之前请将它赋值给一个首字母大写的变量。

下面代码不会按预期运行：

```javascript
import React from 'react';

//这是错误的，这个组件应该为首字母大写
function hello(props) {
    // 这是正确的，因为div是一个有效的html标签
    return <div>Hello {props.name}</div>;
}

function HelloWorld(props) {
    // 这是错误的，因为它是首字母小写，所以React认为<hello />是一个html标签
    return <hello name="zhangyatao" />
}
```

想要修复上面的问题，我们必须将`hello`重命名为`Hello`，通过`<Hello />`来使用该组件：

```javascript
import React from 'react';

// 这是正确的
function Hello(props) {
    return <div>Hello {props.name}</div>;
}

function HelloWorld(props) {
    // 这是正确的
    return <Hello name="zhangyatao" />;
}
```

## 在运行的时候选择组件类型

不能将常规的javascript表达式用作React元素类型。 如果你想使用一个通用表达式来表示元素的类型，只需将它赋值给一个首字母大写的变量即可。

这通常出现在当你想基于同一个props渲染一个不同的组件的情况下：

```javascript
import React from 'react';
import {Com1, Com2} from './Components';

const components = {
    myCom1: Com1,
    myCom2: Com2
}

function RunCom(props) {
    // 这是错误的，JSX的类型不能这么写
    return <components[props.comType] type={props.type} />;
}
```

想要解决上面的问题，只需要将它们赋值给一个首字母大写的变量即可：

```javascript
import React from 'react';
import {Com1, Com2} from './Components';


const components = {
    myCom1: Com1,
    myCom2: Com2
}

function RunCom(props) {
    // 这是正确的，将它们赋值给一个首字母大写的变量
    const MyCom = components[props.comType];
    return <MyCom type={props.type} />;
}
```

## JSX中的Props

在JSX中指定Props有以下几种不同的方法。

### JavaScript表达式

你可以传递任何JavaScript表达式作为Props，用{}括住它们就可以使用。 例如，在这个JSX中：

`<MyComponents foo={1 + 2 + 3 + 4} />`

对于`MyComponent`来说，`props.foo`的值将为`10`，因为是通过表达式`1 + 2 + 3 + 4`计算得到的。

`if`语句和`for`循环在JavaScript中不是表达式，因此它们不能在JSX中直接使用。相反，写完它们之后你可以把JSX放在里面。 例如：

```javascript
function NumberDescriber(props) {
    let description;
    if (props.number % 2 === 0) {
        description = <strong>偶数</strong>
    } else {
        description = <strong>奇数</strong>
    }
    return <div>{props.number}是一个{description}.</div>;
}
```

### 字符串直接量

你可以传递一个字符串内容作为props。 这两个JSX表达式是等价的：

```javascript
<MyComponent message="hi zhangyatao" />

<MyComponent message={'hi zhangyatao'} />
```

当你传递一个字符串直接量时，它的值是经过html转义的。 所以这两个JSX表达式是等价的：

```javascript
<MyComponent message='&lt;3' />

<MyComponent message={'<3'} />
```

### Props默认值为true

如果你没有给Props传入一个值，那么它的默认值为`true`，这两个JSX表达式是等价的：

```javascript
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

一般来说，我们不建议使用它，因为它可以使用ES6对象的简写`{foo}`，也就是`{foo：foo}`的简称会和`{foo：true}`混淆。这种行为在这里只是方便它匹配到HTML行为。

### Props传递

如果你有一个对象类似的数据作为props，并且想在JSX中传递它，你可以使用`...`作为一个`“spread”`运算符传递整个props对象。 这两个组件是等效的：

```javascript
function App() {
    return <Greeting firstName="yatao" lastName="zhang" />;
}

function App() {
    const props = {firstName: 'yatao', lastName: 'zhang'};
    return <Greeting {...props} />;
}
```

当创建一个通用容器时，`spread props`很有用。 

然而，他们也可以让你的代码变得有点凌乱，这样很容易使大量不相关的prps传递给那些不关心它们的组件。 建议您谨慎使用此语法。

## JSX中的子元素和子组件

在包含开始标记和结束标记的JSX表达式中，这些标记之间的内容通过一种特殊的prop：`props.children`传递。 有几种不同的方式传递子组件：

### 字符串直接量

你可以在开始和结束标签之间放一个字符串，那么`props.children`就是那个字符串。这对许多内置的HTML元素很有用。 例如：

```javascript
function MyComponent(props) {
    return <div>{props.children}<div>; //=> <div>hello zhangyatao</div>
}

<MyComponent>Hello zhangyatao</MyComponent>
```

这是有效的JSX，并且`MyComponent`中的`props.children`将是字符串`“Hello zhangyatao”`。 HTML标签是不会经过转义的，所以你一般可以写JSX就像你写HTML一样：

`<div>这是一个html标签 &amp; 同时也是个JSX</div>`

JSX会删除行的开始和结尾处的空格。它也会删除中间的空行。 与标签相邻的空行被会被删除;
在字符串文本中间出现的空行会缩合成一个空格。所以这些都渲染相同的事情：

```javascript
<div>hello zhangyatao</div>

<div>
    hello zhangyatao
</div>

<div>
    hello
    zhangyatao
</div>

<div>

hello zhangyatao
</div>
```

### JSX子元素

你可以使用很多个JSX元素作为子元素。这对需要嵌套的显示类型组件很有用：

```javascript
<Dialog>
    <DialogHeader />
    <DialogBody />
    <DialogFooter />
</Dialog>
```

你可以将不同类型的子元素混合在一起，因此JSX子元素可以与字符串直接量一起使用。这是JSX的另一种方式，就像一个HTML一样：

```javascript
<div>
    这是一个列表
    <ul>
        <li>item 1</li>
        <li>item 2</li>
    </ul>
</div>
```

一个React组件不可能返回多个React元素，但是一个JSX表达式可以包含多个子元素，因此如果你想让一个组件渲染多个东西，你可以将它们统一放置在就像上面那样的div中。

### Javascript表达式

您可以将任何JavaScript表达式放在{}中作为子组件传递。 例如，下面这些表达式是等价的：

```javascript
function MyComponent(props) {
    return <div>{props.children}<div>; //=> <div>hi zhangyatao</div>
}

<MyComponent>hi zhangyatao</MyComponent>

<MyComponent>{'hi zhangyatao'}</MyComponent>
```

这通常用于渲染任意长度的JSX表达式列表。例如，这将渲染一个HTML列表：

```javascript
function Item(props) {
    return <li>{props.message}</li>;
}

function TodoList(props) {
    const todos = ['完成文档', '出去逛街', '打一局dota'];
    return (
        <ul>
            {todos.map(message => <Item key={message} message={message} />)}
        </ul>
    );
}
```

JavaScript表达式可以与其他类型的子元素混合使用。 这通常用于替换字符串模板：

```javascript
function Hello(props) {
    return <div>Hello {props.name}</div>;
}
```

### 使用函数作为子元素

通常，插入JSX中的JavaScript表达式都最终返回为一个字符串、React元素、一个列表。

当然，`props.children`可以像任何其他props那样工作，它可以传递任何类型的数据，并不局限于那些告诉React应该如何渲染的东东。例如，如果您有一个自定义组件，您可以将`props.children`作为一个回调函数：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function Repeat(props) {
    let items = [];
    let callback = props.children;
    var numTimes = props.numTimes;
    for(var i = 0 ; i < numTimes ; i++ ){
        items.push(callback(i));
    }
    return <div>{items}</div>;
}

function ListOfTenThings(props) {
    return (
        <Repeat numTimes={10}>
            {index => <div key={index}>这是列表中的第{index}项</div>}
        </Repeat>
    );
}
ReactDOM.render(
    <ListOfTenThings/>,
    document.getElementById('root')
);
```

传递给自定义组件的子元素可以是任何东西，只要在React在渲染之前，该组件将它们转换为可以理解的东西即可。 这种用法并不常见，如果你想扩展JSX的其他能力，可以通过这个例子了解下它的工作原理。

### 布尔值、null、undefined在渲染时会被自动忽略

`false`，`null`，`undefined`和`true`是有效的子元素，不过他们从根本上讲是不参与渲染的。这些JSX表达式将渲染处相同的东西：

```javascript
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{true}</div>
```

这对于有条件地呈现React元素很有用。 如果`showHeader`为`true`，那么这个JSX只渲染一个`<Header />`：

```javascript
<div>
    {showHeader && <Header />}
    <Content />
</div>
```

如果返回一些`“假的”`值就会收到一个警告，如数字`0`，不过React仍然会渲染。例如，此代码将不会像您预期的那样工作，因为当`props.messages`是空数组时将打印`0`：

```javascript
<div>
    {props.messages.length && <Message messages={props.messages} />}
</div>
```

想要修复上面的问题，你要确定这个表达式在&&之前总返回`布尔值`:

```javascript
<div>
    {props.messages.length > 0 && <Message messages={props.messages} />}
</div>
```

相反，如果你想要一个值如`false`，`true`，`null`或`undefined`出现在输出中，你必须先将它转换为`字符串`：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function MyVariable(props) {
    const myVariable = false;
    // 如果这里不把false转换为字符串，这只会输出『我的javascript变量是』
    const convertedVar = String(myVariable);
    return (
        <div>
            我的javascript变量是{convertedVar}
        </div>
    );
}
ReactDOM.render(
    <MyVariable/>,
    document.getElementById('root')
);
```

# 使用PropTypes进行类型检测

随着你的应用的变得越来越大，你可以通过`typechecking`来找到更多的bug。 对于某些应用，您可以使用JavaScript扩展（如`Flow`或`TypeScript`）对整个应用程序进行类型检查。

即使你不使用这些，React也有一些内置的`typechecking`能力。 要在组件的`props`上运行`typechecking`，可以分配特殊的`propTypes`属性：

```javascript
class Greeting extends React.Component {
    render() {
        return (
            <h1>Hello {this.props.name}</h1>
        )
    };
}
Greeting.propTypes = {
    name: React.PropTypes.string.isRequired
};
```

`React.PropTypes`返回的是一系列验证函数，用于确保接收的数据类似是否是有效的。 

在这个例子中，我们使用`React.PropTypes.string.isRequire`检测`name`是否为字符串，并且是必填的。 

当为prop提供无效值时，JavaScript控制台中将显示警告。 出于性能原因，仅在开发模式下检查`propTypes`。

## React.PropTypes

下面是一个示例，其中提供了不同的验证函数：

```javascript
MyComponent.propTypes = {
  // 你可以定义一个js原始类型的prop,默认请情况下，这是都是可选的
  optionalArray: React.PropTypes.array,
  optionalBool: React.PropTypes.bool,
  optionalFunc: React.PropTypes.func,
  optionalNumber: React.PropTypes.number,
  optionalObject: React.PropTypes.object,
  optionalString: React.PropTypes.string,
  optionalSymbol: React.PropTypes.symbol,

  // 任何可以渲染的东西：数字，字符串，元素或数组（或片段）。
  optionalNode: React.PropTypes.node,

  // React元素
  optionalElement: React.PropTypes.element,

  // 你也可以声明prop是某个类的实例。 内部使用的是JS的instanceof运算符。
  optionalMessage: React.PropTypes.instanceOf(Message),

  // 你可以通过将它作为枚举来确保你的prop被限制到特定的值。
  optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),

  // 可以是许多类型之一的对象
  optionalUnion: React.PropTypes.oneOfType([
    React.PropTypes.string,
    React.PropTypes.number,
    React.PropTypes.instanceOf(Message)
  ]),

  // 某种类型的数组
  optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),

  // 具有某种类型的属性值的对象
  optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),

  // 采取特定样式的对象
  optionalObjectWithShape: React.PropTypes.shape({
    color: React.PropTypes.string,
    fontSize: React.PropTypes.number
  }),

  // 你可以用`isRequired`来连接到上面的任何一个类型，以确保如果没有提供props的话会显示一个警告。
  requiredFunc: React.PropTypes.func.isRequired,

  // 任何数据类型
  requiredAny: React.PropTypes.any.isRequired,

  // 您还可以指定自定义类型检查器。 如果检查失败，它应该返回一个Error对象。 不要`console.warn`或throw，因为这不会在`oneOfType`内工作。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 您还可以为`arrayOf`和`objectOf`提供自定义类型检查器。 如果检查失败，它应该返回一个Error对象。 
  // 检查器将为数组或对象中的每个键调用验证函数。 
  // 检查器有两个参数，第一个参数是数组或对象本身，第二个是当前项的键。
  customArrayProp: React.PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

## 要求只能是单个子元素

使用`React.PropTypes.element`，您可以指定只有一个子元素可以作为内容传递的组件。

```javascript
class MyComponent extends React.Component {
    render() {
        // 只能包含一个子元素，否则会给出警告
        const children = this.props.children;
        return (
            <div>{children}</div>
        );
    }
}

MyComponent.propTypes = {
    children: React.PropTypes.element.isRequired
}
```

## 设置Prop默认值

您可以通过使用`defaultProps`属性来定义props的默认值：

```javascript
class Greeting extends React.Component {
    render() {
        return <h1>hello {this.props.name}</h1>;
    };
}

// 如果name没有传值，则会将name设置为默认的zhangyatao
Greeting.defaultProps = {
    name: 'zhangyatao'
}

// 会渲染处<h1>hi zhangyatao</h1>
ReactDOM.render(
    <Greeting />,
    document.getElementById('root')
)
```

如果父组件没有设置并传入`name`，`defaultProps`将确保`this.props.name`将有一个默认值。 `propTypes`类型检查发生在`defaultProps`解析之后，因此类型检查也将应用于`defaultProps`。

# refs和DOM元素

在典型的React数据流中，props是父组件与其子组件交互的唯一方式。 要修改子组件，需要使用一个新的props进行重新渲染。

但是，在某些情况下，您需要在典型数据流之外强制修改子组件。 要修改的子组件可以是React组件实例，也可以是DOM元素。 对于这两种情况，React提供了一个以下这样的功能。

## 通过ref属性设置回调函数

React提供可以附加到任何组件的特殊属性。`ref`属性接受一个回调函数，回调函数将在组件被挂载或卸载后立即执行。

当在HTML元素上使用`ref`属性时，`ref`回调函数接收一个基础的DOM元素作为其参数。例如，此代码使用`ref`回调函数来存储对DOM节点的引用：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class CustomTextInput extends React.Component {
    constructor(props) {
        super(props);
        this.focus = this.focus.bind(this);
    }

    focus() {
        // textInput是一个标准的DOM元素
        this.textInput.focus();
    }

    render() {
        return (
            <div>
                <input type="text" ref={input => {
                    this.textInput = input;
                }}/>
                <input type="button" value="选中上面的text input" onClick={this.focus}/>
            </div>
        );
    }
}
ReactDOM.render(
    <CustomTextInput/>,
    document.getElementById('root')
);
```

当组件装载(mounting)时，React将使用DOM元素调用`ref`回调函数，并在卸载时用null调用它。

使用`ref`回调函数是为类设置一个属性来访问DOM元素的常见模式。 如果您目前正在使用`this.refs.myRefName`来访问DOM引用的话，我会建议你使用此模式。

当在自定义组件上使用`ref`属性时，`ref`回调接收组件的已装入的组件实例作为其参数。例如，如果我们想要包装上面的`CustomTextInput`来模拟它在装载(mounting)后立即被点击：

```javascript
class AutoFocusTextInput extends React.Component {
    componentDidMount() {
        this.textInput.focus();
    }
    render() {
        return (
            <CustomTextInput ref={input => {this.textInput = input; }} />
        );
    }
}
```

您不能在功能性组件上使用`ref`属性，因为它们没有实例。 但是，您可以使用功能性组件的render函数内的`ref`属性：

```javascript
function CustomTextInput(props) {
    // 这里必须提前顶一个textInput，只有这样才可以正常执行ref回调函数
    let textInput = null;
    function click() {
        textInput.focus();
    }
    return (
        <div>
            <input type="text" ref={input => { textInput = input; }} />
            <input type="button" value="选中这个输入框" onClick={click} />
        </div>
    );
}
```

## 不要过度使用ref

你的第一个倾向可能是使用`refs`在你的应用中`“make things happen”`。

如果是这种情况，你必须花一点时间，关键去考虑在组件层次结构中应该拥有什么状态。

通常，在层次结构中处于更高级别的组件“拥有”状态是一个让一切便清除的最适当位置。有关示例，请参阅本系列的《提升state》。

# 不可控组件

在大多数情况下，我们建议使用`可控组件`(参考本系列《表单处理》)来实现表单功能。在可控组件中，表单数据由React组件处理。 替代方法是使用`不可控组件`，其中表单数据是由DOM本身处理。

要编写不可控组件，不需要为每个状态更新都去编写事件处理程序，你可以直接使用ref从DOM获取表单值。

例如，此代码在不可控组件中接受一个name：

```javascript
class NameForm extends React.Component {
    constructor(props) {
        super(props);
        this.submit = this.submit.bind(this)
    }
    submit(e) {
        e.preventDefault();
        console.log('有一个name提交了：',this.input.value);
    }
    render() {
        return (
            <form onSubmit={this.submit}>
                <label>
                    Name:
                    <input type="text" ref={input => { this.input = input; }} />
                </label>
                <input type="submit" value="submit" />
            </form>
        );
    }
}
```

由于不可控组件在DOM中保持『真实的唯一来源』，因此当使用不可控组件时，有时更容易集成React和非React代码。 如果你想要快速创建并且不顾代码质量的话，它也可以稍微少写一些代码。 否则，通常都应该使用`受控组件`。

如果仍然不清楚在特定的情况下应该使用哪种类型`(受控组件/不受控组件)`的组件，您可能会发现这篇文章中的`控制输入`和`不可控输入`对加深你的理解是很有帮助的。

## 默认值

在React渲染生命周期中，表单元素的`value`属性将覆盖DOM中的值。 对于不可控组件，您通常希望React指定初始值，但不去控制DOM的后续更新。要处理这种情况，可以指定`defaultValue`属性，而不是`value`。

```javascript
render() {
    return (
        <form onSubmit={this.submit}>
            <label>
                Name:
                <input defaultValue="zhangyatao" type="text"
                    ref={input => { this.input = input; }} />
            </label>
            <input type="submit" value="submit" />
        </form>
    );
}
```

同样，`<input type =“checkbox”>`和`<input type =“radio”>`支持`defaultChecked`，并且`<select>`也同样支持`defaultValue`。

# 性能优化

在React内部，React使用了几种比较聪明的技术来实现`最小化更新UI所需的昂贵的DOM操作`的数量。

对于许多应用来说，使用React将很快速的渲染出用户界面，从而无需进行大量工作来专门做优化性能的工作。

大概有以下有几种方法来加快你的React应用程序。

## 使用生产环境的配置进行构建

如果你在React应用中进行基准测试或这遇到了性能问题，请首先确保你是使用的压缩后线上版本js文件来进行的测试：

- 对于`Create React App`来说，你需要在构建时运行`npm run build`。

- 对于单文件来说，我们提供了生产环境版本`.min.js`。

- 使用的是Browserify，你先设置`NODE_ENV=production`然后运行。

- 使用的是webpack，你需要在生产环境配置中加入以下插件：

```javascript
new webpack.DefinePlugin({
    'process.env': {
        NODE_ENV: JSON.stringify('production')
    }
}),
new webpack.optimize.UglifyJSPlugin();
```

在构建应用程序时开发构建工具可以打印一些有帮助的额外警告。

但是由于需要额外地记录这些警告信息，所以它也会变得更慢。

## 避免重复处理DOM

React会创建并维护所渲染的UI内部表示信息。其中包括从组件返回的React元素。 此表示信息使React避免创建DOM节点和访问那些没有必要的节点，因为这样做可能会比JavaScript对象上的一些操作更慢。 有时它被称为`“虚拟DOM”`。

当组件的`props`或`state`更改时，React通过将最新返回的元素与先前渲染的元素进行比较来决定是否需要实际的DOM更新。 当它们不相等时，React将更新DOM。

在某些情况下，您的组件可以通过重写生命周期函数`shouldComponentUpdate`来加快所有这些操作。这个函数会在重新渲染之前触发。 此函数的默认实现返回`true`，让React执行更新：

```javascript
shouldComponentUpdate(nextProps, nextState) {
    return true;
}
```

如果你知道在某些情况下你的组件不需要更新，你可以从`shouldComponentUpdate`中返回`false`，而不是跳过整个渲染过程，其中包括调用当前组件和下面的`render()`。

## shouldComponentUpdate的应用

这里是一个组件的子树。对于其中每一个子树来说，`SCU`指示`shouldComponentUpdate`返回什么，`vDOMEq`指示渲染的React元素是否相等。 最后，圆圈的颜色表示组件是否必须重新处理。

![](https://segmentfault.com/img/bVGXZA?w=977&h=692)

因为`shouldComponentUpdate`对于以C2为根的子树返回了`false`，所以React没有尝试渲染C2，因此甚至不必在C4和C5上调用`shouldComponentUpdate`。

对于C1和C3，`shouldComponentUpdate`返回true，因此React必须下到子树中并检查它们。对于C6子树`shouldComponentUpdate`返回`true`，并且因为渲染的元素不是相同的，React不得不更新DOM。

最后一个有趣的例子是C8。React不得不渲染这个组件，不过由于React元素返回的元素等于之前渲染的元素，所以它不必更新DOM。

注意，React只需要做C6的DOM重新处理，这是不可避免的。
对于C8，它通过比较渲染的React元素来决定是否重新处理DOM。至于C2的子树和C7，我们在`shouldComponentUpdate`返回`false`时它甚至都不需要比较元素，并且也没有调用`render()`。

## 例子

如果你的组件的唯一的改变方式就是改变`props.color`或`state.count`，你可以用`shouldComponentUpdate`检查：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class CounterButton extends React.Component {
    constructor(props) {
        super(props);
        this.state = {count: 1};
        this.click = this.click.bind(this);
    }

    click() {
        this.setState(prevState => ({
            count: prevState.count + 1
        }));
    }

    shouldComponentUpdate(nextProps, nextState) {
        if (this.props.color !== nextProps.color) {
            return true;
        }
        return this.state.count !== nextState.count;
    }

    render() {
        return (
            <button color={this.props.color} onClick={this.click}>
                Count：{this.state.count}
            </button>
        );
    }
}
ReactDOM.render(
    <CounterButton color="blue"/>,
    document.getElementById('root')
);
```

在这段代码中，`shouldComponentUpdate`只是检查`props.color`或`state.count`是否有任何变化。如果它们的值没有更改，则组件不更新。 如果你的组件比这个例子中的组件更复杂，你可以使用类似的模式在props和state的所有字段之间做一个`“浅比较”`，以确定组件是否应该更新。

比较常见的模式是使用React提供的一个帮助对象来使用这个逻辑，可以直接继承`React.PureComponent`。所以上面这段代码有一个更简单的方法来实现同样的事情：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class CounterButton extends React.PureComponent {
    constructor(props) {
        super(props);
        this.state = {count: 1};
        this.click = this.click.bind(this);
    }

    click() {
        this.setState(prevState => ({
            count: prevState.count + 1
        }));
    }

    render() {
        return (
            <button color={this.props.color} onClick={this.click}>
                Count: {this.state.count}
            </button>
        );
    }
}
ReactDOM.render(
    <CounterButton color="blue"/>,
    document.getElementById('root')
);
```

大多数时候，你可以使用`React.PureComponent`而不是编写自己的`shouldComponentUpdate`。 它只做一个浅层的比较，所以你不需要直接使用它，如果你的组件内部`props`或`state`的数据有可能会`突然变化`，那么`浅比较`将失效。

`浅比较`的失效可能是一个更加复杂的数据结构问题`(突然变化)`。 例如，假设您想要一个以逗号分隔单词列表的`ListOfWords`组件，使用一个父`WordAdder`组件，当你单击一个按钮用来添加一个单词到列表中时。 下面的代码将无法正常工作：

```javascript
// PureComponent在内部会帮我们对props和state进行简单对比(浅比较)
// 值类型比较值，引用类型比较引用，但是不会比较引用类型的内部数据是否改变。
// 所以就会出现一个bug，不管你怎么点button，div是不会增加的。
class ListOfWords extends React.PureComponent {
    render() {
        return <div>{this.props.words.join(',')}</div>;
    }
}

class WordAdder extends React.Component {
    constructor(props) {
        super(props);
        this.state = {words: ['zhangyatao']};
        this.click = this.click.bind(this);
    }
    click() {
        // 这么写是不对的，因为state的更新是异步的，所以可能会导致一些不必要的bug
        const words = this.state.word;
        words.push('zhangyatao');
        this.setState({words: words});
    }
    render() {
        return (
            <div>
                <button onClick={this.click} />
                <ListOfWords words={this.state.words} />
            </div>
        );
    }
}
```

问题是`PureComponent`将对`this.props.words`的旧值和新值进行简单比较。 由于这个代码在`WordAdder`的`click`方法中改变了单词数组，所以即使数组中的实际单词已经改变，`ListOfWords`组件中的`this.props.words`的旧值和新值还是相等的。因此即便`ListOfWords`具有要被渲染出来的新单词它也还是不更新任何内容。

## 超能力之『不会突然变化的数据』

避免此问题的最简单的方法就是避免将那些可能突然变化的数据作为你的`props`或`state`。例如，上面的`click`方法里面使用`concat`代替`push`：

```javascript
click() {
    this.setState(prevState => ({
        count: prevState.words.concat(['zhangyatao'])
    }));
}
```

ES6支持数组的`spread`语法可以让这变得更容易。如果您使用的是`Create React App`，那么此语法默认可以使用的。

```javascript
click() {
    this.setState(prevState => ({
        words: [...prevState.words, 'zhangyatao']
    }));
}
```

您还可以把那部分有可能突然变化的数据的代码按照上面的方式给重写下，从而以避免这种问题。 

例如，假设我们有一个名为`colormap`的对象，我们要写一个函数，将`colormap.right`改为`'blue'`。 我们可以写：

```javascript
function updateColorMap(colormap) {
    return Object.assign(colormap, {right: 'blue'});
}
```

要将上面的代码写成不会濡染改变的对象，我们可以使用`Object.assign`方法：

```javascript
function updateColorMap(colormap) {
    return Object.assign(colormap, {right: 'blue'});
}
```

`updateColorMap`现在会返回一个新对象，而不是改变之前的旧对象。 `Object.assign`在ES6中，需要`polyfill`。

有一个`JavaScript`提议来添加对象`spread`属性，以便不会突然变化的更新对象：

```javascript
function updateColorMap(colormap) {
    return {...colormap, right: 'blue'};
}
```

如果使用`Create React App`，默认情况下`Object.assign`和对象`spread`语法都可用。

## 使用不突变的数据结构

`Immutable.js`是另一种解决这个问题的方法。它提供不可变的，持久的集合，通过结构共享工作：

- 不可变：一旦创建，集合不能在另一个时间点更改。

- 持久性：可以从先前的集合和类集合的突变中创建处一个新集合。创建新集合后，原始集合仍然有效。

- 结构共享：使用尽可能多的与原始集合相同的结构创建新集合，从而将最低程度的减少复制来提高性能。

# 不使用ES6编写React应用

通常你可以使用一个JavaScript的class功能来定义一个React组件：

```javascript
class Greeting extends React.Component {
    render() {
        return <h1>hello {this.props.name}</h1>;
    }
}
```

要是你还没有使用ES6的话，你就得使用`React.createClass`来创建一个组件了:

```javascript
var Greeting = React.createClass({
    render: function() {
        return <h1>hello {this.props.name}</h1>;
    }
});
```

使用ES6的`class`来创建一个组件有点类似于`React.createClass`，但是会有一些例外。

## 定义PropTypes和Props默认值

对于`功能性组件`和通过ES6的`class`创建的`类组件`，`propTypes`和`defaultProps`都可以定义组件的自身属性：

```javascript
class Greeting extends React.Component {
    // 内部逻辑
}
Greeting.propTypes = {
    name: React.PropTypes.string.isRequired
}
Greeting.defaultProps = {
    name: 'zhangyatao'
}
```

对于`React.createClass`,你需要在传递的对象上定义一个`propTypes`的属性和一个`getDefaultProps()`方法。

```javascript
var Greeting = React.createClass({
    propTypes: {
        name: React.PropTypes.name.isRequired
    }
    getDefaultProps: function() {
        return {name: 'zhangyatao'}
    }
    // 内部逻辑
});
```

## 设置初始化state

在ES6的`class`中，你只需要在构造函数中通过`this.state`设置初始化`state`。

```javascript
class Greeting extends React.Component {
    constructor(props) {
        super(props);
        this.state = {name: 'zhangyatao'};
    }
    // 业务逻辑
}
```

对于`React.createClass`，你需要写一个单独的方法`getInitialState()`来返回初始化的`state`。

```javascript
var Greeting = React.createClass({
    getInitialState: function() {
        return {name: 'zhangyatao'};
    }
    // 内部逻辑
})
```

## 设置初始化state

在ES6的`class`中，你只需要在构造函数中通过`this.state`设置初始化`state`。

```javascript
class Greeting extends React.Component {
    constructor(props) {
        super(props);
        this.state = {name: 'zhangyatao'};
    }
    // 业务逻辑
}
```

对于`React.createClass`，你需要写一个单独的方法`getInitialState()`来返回初始化的`state`。

```javascript
var Greeting = React.createClass({
    getInitialState: function() {
        return {name: 'zhangyatao'};
    }
    // 内部逻辑
})
```

## 自动绑定this

在通过ES6的`class`定义的组件中，内部方法必须与ES6的`class`保持相同的语义。这意味着它们不会自动将`this`绑定到当前实例。 你必须在构造函数中明确使用`.bind(this)`来绑定`this`：

```javascript
class ShowMyName extends React.Component {
    constructor(props) {
        super(props);
        this.name = 'zhangyatao';
        this.showName = this.showName.bind(this);
    }
    showName() {
        console.log('我的名字是', this.name);
    }
    render() {
        return (
            <button onClick={this.showName}>
                打印我的名字
            </button>
        );
    }
}
```

对于`React.createClass`你就不需要给每个内部方法绑定`this`。

```javascript
var ShowName = React.createClass({
    name: 'zhangyatao',
    showName: function() {
        console.log('我的名字是', this.name);
    },
    render: function() {
        return <button onClick={this.showName}>打印我的名字</button>;
    }
});
```

这意味着编写通过ES6的`class`定义的组件，会写一些更多的代码用于处理内部方法（就像一些事件处理函数）中的`this`，但是好在大型的应用中上面的那种写法性能略好。

如果代码这么写的话太不吸引你，你大可以在使用Babel时启用那些处于`实验阶段（stage-2）`的`类属性`语法：

```javascript
class ShowName extends React.Component {
    constructor(props) {
        super(props);
        this.name = 'zhangyatao';
    }
    // 使用箭头函数，因为剪头函数会直接绑定this
    click = () => {
        console.log('我的名字是', this.name)
    }
    render() {
        return <button onClick={this.click}>打印我的名字</button>;
    }
}
```

如果你想安全地摆弄这些东西，你有几个选择：

- 在构造函数中绑定`this`关键字

- 使用箭头函数,就像例子中的`click = () => {console.log('我的名字是', this.name)}`

- 一直使用`React.createClass`

## 混合(mixins)属性

> **note：**<br/>
> ES6对`mixins`没有任何支持。 因此，在使用ES6的`class`编写React组件时，不支持`mixins`。
**我在代码中使用`mixins`时发现了许多问题，因此不建议在新代码中使用。**

有时，一些不同的组件需要共享一些常见的功能。一般称为`横切`。`React.createClass`允许你使用一个`mixins`属性来实现这个需求。

一个比较常见的用例就是你希望通过一定的时间间隔来更新组件自身。 

这很容易使用`setInterval()`来实现，但重要的是当你不再需要它的时候如何取消`setInterval`来节省内存。 

React提供了`生命周期函数`，让你知道具体在什么时候去创建或销毁组件。下面让我们创建一个简单的`mixins`，这些方法提供一个简单的`setInterval()`，当你的组件被销毁时，它会自动清理`setInterval`。

```javascript
var SetIntervalMixin ={
    // 组件将要被装载到DOM中
    componentWillMount: function() {
        this.intervals = [];
    },
    setInterval: function() {
        this.intervals.push(setInterval.apply(null, arguments));
    },
    // 组件将要从DOM中卸载
    componentWillUnmount: function() {
        this.intervals.forEach(clearInterval);
    }
}

var TickTock = React.createClass({
    // 更新mixins
    mixins: [SetIntervalMixin],
    // 获取并设置默认state
    getInitialState: function() {
        return {seconds: 0};
    },
    // 组件已经被加载到DOM中
    componentDidMount: function() {
        this.setInterval(this.tick, 1000);
    },
    tick: function() {
        this.setState(function(prevState) {
            return {seconds: prevState.seconds + 1};
        });
    },
    render: function() {
        return <p>已经运行了{this.state.seconds}秒</p> 
    }
});

ReactDOM.render(
    <TickTock />,
    document.getElementById('root')
)
```

如果一个组件使用多个`mixins`，并且一些`mixins`定义了相同的生命周期函数（即，当组件被销毁时，一些`mixins`想要做一些清理），所有的生命周期函数保证都可以被调用到。 React会列出在mixins中运行的那些在公共mixins上定义的方法，然后在组件上依次调用这些方法。

# 不使用JSX编写React应用

React并不是强制要求使用JSX来编写应用。 当您不想在构建时设置并编译JSX，那么在开发阶段只是单独使用React而不去使用JSX会让应用在构建时变得特别方便。

每个JSX元素只是调用`React.createElement(component，props，... children)`的语法糖。 所以，你用JSX做的任何事也可以使用纯Javascript来完成。

例如，使用JSX编写一个组件：

```javascript
class Hello extends React.Component {
    render() {
        return <h1>hello zhangyatao</h1>;
    }
}
ReactDOM.render(<Hello />, document.getElementById('root'));
```

上面这段代码会被编译为下面这样：

```javascript
class Hello extends React.Component {
    render() {
        return React.createElement('h1', null, 'hello zhangyatao');
    }
}
ReactDOM.render(<Hello />, document.getElementById('root'));
```

如果你很想看到更多的JSX如何转换成JavaScript的例子，你可以试试在线Babel编译器。

组件可以是一个字符串，也可以是继承了`React.Component`的子类，或者是一个没有内部状态的纯函数。

如果您不喜欢一直重复地写`React.createElement`，可以使用一个变量代替它：

```javascript
const makeE = React.createElement;

ReactDOM.render(
    makeE('h1', null, 'hello zhangyatao'),
    document.getElementById('root')
)
```

如果您在使用`React.createElement`时用了这种方式，那么即便不使用JSX来编写React也是很方便的。

# 彻底理解React如何重新处理DOM(Diffing算法)

React提供了一个声明式的API，所以你不必担心每次DOM更新时内部会修改哪些东西。虽然在React中并不是那么明显地告诉你具体如何实现的，不过这也让编写应用变得更加容易。

本文会详细解释在React中的`“diffing”`算法是怎么做的，以便组件更新是可预测的，从而让高性能应用变得足够快。

## 动机

当使用React时，在单个时间点，您可以将`render()`函数看做是在创建React元素树。 在下一个`state`或`props`更新时`render()`函数将返回一个不同的React元素树。 React需要弄清楚如何高效地更新UI去匹配上最新的元素树。

对于将一个树变换成另一个树的最小操作数的算法问题，现在已经存在一些比较通用的解决方案。 然而，那些现有的最先进的技术算法都有`O(n^3)`的复杂度（n是树中的元素的数量）。

如果在React中使用这些算法，显示1000个元素将需要大约十亿次比较。 这个真的代价太昂贵了。 相反，React实现了一个`基于两个假设`直观推断出的O(n)算法：

- 不同类型的两个元素将产生不同的树。

- 开发人员可以在不同渲染之间使用`key`属性来表示哪些子元素是稳定的。

实际上，这两条假设对几乎所有的实际使用都是有效的。

## Diffing算法

当比较两棵DOM树的差异时，React首先比较两个根元素。如果根元素的类型不同，那么行为也是不同的。

### 不同类型的DOM元素

每当根元素是不同的类型时，React将删除旧的DOM树并从头开始重新构建新的DOM树。 从`<a>`到`<img>`、从`<Article>`到`<Comment>`、从`<Button>`到`<div>`，只要不一样就会完全重新构建。

当删除就的DOM树时，旧的DOM节点也被删掉。这个时候组件实例触发`componentWillUnmount()`函数 。当构建一个新的DOM树时，新的DOM节点会被插入到DOM中。组件实例触发`componentWillMoun()`和`componentDidMount()`。 与之前旧的DOM树相关联的任何`state`也都将丢失。

在根元素之下的任何组件将被卸载并且它们的`state`也会全部丢失。 例如：

```javascript
// 从
<div>
    <Counter />
</div>
// 变为
<span>
    <Counter />
</span>
```

因为根元素从`div`变为了`span`，所以旧的`Counter`组件将被销毁，然后再重新构建一个新的。

### 相同类型的DOM元素

当比较相同类型的两个React DOM元素时，React会先查看两者的属性差异，然后保留相同的底层DOM节点，仅仅去更新那些被更改的属性。 例如：

```javascript
<div className="before" title="hello" />

<div className="after" title="hello" />
```

通过比较这两个元素属性，React就会知道只需要修改底层DOM节点上的`className`即可。

当更新`style`属性时，React也会知道只需要更新`style`中的那些已更改的属性。 例如：

```javascript
<div style={{color: 'red', width: '300px'}} />

<div style={{color: 'red', width: '400px'}} />
```

当在这两个元素之间转换时，React知道只需修改`width`，而不是`color`。

**处理根DOM节点后，React会根据上面的判断逻辑对子节点进行递归扫描。**

### 相同类型的组件元素

当组件更新时，实例保持不变，因此在不同的渲染之间组件内的`state`是保持不变的。React会更新底层组件实例的`props`来匹配新元素，并在底层实例上调用`componentWillReceiveProps()`和`componentWillUpdate()`。

接下来，调用`render()`方法，`diff`算法就会对上一个结果和新结果进行递归比较。

### 递归子元素

默认情况下，当对DOM节点的子元素进行递归时，React只是同时迭代两个子元素lists，并在有差异时产生变化。

例如，当在子元素的末尾再添加一个元素时，这两个树之间就会有一个的很好转换效果：

```javascript
<ul>
    <l1>one</li>
    <li>two</li>
</ul>

<ul>
    <li>one</li>
    <li>two</li>
    <li>three</li>
</ul>
```

React将匹配两个`<li>one</li>`树，匹配两个`<li>two</li>`树，然后插入一个`<li>three</li>`树。

但是，不要太天真了。如果在子元素的开头部分插入一个元素的话，性能会便的很差。例如，这两棵树之间的转换效果就很差：

```javascript
<ul>
    <li>one</li>
    <li>two</li>
<ul>

<ul>
    <li>zero</li>
    <li>one</li>
    <li>two</li>
<ul>
```

这种情况React将更改每个子元素，而不会意识到它可以保持`<li>one</li>`和`<li>two</li>`子元素树完好无损。 这种低效率的情况是一个必须注意的问题。

### keys

为了解决上面的问题，React提供了一个`key`属性。当子元素有`key`属性时，React使用`key`将原始树中的子元素与后续树中的子元素进行匹配。例如，上面的那个低效例子添加一个`key`就可以让子元素树转换变的很有效：

```javascript
<ul>
    <li key="1">one</li>
    <li key="2">two</li>
<ul>

<ul>
    <li key="0">zero</li>
    <li key="1">one</li>
    <li key="2">two</li>
<ul>
```

现在React就可以知道`key="0"`的元素是新的，并且`key="1"`和`key="2"`的元素只需移动即可。

在实践中，使用一个唯一的`key`并不难。您要显示的元素可能已具有唯一的`ID`，因此`key`可以来自你自己的数据中：

`<li key={item.id>{item.name}</li>`

如果不是这样，你可以向数据模型中给每一项数据添加一个新的`ID`属性，或者对内容的某些部分进行哈希生成`key`。 `key`属性只有在其兄弟元素之间是唯一的，并不是全局唯一的。

最后一种方式是可以将数组中的索引作为`key`。如果数组中的每一项不需要重新排序，同样也可以很好地工作，但是万一需要重新排序的话，这会变的很慢。

## 权衡利弊

要记住重要的是，`diffing`算法是一个具体的实现细节。React可以在每个操作上去重新渲染应用; 最终结果都是一样的。

在当前的实现中，你可以看到一个事实是一个子树已经成功移动到它的兄弟元素当中，但你不能告诉它已经移动到别的地方。 该算法将重新渲染这个完整的子元素树。

因为React很依赖这个直观推断的算法来判断DOM是否需要重新处理，如果不能满足这个算法的那两个假设条件前提，应用的性能将会受到很大影响。

- 该算法不会去尝试匹配那些不同组件类型的子元素树。如果你看到自己在返回相似输出结果的两个组件类型之间来来回回，你可能需要把它们改为相同的类型组件。

- `key`属性应该是稳定，可预测和唯一的。不稳定的键(如使用`Math.random()`生的`key`)将导致许多组件实例和DOM节点进行不必要地重复创建，这可能导致子组件中的性能降低和`state`丢失。

# 用上下文管理应用

使用React可以很容易通过React组件跟踪数据流。当你看到一个组件，你就可以看到哪些`props`正在传递，这使得你的应用很容易知道在做什么。

在某些情况下，你希望通过组件树传递数据，而不是在每个级别的中组件手动传递`props`，你可以直接在React中使用强大的`“context”`来做到这一点。

## 为什么不去使用Context

绝大多数的应用不需要直接使用Context。

如果你希望你的应用是稳定的，那么就不要使用Context。 因为这是一个实验性质的API，它可能会在未来的React版本中移除。

如果你不熟悉state管理库如`Redux`或`MobX`，不要使用Context。对于许多实际的应用，这些state管理库和React一起使用来管理那些与组件相关的state一个很不错的选择。很多情况下使用`Redux`就可以解决你的问题，而不是使用Context。

如果你不是一个有经验的React开发人员，不要使用Context。使用`props`和`state`来实现功能是一个更好的方法。

尽管有上面这些警告你还坚持使用Context，那么请将Context单独隔离到一个小区域中，并尽可能地避免直接使用Context，以便在这个API更改时应用能更容易升级。

## 如何使用Context

假设你有一个结构：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyButton extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        const style = {backgroundColor: this.props.color};
        return <button style={style}>{this.props.children}</button>;
    }
}
class Message extends React.Component {
    render() {
        return (
            <div>
                {this.props.text}
                <MyButton color={this.props.color}>删除</MyButton>
            </div>
        )
    }
}
class MessageList extends React.Component {
    render() {
        const color = 'red';
        const children = this.props.messages.map(msg => <Message text={msg} color={color}/>);
        return <div>{children}</div>;
    }
}
const messages = ['zhangyato', 'Re: zhangyatao', 'Re:Re:zhangyatao'];
ReactDOM.render(
    <MessageList messages={messages}/>,
    document.getElementById('root')
);
```

在这个例子中，我们手动传入一个`color` props进行传递，以便适当地设置`MyButton`和`Message`组件的样式。使用Context，我们可以让`color`自动在组件树中传递：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyButton extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        const style = {backgroundColor: this.context.color};
        return <button style={style}>{this.props.children}</button>;
    }
}
MyButton.contextTypes = {
    color: React.PropTypes.string.isRequired
};
class Message extends React.Component {
    render() {
        return (
            <div>
                {this.props.text}
                <MyButton>删除</MyButton>
            </div>
        )
    }
}
class MessageList extends React.Component {
    getChildContext() {
        return {color: 'red'};
    }

    render() {
        const children = this.props.messages.map(msg => <Message text={msg}/>);
        return <div>{children}</div>;
    }
}
MessageList.childContextTypes = {
    color: React.PropTypes.string.isRequired
};
const messages = ['zhangyato', 'Re: zhangyatao', 'Re:Re:zhangyatao'];
ReactDOM.render(
    <MessageList messages={messages}/>,
    document.getElementById('root')
);
```

通过向`MessageList`（Context提供者）添加`childContextTypes`和`getChildContext`，React就会自动传递Context信息，子组件树中的任何组件（`Button`）都可以通过定义`contextTypes`来访问它。

如果未定义`contextTypes`，那么Context将是一个空对象。

## 父子组件之间的耦合

Context还可以让你实现父组件和子组件之间的交互。例如，以这种方式工作的一个比较知名的库为`React Router V4`：

```javascript
const RouterExample = () => {
    <Router>
        <div>
            <ul>
                 <li><Link to="/">主页</Link></li>
                 <li><Link to="/recyclebin">回收站</Link></li>
                 <li><Link to="/timeline">图片</Link></li>
            </ul>

            <hr />

            <Match exactly pattern="/" component={Home} />
            <Match pattern="/recyclebin" component={RecycleBin} />
            <Match pattern="/timeline" component={TimeLine} />

        </div>
    </Router>
}
```

通过从`RouterExample`组件传递一些信息，每个`Link`和`Match`可以回传到包含的`Router`中。

在使用类似于此的API进行构建组件之前，请考虑是否有更好的替代品。例如，你可以传递整个React组件作为props。

## 生命周期方法中使用Context

如果在一个组件中定义了`contextTypes`，则以下生命周期方法将多接收个参数，就是Context对象：

- `constructor(props, context)`

- `componentWillReceiveProps(nextProps, nextContext)`

- `shouldComponentUpdate(nextProps, nextState, nextContext)`

- `componentWillUpdate(nextProps, nextState, nextContext)`

- `componentDidUpdate(prevProps, prevState, prevContext)`

## 在无状态功能性组件中使用Context

如果`contextTypes`被定义为无状态功能性组件的属性，无状态功能性组件也能够引用Context。 下面的代码显示了被写成一个无状态的功能组件的`MyButton`组件。

```javascript
function MyButton(props, context) {
     const children = props.children;
     return (
         <button style={{backgroundColor: context.color}}>
             {children}
         </button>
     );
}
MyButton.contextTypes = {
    color: React.PropTypes.string.isRequired
};
```

## 更新Context

**千万不要这么做！！！**

React有一个API来更新Context，但它从根本上破坏了Context，所以你不应该使用它。

当state或props改变时，`getChildContext`函数将被调用。为了更新Context中的数据，使用`this.setState()`触发组件内的state更新。这也将触发一个新的Context，并且Context改变的内容也会被子组件接收到。

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MediaType extends React.Component {
    render() {
        return <div>type is {this.context.type}</div>
    }
}
MediaType.contextTypes = {
    type: React.PropTypes.string
};
class MediaQuery extends React.Component {
    constructor(props) {
        super(props);
        this.state = {type: 'PC端'};
        this.checkMediaQuery = this.checkMediaQuery.bind(this);
    }

    getChildContext() {
        return {type: this.state.type}
    }

    checkMediaQuery() {
        let type = window.matchMedia('<code>zhangyatao</code>(max-width: 760px)').matches ? '移动端' : 'PC端';
        this.setState({type: type});
    }

    componentDidMount() {
        window.addEventListener('resize', this.checkMediaQuery, false);
        this.checkMediaQuery();
    }

    render() {
        return <div>{this.props.children}</div>;
    }
}
MediaQuery.childContextTypes = {
    type: React.PropTypes.string
};
ReactDOM.render(
    <MediaQuery>
        <MediaType />
    </MediaQuery>,
    document.getElementById('root')
);
```

问题是，Context的值通过组件更新来提供，如果中间的父组件在`shouldComponentUpdate()`返回false，那么使用该Context值的后代组件永远不会被更新。使用Context完全不受组件的控制，所以基本上没有办法可靠地更新Context。这篇文章很好的解释了为什么这是一个问题，以及你应该如何摆脱它。

# React顶级API

全局变量`React`是React库的入口。如果你通过一个script标签使用的React，那么它的顶级API都会在全局环境下一个名称为React的变量上。如果你是通过npm使用的ES6，你可以这样写：`import React from 'react'`;。你是通过npm使用的ES5，你可以这样写`var React = require('react');`。

## 总览

### Components

React组件可以让你将UI部分独立出来，成为可重用的部分。从而单独考虑分离出来的每一部分功能。可以通过`React.Component`或者`React.PureComponent`来创建React组件。

- React.Component

- React.PureComponent

如果你不是用ES6的class功能,你可以使用以下代替:

- React.createClass()

### 创建一个React元素

我们推荐使用JSX来定义UI。每个JSX元素都是`React.createElement(component, props, children)`的语法糖。使用`JSX`就意味着你不需要直接调用下面的方法:

- React.createElement()

- React.createFactory()

### 处理React元素

`React`同时还为处理元素提供了一些其他APIs:

- React.cloneElement()

- React.isValidElement()

- React.Children

### 使用PropTypes进行类型检测

你可以使用`React.PropTypes`为一个组件上的props进行类型检测。

- React.PropTypes

- React.PropTypes.array

- React.PropTypes.bool

- React.PropType.func

- React.PropTypes.number

- React.PropTypes.object

- React.PropTypes.string

- React.PropTypes.symbol

- React.PropTypes.node

- React.PropTypes.element

- React.PropTypes.instanceOf()

- React.PropType.oneOf()

- React.PropType.oneOfType()

- React.PropType.arrayOf()

- React.PropType.objectOf()

- React.PropTypes.shape()

- React.PropTypes.any

以上的验查器默认都是可选的。你可以使用`isRequired`来标记一个必填属性。如果用户没有根据指定类型传入props或者压根没有传入props的话则会给出一个错误提示。

## 插件

如果你使用了`react-with-addons.js`,那么React组件可以通过变量`React.addons`使用。

- React.addons

## 使用方法

### React.Component

`React.Component`是所有React组件的基类,当使用ES6 `classes`定义一个组件的用法如下:

```javascript
class Greeting extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>
    }
}
ReactDOM.render(
    <Greeting name={"zhangyatao"}/>,
    document.getElementById('root)
)
```

### React.PureComponet

`React.PureComponent`表面上很像`React.Component`,但是它实现了`shouldComponentUpdate()`对`props`和`state`的进行浅比较。

如果你React组件的`render()`方法每次使用相同的props和state并且渲染出相同的结果。这种情况你可以使用`React.PureComponent`来提升性能。

> 提示<br/>
React.PureComponent的shouldComponentUpdate()仅会对对象进行浅比较,如果对象包含复杂的数据结构,对于深层次的差异有可能会产生false-negatives(假阴性,相当于医院中的错诊)。

### React.createClass()

`React.createClass(specification)`

如果你还没有使用ES6,你可以使用`React.createClass()`来创建一个组件类。

```javascript
var Greeting = React.createClass({
    setInitialState: function() {
        return {value: 0};
    },
    render: function() {
        return <h1>Hello, {this.props.name}</h1>;
    }
})
```

### React.createElement()

```javascript
React.ceateElement(
    type,
    [props],
    [...children]
)
```

通过传入的类型和属性以及子元素来创建并返回一个新的`React元素`。其中type参数可以传入一个html标签名称字符串(例如'div'或者'span')，或者传入一个`React组件`(一个类组件或功能性组件)。

`React.DOM`提供了DOM组件可以比较便捷地通过`React.createElement()`包装的方法。例如，`React.DOM.a(...)`就是`React.createElement('a',...)`的便捷包装。这种方法可以是历史版本遗留产物，所以我们推荐你是用JSX或者使用`React.createElement()`来直接代替。

使用JSX写的代码会被转换为`React.createElement()`。如果你使用了JSX的话，通常不需要直接调用`React.createElement()`。

### React.cloneElement()

```javascript
React.cloneElement(
    element,
    [props],
    [...children]
)
```

传入一个React元素进行克隆并返回一个新的React元素。

### React.createFactory()

`React.createFactory(type)`

返回一个生成给定类型的React元素的函数。就像`React.createElement()`，其中type参数可以传入一个html标签名称字符串(例如'div'或者'span')，或者传入一个`React组件`类型(一个类型组件或功能性组件)。

这种方法可以是一个历史版本遗留产物，我们推荐你是用JSX或者使用`React.createElement()`来直接代替。

使用JSX写的代码会被转换为`React.createElement()`。如果你使用了JSX的话，通常不需要直接调用`React.createElement()`。

### React.isValidElement()

`React.isValidElement(Object)`

验证一个对象是否是React元素，返回`true`或者`false`。

### React.Children

React.children提供了处理`this.props.children`中那些不透明的数据结构的一些工具函数。

React.Children.map

`React.Children.map(children, function[(thisArg))`

### React.Children.forEach

`React.Children.forEach(children, function[(thisArg)])`

和`React.Children.map`相同，只不过不会返回一个数组。

### React.Children.count

`React.Children.count(children)`

返回children中的组件总数。

React.Children.only

`React.Children.only(children)`

然会children中的只出现一次的子元素。否则抛出。

React.Children.toArray

`React.Children.toArray(children)`

将子元素中的不透明数据结构作为一个一维数组返回。如果你想在`render`方法中操作children集合，特别是如果你想在传递它之前重新排序或切割`this.props.children`，这歌方法将非常有用。

### React.PropTypes

`React.PropTypes`是一系列类型验证器的集合，可以与组件的propTypes对象一起使用，以验证传递到组件的props。

`React.PropTypes.array`

验证prop是一个数组类型。

`React.PropTypes.bool`

验证prop是一个布尔值。

`React.PropTypes.func`

验证prop是一个函数。

`React.PropTypes.number`

验证prop是一个数字。

`React.PropTypes.object`

验证prop是一个对象。

`React.PropTypes.string`

验证prop是一个字符串。

`React.PropTypes.symbol`

验证prop是一个symbol。

`React.PropTypes.node`

验证prop是一个可以渲染的东西：数字，字符串，元素 或者包含这些类型的数组（或片段）。

`React.PropTypes.element`

验证prop是一个React元素。

`React.PropTypes.instanceOf(class)`

验证prop是否是class的实例，使用Javascript中的instaceof操作符。

`React.PropTypes.oneOf(arrayOfValues)`

通过将其视为枚举来验证prop是否受限于特定值。

```javascript
MyComponent.propTypes = {
    optionalEnum: React.PropTypes.oneOf(['News', 'Photos']);
}
```

`React.PropTypes.oneOfType()`

验证prop是可以是多种类型之一的对象。

```javascript
MyComponent.propTypes = {
    optionalUnion: React.PropTypes.oneOfType([
        React.PropTypes.string,
        React.PropTypes.number,
        React.PropTypes.instanceOf(Message)
    ]),
}
```

`React.PropTypes.arrayOf(propType)`

```javascript
MyComponent.propTypes = {
    optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),
}
```

验证porp是一个特定类型的数组。

`React.PropTypes.objectOf(propType)`

验证prop是具有某个类型的属性值的对象。

```javascript
MyComponent.propTypes = {
    optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),
}
```

`React.PropTypes.shape(object)`

验证prop是采取特定形状的对象。

```javascript
MyComponent.propTypes = {
    optionalObjectWithShape: React.PropTypes.shape({
        color: React.PropTypes.string,
        fontSize: React.PropTypes.number
    }),
}
```

`React.PropTypes.any`

验证prop具有任何数据类型的值。 通常后面是`isRequired`。

```javascript
MyComponent.propTypes = {
    requiredAny: React.PropTypes.any.isRequired,
}
```

`propTypes.isRequired`

您可以使用isRequired链接上述任何验证器，以确保在未提供prop的情况下显示警告。

```javascript
MyComponent.propTypes = {
    requiredFunc: React.PropTypes.func.isRequired,
}
```

`React.addons`

`React.addons`导出一系列附加组件，只有在使用`react-with-addons.js`时才可用。

# React.Component用法

`组件(Components)`允许您将UI拆分为独立的可重用的部分，并单独的考虑每个部分。

## 总览

`React.Component`是一个抽象基类。这意味着直接引用`React.Component`是毫无意义的。你可以实现一个它的子类，并且至少定义一个`render()`方法即可使用。

你可以使用ES6中`class`定义一个React组件：

```javascript
class Greeting extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</hr>;
    }
}
```

如果你还没有使用ES6，你可以使用`React.createClass()`。

## 组件的生命周期方法

每个组件都有几个『生命周期方法』，您可以重写这些方法，已便在React执行过程中的指定时间运行自己的代码。前缀为`Will`的生命周期方法会在在一些事情发生之前被调用，带有`Did`前缀的方法在某些事情发生之后被调用。

**Mounting(加载组件)**

当创建组件的实例并将其插入DOM时，会依次调用这些方法：

- constructor()

- componentWillMount()

- render()

- componentDidMount()

**Updating(更新状态)**

更新可以由prop或者state的改变引起。在重新渲染组件时依次调用这些方法：

- componentWillReceiveProps()

- shouldComponentUpdate()

- componentWillUpdate()

- render()

- componentDidUpdate()

**Unmounting(卸载组件)**

当从DOM中删除组件时，将调用此方法：

- componentWillUnmount()

## 其他API

每个组件还提供了一些其他API：

- setState()

- forceUpdate()

组件属性

- defaultProps

- displayName

- propTypes

实例内部属性

- props

- state

## 使用方法

### render()

```javascript
render() {
    // return React Elements
}
```

注意：`render()`方法是必须写的。

当这个方法被调用时，它会检测`this.props`和`this.state`并返回一个React元素。此元素可以是本地DOM组件的形式，例如`<div/>`，也可以是您自己定义的一个复合组件。

你也可以返回`null`或`false`来表示你不想做任何渲染操作。当返回`null`或`false`时，`ReactDOM.findDOMNode(this)`将返回`null`。

`render()`方法应该是纯的`(pure function`，见函数式编程)，这意味着它并不会修改组件state，每次调用它时都会返回相同的结果，它不会直接与浏览器交互。如果您需要与浏览器直接交互，请改用`componentDidMount()`方法或者其他生命周期方法来执行你的逻辑。保持`render()`的纯可以让组件更容易去思考自己应该做什么。

> 提示
如果`shouldComponentUpdate()`返回`false`，那么`render()`不会被执行。

### constructor()

`constructor(props)`

在装载组件(mounting)之前调用会React组件的构造函数。当实现`React.Component`子类的构造函数时，应该在任何其他语句之前调用`super(props)`。否则，`this.props`将在构造函数中未定义，这可能导致错误。

构造函数是初始化state的标准位置。如果不初始化state，并且不绑定组件内部方法的this指向，则不需要为React组件实现构造函数。

如果你知道你在做什么的话，你可以根据props来初始化state。这里有一个有效的`React.Component`子类构造函数的例子：

```javascript
constructor(props) {
    super(props);
    this.state = {
        color: props.initialColor
    };
}
```

注意这种模式，因为它会将props复制一份在state中，这就可能导致一个意外的bug。所以不应该将props复制到state中。相反，你需要使用`提升state`的技巧，该技巧我们在前面的文章提到过。

如果你使用props复制到state中，你还需要实现`componentWillReceiveProps(nextProps)`来保持state是最新的。这个时候使用提升state的方法反而会更容易，也能产生更少的bug。

### componentWillMount()

`componentWillMount()`

`componentWillMount()`是在装载(mounting)发生之前被调用。它在`render()`之前调用，所以在此方法中的设置state不会造成重新渲染。另外，应该避免在此方法中引入有任何`副作用的东西(见函数式编程)`。

在服务器渲染上这是唯一一个调用的生命周期钩子函数。一般来说，我们建议使用`constructor()`。

### componentDidMount()

`componentDidMount()`

`componentDidMount()`在组件装载到DOM后立即调用。如果需要进行DOM节点的初始化则应该在这里来执行该逻辑。如果需要从远程端点加载数据(`ajax`)，那么这是处理网络请求的最好地方。在此方法中设置state会去重新渲染DOM。

### componentWillReceiveProps()

`componentWillReceiveProps(nextProps)`

`componentWillReceiveProps()`在安装好的组件接收新props之前被调用。如果你需要更新state用来响应props的更改（例如，重置它），你可以在此方法中比较`this.props`和`nextProps`并使用this.setState()来替换并重置state。

注意，即使props没有改变，`React`也可以调用这个方法，因此如果你只想处理props改变的情况，请确保比较当前值和下一个值是否不同。当父组件引起你的组件重新渲染时，就有可能会发生这种情况。

如果你只是调用`this.setState()`，那么`componentWillReceiveProps()`不会被调用。

### shouldComponentUpdate()

`shouldComponentUpdate(nextProps, netState)`

使用`shouldComponentUpdate()`让React知道组件是否受当前state或props变化的影响。 默认行为是在每次state更改时都会去重新渲染DOM，在绝大多数情况下，你应该依赖于这个默认行为。

当接收到新的props或state时，`shouldComponentUpdate()`在渲染之前被调用。 默认为`true`对于初始渲染或使用`forceUpdate()`，不调用此方法。

返回`false`不会阻止子组件在state更改时重新渲染。

目前，如果`shouldComponentUpdate()`返回`false`，那么将不会调用`componentWillUpdate()`，`render()`和`componentDidUpdate()`。 注意，在将来React可以将`shouldComponentUpdate()`作为提示而不是严格的操作指令，返回false仍然可能导致组件的重新渲染。

如果你确定某些组件在某些操作时有点缓慢，你可以让它继承`React.PureComponent`，而不是继承`React.Component`。

`React.PureComponent`实现了`props`和`state`进行浅比较的`shouldComponentUpdate()`方法。 如果你确定想人肉处理这个浅比较操作，你可以自己在这个函数中比较`this.props和nextProps`、`this.state`和`nextState`是否相同。相同返回false，不同返回true，那么React就会根据返回值来确认是否跳过本次DOM渲染。

### componentWilUpdate()

`componentWillUpdate(nextProps, nextState)`

当组件在收到新的props或state时，`componentWillUpdate()`在渲染之前会立即调用这个方法。 使用这个方法来判断是非需要重新渲染DOM。 第一次渲染DOM不调用此方法。

注意，`this.setState()`不会调用此方法。 如果你需要根据state和props来进行重新渲染DOM，请改用`componentWillReceiveProps()`。

### componentDidUpdate()

`componentDidUpdate(prevProps, prevState)`

`componentDidUpdate()`在重新渲染DOM之后被调用。 第一次渲染不调用此方法。

当组件已经重新渲染后，此方法是一个执行DOM操作的好机会，同时也是一个处理网络请求的好地方，前提是你需要比较当前props与之前的props是否相同（例如，如果props没有改变，那么可能不需要进行网络请求）。

> **note**<br/>
如果shouldComponentUpdate()返回false，则不会调用componentDidUpdate()。

### componentWillUnmount()

`componentWillUnmount()`

`componentWillUnmount()`在组件被卸载和销毁之前立即被调用。此方法可以执行任何有必要的清理工作，例如清理计时器，取消网络请求或清理在`componentDidMount()`中创建的DOM元素。

### setState()

`setState(nextState, callback)`

将nextState和当前state进行浅合并。这是用于从事件处理函数和服务器请求回调中触发UI重新渲染的主要方法。

第一个参数可以是一个对象（包含一个或多个要更新的state属性），也可以是返回将要引起重新渲染的对象（state和props）。

这里有一个简单的用法：

`this.setState({myKey: 'my new value'});`

它也可以传入一个带有参数的函数`function（state，props）=> newState`。例如，假设我们想要当前state中的myInteger加上`props.step`：

```javascript
this.setState(prevState, props) => {
    return {myInteger: prevState.myInteger + props.step};
}
```

第二种参数是回调函数，一旦`setState`完成并且组件被重新渲染，它就会被执行。 通常我们建议使用`componentDidUpdate()`代替这样的逻辑。

`setState()`不会立即改变`this.state`，但会创建并挂起state的修改。所以在调用此方法中访问`this.state`可能会返回现有值。

不能保证对`setState`的所以调用都是同步操作，因为这样做是为了将多次state修改合并为一次以便提高性能。

`setState()`总会重新渲染DOM，除非`shouldComponentUpdate()`回false。 如果你正在使用可突变对象，并且无法在`shouldComponentUpdate()`实现条件渲染逻辑，则只有当新state与先前state不同时调用`setState()`才能避免不必要的重新渲染。

### forceUpdate()

`component.forceUpdate(callback)`

默认情况下，当组件的state或props改变时，组件将重新渲染。 如果你的render()方法依赖于一些其他数据，你可以告诉React组件需要通过调用`forceUpdate()`来重新渲染。

调用forceUpdate()会导致在组件上调用render()，跳过`shouldComponentUpdate()` 这将触发子组件的正常生命周期方法，包括每个子组件的`shouldComponentUpdate()`方法。 如果标记更改，React仍将更新DOM。

通常你应该尽量避免`forceUpdate()`的所有使用，并且只能从`render()`中的`this.props`和`this.state`中读取。

## 类属性

### defaultProps

`defaultProps`是类组件本身的属性，用来设置类组件的默认props。可以用来给未传入值的props设置默认值。 例如：

```javascript
class CustomButton extends React.Component {
    // ...
}
CustomButton.defaultProps = {
    color: 'blue';
}
```

如果`props.color`没有定义，就是将它设置为默认值`blue`：

```javascript
render() {
    return <CustomButton />; // props.color will be set to blue
}
```

如果`props.color`被设置为`null`，那么它将会被重新赋值为`null`：

```javascript
render() {
    return <CustomButton color={null} />; // props.color will remain null
}
```

### displayName

`displayName`字符串用于调试消息。 JSX自动设置此值;

### propTypes

`propTypes`也是类组件本身上的一个属性，用来规范props应该是什么类型。它是从props的名称到`React.PropTypes`中定义的类型的映射。在开发模式下，当为prop设置一个不是指定格式的无效值时，会在JavaScript控制台中显示警告信息。在生产模式下，为了提高效率，不会进行propTypes检查。

例如，此代码确保颜色prop是一个字符串：

```javascript
class CustomButton extends React.Component {
    // ...
}
CustomButton.propTypes = {
   color: React.PropTypes.string
};
```

我们建议尽可能使用`Flow`，以便在编译时进行类型检查，而不是在运行时进行类型检查。Flow在React中内置支持，因此可以轻松地在React应用程序上运行静态分析。

```javascript
// @flow
function foo(x) {
  return x * 10;
}
foo('Hello, world!');

// @flow
function bar(x): string {
  return x.length;
}
bar('Hello, world!');
```

## 实例属性

### props

`this.props`包含由此组件的调用者定义的props。

特别地，`this.props.children`是一个特殊的props，通常由JSX表达式中的子标签而不是标签本身定义。

### state

`state`包含特定于此组件的数据，可能随时间更改。`state`是用户定义的，它应该是纯JavaScript对象。

如果你不在render()中使用它，它不应该设置state。 例如，您可以将定时器ID直接放在实例上。

永远不要直接改变`this.state`，因为调用`setState()`之后可以替换你所做的各种变化，通常应该把`this.state`看作是不可变的。

# ReactDOM的用法

如果你使用script标签引用React，那么这些顶级API在全局变量ReactDOM上可直接使用。如果你使用了ES6和npm，你可以这么写`import ReactDOM from 'react-dom'`。如果你使用ES5和npm，你可以这写`var ReactDOM = require('react-dom')`。

## 总览

`react-dom`包提供了DOM特定的方法，如果你需要，可以在你的应用中使用并作为一个交互获取外面的React数据。 大多数你的组件不需要使用此模块。

- render()

- unmountComponentAtNode()

- findDOMNode()

## 浏览器支持

React支持所有流行的浏览器，包括Internet Explorer 9及更高版本。

> **note**<br/>
React无法在那些不支持ES5方法的旧版浏览器上运行，但如果页面中包括诸如es5-shim和es5-sham之类的polyfills，您就会发现您的应用在旧版浏览器中可以正常工作。当然，你对你自己选择的路由完全的选择权，我们也管不了你。。。

## 使用方法

### render()

```javascript
ReactDOM.render(
    element,
    container,
    [callback]
)
```

将React元素呈现到提供的`container`中的DOM中，并返回对组件的引用（或对无状态组件返回null）。

如果React元素之前已经被渲染到容器中，这么做会对它执行重新渲染操作，并且只需要根据需要修改DOM以返回最新的React元素。

如果提供了可选的回调，它将在组件渲染或重新渲染后执行。

> **note**<br/>
ReactDOM.render()用来控制传入的container节点中的内容。当第一次调用时，内部的任何现有DOM元素都将被替换。 后面的操纵使用React的DOM diffing算法来进行有效的更新。
ReactDOM.render()会修改container的内部DOM节点（只修改容器的子节点）。也可以将组件插入现有DOM节点中，而不覆盖现有子项。

### unmountComponentAtNode()

`ReactDOM.unmountComponentAtNode(container)`

从DOM中删除已安装(mounted)React组件，并清除其event handle和state。 如果在container中没有mounted组件，调用此函数什么也不做。 如果组件被unmounted，则返回true，如果没有要卸载的组件，则返回false。

### findDOMNode()

`ReactDOM.findDOMNode(component)`

如果此组件已装载到DOM中，则返回相应的html DOM元素。 此方法对于从DOM中读取值（例如表单字段值和执行DOM测量）很有用。当`render`返回`null`或`false`时，`findDOMNode`返回`null`。

**在大多数情况下，你可以使用refs处理DOM节点，并避免使用findDOMNode。**

> **note**<br/>
`findDOMNode`是一个用于访问底层DOM节点的接口。 在大多数情况下，不建议使用此就接口。
`findDOMNode`仅适用于已安装的组件（即已放置在DOM中的组件）。如果你试图在一个尚未安装的组件上调用它（就像在一个尚未创建的组件`render()`方法里调用`findDOMNode()`），将抛出一个异常。`findDOMNode`不能用于功能组件。

# DOM Elements

React实现了一个独立于浏览器的DOM系统，用于提高性能和处理浏览器兼容性。React作者借此机会在浏览器DOM实现中清理了一些粗糙的实现（恶心的原生DOM操作）。

在React中，所有`DOM properties`和`attributes`（包括`event handle`）都应该是`驼峰命名法`。 例如，HTML属性`tabindex`对应于React中的`tabIndex`属性。 特殊的是`aria- *`和`data- *`属性，应该是全部小写的。

## Attributes的区别

有许多属性在React和HTML之间有不同的表现：

### checked

checked属性在`<input type="checkbox">`或`<input type="radio">`中使用。您可以使用它来设置是否选中该组件。 这对可控组件很有用。`defaultChecked`是默认选中，它在首次装入组件时是默认选中的。

### className

因为`class`是Javascript的关键字，所以在指定CSS类时，使用`className`属性。这适用于所有常规DOM和SVG元素，就像`<div>`，`<a>`或者其他标签。
如果使用React 和 `Web Components`（这是不常见的），请把`ClassName`改为`class`属性。

### dangerouslySetInnerHTML

`dangerouslySetInnerHTML`是`React`替换在浏览器DOM中使用`innerHTML`。 
一般来说，从代码设置HTML是有风险的，因为很容易无意中将用户暴露于`跨站点脚本（XSS）`攻击。 所以，你可以直接从React设置HTML，但是你必须输入`dangerouslySetInnerHTML`并使用`__html`键传递一个对象，提醒自己这是危险的。 例如：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function createMarkup() {
    return {__html: 'First <<>> Second'};
}
function MyComponent() {
    return <div dangerouslySetInnerHTML={createMarkup()} />;
}
ReactDOM.render(
    <MyComponent />,
    document.getElementById('root')
);
```

### htmlFor

由于`for`是JavaScript中的保留字，React元素使用`htmlFor`。

### onChanged

`onChange`事件的行为与您期望的一样：每当表单字段更改时，将触发此事件。我们故意不使用现有的浏览器行为，因为`onChange`是其行为的误称，React依赖此事件来实时处理用户输入。

### selected

`selected`属性在`<option>`中使用。您可以使用它来设置是否选择了组件。这对控制类的组件很有用。

### style

`style`属性接受一个驼峰命名法的JavaScript对象，而不是CSS字符串。这与DOM元素style的JavaScript属性一致，更高效，并防止XSS安全漏洞。 例如：

```javascript
const divStyle = {
    color: 'blue',
    backgroundImage: 'url' + imgUrl + ')',
};
function HelloWorldComponent() {
    return <div style={divStyle}>HelloWorld</div>;
}
```

请注意，样式不自动设置css的前缀。要支持旧版本的浏览器，您需要提供相应的样式属性：

```javascript
const divStyle = {
    WebkitTransition: 'all',
    msTransition: 'all',
};
function ComponentWithTransition() {
   return <div style={divStyle}>This should work cross-browser</div>;
}
```

style的key是驼峰命名法，以便与从JS访问DOM节点上的属性（例如`node.style.backgroundImage`）一致。 ms以外的浏览器前缀应以大写字母开头。这就是为什么`WebkitTransition`有一个大写“W”。

### value

`<input>`和`<textarea>`支持value属性。您可以使用它来设置元素的值。这对控制类组件很有用。`defaultValue`是默认值，在组件首次装入时将会设置这个默认值。