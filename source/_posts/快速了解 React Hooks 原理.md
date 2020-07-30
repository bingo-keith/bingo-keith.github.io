---

title: 快速了解 React Hooks 原理
date: 2020/7/30 16:47:29 
tags: JavaScript React Hooks Hook 新技术 API

---

**为了保证的可读性，本文采用意译而非直译。**

我们大部分 React 类组件可以保存状态，而函数组件不能？ 并且类组件具有生命周期，而函数组件却不能？

React 早期版本，类组件可以通过继承`PureComponent`来优化一些不必要的渲染，相对于函数组件，React 官网没有提供对应的方法来缓存函数组件以减少一些不必要的渲染，直接 16.6 出来的 `React.memo`函数。

**React 16.8** 新出来的`Hook`可以让React 函数组件具有状态，并提供类似 `componentDidMount`和`componentDidUpdate`等生命周期方法

<!-- more -->

## 类被会替代吗？

`Hooks`不会替换类，它们只是一个你可以使用的新工具。React 团队表示他们没有计划在React中弃用类，所以如果你想继续使用它们，可以继续用。

我能体会那种总有新东西要学的感觉有多痛苦，不会就感觉咱们总是落后一样。`Hooks` 可以当作一个很好的新特性来使用。当然没有必要用 Hook 来重构原来的代码, React团队也建议不要这样做。

## Go Go

来看看**Hooks**的例子,咱们先从最熟悉的开始:函数组件。

以下 OneTimeButton 是函数组件，所做的事情就是当我们点击的时候调用 `sayHi` 方法。

​                        

```
import React from 'react';
import { render } from 'react-dom';

function OneTimeButton(props) {
  return (
    <button onClick={props.onClick}>
        点我点我
    </button>
  )
}

function sayHi() {
  console.log('yo')
}

render(
  <OneTimeButton onClick={sayHi}/>,
  document.querySelector('#root')
)
```

我们想让这个组件做的是，跟踪它是否被点击，如果被点击了，禁用按钮，就像一次性开关一样。

但它需要一个state，因为是一个函数，它不可能有状态(React 16.8之前)，所以需要重构成类。

函数组件转换为类组件的过程中大概有5个阶段：

*否认：也许它不需要是一个类，我们可以把 state 放到其它地方。

- 实现： 废话，必须把它变成一个`class`，不是吗？
- 接受：好吧，我会改的。
- 努力加班重写：首先 写 `class Thing extends React.Component`，然后 实现 `render`等等 。
- 最后：添加state。

​                        

```
class OneTimeButton extends React.Component {
  state = {
    clicked: false
  }

  handleClick = () => {
    this.props.onClick();

    // Ok, no more clicking.
    this.setState({ clicked: true });
  }

  render() {
    return (
      <button
        onClick={this.handleClick}
        disabled={this.state.clicked}
      >
        You Can Only Click Me Once
      </button>
    );
  }
}
```

这是相当多的代码，组件的结构也发生了很大的变化， 我们需要多个小的功能，就需要改写很多。

## 使用　Hook　轻松添加　State

接下来，使用新的　`useState`　hook向普通函数组件添加状态：

​                        

```
import React, { useState } from 'react'

function OneTimeButton(props) {
  const [clicked, setClicked] = useState(false)
  
  function doClick() {
    props.onClick();
    setClicked(true)
  }

  return (
    <button
      onClick={clicked ? undefined : doClick}
      disabled={clicked}
    >
      点我点我
    </button>
  )
}
```

## 这段代码是如何工作的

这段代码的大部分看起来像我们一分钟前写的普通函数组件，除了`useState`。

`useState`是一个hook。 它的名字以**“use”**开头（这是[Hooks的规则](https://daveceddia.com/intro-to-hooks/#rules-of-hooks)之一 - 它们的名字必须以“use”开头）。

`useState` hook 的参数是 state 的初始值，返回一个包含两个元素的数组:当前state和一个用于更改state 的函数。

类组件有一个大的state对象，一个函数`this.setState`一次改变整个state对象。

函数组件根本没有状态，但`useState` hook允许我们在需要时添加很小的状态块。 因此，如果只需要一个布尔值，我们就可以创建一些状态来保存它。

由于`Hook`以某种特殊方式创建这些状态，并且在函数组件内也没有像`setState`函数来更改状态，因此 Hook 需要一个函数来更新每个状态。 所以 `useState` 返回是一对对应关系：一个值，一个更新该值函数。 当然，值可以是任何东西 - 任何JS类型 - 数字，布尔值，对象，数组等。

现在，你应该有很多疑问，如：

- 当组件重新渲染时，每次都不会重新创建新的状态吗？ React如何知道旧状态是什么？
- 为什么hook 名称必须以**“use”**开头？ 这看起来很可疑。
- 如果这是一个命名规则，那是否意味着我可以自定义 Hook。
- 如何存储更复杂的状态，很多场景不单单只有一个状态值这么简单。

## Hooks 的魔力

将有状态信息存储在看似无状态的函数组件中，这是一个奇怪的悖论。这是第一个关于钩子的问题，咱们必须弄清楚它们是如何工作的。

原作者得的第一个猜测是某种编译器的在背后操众。搜索代码`useWhatever`并以某种方式用有状态逻辑替换它。

然后再听说了调用顺序规则(它们每次必须以相同的顺序调用)，这让我更加困惑。这就是它的工作原理。

React第一次渲染函数组件时，它同时会创建一个对象与之共存，该对象是该组件实例的定制对象，而不是全局对象。只要组件存在于DOM中，这个组件的对象就会一直存在。

使用该对象，`React`可以跟踪属于组件的各种元数据位。

请记住，React组件甚至函数组件都从未进行过自渲染。它们不直接返回`HTML`。组件依赖于`React`在适当的时候调用它们，它们返回的对象结构`React`可以转换为`DOM`节点。

React有能力在调用每个组件之前做一些设置，这就是它设置这个**状态**的时候。

其中做的一件事设置 **Hooks** 数组。 它开始是空的, 每次调用一个`hook`时，React 都会向该数组添加该 `hook`。

## 为什么顺序很重要

假设咱们有以下这个组件：

​                        

```
function AudioPlayer() {
  const [volume, setVolume] = useState(80);
  const [position, setPosition] = useState(0);
  const [isPlaying, setPlaying] = useState(false);

  .....
}
```

因为它调用`useState` 3次，React 会在第一次渲染时将这三个 hook 放入 Hooks 数组中。

下次渲染时，同样的`3`个`hooks`以相同的顺序被调用，所以`React`可以查看它的数组，并发现已经在位置`0`有一个`useState` hook ，所以`React`不会创建一个新状态，而是返回现有状态。

这就是React能够在多个函数调用中创建和维护状态的方式，即使变量本身每次都超出作用域。

## 多个useState 调用示例

让咱们更详细地看看这是如何实现的，第一次渲染：

1. React 创建组件时，它还没有调用函数。React 创建元数据对象和Hooks的空数组。假设这个对象有一个名为`nextHook`的属性，它被放到索引为`0`的位置上，运行的第一个hook将占用位置`0`。
2. React 调用你的组件（这意味着它知道存储`hooks`的元数据对象）。
3. 调用`useState`，React创建一个新的状态，将它放在`hooks`数组的第`0`位，并返回`[volume，setVolume]`对，并将`volume` 设置为其初始值`80`，它还将`nextHook`索引递增1。
4. 再次调用`useState`，React查看数组的第`1`位，看到它是空的，并创建一个新的状态。 然后它将`nextHook`索引递增为`2`，并返回`[position，setPosition]`。
5. 第三次调用`useState`。 React看到位置`2`为空，同样创建新状态，将`nextHook`递增到`3`，并返回`[isPlaying，setPlaying]`。

现在，`hooks` 数组中有`3`个hook，渲染完成。 下一次渲染会发生什么？

1.  `React`需要重新渲染组件, 由于 React 之前已经看过这个组件，它已经有了元数据关联。
2.  `React`将`nextHook`索引重置为`0`，并调用组件。
3. 调用`useState`，React查看索引`0`处的hooks数组，并发现它已经在该槽中有一个hook。，所以无需重新创建一个，它将`nextHook`推进到索引`1`并返回`[volume，setVolume]`，其中`volume`仍设置为`80`。
4. 再次调用`useState`。 这次，`nextHook`为`1`，所以`React`检查数组的索引`1`。同样，hook 已经存在，所以它递增`nextHook`并返回`[position，setPosition]`。
5. 第三次调用`useState`，我想你知道现在发生了什么。

就是这样了，知道了原理，看起来也就不那么神奇了， 但它确实依赖于一些规则，所以才有使用 **Hooks** 规则。

## Hooks 的规则

[自定义 hooks 函数](https://daveceddia.com/custom-hooks/)只需要遵守规则 3 ：它们的名称必须以**“use”**为前缀。

例如，我们可以从`AudioPlayer`组件中将3个状态提取到自己的自定义钩子中：

​                        

```
function AudioPlayer() {
  // Extract these 3 pieces of state:
  const [volume, setVolume] = useState(80);
  const [position, setPosition] = useState(0);
  const [isPlaying, setPlaying] = useState(false);

  // < beautiful audio player goes here >
}
```

因此，咱们可以创建一个专门处理这些状态的新函数，并使用一些额外的方法返回一个对象，以便更容易启动和停止播放，例如：

​                        

```
function usePlayerState(lengthOfClip) {
  const [volume, setVolume] = useState(80);
  const [position, setPosition] = useState(0);
  const [isPlaying, setPlaying] = useState(false);

  const stop = () => {
    setPlaying(false);
    setPosition(0);
  }

  const start = () => {
    setPlaying(true);
  }

  return {
    volume,
    position,
    isPlaying,
    setVolume,
    setPosition,
    start,
    stop
  };
}
```

像这样提取状态的一个好处是可以将相关的逻辑和行为组合在一起。可以提取一组状态和相关事件处理程序以及其他更新逻辑，这不仅可以清理组件代码，还可以使这些逻辑和行为可重用。

另外，通过在自定义`hooks`中调用自定义`hooks`，可以将`hooks`组合在一起。hooks只是函数，当然，函数可以调用其他函数。

## 总结

Hooks 提供了一种新的方式来处理React中的问题，其中的思想是很有意思且新奇的。

React团队整合了一组很棒的[文档](https://reactjs.org/docs/hooks-intro.html)和一个常见[问题解答](https://reactjs.org/docs/hooks-faq.html)，从是否需要重写所有的类组件到钩Hooks是否因为在渲染中创建函数而变慢? 以及两者之间的所有东西，所以一定要看看。

原文：[快速了解 React Hooks 原理](https://segmentfault.com/a/1190000019966124)