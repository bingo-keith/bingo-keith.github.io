---

title: JavaScript的语法糖（类）
date: 2017/8/15 23:46:34 
tags: JavaScript 语法糖 类

---
# 前言

语法糖(Syntactic sugar),是由Peter J. Landin(和图灵一样的天才人物，是他最先发现了Lambda演算，由此而创立了函数式编程)创造的一个词语，它意指那些没有给计算机语言添加新功能，而只是对人类来说更“甜蜜”的语法。语法糖往往给程序员提供了更实用的编码方式，有益于更好的编码风格，更易读。不过其并没有给语言添加什么新东西。

之所以叫「语法」糖，不只是因为加糖后的代码功能与加糖前保持一致，更重要的是，糖在不改变其所在位置的语法结构的前提下，实现了运行时等价。可以简单理解为，加糖后的代码编译后跟加糖前一毛一样。之所以叫语法「糖」，是因为加糖后的代码写起来很爽，包括但不限于：代码更简洁流畅，代码更语义自然... 写得爽，看着爽，就像吃了糖。效率高，错误少... 我觉着 js 里的对象原型继承有点「咸」，不然 ES6 也不会急着加糖（纯吐槽）简单说，语法糖就是为了避免coder出现错误并提高效率的语法层面的一种优雅的解决方案。

听到了面向对象，可能很多人觉得很兴奋，毕竟现在JS是稳稳前十的计算机语言，随着全栈时代的到来，JS的市场日益壮大，不过，说到类，JS还真是与众不同。
JS刚开始是一个脚本语言，并没有像C++，JAVA，PHP这样正统的类，JS的类，只是基于原型的语法糖，所以，理解上，还是会有点别扭。

<!-- more -->

# 类的定义

JS中的类，一般使用函数作为载体而存在，而JS中函数又是对象，所以这里温习一下对象的定义：

```
//对象的直接量
var Biology_1 = {};
 
//new来创建
var Biology_2 = new Object();
 
//Object.create()创建,es5以上支持
var Biology_3 = Object.create(null);
```

> 与此同时，顺便提下三个词：
- prototype 实例指向原型，构造函数指向原型。
- construtor 原型指向构造函数。该构造函数的原型指向Object，而该Object的的construtor 又指回该构造函数。或者又说，属性返回对创建此对象的数组函数的引用。
- __proto__ 实例指向构造函数的原型（其中，如果原型是Object,则它的__proto__ 指向function,而该function的__proto__ 又指向Object）。
- 听着有点绕，看看实例就明白了。

想必听过诸多所谓类创建的模式，无非是使用函数与对象模拟：

## 工厂模式

```
function Biology(name) {
    var tempObj = new Object();
    tempObj.name = name;
    tempObj.printName = function(){
        console.log(this.name);
    };
    return tempObj;
}
var animals = Biology('animals');
var plants = Biology('plants');
animals.printName();//animals
plants.printName();//plants
animals.constructor;//function Object() { [native code] }
```

> 其实是实例化Object，缺点是无法识别对象类型

## 构造模式

```
function Biology(name) {
    this.name = name;
    this.printName = function(){
        console.log(this.name);
    };
}
var animals = new Biology('animals');
var plants = new Biology('plants');
animals.printName();//animals
plants.printName();//plants
animals.constructor;//function Biology(name){...}
```

> 构造函数的方式，实现对象类型识别,不过性能或封装性差

## 原型模式

```
function Biology() {};
Biology.prototype.name = 'biology';
Biology.prototype.printName = function(){
    console.log(this.name);
};
var animals = new Biology();
animals.printName();//biology
var plants = new Biology();
plants.printName();//biology
 
console.log(animals.name);//biology
animals.name = 'animals';
console.log(animals.name);//animals
console.log(plants.name);//biology
animals.printName = function(){
    console.log('animals');
};
animals.printName();//animals
plants.printName();//biology
```

> 原型的属性方法是所有实例共享的，而如果实例属性（方法）与原型属性（方法）重复时，优先实例属性（方法），其中有个hasOwnProperty用来判断是否实例属性。

因为上述例子中，使用Biology.prototype.name,Biology.prototype.printName这样的直接修改，过于分散与重复，于是我们进行一个合并：

```
function Biology() {};
Biology.prototype = {
    name : 'biology',
    printName : function(){
        console.log(this.name);
    }
};
var biology = new Biology();
console.log(biology.name);//biology
console.log(biology.constructor);//function Object() {}
```

> 合并后，看起来没什么问题，不过biology的构建函数不再是Biology，那并不是我们想要的。所以把构建函数指回Biology

```
function Biology() {};
Biology.prototype = {
    constructor : Biology,
    name : 'biology',
    printName : function(){
        console.log(this.name);
    }
};
var biology = new Biology();
console.log(biology.name);//biology
console.log(biology.constructor);//function Biology() {}
```

> 已经如我们所愿，这是因为创建一个函数，同是会创建他的prototype对象，同时这对象自动获得constructor，重写prototype时，constructor不再指向原Biology。

那如果实例化之后修改原型，会发生什么？

```
function Biology() {};
var biology = new Biology();
Biology.prototype = {
    constructor : Biology,
    name : 'biology',
    printName : function(){
        console.log(this.name);
    }
};
console.log(biology.name);//undefined
```

> undefined!，你没有看错。
> 如之前所说，同时，因为实例指向的是原型，而不是构造函数，所以，如果在实例化之后原型有变化，原来的实例指向的还是旧原型

不过总的来说，原型模式如果修改的原型属性或方法，之后的创建的实例都会变化，没有隐私而言，所以还有改进空间

## 构造模式与原型模式组合

```
function Biology(age) {
    this.age = age;
};
Biology.prototype = {
    constructor : Biology,
    name        : 'biology',
    printAll   : function(){
        console.log(this.name + ':' + this.age);
    }
};
var biology_1 = new Biology(16);
var biology_2 = new Biology(18);
biology_1.printAll();//biology:16
biology_2.printAll();//biology:18
```

> 把共享部分使用原型，不公享部分使用构造，本方法比较常见

其他像的模式还有很多，像什么动态原型模式、寄生构造函数模式，原理上都是使用上述方式自由组合，就不一一列举了。

# 类的属性与方法

比较Java，类有私有属性，公共属性，私有方法，公共方法，静态属性，静态方法，可惜在JS没有这些好东西，不过，我们知道JS中var有作用域以及原型的概念，所以我们可以模拟出这些东西。

```
function Biology(){
    //私有方法
    function privateMethod(){
        return '私有方法';
    }
 
    //私有属性
    var privateProperty = '私有属性';
    //公共属性
    this.publicProperty = '公共属性';
    //静态属性
    Biology.prototype.staticProperty = '静态属性';
    //私有方法
    /*function privateMethod(){
        return '私有方法';
    }*/
    var privateMethod = function(){
        return '私有方法';
    };
    //公共方法
    this.publicMethod = function(){
        return '公共方法';
    };
    //静态方法
    Biology.prototype.staticMethod = function(){
        return '静态方法';
    };
    //获得私有属性
    this.getPrivateProperty = function(){
        return privateProperty;
    };
    //设置私有属性
    this.setPrivateProperty = function(property){
        privateProperty = property;
    };
    //执行私有方法
    this.getPrivateMethod = function(){
        return privateMethod();
    };
}
var biology = new Biology();
var biology_2 = new Biology();
 
console.log(biology.privateProperty);//undefined
 
console.log(biology.publicProperty);//公共属性
biology.publicProperty = '修改后的公共属性';
console.log(biology.publicProperty);//修改后的公共属性
console.log(biology_2.publicProperty);//公共属性
 
console.log(biology.getPrivateProperty());//私有属性
biology.setPrivateProperty('改变后的私有属性');
console.log(biology.getPrivateProperty());//改变后的私有属性
console.log(biology_2.getPrivateProperty());//私有属性
console.log(privateProperty);//undefined
console.log(biology.privateProperty);//underfined
 
console.log(biology.staticProperty);
Biology.prototype.staticProperty = '修改后的静态属性';
console.log(biology.staticProperty);
console.log(biology_2.staticProperty);
 
 
console.log(biology.privateMethod);//undefined
console.log(biology.publicMethod());//公共方法
console.log(biology.getPrivateMethod());//私有方法
 
console.log(biology.staticMethod());
Biology.prototype.staticMethod = function(){
    return '修改后的静态方法';
};
console.log(biology.staticMethod());//修改后的静态方法
console.log(biology_2.staticMethod());//修改后的静态方法
```

# 类的继承

## 原型继承

```
function Biology(name) {
    this.name = name;
}
Biology.prototype = {
    constructor : Biology,
    printName : function(){
        console.log(this.name);
    }
};
function Animals(){};
Animals.prototype = new Biology('animals');
var animals = new Animals();
animals.printName();//animals
```

不过，为了便于使用与理解，我们一般会换成形式写

```
var Biology = {
    name : 'I am Biology'
};
 
//继承parent
function inherit(parent){
    var type = typeof parent;
    if(!(type === 'object' || type === 'function')) throw TypeError;
    if(Object.create){
        return Object.create(parent);
    }
    function temp() {};
    temp.prototype = parent;
    return new temp;
}
 
var animals = inherit(Biology);
 
console.log(animals.name);//I am Biology
```

## 构造函数继承

```
//构造函数继承
function Biology(name) {
    this.name = name;
};
function Animals(name,age){
    this.temp = Biology;
    this.temp(name);
    this.age = age;
    this.printAll = function(){
        console.log(this.name + ':' + this.age);
    }
}
var animals = new Animals('animals',16);
animals.printAll();//animals:16
```

## apply,call继承

```
function Biology(name) {
    this.name = name;
    this.printName = function(){
        console.log(this.name);
    }
}
function Animals(age){
    this.age = age;
    Biology.call(this,age);
}
function Plants(address){
    this.address = address;
    Biology.apply(this,[address]);
}
var animals = new Animals('animals',16);
animals.printName();//animals
var plants = new Plants('plants','深圳');
plants.printName();//plants
```

# ES6类

ES6则在上面基础上，增加了像class,extends,super等面向对象关键字，在写法上简化了类的写法，然后，这些关键字也是在之前的基础上模拟出来的，并且与原型息息相关，甚至有时带来更多的问题。

下面这个例子，简单的对ES6对类进行阐述：

```
class Biology {
    constructor(name){
        this.name = name;
    }
    printName(){
        console.log(this.name);
    }
    static printNameTwice(){
        console.log(this.name + ',' + this.name);
    }
}
var biology = new Biology('biology');
biology.printName();//biology
Biology.printNameTwice();//biology,biology
 
class Animals extends Biology {
    constructor(name,age){
        super(name);
        this.age = age;
    }
    printAll(){
        console.log(this.name + ':' + this.age);
    }
    printName(){
        console.log('animals name');
    }
    get name() {
        return this._name.toUpperCase();
    }
    set name(name) {
        this._name = name;
    }
}
var animals = new Animals('animals',16);
console.log(animals.name);//ANIMALS
console.log(animals._name);//animals
animals.printName();//animals name
animals.printAll();//ANIMALS:16
Animals.printNameTwice();//Animals,Animals
```
