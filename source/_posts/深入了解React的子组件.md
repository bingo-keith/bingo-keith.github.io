---

title: 深入了解React的子组件
date: 2018/1/7 14:16:12  
tags: react 组件 模块 javascript 框架 库

---

React的核心是组件。你可以像嵌套HTML标签一样来嵌套这些组件，这使得写JSX变得容易，因为他和标记语言很像。 当刚开始学习React的时候，我认为“会用`props.children`，我就知道了关于子组件的一切”。难道，我有错吗？ 因为我们在使用JavaScript，我们可以改变子组件。我们可以向他们发送特定的属性，决定我们是否希望它们显示，并且可以按照我们的意愿操作它们。让我们探究一下React中的子组件。

<!-- more -->

假设我们有一个可以包含若干组件的组件。你会像这样使用它：

```
<Grid>
  <Row />
  <Row />
  <Row />
</Grid>
```

![](https://i.imgur.com/skCYXj1.png)

通过`props.children`，`Grid`组件可以获得这三个`Row`组件。父组件通过一个表现容器来渲染这些子组件：

```
class Grid extends React.Component {
  render() {
    return <div>{this.props.children}</div>
  }
}
```

父组件也可以决定不渲染任何子组件或者在渲染他们之前操控它们。比如说，下面这个组件就不渲染任何子组件：

```

class Fullstop extends React.Component {
  render() {
    return <h1>Hello world!</h1>
  }
}

```

不管你传递哪个组件给上面这个组件，它总是会只显示“Hello world！”。

> 注意： 上面例子中的`Fullstop`组件（非常像HTML的原语），在渲染‘Hello World!’的情况下，不会渲染它的子组件。

一切都可以是子组件

在React中，子组件没有必要必须是一个组件，他们可以是任意内容。比方说，我们可以给上面的`Grid`组件传递文本作为子组件且结果很完美。

![](https://i.imgur.com/K4EdUB3.png)

JSX会自动清除每行开始和结束处的空格以及空白的行。它还将字符串文字中间的空行浓缩成一个空格。

这意味着下面这些示例都会渲染成相同的内容：

```
<Grid>Hello world!</Grid>

<Grid>
  Hello world!
</Grid>

<Grid>
  Hello
  world!
</Grid>

<Grid>

  Hello world!
</Grid>
```

你还可以很好地混合和搭配多种类型的子组件：

```
<Grid>
  Here is a row:
  <Row />
  Here is another row:
  <Row />
</Grid>
```

![](https://i.imgur.com/0AIrxKD.png)

# 函数子组件

我们可以传递任何JavaScript表达式作为子组件，当然包括函数。

为了说明它看起来是什么样子，下面的组件渲染的就是一个传递给它的函数子组件：

```
class Executioner extends React.Component {
  render() {
    // 请看我们如何调用一个函数子组件?
    //                        ↓
    return this.props.children()
  }
}
```

你会像这样使用这个组件：

```
<Executioner>
  {() => <h1>Hello World!</h1>}
</Executioner>
```

这个具体的例子当然没有什么用，但它说明了这个想法。

假设必须从服务器获取一些数据。你可能有多种方式做到这一点，但是通过下面的函数子组件也可以：

```
<Fetch url="api.myself.com">
  {(result) => <p>{result}</p>}
</Fetch>
```

# 操控子组件

如果你看过React的文档，你会发现它有说“子组件是一种不透明的数据结构”。他们本质上，是想告诉我们props.children可以是任意类型，比如数组、函数、对象等等，因为你可以传递任何内容，所以你永远也不能确定它。

React提供了一系列帮助函数，使操作子组件变得轻松和惬意。它们都在React.Children上适用。

# 遍历子组件

最明显的两个帮助类函数是`React.Children.map` 和 `React.Children.forEach`。它们和数组中的map及foreach工作完全相同，除了一点：当子组件是函数、对象或者任何内容时，它们仍能正常工作。

```
class IgnoreFirstChild extends React.Component {
  render() {
    const children = this.props.children
    return (
      <div>
        {React.Children.map(children, (child, i) => {
          // 忽略第一个子组件
          if (i < 1) return
          return child
        })}
      </div>
    )
  }
}
```

这里的`IgnoreFirstChild`组件遍历它所有的子组件，忽略掉第一个子组件，渲染剩余子组件。

```
<IgnoreFirstChild>
  <h1>First</h1>
  <h1>Second</h1> // <- 只有这个被渲染
</IgnoreFirstChild>
```

![](https://i.imgur.com/0iVe7O6.png)

在这种情况下，我们也可以使用`this.props.children.map`。但是，如果有人传进来一个函数子组件会发生什么？`this.props.children`将会是一个函数而不是数组，我们会看到一个报错！ 😱

![](https://i.imgur.com/ovt61Qu.png)

然而使用`React.Children.map`函数，就不会发生这种问题：

```
<IgnoreFirstChild>
  {() => <h1>First</h1>} // <- 忽略掉 💪
</IgnoreFirstChild>
```

# 统计子组件

由于`this.props.children`可以是任意类型，统计一个组件有多少子组件是一件很困难的事情。当传递的子组件是一个字符串或者函数时，使用`this.props.children.length`就会失效。比方说，我们有一个子组件，`"Hello World!"`，但是`.length`将会是12。

这就是为什么我们会有`React.Children.count`:

```
class ChildrenCounter extends React.Component {
  render() {
    return <p>React.Children.count(this.props.children)</p>
  }
}
```

它会返回子组件的数量，不管子组件的类型是什么：

```
// Renders "1"
<ChildrenCounter>
  Second!
</ChildrenCounter>

// Renders "2"
<ChildrenCounter>
  <p>First</p>
  <ChildComponent />
</ChildrenCounter>

// Renders "3"
<ChildrenCounter>
  {() => <h1>First!</h1>}
  Second!
  <p>Third!</p>
</ChildrenCounter>
```

# 转换子组件为数组

作为最后的手段，如果上面的方法都不满足你的的需求，你可以使用`React.Children.toArray`将你的子组件转换为一个数组。这将会很有用，如果你需要比方说将它们排序：

```
class Sort extends React.Component {
  render() {
    const children = React.Children.toArray(this.props.children)
    // 排序并渲染子组件
    return <p>{children.sort().join(' ')}</p>
  }
}
```

```
<Sort>
  // 我们使用花括号来确保我们的字符串是作为三个子组件传入，
  // 而不是一个。
  {'bananas'}{'oranges'}{'apples'}
</Sort>
```

上面的例子渲染出字符串，但是是排序后的：

![](https://i.imgur.com/CvXLz2D.png)

# 限制单一子组件

回想我们上面的`Executioner`组件，它只希望传递过来一个子组件，并且还必须是一个函数子组件。

```
class Executioner extends React.Component {
  render() {
    return this.props.children()
  }
}
```

我们可以用`proptypes`来控制，它看起来会像这样：

```
Executioner.propTypes = {
  children: React.PropTypes.func.isRequired,
}
```

这将把信息记录到控制台，然而有些开发者可能会忽略掉这里的报警信息。因此，取而代之，我们可以在`render`方法中使用`React.Children.only`。

```
class Executioner extends React.Component {
  render() {
    return React.Children.only(this.props.children)()
  }
}
```

这将返回`this.props.children`中唯一的子组件。如果多余一个子组件，它就会抛出一个错误，因此，可以降低我们应用变得不完美的风险，可以避免懒惰的开发者试图扰乱我们的组件。😎

# 编辑子组件

我们不仅可以通过子组件的方式，渲染任意组件，而且还可以通过父组件来控制它们，而不是在子组件内部。为了说明这点，假设我们有一个`RadioGroup`组件，这个组件中包含多个`RadioButton`组件（渲染一个单选框）。

这些`RadioButton`组件并不是在`RadioGroup`组件中渲染的，它们是作为子组件被渲染的。这意味着在我们应用的某个地方有这个组件的代码：

```
render() {
  return(
    <RadioGroup>
      <RadioButton value="first">First</RadioButton>
      <RadioButton value="second">Second</RadioButton>
      <RadioButton value="third">Third</RadioButton>
    </RadioGroup>
  )
}
```

但是这里的代码有一个问题。这些`input`组件并没有被组合起来，这将会导致下面的结果：

![](https://i.imgur.com/gwRdNAx.png)

要将`input`标签组合在一起，它们都需要具有相同的`name`属性。我们当然可以为每一个`RadioButton`组件指定一个`name`属性。

```
<RadioGroup>
  <RadioButton name="g1" value="first">First</RadioButton>
  <RadioButton name="g1" value="second">Second</RadioButton>
  <RadioButton name="g1" value="third">Third</RadioButton>
</RadioGroup>
```

但那是乏味和容易出错的。我们掌握了JavaScript的全部力量！我们能否使用它来告诉我们的`RadioGroup`组件，我们想要添加在所有子组件上的`name`属性，并让它自动添加呢？

改变子组件的属性
在`RadioGroup`组件中，我们将会增加一个新的叫`renderChildren`的方法，用这个方法，我们可以修改子组件的属性：

```
class RadioGroup extends React.Component {
  constructor() {
    super()
    // 将方法绑定到组件上下文
    this.renderChildren = this.renderChildren.bind(this)
  }

  renderChildren() {
    // TODO: 修改所有子组件的name属性为this.props.name
    return this.props.children
  }

  render() {
    return (
      <div className="group">
        {this.renderChildren()}
      </div>
    )
  }
}
```

让我们以遍历所有的子组件，来获取到每一个单独的子组件开始：

```
renderChildren() {
  return React.Children.map(this.props.children, child => {
    // TODO: 改变name属性为this.props.name
    return child
  })
}
```

然而我们改如何编辑它们的属性呢？

# 克隆元素

这就是今天最后一个帮助方法发挥作用的地方了。顾名思义，`React.cloneElement`会克隆一个元素。我们将要克隆的元素作为第一个参数传递给它，然后第二个参数，我们可以传递一个对象，这个对象中包含的就是我们希望在克隆元素上出现的属性。

```
const cloned = React.cloneElement(element, {
  new: 'yes!'
})
```

这个`cloned`元素现在就有了一个值为"yes!"的new属性。

这正是我们完成`RadioGroup`组件所需要的。我们克隆每一个子组件，并且把克隆子组件的`name`属性设置为`this.props.name`:

```
renderChildren() {
  return React.Children.map(this.props.children, child => {
    return React.cloneElement(child, {
      name: this.props.name
    })
  })
}
```

最后一步是为`RadioGroup`组件设置一个唯一的`name`属性：

```
<RadioGroup name="g1">
  <RadioButton value="first">First</RadioButton>
  <RadioButton value="second">Second</RadioButton>
  <RadioButton value="third">Third</RadioButton>
</RadioGroup>
```

![](https://i.imgur.com/AwAip0N.png)

成功了！🎉 取代手动地去设置每一个`RadioButton`组件的`name`属性，我们只需要告诉`RadioGroup`组件我们想要的`name`属性值，它就会自动地去做好了。

# 总结

子组件使React组件看起来就像一系列的标记，而不是分割的个体。使用JavaScript和React的帮助类函数，我们可以创建声明类的API，让我们的开发更轻松。