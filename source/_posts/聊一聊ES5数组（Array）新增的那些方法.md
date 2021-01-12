---

title: 聊一聊ES5数组（Array）新增的那些方法
date: 2017/8/14 22:40:58 
tags: ES5 ECMAScript5 数组 Array 方法

---

今天把ES5新增的一些数组的方法学习了一下，这个对于处理数据还是非常有用的。现在把自己的理解和心得记录下来，方便以后查阅。

ES5新增数组方法大致可以分为几类：
- 索引方法：`indexOf()`和`lastIndexOf`；
- 迭代方法：`forEach()`、`map()`、`filter()`、`some()`、`every()`；
- 归并方法：`reduce()`和`reduceRight()`；

↓↓↓↓↓↓↓↓↓↓↓↓该上代码啦↓↓↓↓↓↓↓↓↓↓↓↓
<!-- more -->

# 索引方法
索引方法包含indexOf()和lastIndexOf()两个方法，这两个方法都接收两个参数，第一个参数是要查找的项，第二个参数是查找起点位置的索引，该参数可选，如果缺省或是格式不正确，那么默认为0。两个方法都返回查找项在数组中的位置，如果没有找到，那么返回-1。`区别就是一个从前往后找，一个从后往前找。`

先来看demo吧

```
var arr = [2, 3, 1, 4, 7, 6, 4];
console.log(arr.indexOf(7));  //4
console.log(arr.indexOf(4, 3)); //3
console.log(arr.lastIndexOf(6)); //5
console.log(arr.lastIndexOf('6')); //-1
```

> 这个demo纠结了半天，原因在于错认为`lastIndexOf方法`是从后往前找，所以理所当然的认为打印出来的索引也应该是从后往前的，后来查了资料才明白`indexOf()方法`和`lastIndexOf()方法`返回的位置都是从前开始计算索引的。`lastIndexOf()方法`第二个参数是起始位置，从后往前数的，但是在计算索引值的时候还是从前往后计算的。。算是踩了个坑，不过及时填上了。^-^

# 迭代方法
迭代方法包含`some()`、`every()`、`filter()`、`map()`和`forEach()`五个方法，这些方法都接收两个参数，第一个参数是一个函数，他接收三个参数，数组当前项的值、当前项在数组中的索引、数组对象本身。第二个参数是执行第一个函数参数的作用域对象，也就是上面说的函数中this所指向的值。`注意，这几种方法都不会改变原数组。`

> every()方法和some()方法有些类似，前者是对数组中每一项执行指定方法，如果返回值都为真则返回true，有一个假就返回false；后者也是对数组中每一项执行指定方法，区别在于如果返回值有一个真就返回true，否则返回false。这让我想到了短路操作中的逻辑运算符&&和||了，有点那个意思吧。

下面上demo了
```
var arr = [2, 3, 1, 4, 7, 6, 4];
var res = arr.every(function(v, i, arr) {//v当前项，i当前索引，arr原数组
  return v > 0 && v < 7;
})
var res2 = arr.some(function(v, i, arr) {
  return v > 6;
})
console.log(res);  //false
console.log(res2);  /true
```

- 而且值得注意的是，some方法会在数组中任一项执行函数返回true之后，不在进行循环。

--- 

> filter()方法对数组中每一项执行指定方法，生成返回值为true的新数组。利用这个方法可以对数组元素进行过滤筛选。

- 不过值得注意的是该方法支持的是弱等（==），而不是全等（===）。该上demo了。

```
var arr = [2, 3, 1, 4, 7, 6, 4];
var res3 = arr.filter(function(v, i, arr) {
  return v > '3';
})
console.log(res3);  //[4, 7, 6, 4]
```

> map()方法对数组中的每一项执行指定方法，生成该方法返回值组成的新数组。

```
var arr = [2, 3, 1, 4, 7, 6, 4];
var res4 = arr.map(function(v, i, arr) {
  return v * i;
})
console.log(res4);  //[0, 3, 2, 12, 28, 30, 24]
```

> forEach()方法对数组中的每一项执行指定方法，这个方法没有返回值。这个方法和for循环、jQuery中的each()方法一样。

```
var arr = [2, 3, 1, 4, 7, 6, 4];
var res5 = arr.forEach(function(v, i, arr) {
  console.log(v);  //把原数组中每一项打印出来
})
```

# 归并方法
归并方法包含reduce()和reduceRight()两个方法，这两个方法都会遍历数组中的每一项，然后生成一个最终的返回值。他们都接收两个参数，第一个参数的每一项调用的函数，`该函数接收4个参数：初始值，当前值，索引值和当前的数组`，函数的返回值会在下一次迭代中作为第一个参数也就是初始值。`第二个参数是迭代的初始值，参数可选`，如果缺省那么初始值为数组的第一项，缺省参数要比正常传值少一次运算。

> reduce()方法从数组的第一项开始，逐个遍历到最后一项，其中一个应用场景是数组求和或乘积。

```
var arr = [2, 3, 1, 4, 7, 6, 4];
var res6 = arr.reduce(function(prev, cur, index, arr) {//1.初始值，2.当前值，3.当前索引，4.原数组
  return prev + cur;
})
console.log(res6);  //27
var res7 = arr.reduce(function(prev, cur, index, arr) {//1.初始值，2.当前值，3.当前索引，4.原数组
  return prev + cur;
},5)
console.log(res7);  //32
```

- 我们可以看出，当设置了第二个参数。会把第二个参数作为初始值，参与方法的运算，这样会多执行一次。
- 利用这种方法可以快速的把二维数组转化为一维数组

```
var arr = [[1,2],[3,4]];
var newArr = arr.reduce(function(prev, cur, index, arr){
  return prev.concat(cur);
})
console.log(newArr);  //[1,2,3,4]

```

> refuceRight()方法就是从右到左，其他的和reduce()方法一样。

# 兼容性问题

> ES5处理

ES5里这些处理数组的新方法，在IE6-IE8浏览器还未得到支持，所以我们需要在IE这些低版本浏览器中引入这个es5-shim补丁，这样我们就可以使用它了。[https://github.com/bingo-keith/es5-shim](https://github.com/bingo-keith/es5-shim "补丁地址")