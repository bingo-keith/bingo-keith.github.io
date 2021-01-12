---

title: Web前端开发标准规范
date: 2020/4/8 14:28:20
tags: 代码 前端 开发 标准 规范 质量

---

# 目录

- ## 命名规则

- ## HTML代码规范

- ## CSS和SCSS代码规范

- ## JavaScript代码规范

- ## jQuery代码规范

- ## vue代码规范

<!-- more -->

# 参考

1. [腾讯前端开发规范](https://tgideas.qq.com/doc/frontend/)
2. [通过分析github代码库总结出来的工程师代码书写习惯](http://alloyteam.github.io/CodeGuide/#project-naming)
3. [JavaScript Standard Style](https://standardjs.com/readme-zhcn.html)。
4. [Vue官网风格指南](<https://cn.vuejs.org/v2/style-guide/>)

# 命名规则

## 项目命名

- 全部采用小写方式， 以下划线分隔；
- 有明显分类的，使用语义化的命名，无明确意义的，可用page01.html；
- 禁止使用特殊字符。

```js
例：my_project_name
```

## 目录命名

- 参照项目命名规则；
- 有复数结构时，要采用复数命名法；
- 图片文件夹：img；
- css文件夹：css；
- js文件夹：js；
- include页面片段文件夹：inc。

```js
例：scripts, styles, images, data_models
```

## JS文件命名

- 参照项目命名规则 。

```js
例：account_model.js
```

## CSS, SCSS文件命名

- 参照项目命名规则。

```js
例：retina_sprites.scss
```

## HTML文件命名

- 参照项目命名规则。

```js
例：error_report.html
```

------



# HTML代码规范

## 语法规范

- `doctype`声明使用`html5` ；
- 在html标签上加上lang属性，更多关于 `lang` 属性的说明[在这里](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-html-element)，在sitepoint上可以查到[语言列表](http://reference.sitepoint.com/html/lang-codes)；
- 缩进使用soft tab（4个空格），嵌套的节点应该缩进；
- 清晰、简介的层级嵌套结构，保证嵌套层级最少；
- 在属性上，使用双引号，不要使用单引号；
- 标签、标签属性全部小写，用中划线做分隔符；
- 在无法用标签表明语义的场景下增加适当的注释；
- 所有html标签必须有结束符，`<img />`, `<col />`, `<base />`, `<link />`, `<meta />`,` <input />` 除外；
- 标签语法无错误，需要符合语义化；
- 较长的HTML文件，请在板块之间加入注释，使得结构更清晰，也更利于后期维护；
- 除非有特定的功能、组件要求等，禁止随意使用id来定义元素样式 ；
- 统一页面编码格式为`utf-8` , `meta`标签`charset`设置为`utf-8`;
- 给 `<a>` 标签加上title属性；
- `<a>`标签的href属性必须写上链接地址，暂无的加上`javascript:alert('敬请期待！')`；
- 非本专题的页面间跳转，采用打开新窗口模式：`target="_blank"`，同时为了避免[安全问题](https://blog.csdn.net/chinashanzhang/article/details/88537581)加上`rel="noopener"`；
- 标签自定义属性使用`data-name="value"`的形式来写, 如果自定义属性特别多, 可以考虑使用标准 json 的方式去写:` data-json='{"a":"a", "b":"b"}'`；
- css 引用置于头部` <head> `标签内；
- js 引用置于底部` </body> `标签前；
- `img`标签必须加`alt`，尤其是logo、商品图片等关键图片信息，对SEO友好 ；
- 不要忽略可选的关闭标签，例：</li> 和 </body>。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>Page title</title>
    </head>
    <body>
        <img src="images/company_logo.png" alt="Company">
        <h1 class="hello-world">Hello, world!</h1>
        <!-- 活动板块 开始 -->
        <div class="part-act">
            <img/>
        	...
        </div>
        <!-- 活动板块 结束 -->
    </body>
</html> 
```

## 浏览器兼容

- 兼容IE11及以上、Firefox、chrome、QQ浏览器；
- 页面如无指定，不需要兼容IE6、7、8、9、10；
- 不兼容IE6、7、8、9、10的情况下，需要在页面顶部加入提示，并对.browser-tips使用醒目的颜色标识样式。

## 样式命名

- class、id都需小写，首字符必须是字母，禁止数字或其他特殊字符。由以字母开头的小写 字母`（a-z）`、数字`（0-9）`、中划线 `（-）`组成；
- 可以是单个单词，也可以是组合单词，要求能够描述清楚模块和元素的含义，使其具有语义化；
- 双单词或多单词组合方式：`命名空间-模块-元素-修饰`，如：`wb-demo-header-title`；
- 样式名不能包含`ad`、ads、adv、banner、sponsor、guanggao`、`ads`、`gg`是广告含义的关键词，避免元素被网页拓展、插件屏蔽；
- 涉及数据、交互等需要联调的部分，禁止通过id选择器定义样式，以免开发过程中id名变化，引起页局错乱；
- 除非是样式reset需要，禁止对纯元素选择器设置特定样式，避免样式污染；
- 自己写的z-index的值不能超过100（通用组件除外），元素内容不能超过10，弹层提示在10-99之间，不允许直接使用1000-9999之类大值；
- 命名词穷了怎么办？试下这个工具：[codelf]( https://unbug.github.io/codelf/ )。

## 通用命名

### 页面框架命名，一般具有唯一性，推荐用ID命名

| ID名称 | 命名    | ID名称 | 命名    |
| :----- | :------ | :----- | :------ |
| 头部   | header  | 主体   | main    |
| 脚部   | footer  | 容器   | wrapper |
| 侧栏   | sidebar | 栏目   | column  |
| 布局   | layout  |        |         |

### 模块结构命名

| Class名称          | 命名           | Class名称 | 命名        |
| :----------------- | :------------- | :-------- | :---------- |
| 模块(如：新闻模块) | mod (mod-news) | 标题栏    | title       |
| 内容               | content        | 次级内容  | sub-content |

### 导航结构命名

| Class名称 | 命名    | Class名称 | 命名     |
| :-------- | :------ | :-------- | :------- |
| 导航      | nav     | 主导航    | main-nav |
| 子导航    | sub-nav | 顶部导航  | top-nav  |
| 菜单      | menu    | 子菜单    | sub-menu |

### 一般元素命名

| Class名称  | 命名       | Class名称  | 命名                 |
| :--------- | :--------- | :--------- | :------------------- |
| 二级       | sub        | 面包屑     | breadcrumb           |
| 标志       | logo       | 广告       | bner(禁用banner或ad) |
| 登陆       | login      | 注册       | register/reg         |
| 搜索       | search     | 加入       | join                 |
| 状态       | status     | 按钮       | btn                  |
| 滚动       | scroll     | 标签页     | tab                  |
| 文章列表   | list       | 短消息     | msg/message          |
| 当前的     | current    | 提示小技巧 | tips                 |
| 图标       | icon       | 注释       | note                 |
| 指南       | guide      | 服务       | service              |
| 热点       | hot        | 新闻       | news                 |
| 下载       | download   | 投票       | vote                 |
| 合作伙伴   | partner    | 友情链接   | link                 |
| 版权       | copyright  | 演示       | demo                 |
| 下拉框     | select     | 摘要       | summary              |
| 翻页       | pages      | 主题风格   | themes               |
| 设置       | set        | 成功       | suc                  |
| 按钮       | btn        | 文本       | txt                  |
| 颜色       | color/c    | 背景       | bg                   |
| 边框       | border/bor | 居中       | center               |
| 上         | top/t      | 下         | bottom/b             |
| 左         | left/l     | 右         | right/r              |
| 添加       | add        | 删除       | del                  |
| 间隔       | sp         | 段落       | p                    |
| 弹出层     | pop        | 公共       | global/gb            |
| 操作       | op         | 密码       | pwd                  |
| 透明       | tran       | 信息       | info                 |
| 重点       | hit        | 预览       | pvw                  |
| 单行输入框 | input      | 首页       | index                |
| 日志       | blog       | 相册       | photo                |
| 留言板     | guestbook  | 用户       | user                 |
| 确认       | confirm    | 取消       | cancel               |
| 报错       | error      |            |                      |

## 引入CSS, JS

> 根据HTML5规范, 通常在引入CSS和JS时不需要指明 `type`，因为 `text/css` 和 `text/javascript` 分别是他们的默认值。 

### HTML5 规范链接

- [使用link](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-link-element)
- [使用style](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-style-element)
- [使用script](http://www.w3.org/TR/2011/WD-html5-20110525/scripting-1.html#the-script-element)

```javascript
<!-- 外联样式 -->
<link rel="stylesheet" href="reset.css">

<!-- 内联样式 -->
<style>
    ...
</style>

<!-- 外部JS -->
<script src="index.js"></script>

<!-- 内部JS -->
<script>
    ...
</script>
```

## 属性顺序

> 属性应该按照特定的顺序出现以保证易读性。

- `id`
- `class`
- `name`
- `data-*`
- `src`, `for`, `type`, `href`, `value` , `max-length`, `max`, `min`, `pattern`
- `placeholder`, `title`, `alt`
- `aria-*`, `role`
- `required`

> id更加具体且应该尽量少使用，它与JS代码息息相关，所以将它放在第一位；class是为高可复用组件设计的，所以将它放在id后面（第一/二位）。

```html
<a id="..." class="..." data-modal="toggle" href="#">Example link</a>
<input class="form-control" type="text">
<img src="..." alt="...">
```

## boolean属性

> boolean属性指不需要声明取值的属性，XHTML需要每个属性声明取值，但是HTML5并不需要；

> 更多内容可以参考 [WhatWG section on boolean attributes](http://www.whatwg.org/specs/web-apps/current-work/multipage/common-microsyntaxes.html#boolean-attributes)：

> *boolean属性的存在表示取值为true，不存在则表示取值为false。*

```html
<input type="text" disabled>
<input type="checkbox" value="1" checked>
<select>
    <option value="1" selected>1</option>
</select>
```

## JS生成标签

> 在JS文件中生成标签让内容变得更难查找，更难编辑，性能更差。应该尽量避免在javascript中生成DOM。

## 减少标签数量

> 在编写HTML代码时，需要尽量避免多余的父节点；很多时候，需要通过迭代和重构来使HTML变得更少。

```html
<!-- bad -->
<span class="avatar">
    <img src="...">
</span>

<!-- good -->
<img class="avatar" src="...">
```

## 实用高于完美

> 尽量遵循HTML标准和语义，但是不应该以浪费实用性作为代价；任何时候都要用尽量小的复杂度和尽量少的标签来解决问题。

------



# CSS和SCSS代码规范

## 选择器

- CSS类名命名参考样式命名；
- 让规则越具体越好，尽量不要使用**`ul li a {}`**这样长的选择器，最好使用**`.wb-list-wrapper {}`**之类的选择器；
- 禁止使用层级过深的选择器，最多5级；
- 除非有特定的功能、组件要求等，禁止随意使用id来定义元素样式；
- 除非是样式reset需要，禁止对标签选择器设置特定样式，避免样式污染；
- 样式文件中不要出现大写的标签定义, 不要对 JS 钩子进行样式定义；
- 避免使用*来选择元素；
- 避免使用float、position，优先使用flex和grid；
- 避免使用`!important`, 如果必须请加注释；

```css
 //会导致页面所有的a标签都被加上背景
 a{background:url(xxx);}

 //后期修改可能会添加一些span标签，如果刚好在div里面，会被污染；不利于后期维护
 div span{display:block}
```

## 缩进

> 使用soft tab（4个空格）。 

```scss
.element {
    position: absolute;
    top: 10px;
    left: 10px;

    border-radius: 10px;
    width: 50px;
    height: 50px;
}
```

## 分号

> 每个属性声明末尾都要加分号。

```scss
.element {
    width: 20px;
    height: 20px;

    background-color: red;
}
```

## 空格

> 以下几种情况不需要空格：

- 属性名后
- 多个规则的分隔符','前
- `!important` '!'后
- 属性值中'('后和')'前
- 行末不要有多余的空格

> 以下几种情况需要空格：

- 属性值前
- 选择器'>', '+', '~'前后
- '{'前
- `!important` '!'前
- `@else` 前后
- 属性值中的','后
- 注释'/\*'后和'\*/'前

```scss
/* not good */
.element {
    color :red! important;
    background-color: rgba(0,0,0,.5);
}

/* good */
.element {
    color: red !important;
    background-color: rgba(0, 0, 0, .5);
}

/* not good */
.element ,
.dialog{
    ...
}

/* good */
.element,
.dialog {

}

/* not good */
.element>.dialog{
    ...
}

/* good */
.element > .dialog{
    ...
}

/* not good */
.element{
    ...
}

/* good */
.element {
    ...
}

/* not good */
@if{
    ...
}@else{
    ...
}

/* good */
@if {
    ...
} @else {
    ...
}
```

## 空行

> 以下几种情况需要空行：

- 文件最后保留一个空行；
- '}'后最好跟一个空行，包括scss中嵌套的规则；
- 属性之间需要适当的空行，具体见后面属性声明顺序。

```scss
/* not good */
.element {
    ...
}
.dialog {
    color: red;
    &:after {
        ...
    }
}

/* good */
.element {
    ...
}

.dialog {
    color: red;

    &:after {
        ...
    }
}
```

## 换行

> 以下几种情况不需要换行：

- '{'前

> 以下几种情况需要换行：

- '{'后和'}'前
- 每个属性独占一行
- 多个规则的分隔符','后

```scss
/* not good */
.element
{color: red; background-color: black;}

/* good */
.element {
    color: red;
    background-color: black;
}

/* not good */
.element, .dialog {
    ...
}

/* good */
.element,
.dialog {
    ...
}
```

## 注释

- 注释统一用'/* */'（scss中也不要用'//'），具体参照下面的写法；
- 缩进与下一行代码保持一致；
- 难于理解的代码添加注释；
- 可能被错误理解的代码添加注释；
- 工具类等公共方法添加文档注释；
- 浏览器特性hack添加注释；
- 修改别人的 CSS 请添加注释 ；
- **可位于一个代码行的末尾，与代码间隔一个空格。**

```scss
/* Modal header */
.modal-header {
    ...
}

/*
 * Modal header
 */
.modal-header {
    ...
}

.modal-header {
    /* 50px */
    width: 50px;

    color: red; /* color red */
}
```

## 引号

- 最外层统一使用双引号；
- url的内容要用引号；
- 属性选择器中的属性值需要引号。

```scss
.element:after {
    content: "";
    background-image: url("logo.png");
}

li[data-type="single"] {
    ...
}
```

## 命名

- 类名使用小写字母，以中划线分隔。推荐BEM命名法
- id采用驼峰式命名
- scss中的变量、函数、混合、placeholder采用驼峰式命名

```scss
/* class */
.block-element-modifier {
    ...
}

/* id */
#myDialog {
    ...
}

/* 变量 */
$colorBlack: #000;

/* 函数 */
@function pxToRem($px) {
    ...
}

/* 混合 */
@mixin centerBlock {
    ...
}

/* placeholder */
%myDialog {
    ...
}
```

## 属性声明顺序

> 相关的属性声明按下面的顺序做分组处理，组之间需要有一个空行。
>
> - 位置属性（position，top。，right， z-index， display， float等）；
> - 大小（width, height, padding, margin）；
> - 文字系列（font，line-height， letter-spacing，color- text-align等）；
> - 背景边框（background，border等）；
> - 其他（animation，transition等）；

```scss
.declaration-order {
    display: block;
    float: right;

    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;

    border: 1px solid #e5e5e5;
    border-radius: 3px;
    width: 100px;
    height: 100px;

    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5;
    text-align: center;

    color: #333;
    background-color: #f5f5f5;

    opacity: 1;
}
// 下面是推荐的属性的顺序
[
    [
        "display",
        "visibility",
        "float",
        "clear",
        "overflow",
        "overflow-x",
        "overflow-y",
        "clip",
        "zoom"
    ],
    [
        "table-layout",
        "empty-cells",
        "caption-side",
        "border-spacing",
        "border-collapse",
        "list-style",
        "list-style-position",
        "list-style-type",
        "list-style-image"
    ],
    [
        "-webkit-box-orient",
        "-webkit-box-direction",
        "-webkit-box-decoration-break",
        "-webkit-box-pack",
        "-webkit-box-align",
        "-webkit-box-flex"
    ],
    [
        "position",
        "top",
        "right",
        "bottom",
        "left",
        "z-index"
    ],
    [
        "margin",
        "margin-top",
        "margin-right",
        "margin-bottom",
        "margin-left",
        "-webkit-box-sizing",
        "-moz-box-sizing",
        "box-sizing",
        "border",
        "border-width",
        "border-style",
        "border-color",
        "border-top",
        "border-top-width",
        "border-top-style",
        "border-top-color",
        "border-right",
        "border-right-width",
        "border-right-style",
        "border-right-color",
        "border-bottom",
        "border-bottom-width",
        "border-bottom-style",
        "border-bottom-color",
        "border-left",
        "border-left-width",
        "border-left-style",
        "border-left-color",
        "-webkit-border-radius",
        "-moz-border-radius",
        "border-radius",
        "-webkit-border-top-left-radius",
        "-moz-border-radius-topleft",
        "border-top-left-radius",
        "-webkit-border-top-right-radius",
        "-moz-border-radius-topright",
        "border-top-right-radius",
        "-webkit-border-bottom-right-radius",
        "-moz-border-radius-bottomright",
        "border-bottom-right-radius",
        "-webkit-border-bottom-left-radius",
        "-moz-border-radius-bottomleft",
        "border-bottom-left-radius",
        "-webkit-border-image",
        "-moz-border-image",
        "-o-border-image",
        "border-image",
        "-webkit-border-image-source",
        "-moz-border-image-source",
        "-o-border-image-source",
        "border-image-source",
        "-webkit-border-image-slice",
        "-moz-border-image-slice",
        "-o-border-image-slice",
        "border-image-slice",
        "-webkit-border-image-width",
        "-moz-border-image-width",
        "-o-border-image-width",
        "border-image-width",
        "-webkit-border-image-outset",
        "-moz-border-image-outset",
        "-o-border-image-outset",
        "border-image-outset",
        "-webkit-border-image-repeat",
        "-moz-border-image-repeat",
        "-o-border-image-repeat",
        "border-image-repeat",
        "padding",
        "padding-top",
        "padding-right",
        "padding-bottom",
        "padding-left",
        "width",
        "min-width",
        "max-width",
        "height",
        "min-height",
        "max-height"
    ],
    [
        "font",
        "font-family",
        "font-size",
        "font-weight",
        "font-style",
        "font-variant",
        "font-size-adjust",
        "font-stretch",
        "font-effect",
        "font-emphasize",
        "font-emphasize-position",
        "font-emphasize-style",
        "font-smooth",
        "line-height",
        "text-align",
        "-webkit-text-align-last",
        "-moz-text-align-last",
        "-ms-text-align-last",
        "text-align-last",
        "vertical-align",
        "white-space",
        "text-decoration",
        "text-emphasis",
        "text-emphasis-color",
        "text-emphasis-style",
        "text-emphasis-position",
        "text-indent",
        "-ms-text-justify",
        "text-justify",
        "letter-spacing",
        "word-spacing",
        "-ms-writing-mode",
        "text-outline",
        "text-transform",
        "text-wrap",
        "-ms-text-overflow",
        "text-overflow",
        "text-overflow-ellipsis",
        "text-overflow-mode",
        "-ms-word-wrap",
        "word-wrap",
        "-ms-word-break",
        "word-break"
    ],
    [
        "color",
        "background",
        "filter:progid:DXImageTransform.Microsoft.AlphaImageLoader",
        "background-color",
        "background-image",
        "background-repeat",
        "background-attachment",
        "background-position",
        "-ms-background-position-x",
        "background-position-x",
        "-ms-background-position-y",
        "background-position-y",
        "-webkit-background-clip",
        "-moz-background-clip",
        "background-clip",
        "background-origin",
        "-webkit-background-size",
        "-moz-background-size",
        "-o-background-size",
        "background-size"
    ],
    [
        "outline",
        "outline-width",
        "outline-style",
        "outline-color",
        "outline-offset",
        "opacity",
        "filter:progid:DXImageTransform.Microsoft.Alpha(Opacity",
        "-ms-filter:\\'progid:DXImageTransform.Microsoft.Alpha",
        "-ms-interpolation-mode",
        "-webkit-box-shadow",
        "-moz-box-shadow",
        "box-shadow",
        "filter:progid:DXImageTransform.Microsoft.gradient",
        "-ms-filter:\\'progid:DXImageTransform.Microsoft.gradient",
        "text-shadow"
    ],
    [
        "-webkit-transition",
        "-moz-transition",
        "-ms-transition",
        "-o-transition",
        "transition",
        "-webkit-transition-delay",
        "-moz-transition-delay",
        "-ms-transition-delay",
        "-o-transition-delay",
        "transition-delay",
        "-webkit-transition-timing-function",
        "-moz-transition-timing-function",
        "-ms-transition-timing-function",
        "-o-transition-timing-function",
        "transition-timing-function",
        "-webkit-transition-duration",
        "-moz-transition-duration",
        "-ms-transition-duration",
        "-o-transition-duration",
        "transition-duration",
        "-webkit-transition-property",
        "-moz-transition-property",
        "-ms-transition-property",
        "-o-transition-property",
        "transition-property",
        "-webkit-transform",
        "-moz-transform",
        "-ms-transform",
        "-o-transform",
        "transform",
        "-webkit-transform-origin",
        "-moz-transform-origin",
        "-ms-transform-origin",
        "-o-transform-origin",
        "transform-origin",
        "-webkit-animation",
        "-moz-animation",
        "-ms-animation",
        "-o-animation",
        "animation",
        "-webkit-animation-name",
        "-moz-animation-name",
        "-ms-animation-name",
        "-o-animation-name",
        "animation-name",
        "-webkit-animation-duration",
        "-moz-animation-duration",
        "-ms-animation-duration",
        "-o-animation-duration",
        "animation-duration",
        "-webkit-animation-play-state",
        "-moz-animation-play-state",
        "-ms-animation-play-state",
        "-o-animation-play-state",
        "animation-play-state",
        "-webkit-animation-timing-function",
        "-moz-animation-timing-function",
        "-ms-animation-timing-function",
        "-o-animation-timing-function",
        "animation-timing-function",
        "-webkit-animation-delay",
        "-moz-animation-delay",
        "-ms-animation-delay",
        "-o-animation-delay",
        "animation-delay",
        "-webkit-animation-iteration-count",
        "-moz-animation-iteration-count",
        "-ms-animation-iteration-count",
        "-o-animation-iteration-count",
        "animation-iteration-count",
        "-webkit-animation-direction",
        "-moz-animation-direction",
        "-ms-animation-direction",
        "-o-animation-direction",
        "animation-direction"
    ],
    [
        "content",
        "quotes",
        "counter-reset",
        "counter-increment",
        "resize",
        "cursor",
        "-webkit-user-select",
        "-moz-user-select",
        "-ms-user-select",
        "user-select",
        "nav-index",
        "nav-up",
        "nav-right",
        "nav-down",
        "nav-left",
        "-moz-tab-size",
        "-o-tab-size",
        "tab-size",
        "-webkit-hyphens",
        "-moz-hyphens",
        "hyphens",
        "pointer-events"
    ]
]
```

## 颜色

> 颜色16进制用小写字母；颜色16进制尽量用简写。

```scss
/* not good */
.element {
    color: #ABCDEF;
    background-color: #001122;
}

/* good */
.element {
    color: #abcdef;
    background-color: #012;
}
```

## 属性简写

> 属性简写需要你非常清楚属性值的正确顺序，而且在大多数情况下并不需要设置属性简写中包含的所有值，所以建议尽量分开声明会更加清晰；

> `margin` 和 `padding` 相反，需要使用简写；

> 常见的属性简写包括：

- `font`
- `background`
- `transition`
- `animation`

```scss
/* not good */
.element {
    transition: opacity 1s linear 2s;
}

/* good */
.element {
    transition-delay: 2s;
    transition-timing-function: linear;
    transition-duration: 1s;
    transition-property: opacity;
}
```

## 媒体查询

尽量将媒体查询的规则靠近与他们相关的规则，不要将他们一起放到一个独立的样式文件中，或者丢在文档的最底部，这样做只会让大家以后更容易忘记他们。

```scss
.element {
    ...
}

.element-avatar{
    ...
}

@media (min-width: 480px) {
    .element {
        ...
    }

    .element-avatar {
        ...
    }
}
```

## SCSS相关

- 声明顺序：
  - `@extend`
  - 不包含 `@content` 的 `@include`
  - 包含 `@content` 的 `@include`
  - 自身属性
  - 嵌套规则
- 提交的代码中不要有 `@debug`；
- `@import` 引入的文件不需要开头的'_'和结尾的'.scss'；
- 嵌套最多不能超过5层；
- `@extend` 中使用placeholder选择器；
- 去掉不必要的父级引用符号'&'。

```scss
/* not good */
@import "_dialog.scss";

/* good */
@import "dialog";

/* not good */
.fatal {
    @extend .error;
}

/* good */
.fatal {
    @extend %error;
}

/* not good */
.element {
    & > .dialog {
        ...
    }
}

/* good */
.element {
    > .dialog {
        ...
    }
}
```

## 杂项

- 不允许有空的规则；
- 元素选择器用小写字母；
- 去掉小数点前面的0；
- 去掉数字中不必要的小数点和末尾的0；
- 属性值'0'后面不要加单位；
- 同个属性不同前缀的写法需要在垂直方向保持对齐，具体参照右边的写法；
- 无前缀的标准属性应该写在有前缀的属性后面；
- 不要在同个规则里出现重复的属性，如果重复的属性是连续的则没关系；
- 不要在一个文件里出现两个相同的规则；
- 用 `border: 0;` 代替 `border: none;`；
- 选择器不要超过5层（在scss中如果超过5层应该考虑用嵌套的方式来写）；
- 发布的代码中不要有 `@import`；
- 尽量少用'*'选择器。

```scss
/* not good */
.element {
}

/* not good */
LI {
    ...
}

/* good */
li {
    ...
}

/* not good */
.element {
    color: rgba(0, 0, 0, 0.5);
}

/* good */
.element {
    color: rgba(0, 0, 0, .5);
}

/* not good */
.element {
    width: 50.0px;
}

/* good */
.element {
    width: 50px;
}

/* not good */
.element {
    width: 0px;
}

/* good */
.element {
    width: 0;
}

/* not good */
.element {
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;

    background: linear-gradient(to bottom, #fff 0, #eee 100%);
    background: -webkit-linear-gradient(top, #fff 0, #eee 100%);
    background: -moz-linear-gradient(top, #fff 0, #eee 100%);
}

/* good */
.element {
    -webkit-border-radius: 3px;
       -moz-border-radius: 3px;
            border-radius: 3px;

    background: -webkit-linear-gradient(top, #fff 0, #eee 100%);
    background:    -moz-linear-gradient(top, #fff 0, #eee 100%);
    background:         linear-gradient(to bottom, #fff 0, #eee 100%);
}

/* not good */
.element {
    color: rgb(0, 0, 0);
    width: 50px;
    color: rgba(0, 0, 0, .5);
}

/* good */
.element {
    color: rgb(0, 0, 0);
    color: rgba(0, 0, 0, .5);
}
```

## 图片处理

### 图片命名

- 图片名称必须小写，禁止使用特殊字符、中文；
- 使用英文或拼音缩写，禁止特殊字符；
- 名称间隔使用-符号；
- 命名需要能体现图片的大概用途；
- 禁止文件名和实际图片内容不符。反面例子：图片名为'weixin-qrcode'，图片内容却是头像。

### 图片优化

- 图片体积不能超过300K；
- JPG图片必须压缩，一般80%品质即可，保证文字清晰；
- JPG图片必须使用渐进式图片：使用Photoshop的“存储为web所用格式”时候，勾选“连续”；
- 透明PNG图片必须使用[压缩工具](https://tgideas.qq.com/doc/frontend/tool/imagecompress.html)压缩后提供。

### 图片标签

- img图片必须填写width、height、alt属性；
- 移动端必须填写`alt`属性；
- `alt`不能为无意义字符，需要能表现出图片的含义，如图片为道具图，则应该为道具的名称。

### 合理切图

- 文字禁止切到图片中，如果不确定是否需要变动，请咨询接口人；
- 序后台动态生成的图片，如道具图片、头像、奖品，必须单独切割出来；
- 图片合并成精灵图或做成字体图标，减少页面请求。

------



# JavaScript代码规范

## 缩进

> 使用soft tab（2个空格）。

```js
if (x < y) {
  x += 10;
} else {
  x += 1;
}
```

## 单行长度

> 不要超过100，但如果编辑器开启word wrap可以不考虑单行长度。

## 分号

> 以下几种情况后需加分号：

- 变量声明
- 表达式
- return
- throw
- break
- continue
- do-while

> 以下几种情况后无需分号

- for
- function
- if
- switch
- try
- while

```js
/* var declaration */
var x = 1;

/* expression statement */
x++;

/* do-while */
do {
    x++;
} while (x < 10);
```

## 空格

> 以下几种情况不需要空格：

- 对象的属性名后；
- 前缀一元运算符后；
- 后缀一元运算符前；
- 函数调用括号前；
- 无论是函数声明还是函数表达式，'('前不要空格；
- 数组的'['后和']'前；
- 对象的'{'后和'}'前；
- 运算符'('后和')'前。

> 以下几种情况需要空格：

- 二元运算符前后；
- 三元运算符'?:'前后；
- 代码块'{'前；
- 下列关键字前：`else`, `while`, `catch`, `finally；`
- 下列关键字后：`if`, `else`, `for`, `while`, `do`, `switch`, `case`, `try`, `catch`, `finally`, `with`, `return`, `typeof；`
- 单行注释'//'后（若单行注释和代码同行，则'//'前也需要），多行注释'*'后；
- 对象的属性值前；
- for循环，分号后留有一个空格，前置条件如果有多个，逗号后留一个空格；
- 无论是函数声明还是函数表达式，'{'前一定要有空格；
- 函数的参数之间。

```js
// not good
var a = {
    b :1
};

// good
var a = {
    b: 1
};

// not good
++ x;
y ++;
z = x?1:2;

// good
++x;
y++;
z = x ? 1 : 2;

// not good
var a = [ 1, 2 ];

// good
var a = [1, 2];

// not good
var a = ( 1+2 )*3;

// good
var a = (1 + 2) * 3;

// no space before '(', one space before '{', one space between function parameters
var doSomething = function(a, b, c) {
    // do something
};

// no space before '('
doSomething(item);

// not good
for(i=0;i<6;i++){
    x++;
}

// good
for (i = 0; i < 6; i++) {
    x++;
}
```

## 空行

> 以下几种情况需要空行：

- 变量声明后（当变量声明在代码块的最后一行时，则无需空行）；
- 连续空行数控制在 1 ~ 3 行（根据代码块层次来控制空行数）；
- 注释前（当注释在代码块的第一行时，则无需空行）；
- 代码块后（在函数调用、数组、对象中则无需空行）；
- 语法意义上相互独立的代码将用空行分隔；
- 文件最后保留一个空行。

```js
// need blank line after variable declaration
var x = 1;

// not need blank line when variable declaration is last expression in the current block
if (x >= 1) {
    var y = x + 1;
}

var a = 2;

// need blank line before line comment
a++;

function b() {
    // not need blank line when comment is first line of block
    return a;
}

// need blank line after blocks
for (var i = 0; i < 2; i++) {
    if (true) {
        return false;
    }

    continue;
}

var obj = {
    foo: function() {
        return 1;
    },

    bar: function() {
        return 2;
    }
};

// not need blank line when in argument list, array, object
func(
    2,
    function() {
        a++;
    },
    3
);

var foo = [
    2,
    function() {
        a++;
    },
    3
];


var foo = {
    a: 2,
    b: function() {
        a++;
    },
    c: 3
};
```

## 换行

- (建议)每行代码不要超过 80 字符。当一条语句一行写不下时, 折行；
- "{}" (大括号)前面不需要换行, 例如函数定义、if 语句、while 语句、switch 等；
- 换行的地方，行末必须有','或者运算符；
- 以下几种情况不需要换行：
  - 下列关键字后：`else`, `catch`, `finally`
  - 代码块'{'前
- 以下几种情况需要换行：
  - 代码块'{'后和'}'前
  - 变量赋值后

```js
// not good
var a = {
    b: 1
    , c: 2
};

x = y
    ? 1 : 2;

// good
var a = {
    b: 1,
    c: 2
};

x = y ? 1 : 2;
x = y ?
    1 : 2;

// no need line break with 'else', 'catch', 'finally'
if (condition) {
    ...
} else {
    ...
}

try {
    ...
} catch (e) {
    ...
} finally {
    ...
}

// not good
function test()
{
    ...
}

// good
function test() {
    ...
}

// not good
var a, foo = 7, b,
    c, bar = 8;

// good
var a,
    foo = 7,
    b, c, bar = 8;
```

## 单行注释

- 双斜线后，必须跟一个空格；
- 缩进与下一行代码保持一致；
- 可位于一个代码行的末尾，与代码间隔一个空格。

```js
if (condition) {
    // if you made it here, then all security checks passed
    allowed();
}

var zhangsan = 'zhangsan'; // one space after code
```

## 多行注释

- 最少三行, '*'后跟一个空格，具体参照右边的写法；
- 建议在以下情况下使用：
  - 难于理解的代码段
  - 可能存在错误的代码段
  - 浏览器特殊的HACK代码
  - 业务逻辑强相关的代码

```js
/*
 * one space after '*'
 */
var x = 1;
```

## 文档注释

> 各类标签@param, @method等请参考[usejsdoc](http://usejsdoc.org/)和[JSDoc Guide](http://yuri4ever.github.io/jsdoc/)；

> 建议在以下情况下使用：

- 所有常量
- 所有函数
- 所有类

```js
**
 * @func
 * @desc 一个带参数的函数
 * @param {string} a - 参数a
 * @param {number} b=1 - 参数b默认值为1
 * @param {string} c=1 - 参数c有两种支持的取值</br>1—表示x</br>2—表示xx
 * @param {object} d - 参数d为一个对象
 * @param {string} d.e - 参数d的e属性
 * @param {string} d.f - 参数d的f属性
 * @param {object[]} g - 参数g为一个对象数组
 * @param {string} g.h - 参数g数组中一项的h属性
 * @param {string} g.i - 参数g数组中一项的i属性
 * @param {string} [j] - 参数j是一个可选参数
 */
function foo(a, b, c, d, g, j) {
    ...
}
```

## 引号

> 最外层统一使用单引号。

```js
// not good
var x = "test";

// good
var y = 'foo',
    z = '<div id="test"></div>';
```

## 变量命名

- 精简短小, 见名知意。 必须是有意义的文件名， 文件名中严禁出现中文 ， 多个英文单词请用大小驼峰命名法。尽量保持所有名称最短, 但是一定要保证名称具有描述性；
- 字符串变量值统一用单引号；
- 变量的命名, 不得使用 js 保留字；
- 请不要在文件名后加数字, 来实现相同功能的多个版本；
- 除非必要, 请不要使用全局变量, 避免出现全局变量污染。
- 函数内部的变量, 请在函数定义的头部声明。出于避免造成不必要消耗的考虑, 复杂类型的变量可以只声明不赋值。唯一特殊的情况, 是 for 循环的下标变量, 可以使用的时候实时声明；
- 函数内两次以上使用到的同一全局变量或者外部对象, 定义为一个局部变量, 以保证程序性能最优, 例如: var doc = document, win = window；
- 私有化变量和方法名应该以下划线 _ 开头；
- 变量不要先使用后声明；
- 不使用(汉语拼音/拼音英文数字组合/不规范的英文缩写)作为文件名；
- 尽量使用英文小写作为文件名, 免某些系统不区分文件名大小写；
- 常量全大写，用下划线连接，避免魔术常量，优先使用const，其次let，最后才是var；
- 变量名fetch或async代表异步，get代表获取，set或put代表修改，is、has、can代表布尔值，handle代表普通函数。

```js
var thisIsMyName;

var MAX_COUNT = 10;

function Person(name) {
    this.name = name;
}
```

## 变量声明

> 一个函数作用域中所有的变量声明尽量提到函数首部，用一个var声明，不允许出现两个连续的var声明。 

```js
function doSomethingWithItems(items) {
    // use one var
    var value = 10,
        result = value + 10,
        i,
        len;

    for (i = 0, len = items.length; i < len; i++) {
        result += 10;
    }
}
```

## 条件语句/循环

### 条件语句

- if语句, 即使只有单行, 也要用花括号括起来；
- 使用三元运算符, 替代单一的 if else 语句；
- if/else/while/for 条件表达式必须有小括号, 且自占一行；
- 利用 && 和 || 短路来简化代码；
- 使用严格的条件判断符。用 === 代替 ==, 用!== 代替 !=。

```js
if (val != 0) {
     return foo();
} else {
    return bar();
}

// 可以写作:
return val ? foo() : bar();

function foo(opt_win) {
    var win;
    if (opt_win) {
        win = opt_win;
    } else {
        win = window;
    }
    // ...
}

// 可以写作:
function foo(opt_win) {
    var win = opt_win || window;
    // ...
}
```

### && 短路:

```javascript
if (node) {
    if (node.kids) {
        if (node.kids[index]) {
            foo(node.kids[index]);
        }
    }
}

// 可以写作:
if (node && node.kids && node.kids[index]) {
    foo(node.kids[index]);
}

// 或者
var kid = node && node.kids && node.kids[index];
if (kid) {
    foo(kid);
}
```

### 循环

- 尽量避免 for-in 循环, 只用于 object/hash 的遍历, 数组的遍历使用 for 循环；
- for-in 循环体中必须用 hasOwnProperty 方法检查成员是否为自身成员, 避免来自原型链上的污染；
- 避免在 if 和 while 语句的条件部分进行赋值。

```js
// (错误)
var i = 10;
while (i = i - 2) {
    statement;
}
// 应该写作: (正确)
var i = 10;
while (i > 0) {
    statement;
    i = i - 2;
}
```

## 函数

- 一个函数的内容不宜太长, 较复杂的逻辑, 需拆分成多个函数来实现, 使代码逻辑清晰；
- 对外暴露的 API 型函数, 尽量保持输入输出的稳定, 减小调用者修改代码的成本和风险；
- 无论是函数声明还是函数表达式，'('前不要空格，但'{'前一定要有空格；
- 函数调用括号前不需要空格；
- 立即执行函数外必须包一层括号；
- 参数之间用', '分隔，注意逗号后有一个空格；
- 拥抱函数式编程，遵循单一原则；
- 尽量减少参数数量，一般超过3个参数用对象代替；
- 可以嵌套函数, 用于减少重复代码, 隐藏一些局部函数等, 但不要在块内声明一个函数。因为 JS 并不支持块级作用域, 虽然很多 js 引擎都支持块内声明函数, 但它不属于 ECMAScript 规范 (见 ECMA-262, 第 13 和 14 条)。各个浏览器糟糕的实现相互不兼容, 有些也与未来 ECMAScript 草案相违背。ECMAScript 只允 许在脚本的根语句或函数中声明函数. 如果确实需要在块中定义函数, 建议使用函数表达式来初始化变量。

```js
// (错误)
if (x){
    function foo () {}
}
// 应该写作: (正确)
if (x) {
    var foo = function () {};
}

// no space before '(', but one space before'{'
var doSomething = function(item) {
    // do something
};

function doSomething(item) {
    // do something
}

// not good
doSomething (item);

// good
doSomething(item);

// requires parentheses around immediately invoked function expressions
(function() {
    return 1;
})();

// not good
[1, 2].forEach(function x() {
    ...
});

// good
[1, 2].forEach(function() {
    ...
});

// not good
var a = [1, 2, function a() {
    ...
}];

// good
var a = [1, 2, function() {
    ...
}];

// use ', ' between function parameters
var doSomething = function(a, b, c) {
    // do something
};
```

## 数组、对象

- 对象属性名不需要加引号；
- 对象以缩进的形式书写，不要写在一行；

```js
// not good
var a = {
    'b': 1
};

var a = {b: 1};

var a = {
    b: 1,
    c: 2,
};

// good
var a = {
    b: 1,
    c: 2,
};

```

## 类型转换

- number to string的转换，建议使用**`1 + ''`** 或 **`Sring(1)`**，不使用**`new String(1)`** 或 **`1.tiString()`** 的方式；
- string to number的转换，建议使用**`parseInt`**；
- float to integer 的转换，建议使用**`Math.floor/Math.round/Math.ceil`**方法，不使用**`parseInt`**。

## 括号

> 下列关键字后必须有大括号（即使代码块的内容只有一行）：`if`, `else`, `for`, `while`, `do`, `switch`, `try`, `catch`, `finally`, `with`。

```js
// not good
if (condition)
    doSomething();

// good
if (condition) {
    doSomething();
}
```

## null

> 适用场景：

- 初始化一个将来可能被赋值为对象的变量
- 与已经初始化的变量做比较
- 作为一个参数为对象的函数的调用传参
- 作为一个返回对象的函数的返回值

> 不适用场景：

- 不要用null来判断函数调用时有无传参
- 不要与未初始化的变量做比较

```js
// not good
function test(a, b) {
    if (b === null) {
        // not mean b is not supply
        ...
    }
}

var a;

if (a === null) {
    ...
}

// good
var a = null;

if (a === null) {
    ...
}
```

## undefined

- 永远不要直接使用undefined进行变量判断；
- 使用typeof和字符串'undefined'对变量进行判断。

```js
// not good
if (person === undefined) {
    ...
}

// good
if (typeof person === 'undefined') {
    ...
}
```

## 其它

- 不要使用 void、eval, with, 只在将 ajax 响应的文本解析为 json 时使用 eval()；
- 不要使用 function 构造器；
- 代码中调试用的 alert、console.log、console.dir、debugger 等代码, 在提交到 git 之前, 必须完全清理掉；
- 不要在内置对象的原型上添加方法，如Array, Date；
- 不要在内层作用域的代码里声明了变量，之后却访问到了外层作用域的同名变量；
- 不要在一句代码中单单使用构造函数，记得将其赋值给某个变量；
- 不要在同个作用域下声明同名变量；
- 不要在一些不需要的地方加括号，例：delete(a.b)；
- 不要使用未声明的变量；
- 不要声明了变量却不使用；
- 不要在应该做比较的地方做赋值；
- 数组中不要存在空元素；
- 不要在循环内部声明函数；
- 不要像这样使用构造函数，例：`new function () { ... }`, `new Object`；
- 不要混用tab和space；
- 对上下文this的引用只能使用'_this', 'that', 'self'其中一个来命名；
- 行尾不要有空白字符；
- switch的falling through和no default的情况一定要有注释特别说明；
- 避免硬编码；
- 关键的代码添加适当的异常处理；
- 自测要保证每一个方法、每一个变量的所有场景都覆盖到；
- 不允许有空的代码块。

```js
	
// not good
if (a == 1) {
    a++;
}

// good
if (a === 1) {
    a++;
}

// good
for (key in obj) {
    if (obj.hasOwnProperty(key)) {
        // be sure that obj[key] belongs to the object and was not inherited
        console.log(obj[key]);
    }
}

// not good
Array.prototype.count = function(value) {
    return 4;
};

// not good
var x = 1;

function test() {
    if (true) {
        var x = 0;
    }

    x += 1;
}

// not good
function test() {
    console.log(x);

    var x = 1;
}

// not good
new Person();

// good
var person = new Person();

// not good
delete(obj.attr);

// good
delete obj.attr;

// not good
if (a = 10) {
    a++;
}

// not good
var a = [1, , , 2, 3];

// not good
var nums = [];

for (var i = 0; i < 10; i++) {
    (function(i) {
        nums[i] = function(j) {
            return i + j;
        };
    }(i));
}

// not good
var singleton = new function() {
    var privateVar;

    this.publicMethod = function() {
        privateVar = 1;
    };

    this.publicMethod2 = function() {
        privateVar = 2;
    };
};
// not good
var a   = 1;

function Person() {
    // not good
    var me = this;

    // good
    var _this = this;

    // good
    var that = this;

    // good
    var self = this;
}

// good
switch (condition) {
    case 1:
    case 2:
        ...
        break;
    case 3:
        ...
    // why fall through
    case 4
        ...
        break;
    // why no default
}

// not good with empty block
if (condition) {

}
```

------



# jQuery代码规范

## jQuery变量

- 所有用于存储、缓存jQuery对象的变量应该以$前缀命名；
- 最好把使用选择器返回的jQuery对象缓存到变量里，以便复用；
- 使用[驼峰法](http://en.wikipedia.org/wiki/CamelCase)命名变量。

```js
const $userName = $('#userName');
```



## 选择器

- 尽可能的使用ID选择器；
- 使用类（class）选择器时，不要使用元素类型；
- 对于ID->child的方式，使用find比用嵌套选择器高效；
- 给选择器一个范围，如**`$('.class', '.context')`**；
- 避免使用全局选择器，如**`$('div.class > *')`**；

## DOM操作

- 字符串拼接可用ES6的模版字符串；
- 使用字符串连接符或者array.join()，比append高效；
- 不要操作空对象。

## 事件

- 每个页面只使用一次document的ready事件；
- 尽量不要使用匿名函数绑定事件，因为匿名函数不利于调试、维护、测试、重用；
- 不要在html标签中写事件调用，始终使用jquery来绑定、删除事件；
- 如果有必要，使用自定义事件（即：给事件一个作用域），这样可以方便解除事件，而不影响其它事件；
- 当需要给多个对象绑定相同事件时，使用事件委托。

```js
$("#J_my-link").on("click.mySpecialClick", myEventHandler);
```

## 链式操作

- 将链式操作看成变量缓存和多选择器请求的替代方式；
- 当链式操作超过三个或者因为事件绑定变得复杂时，使用换行和缩进提高链式操作可读性；
- 对于长的链式操作来说，也可以把中间对象缓存成一个变量。

```js
$('#userName').html('zhangsan')
			  .siblings('#userAge')
			  .html('20')
			  .siblings('#gender')
			  .html('男')
```



# vue代码规范

## 必要的

- 组件的`data`必须是一个函数；
- `Prop`定义应该尽量详细；
- 为`v-for`设置键值`key`；
- 避免`v-if`和`v-for`用在一起，`v-for`的优先级要高于`v-if`；
- 为组件样式设置作用域**（注：scss有替代解决方案）**；

```js
export default {
  data() {
    return {
      foo: 'bar'
    }
  },
  props: {
    status: {
      type: String,
      required: true,
      validator: function (value) {
        return [
          'syncing',
          'synced',
          'version-conflict',
          'error'
        ].indexOf(value) !== -1
      }
    }
  }
}
```

```html
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.text }}
  </li>
</ul>
```

```scss
<style scoped>
.wrapper {}
.title {}
</style>

<style lang="scss">
.wrapper {
    .title {}
}
</style>
```



## 强烈推荐

- 单文件组件的文件名应该要么始终是大驼峰命名，要么始终是中划线命名；
- 应用特定样式和约定的基础组件（也就是展示类的、无逻辑的或无状态的组件）应该全部以一个特定的前缀开头；
- 只应该用有单个活跃实例的组件应该以`The`前缀命名，以示其唯一性；
- 和父组件紧密耦合的子组件应该以父组件名作为前缀命名；
- 组件名应该以高级别的单词开头，以描述性的修饰词结尾；
- 对于绝大多数项目来说，在单文件组件和字符串模版中组件名应该总是大驼峰命名，但在DOM模版中总是中划线命名；
- 组件名应该倾向于完整单词而不是缩写；
- 在声明`prop`的时候，其命名应该始终使用大驼峰，而在模版中应该始终使用中划线；
- 多个属性的元素应该分多行，每个属性一行；
- 组件模版应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法；
- 应该把复杂计算属性分割为尽可能多的更 简单的属性；
- 非空的属性值应该始终带引号（单引号或双引号，选你JS里不用的那个）；
- 指令缩写；
- 其它参考Vue官网的风格指南。
