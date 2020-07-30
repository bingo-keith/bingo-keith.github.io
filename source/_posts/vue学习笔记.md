---

title: vue学习笔记
date: 2019/4/18 星期四 15:26:26 
tags: vue 框架 学习 笔记
---

最近因为换工作，忙着找下家的缘故，好久没做一些学习总结了，趁着新东家的项目都做完的比较空闲的缘故，把最近新学习的技术归纳整理一下吧！之前在学习react的时候就对比了解过vue和react的优势和区别了，虽然react在大型项目上比vue有优势，社区也更强大，但在武汉这个市场绝大部分还是已vue为主，之前工作中用的是react，所以一直都在学的react，同时vue比较简单，学习曲线比较平缓，所以当时也没急着去研究vue了。这次换了新东家，项目是用的vue+vuex+axios+vux框架+webpack搭的项目，正好也跟着项目仔细地研究了vue技术栈，确实没有react那么复杂，现把一些学习和工作中的学习笔记记录下来，以便以后可以复习。

<!-- more -->

## 安装

- 直接引入链接：建议初学者使用；
- 通过node.js的NPM安装Vue；
- 通过node.js的NPM安装Vue-cli脚手架（推荐安装方式）；

## 基础

### 数据类型

- 字符串：title: 'hello world'；
- 数字：num:123；
- 数组：arrow: ['apple', 'banana','orange']；
- 对象：name: {firstName:'ming',lastName:'Li'}；
- 布尔值：true/false；

### 模版数据绑定

- {{ xxx }}：模版解析；
- v-text：文本绑定；
- v-model：数据双向绑定；v-model.lazy：延迟对数据进行更新；
  - v-model.number：对输入的数据字符串转为数字；
  - v-model.trim：对数据进行裁剪，去除空格等

### 表单数据绑定

- checkbox：储存的数据类型是数组；
- radio：储存的数据类型是字符串；
- select：存储的数据类型是字符串；

### 标签属性

- v-bind：标签属性绑定，属于动态绑定，可以简写为：。绑定后的属性为变量，可以是字符串，数组或者是对象；
- v-if/v-if-else-if/v-else：条件渲染，如果成立则执行，不成立则注销；
- v-show：同样是条件渲染，不同的是不成立是隐藏而不是注销；

### 事件绑定

- 通过v-on:event="eventName"进行绑定，可简写为@:event="eventName";方法通过在methods里进行方法定义；
- 在v-on:event.midiflyer添加修改器；
- 自定义事件：v-on:diyEvent="eventName"，通过$emit来触发自定义事件。`methods: {my-function () {this.$emit('diyEvent'), 参数}}`；

### 计算属性和数据监听

- 传值属性：props: ['xx','xx']。父子组件如果需要传值，必须要在props里进行定义；
- 事件属性：methods: {}。所有事件都在这里进行定义；很多情况下在执行事件时需要取消默认事件。function(e){e.preventDefault();}；
- 计算属性：computed: {方法 () { return 方法 }}；计算属性的优点：可以直接根据data的属性动态的更改（data中myValue的值变化会同步反映到计算属性里）（计算属性会缓存所依赖的那个值，直到那个值发生变化，否则不会重新取值）与方法调用的缺点：调用方法的时候才会更新，即使data中myValue的值没有变化，调用时依然会去重新取值。使用场景：在耗时、大量搜索的情况下使用，减少dom重复渲染的性能支出。
- 创建属性：created:function(){}。方法不需要手动调用，直接执行。
- 数据监听：watch: { 方法 () {}}；

### 应用

#### 动态绑定class

- v-bind:class="{red:true}"；
- v-bind:class={changeClass:changeClass}；
- v-bind:class={active ? true : false}

## 组件

### 命名

- 不强制要求按照W3C规则进行命名，但最好遵循。例如：`my-template`;
- 不管组件是大驼峰还是小驼峰，在模版引用的时候一律要转为中横线的命名方式。例如：组件为`comName`，引用时为：`<com-name></com-name>`；在传递属性时名称也同样。

### 注册

- 全局注册：`Vue.component('my-template', {template: '...'});`  html：`<my-template></my-template>`
- 局部注册：只在使用的场景进行注册。`var myTemplate = {template: '...'};  new Vue({..., components: {'my-template: myTemplate'}})`

### 模版解析

- 特殊标签下的模版需要注意，比如table、ol、ul、select等标签，使用`is`进行挂载。例如:`<table><tr is="my-tr"></tr></table>`;
- 推荐使用字符串模版：

- `<script type="text/x-template">`；
- javascript内联模版字符串；
- vue组件；

- 组件中的data必须是函数。

###  组件组合

- 父组件通过prop进行向下传递；
- 子组件通过事件进行发送信息，子组件触发事件，父组件进行监听；
- 传值时要主要命名的选择和使用，使用props使用的驼峰式明显需要转变为对应的中横线式。`Vue.component('my-template', {props: ['myMessage'],template: '...'}); <my-template   my-message="hello"></my-template>`
- 字面量语法和动态语法；
- slot插槽：父组件向子组件插入template模板，父子之间通过slot属性和name属性进行对应`<p slot="header">我是header</p><span slot="footer">我是footer</span>`；
- 动态组件：利用 `:is = ""` 进行组件的动态绑定，外层可以用内置组件keep-alive 来进行缓存；

### 总结

- 使用组件树设计项目，配置文件链接各个组件-命名转换，动态组件；
- 父组件向内传递属性-动态属性；
- 子组件向外发布事件；
- slot插槽传递模版 - 具名slot；

## 高级用法

### 动画：使用transition 内置组件，有css过渡和js过渡两种方式。

#### css过渡实现原理：给动画的不同阶段加上不同的class名称。

- 四个阶段：v-enter/v-enter-active/v-leave/v-leave-active；使用：`<transition name="fade"></transition>`  .fade-enter/.fade-enter-active/.fade-leave/.fade-leave-active；
- 能够接受动画的元素有：v-show/v-if/动态组件加载；
- 通过mode="out-in"/"in-out"实现动画顺序；
- 对于多元素模版，如果使用的是同标签名，需要使用key来进行区分；

#### js过渡实现原理：通过定义不同的方法来实现动画。

不同方法名：

`
	<transition
	  v-on:before-enter="beforeEnter"
	  v-on:enter="enter"
	  v-on:after-enter="afterEnter"
	  v-on:enter-cancelled="enterCancelled"
	  v-on:before-leave="beforeLeave"
	  v-on:leave="leave"
	  v-on:after-leave="afterLeave"
	  v-on:leave-cancelled="leaveCancelled">
	...
	</transition>
`

### 自定义指令

- 方法：主要有两种：局部定义和全局定义。
- 使用：inserted和bind是指令的两个配置属性，属性值是一个函数，所以用es6语法。讲inserted函数，，然后然后回到组件，处理el表示使用了指令的元素对象，还有一个binding对象，其中binding.value表示的是使用了指令元素的指令的值，可以是json，然后借这个json（里面放着css相关信息）所包含的数据来修改dom的样式。

### 插件

- vue-resource： 发送http请求
- vue-router：   前端路由
- 引入步骤：
  - 入口js文件 import  from  插件 
  - Vue.use(插件)  不过在模块环境中应当始终显式调用 Vue.use() :

## 相关概念 

- MVC
- MVP
- MVVM

## 相关知识点

- Node.js
- NPM
- Mustache
- ECMAscript
- Javascript
- Ajax

## Vue全家桶

- vue.js
- vue-cli
- vue-router
- vue-axios
- vue-lazyload

## 打包工具

- Webpack
- Gulp
- Parcel

## 安装依赖

- Css-loader
- Sass-loader
- Vue-style-loader
- Superagent



## 踩坑

- 在开发过程中，如果修改了配置文件，需要重新启动，否则报错；
- 手写输入的拼写错误问题，一般会提示出来；
- 样式文件中的拼写错误，包括属性、值、图片名称，如果找不到也会报错，一般很难找到，所以出现报错的时候一定先要解决！！
- 使用webpack要进行loader依赖的安装；
- proxyTable 反向代理设置；
- 在组件中template节点下必须只有一个子节点；
- 如果采用webpack进行打包管理，如果数据中有需要在js里引用图片地址，需要使用`require()`的方式进行引用，否则不会被打包到静态文件目录里；
- 在组件使用时候先进行数据绑定；
- 在使用属性的时候一定记得要添加作用域，比如this；
- 在组件或者模版中使用sass或less文件，一定要在style标签上声明lang，否则报错没商量；
- 在组件开发过程中，如果需要传参一定记得定义并且在引用的地方调用；

## 参考资料

- [Vue官网](https://cn.vuejs.org/v2/guide/)