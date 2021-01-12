---

title: 用cssText批量修改样式
date: 2017/7/29 23:49:52 
tags: css cssText 批量 样式 修改 增加

---

> 一般情况下我们用js设置元素对象的样式会使用这样的形式：

<!-- more -->

```
var element= document.getElementById(“id”);
element.style.width=”20px”;
element.style.height=”20px”;
element.style.border=”solid 1px red”;
```

> 样式一多，代码就很多；而且通过JS来覆写对象的样式是比较典型的一种销毁原样式并重建的过程，这种销毁和重建，都会增加浏览器的开销。
> js中有一个cssText的方法：
> 语法为：obj.style.cssText=”样式”;
> 上面的代码我们可以修改成：

```
element.style.cssText=”width:20px;height:20px;border:solid 1px red;”;
```

> 这样就可以尽量避免页面reflow，提高页面性能。

> 但是，这样会有一个问题，会把原有的cssText清掉，比如原来的style中有’display:none;’，那么执行完上面的JS后，display就被删掉了。
> 为了解决这个问题，可以采用cssText累加的方法：

```
Element.style.cssText += ‘width:100px;height:100px;top:100px;left:100px;’
```
> 但是，**cssText（假如不为空）在IE中最后一个分号会被删掉**，比较BT….
> 因此，上面cssText累加的方法在IE中是无效的。
> 最后，可以在前面添加一个分号来解决这个问题：

```
Element.style.cssText += ‘;width:100px;height:100px;top:100px;left:100px;’
```