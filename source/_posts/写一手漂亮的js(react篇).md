---

title: 写一手漂亮的js(react篇)
date: 2018/1/20 21:38:16  
tags: javscript react 代码 优化 习惯

---

> 　　近来有空一直在学习react的技术栈，虽说学习的深度没那么深，但总归初窥门径了，今天把在编码中的一些好方法好习惯记录一下。

<!-- more -->

# 个人原则

1. 既然react是组件化的，那么相同的代码，我不会写第二遍
2. 不在dom结构中夹杂太多js逻辑

# 对生命周期函数排序

```
// bad
class Demo extends React.Component {
  render() {}

  componentDidMount() {}

  componentWillMount() {}
}

// good
class Demo extends React.Component {
  componentWillMount() {}

  componentDidMount() {}

  render() {}
}
```

# 传递多个props时注意换行

```
// bad
<Demo className='a' value={a} onClick={() => {}} />

// goood
<Demo
  className='a'
  value={a}
  onClick={() => {}}
/>
```

# 利用对象展开符传递props

```
const someProps = {
  a: 1,
  b: 2,
  c: 3
}

// bad
<Demo
  a={someProps.a}
  b={someProps.b}
  c={someProps.c}
/>

// goood
<Demo {...someProps} />

// 当有些属性不需要传递的时候
const {
  a,
  ...otherProps
} = someProps

<Demo {...otherProps} />
```

# 利用箭头函数绑定this

```
// bad
class Demo extends React.Component {
  handleClick() {

  }

  render() {
    <Button
      onClick={this.handleClick.bind(this)}
    />
  }
}

// good
class Demo extends React.Component {
  handleClick = () => {

  }

  render() {
    <Button
      onClick={this.handleClick}
    />
  }
}
```

# 提前解构state，props

```
// bad
class Demo extends React.Component {
  handleClick = () => {
    this.props.add(this.state.a + this.state.b)
    this.props.respond()
  }
}

// good
class Demo extends React.Component {
  handleClick = () => {
    const { a, b } = this.state
    const { respond, add } = this.props

    add(a + b)
    respond()
  }
}
```

# map时不要使用index当做key，用item的id

> index没办法利用key来避免不必要的渲染

```
// bad
class Demo extends React.Component {
  render() {
    return arr.map((item, i) => (
      <span key={i}>{item.name}</span>
    ))
  }
}

// good
class Demo extends React.Component {
  render() {
    return arr.map(item => (
      <span key={item.id}>{item.name}</span>
    ))
  }
}
```

# 不要将大段的内联样式写在组件上

> 影响阅读

```
// bad
class Demo extends React.Component {
  render() {
    return (
      <div style={{
        width: '100px',
        height: '100px',
        textAlign: 'center',
        lineHeight: '100px'
      }}>11</div>
    )
  }
}

// good
const styles = {
  container: {
    width: '100px',
    height: '100px',
    textAlign: 'center',
    lineHeight: '100px'
  }
}

class Demo extends React.Component {
  render() {
    return (
      <div style={styles.container}>11</div>
    )
  }
}
```

# 给props加上类型检查

> 一定程度上能及时发现问题，当然更好的选择是flow、ts

```
// bad
class Demo extends React.Component {
  // nothing
}

// good
import PropTypes from 'prop-types';

class Demo extends React.Component {
  static propTypes = {
    className: PropTypes.string,
    style: PropTypes.object,
    url: PropTypes.oneOfType([
      PropTypes.string,
      PropTypes.array,
    ]),
    onClick: PropTypes.func,
  }
}
```

# 尽量不要在渲染组件时传递匿名函数

1. 首先它会影响阅读
2. 每次渲染会生成新的匿名函数，对子组件来说就是新的props，就会触发再一次更新
3. 当然，当函数只有一行的时候，我觉得也是可以这么做的，从代码简洁性考虑

```
// bad
class Demo extends React.Component {
  handleClick = () => {
    a++
    this.props.add()
  }

  render() {
    return (
      <Hello onClick={this.handleClick}>11</Hello>
    )
  }
}

// good
class Demo extends React.Component {
  render() {
    return (
      <Hello onClick={() => {
        a++
        this.props.add()
      }}>11</Hello>
    )
  }
}
```