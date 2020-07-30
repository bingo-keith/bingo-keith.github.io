---

title: JavaScript难点笔记
date: 2018/3/22 21:37:08
tags: Javascript JS ES6 干货

---

# 前言

今天重新回顾了一下JavaScript，以下内容是我平时的学习笔记和其它参考资料整理完善后的内容，都是常用的，使用频率比较高，自己必须得精通的知识点的总结，便于以后再复习参考。

<!-- more -->

# JavaScript原型与原型链

## 构造函数的缺点

> 自定义对象时，以构造函数为模板，对象的属性和方法，可以定义在构造函数内部。每当获取对象时都会在内存中创建新的对象属性和方法，这既是增加页面代码量又很浪费内存。

同一个构造函数的对象实例之间无法共享属性，而所有的方法都是同样的行为（只是参数不一样），因此必要的属性和公共的方法完全应该共享。但构造函数无法实现这一点。

```
function Student(name, age){
	this.name = name;
	this.age = age;
	this.country = 'Chinese';
	this.say = function(){
		console.log(`hi,my name is ${this.name} , I am ${this.age} years old`);
	}
}
//每当用new关键字实例化对象时，就会声明一个country属性和say方法并为每一个都分配内存，
//但实际上country属性和say方法都是一样的，可以公用，这样重复的分配内存很浪费内存
var s1 = new Student('keith',12);
s1.say();//hi,my name is keith,I am 12 years old,I am Chinese
var s2 = new Student('lisa',11);
s2.say();//hi,my name is lisa,I am 11 years old,I am Chinese
```

## 原型属性

> JavaScript中每一个对象都继承另一个对象，父类对象称之为“原型”(prototype)对象。只有null除外，其他都有自己的原型对象，而原型对象上的所有属性和方法，都能被子类对象继承。通过构造方法生成实例化对象时，会自动生成实例化对象分配原型对象。每一个构造方法都有一个prototype属性，这个属性就是实例化对象的原型对象。

> 还是用上面的栗子

```
function Student(name,age){
    this.name=name;
    this.age=age;
}
//抽离对象的公共属性和公共方法，赋给构造函数的原型对象，这样就只会对该对象的公共属性和公共方法分配一次内存，所有实例化的对象共享这一份，节约了内存，提高了性能
Student.prototype.country = 'Chinese';
Student.prototype.say = function(){
    console.log(`hi,my name is ${this.name},I am ${this.age} years old,I am ${this.country}`);
}
var s1 = new Student('keith',12);
s1.say();
var s2 = new Student('lisa',11);
s2.say();
```

构造函数Student的prototype对象就是实例化对象s1和s2的原型对象。在原型对象上添加一个country属性和say方法，这样实例化的对象都有该属性和方法，实现属性共享。

原型对象的属性不是实例化对象自身的属性，但只要修改原型对象，就会同步体现到所有实例化对象上。

如果实例化对象自身拥有同名的属性或方法，那么原型对象上的属性和方法便会失效，遵循了作用域的就近查找原则。

> 总结：原型对象的作用就是定义所有实例化对象共享的属性和方法。这也是被称为原型式继承的原因，而实例化对象可以视作从原型对象派生出来的子对象。

## 原型链

对象的属性和方法，有可能是定义在自身内部，也有可能是定义在它的原型对象上，由于原型对象本身也是对象，又有自己的原型链，所以生成了一条原型链。例如，a对象是b对象的原型，b对象是c对象的原型，依此类推。

如果一层层往上找，所有对象的原型最终都可以找到Object对象上，Object对象再往上找就是null了，null没有自己的原型，至此为止了。

附上自己总结JavaScript原型链图一张：

![函数原型链完整三角形](https://i.imgur.com/IvQD884.jpg)

注意：一级一级网上找，在原型链寻找某个属性，对性能是有影响的。如果寻找某个不存在的属性将会遍历整个原型链。

## 原型操作

### constructor属性

对象有一个constructor属性指向原型对象所在的构造函数：

```
console.log(s1.constructor === Student.prototype.constructor);//true
```

### 设置获取原型对象

`Object.getPrototypeOf()`方法返回一个对象的原型对象，也就是通过它获取原型对象，这是标准的方法。

```
console.log(Object.getPrototypeOf(s1));
```

`Object.setPrototypeOd()`为现有对象设置原型对象，第一个参数是现有对象，第二个是要设置成为原型对象的对象，用这个方法来设置原型对象。

`__proto__`属性：`__proto__`是`prototype`在实例上的副本，定义在`prototype`上的方法能够继承给所有实例，而`__proto__`只能影响到指定的实例，所以最好不用这个，用`Object.getPrototypeOf()`来读取，用`Object.setPrototypeof()`来设置。也就是用这两个方法来对原型对象做读写操作。

### 获取原型对象的方法

```
//构造函数获取原型对象
console.log(Student.prototype.constructor);

//通过实例化的对象获取原型对象
console.log(s1.__proto__);

//ES6提供的获取对象的原型对象
console.log(Object.getPrototypeOf(s1));
```

# 闭包

## 闭包的概念

JavaScript有两种作用域：全局作用域和函数作用域（局部作用域）。函数内部可以直接读取全局变量，但是函数外部无法读取函数内部声明的变量。但是，有时候却需要在函数外部访问函数内部的变量或方法；正常情况下，这是无法访问的，只有通过在函数内部返回一个变量或方法来实现

```
function Person(){
	var name = '张三';
	return name;
}
function Person1(){
	var name = '李四';
	var say = function(){
		console.log(name);
	}
	return say;
}
Person();//张三
var p = Person1();
p();//李四
```
- 说明：函数say可以就在函数Person内部，这时Person函数内部所有的变量对say方法都是可访问的，但是反过来就不行，内部函数中的局部变量对父类函数是不可访问的，这就是JavaScript中特有的链式作用域结构（也可以说词法作用域），子对象会一级一级地向上寻找所有父类对象的变量，所以，父对象的所有变量对子对象都是可见的，反之则不成立。

既然say方法可以读取Person中的局部变量，那么只要把say方法作为返回值，我们就可以在Person外部拂去它内部的变量了。

- 重点：

> 闭包就是上文中的函数say，能读取其它函数内部变量的函数。由于JavaScript中，只要函数内部的子函数才能够读取函数内部的局部变量，因此可以把闭包简单理解成“通过在函数内部返回一个变量或函数来访问其他函数内部的变量”。

## 垃圾回收机制及闭包

### 垃圾回收机制

在函数内部引入一个变量或者函数时，系统都会开辟一块内存空间；还会将这块内存的引用计数器初始化，初始化值为0；如果外部有全局变量或者程序引用了这块空间，则引用计数器会自动进行+1操作，当函数执行完毕后，变量计数器会重新归零，系统会运行垃圾回收机制，将函数运行产生的数据销毁；如果计数器不是0，则不会清除数据；这过程就是JavaScript的垃圾回收机制；

![JavaScript的垃圾回收机制原理图](https://i.imgur.com/zMkIdwk.jpg)

- 用闭包的分析更能体验此原理：

```
function func1(){
	var n = 100;
	function func2(){
		console.log(++n);
	}
	return func2;
}
var f = func();
f();//101
f();//102
f();//103
```

**代码分析（注释）：**

- 因函数fun1被调用时，返回的结果是fun2函数体，也就是说，fun2函数被当做返回值给fun1的调用者，但是fun2函数并没有在此被调用执行（只是把函数体返回到函数外了）;

- 因此整个fun1函数体，无法判断子函数fun2会对其产生何种影响，无法判断变量n是否会被使用，即使fun1函数被调用结束，整个fun1函数始终保留在内存中，不会被垃圾回收机制回收；

- 也就是运行代码发现，函数调用一次（在这里是指fun2），其变量n变化一次；

- 闭包的最大用处：可以读取函数内部的变量。

- 让函数内部读取的变量始终保持在内存中，即闭包可以使得它诞生环境一直存在。

- 注意的是，外层函数每次运行，都会生成一个新的闭包，而这个闭包有会保留外层函数的内部变量，所以内存消耗很大；因此不能滥用闭包，否则会造成网页的性能问题。

# call和apply方法

**关于JavaScript中的this的指向：**

1. 全局作用域下，this指向window对象

2. 构造函数中，this指向实例化对象

**如果要在调用函数是直接修改函数内部的this指向使用call或者apply方法来修改指向。**

1. call方法格式：

函数名称.call(obj,arg1,arg2,...argN);说明其中：obj是函数内this要指向的对象，arg列表是参数列表，参数与参数之间使用一个逗号隔开

2. apply方法格式：

函数名称.apply(obj,[arg1,,arg2,...argN]);说明其中：obj是函数内this要指向的对象，arg列表是参数列表，要求格式为数组

**两种修改this指向方法的区别：**

- 相同点：功能完全一样，都是为了改变函数内部的hits指向，唯一的不同就在于参数传递方式不同

- 不同点：call方法可能多个参数，第一个要指向的对象，其他参数为函数的实参；apply方法最多只能有两个实参，第一个要指向的对象，第二个是数组，数组内容为函数的实参。

# 定时器

前端中的定时器方法是浏览器提供的，并不是ECMAScript规范中的。是window对象的方法。

**JavaScript提供定时执行代码的功能叫做定时器；**

1. `setTimeout()`:用来指定某个函数或某代码，在多少秒之后执行。

2. `setInterval()`:指定某个任务每隔一段时间就执行一次，也就是无限次的定时执行。

> `setTimeout()`,`setInterval()`的第一个参数都是指定执行的函数名称或者代码段，第二个参数是时间：

这两个方法的参数是一模一样的：

1. 正常使用的话，至少需要有两个参数。【这一条可以忽略】

2. 不想出现报错的话，至少必须得一个参数。

3. 都可以传递无数个参数。

4. 第一个参数是要执行的js语句，有三种以上的情况
4.1 字符串："console.log('I am Pelli');"【可读性差，推荐指数：1/10】
4.2 匿名函数:function(){console.log("I am Pelli")}【可读性强，推荐指数：8/10】
4.3 函数名:showName【可读性强,耦合度低，可装逼，推荐指数：9/10】
4.4 其他乱七八糟的内容【非正常程序员干的事情】

5. 第二参数是用来描述时间，以毫秒为单位。有一种以上的情况
5.1 数值【正常情况下】
5.2数值以外的其他数据类型【异类程序员干的事情】

6. 如果第一个参数为函数，那么从第三个参数开始到最后一个参数就是该函数的实参

> 关于JavaScript定时器，可以说的东西太多了，这里暂时先介绍到这里。下面留几个面试题，大家欣赏一下。

```
// 1.下面的代码输出名字的顺序是什么？
console.log("小明");
var myname = setTimeout(function(){
    console.log("小华");
},0);
console.log("小丽");

// 2.下面的代码，会弹出什么内容？
var alert_things = setTimeout(function(){
    alert("Pelli");
},2000);
while(true){}

// 3.下面的代码，会输出什么？
setTimeout(function(){
    console.log(this);
},1000);

// 4.下面的代码，会输出什么？
for (var i = 0; i < 3; i++) {
     setTimeout(function() {
         console.log(i);
     }, 0);
     console.log(i);
}

// 5.下面的代码，会弹出哪些东西？
var len=4;
while(len--){
    setTimeout(function(){
        alert(len);
    },0);
    alert(len);
}

// 6.下面的代码是什么结果？
var timer = setInterval(function(){
    console.log("hello world");
},0);

clearInterval(timer);

// 7.下面的代码会弹出什么？
var t = true;
window.setTimeout(function (){
    t = false;
},0);
while (t){}
alert('end');
```

**另外有一些开发过程中的最佳实践：**

1. setTimeout和setInterval的第一个参数不建议传递字符串，传递字符串和eval有一样的问题，会造成性能方面的问题，甚至引起安全问题。

2. 建议用setTimeout模拟setInterval，不建议使用setInterval。页面中setTimeout和clearTimeout不配合使用基本上不会出现问题，因为setTimeout只执行一次，不会造成累加的问题，使用setInterval的话，强烈建议在适当时候使用clearInterval，在这种情况，很容易出现问题。