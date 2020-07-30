---

title: js优化嵌套的条件语句
date: 2017/12/4 23:02:39 
tags: js 嵌套 性能 优化 javascript

---

> 最近在网上看了一篇文章，讲的是遇到多种分支条件判断的场景下，尽量避免使用swich和if，一个是因为性能不好，一个是因为代码阅读导致调试困难。我觉得文章讲述的这种技巧是可以借鉴的，记录下来，工作中可以采用。

<!-- more -->

```
if (color) {
  if (color === 'black') {
    printBlackBackground();
  } else if (color === 'red') {
    printRedBackground();
  } else if (color === 'blue') {
    printBlueBackground();
  } else if (color === 'green') {
    printGreenBackground();
  } else {
    printYellowBackground();
  }
}
```

> 一种方法来提高嵌套的if语句是用switch语句。虽然它不那么啰嗦而且排列整齐，但是并不建议使用它，因为这对于调试错误很困难。

```
switch(color) {
  case 'black':
    printBlackBackground();
    break;
  case 'red':
    printRedBackground();
    break;
  case 'blue':
    printBlueBackground();
    break;
  case 'green':
    printGreenBackground();
    break;
  default:
    printYellowBackground();
}
```
> 但是我们应该时刻注意避免太多判断在一个条件里，尽量少的使用switch，考虑最有效率的方法：借助object。

```
var colorObj = {
  'black': printBlackBackground,
  'red': printRedBackground,
  'blue': printBlueBackground,
  'green': printGreenBackground,
  'yellow': printYellowBackground
};


if (color in colorObj) {
  colorObj[color]();
}
```
