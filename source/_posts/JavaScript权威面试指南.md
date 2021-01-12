---

title: JavaScript概念整理
date: 2017/12/11 22:44:18 
tags: Javascript 概念 理论 

---

> 这篇文章的目的是汇集 JavaScript 相关概念。有了这篇文章，你就可以在一个地方温故一下你需要了解的关于 JavaScript 的所有知识点。

<!-- more -->

# 类型及其转换

JavaScript 内置了7种类型：`null`, `undefined` , `boolean`, `number`, `string`, `object`以及 `symbol (ES6)`.

除了 object 以外，这几种类型都可以归类为**原始类型（基本类型）**。

```
typeof 0              // number
typeof true           // boolean
typeof 'Hello'        // string
typeof Math           // object
typeof null           // object  !!
typeof Symbol('Hi')   // symbol (New ES6) 

```

- Null 与 Undefined

`Undefined` 表示尚未定义。它被用作表示未初始化变量，未提供的函数参数，对象缺少的属性的默认值。当函数没有返回值时也会默认返回 undefined .

`Null` 则表示空值。它可以被赋值给一个变量来表示的“没有值”。

- 隐式类型转换

让我们来看下面这个栗子：

```
var name = 'Joey';
if (name) {
  console.log(name + " doesn't share food!")  // Joey doesn’t share food!
} 
```

在这种情况下，字符串变量 `name` 会被转换为 `true` ，所以我们能够在命令行里输出这段话。那我们到底要如何确定这些真假值的转换呢？

Falsy 类型的值是指在强制类型转换时会被转换为布尔 `false` 的值。

Falsy 类型值包括：`""`, `0`, `null`, `undefined`, `NaN`, `false`.

除了 Falsy 类型值以外的都被称为 truthy 类型值，它们会被转换为 `true`.

```
Boolean(null)         // false
Boolean('hello')      // true 
Boolean('0')          // true 
Boolean(' ')          // true 
Boolean([])           // true 
Boolean(function(){}) // true 
```

没错。你可能注意到了。空的数组，对象和函数布尔值也会被转换为 `true` ！

- String & Number 运算

你需要注意的第一件事是 + 操作符。这是一个棘手的操作符，因为它同时适用于数值运算和字符串连接。

但是，`*`，`/`和`-`操作符都是数字运算专用的。当这些运算符与字符串一起使用时，会强制转换字符串为数字类型的值。

```
1 + "2" = "12"
"" + 1 + 0 = "10"
"" - 1 + 0 = -1
"-9\n" + 5 = "-9\n5"
"-9\n" - 5 = -14
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$45"
"4" - 2 = 2
"4px" - 2 = NaN
null + 1 = 1
undefined + 1 = NaN 
```

- == 与 ===

对此大多数人的理解肯定是 `==` 只比较值相等，而 `===` 在比较的同时还会检查类型。不过这种解释其实是错误的。

事实上，`==` 是比较强制类型转换之后的结果，而 `===` 则是直接比较。

```
2 == '2'            // True
2 === '2'           // False
undefined == null   // True
undefined === null  // False 
```

强制类型转换有时候会造成一些混乱，我们来看下面这个栗子：

```
let a = '0';
console.log(Boolean(a)); // True
let b = false;
console.log(Boolean(b)); // False 
```
好，根据以上结果，请告诉我下面这此比较的输出：

```
console.log(a == b); //(1) 
```

上述代码事实上返回了 True，那么这是为什么呢？

在 JavaScript 的机制中，当你拿一个其它类型的值和 `boolean` 类型进行比较时，JavaScript 会将这个布尔值转换为 `number` 类型再进行比较。（2）

此时这个问题就变成了 `number` 和 `string` 类型值之间的比较。根据我们之前所说的，JavaScript 会把 `string` 类型转换为 `number` 类型再比较两个数字。（3）

所以最后的表达式被转换为了 `0 == 0`，结果理所当然是 True.

```
'0' == false   //(1)
'0' == 0       //(2)
 0  == 0       //(3) 
```

如果你想全面理解这种比较是如何进行的，可以查阅ES5文档。

这里也有一个 JavaScript 当中各种类型值对比结果的参照表。

下面是一些特殊比较的示例：

```
false == ""  // true
false == []  // true
false == {}  // false
"" == 0      // true
"" == []     // true
"" == {}     // false
0 == []      // true
0 == {}      // false
0 == null    // false 
```

# 赋值与引用

简单值（或者称为原始值 primitives）均是 `null`, `undefined` , `boolean`, `number`, `string`, 以及 `symbol (ES6)`类型的赋值。

复合值则是对 `object`，包含 `array` 以及 `function` 在内的引用。

```
var a = 2;        // 'a' hold a copy of the value 2.
var b = a;        // 'b' is always a copy of the value in 'a'
b++;
console.log(a);   // 2
console.log(b);   // 3
var c = [1,2,3];
var d = c;        // 'd' is a reference to the shared value
d.push( 4 );      // Mutates the referenced value (object)
console.log(c);   // [1,2,3,4]
console.log(d);   // [1,2,3,4]
/* Compound values are equal by reference */
var e = [1,2,3,4];
console.log(c === d);  // true
console.log(c === e);  // false 
```

当你想要复制一份复合值的时候，你需要通过一些方法对其进行**拷贝**（这里区分浅拷贝及深拷贝）。这样引用才不会指向同一个值：

```
const copy = c.slice()    // 'copy' references to a new value
console.log(c);           // [1,2,3,4]
console.log(copy);        // [1,2,3,4]
console.log(c === copy);  // false 
```

# 作用域

作用域是指代码执行时的上下文，它定义了变量以及函数生效的范围。

**全局作用域**指的是最外层的作用域。所有在函数外部声明的变量都会在全局作用域当中，可以在任何地方访问到。在浏览器当中，window 对象就属于全局作用域。

**局部作用域**是指例如在某个函数内部的范围。在局部作用域中声明的变量只能够在其内部被访问到。

```
function outer() {
  let a = 1;
  function inner() {
    let b = 2;
    function innermost() {
      let c = 3;
      console.log(a, b, c);   // 1 2 3
    }
    innermost();
    console.log(a, b);        // 1 2 — 'c' is not defined
  }
  inner();
  console.log(a);             // 1 — 'b' and 'c' are not defined
}
outer(); 
```

为了便于理解，你可以把作用域想象为一扇扇从大到小排列的门。最矮的人（上述代码1，2，3表示高矮）可以进入最小的门（innermost 的作用域），同时也能通过最大的门（outer 的作用域）。

而个子高的人在通过门时则会被卡住（也就是在外部访问不到内部定义的变量）。

# 变量提升 Hoisting

在编译过程中，JavaScript 会自动把 `var` 和 `function` 声明移动到顶部的行为被称为 **hoisting**.

函数声明会被完整地提升。这就意味着你在编写代码时可以在声明一个函数之前就调用它：

```
console.log(toSquare(3));  // 9

function toSquare(n){
  return n*n;
} 
```

变量只会被部分提升。例如只有 `var` 的声明会被提升，而赋值则不会。

`let` 以及 `const` 也不会被提升。

```
{  /* Original code */
  console.log(i);  // undefined
  var i = 10
  console.log(i);  // 10
}

{  /* Compilation phase */
  var i;
  console.log(i);  // undefined
  i = 10
  console.log(i);  // 10
}
// ES6 let & const
{
  console.log(i);  // ReferenceError: i is not defined
  const i = 10
  console.log(i);  // 10
}
{
  console.log(i);  // ReferenceError: i is not defined
  let i = 10
  console.log(i);  // 10
} 
```

# 函数表达式 与 函数声明

- 函数表达式

函数表达式只有被执行之后才可用，它不会被提升（相当于赋值函数表达式给变量）。

```
var sum = function(a, b) {
  return a + b;
} 
```

- 函数声明

函数声明则可以在定义前后被任意调用，因为它最终会被提升。

```
function sum(a, b) {
  return a + b;
}
```

# 变量声明方式：var let cost

在 ES6 之前，只有 `var` 一种声明变量的方法。在某一函数内部声明的变量和方法只能在其函数作用域内部访问到。

一些在例如 `if` 或者 `for` 语句的块作用域内声明的变量，能够在其包含的大括号外被访问到。

注意：未使用 `var`，`let` 或 `const` 关键字声明的变量会自动变成全局变量。

```
function greeting() {
  console.log(s) // undefined
  if(true) {
    var s = 'Hi';
    undeclaredVar = 'I am automatically created in global scope';
  }
  console.log(s) // 'Hi'
}
console.log(s);  // Error — ReferenceError: s is not defined
greeting();
console.log(undeclaredVar) // 'I am automatically created in global scope' 
```

ES6 中我们有了 `let` 和 `const` 这两个新的关键字。它们不会被提升，且可以在块级作用域内生效：

```
let g1 = 'global 1'
let g2 = 'global 2'
{   /* Creating a new block scope */
  g1 = 'new global 1'
  let g2 = 'local global 2'
  console.log(g1)   // 'new global 1'
  console.log(g2)   // 'local global 2'
  console.log(g3)   // ReferenceError: g3 is not defined
  let g3 = 'I am not hoisted';
}
console.log(g1)    // 'new global 1'
console.log(g2)    // 'global 2' 
```

另外有一个普遍的误解是 `const` 是不可变的。事实上它只是不能被重新赋值，但其指向的值是可以被操作的：

```
const tryMe = 'initial assignment';
tryMe = 'this has been reassigned';  // TypeError: Assignment to constant variable.
// You cannot reassign but you can change it…
const array = ['Ted', 'is', 'awesome!'];
array[0] = 'Barney';
array[3] = 'Suit up!';
console.log(array);     // [“Barney”, “is”, “awesome!”, “Suit up!”]
const airplane = {};
airplane.wings = 2;
airplane.passengers = 200;
console.log(airplane);   // {passengers: 200, wings: 2} 
```

# 闭包 Closure

闭包是一个函数及其词法环境的组合。闭包可以让我们在一个函数的作用范围外访问其内部的变量。

```
function sayHi(name){
  var message = `Hi ${name}!`;
  function greeting() {
    console.log(message)
  }
  return greeting
}
var sayHiToJon = sayHi('Jon');
console.log(sayHiToJon)     // ƒ() { console.log(message) }
console.log(sayHiToJon())   // 'Hi Jon!' 
```

1. 获取变量到外部作用域。

返回的 `greeting` 方法访问了其内部作用域的 `message` 变量。

2. 即使在外部函数返回之后也能访问到作用域外的变量。

`sayHiToJon` 是在调用 `sayHi` 方法时对 `greeting` 的引用。这样我们就能够通过 `greeting` 访问到作用域外的 `message` 变量。

闭包比较有用的地方在于我们可以用它来实现**数据封装**。有一些理念认为，有些数据不应该直接暴露在外，我们举个栗子来说明：

例如在下面这段代码当中，我们只能过通过 `elementary` 的实例来访问和操作 `SpringfieldSchool` 内部的 `staff` 变量，而不用担心其在其他地方被改动。

```
function SpringfieldSchool() {
  let staff = ['Seymour Skinner', 'Edna Krabappel'];
  return {
    getStaff: function() { console.log(staff) },
    addStaff: function(name) { staff.push(name) }
  }
}

let elementary = SpringfieldSchool()
console.log(elementary)        // { getStaff: ƒ, addStaff: ƒ }
console.log(staff)             // ReferenceError: staff is not defined
/* Closure allows access to the staff variable */
elementary.getStaff()          // ["Seymour Skinner", "Edna Krabappel"]
elementary.addStaff('Otto Mann')
elementary.getStaff()          // ["Seymour Skinner", "Edna Krabappel", "Otto Mann"] 
```

我们接着再来用闭包解决一个面试中最常见的问题吧：

下面这段代码存在什么问题？如何输出我们想要的结果？

```
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log(`The value ${arr[i]} is at index: ${i}`);
  }, (i+1) * 1000);
} 
```

这段代码执行的输出结果中，每次 `i` 都等于4.这是因为 `setTimeout` 方法执行的时候，循环早已运行结束。

我们可以通过立即执行函数 IIFE 来解决这个问题，它可以创建出独立的作用域来存储每次传入 `i` 的值。

```
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(`The value ${arr[j]} is at index: ${j}`);
    }, j * 1000);
  })(i)
} 
```

另外一个更简洁的答案是使用 `let` 关键字来声明变量 `i`，也能够得到相同的结果：

```
const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log(`The value ${arr[i]} is at index: ${i}`);
  }, (i) * 1000);
}
```

# 立即执行函数表达式 IIFE

正如其名，立即执行函数在定义时就会被执行。我们通常在创建一个新的变量作用域时使用到它。

表达式外的括号用来和函数声明作以区分。

结尾的括号表示对函数的调用。

```
var result = [];
for (var i=0; i < 5; i++) {
  result.push( function() { return i } );
}
console.log( result[1]() ); // 5
console.log( result[3]() ); // 5
result = [];
for (var i=0; i < 5; i++) {
  (function () {
    var j = i; // copy current value of i
    result.push( function() { return j } );
  })();
}
console.log( result[1]() ); // 1
console.log( result[3]() ); // 3 
```

通过使用IIFE你可以：

- 使你能过添加私有数据到一个方法中

- 创建新的作用环境

- 防止污染全局命名空间

# 上下文

上下文的概念经常会同作用域之间混淆。为了保持条理清晰，我们需要注意以下两条：

- 上下文是在函数被调用时确定的。它通常指的是你的代码当中某一部分的值。
- 
- 作用域值的则是变量能过被访问到的范围。

```
// 作用域
var param = 1;      // global scope
function myScope(){
    var param = 2;     // local scope
}
// 上下文
this.prop = 1;      // global context
function myContext(){
    this.prop = 2;   // local context
}
var myInstance = new myContext(); 
```

例如上述实例当中调用 `this` 的位置不同，`this` 的指向也是不同的，也就表示着不同的上下文；而作用域则是我们在编写代码时使用 `var` 关键字来确定的。

# 函数调用方式：call apply bind

这三种方法可以改变函数调用时 `this` 的指向，区别则在于函数调用的时候。

- `.call()` 会立即调用函数，并要求你按次序一个个传入参数。

- `.apply()` 也会立即调用函数，不过你需要以数组的形式传参。

- `.call()` 和 `.apply()` 效用几乎是相同的，它们都可以用来调用对象中的某个方法，具体怎么使用取决于你的使用场景里如何传参更方便。

应用示例：

```
const Snow = {surename: 'Snow'}
const char = {
  surename: 'Stark',
  knows: function(arg, name) {
    console.log(`You know ${arg}, ${name} ${this.surename}`);
  }
}
char.knows('something', 'Bran');              // You know something, Bran Stark
char.knows.call(Snow, 'nothing', 'Jon');      // You know nothing, Jon Snow
char.knows.apply(Snow, ['nothing', 'Jon']);   // You know nothing, Jon Snow 
```

注意： 如果你在使用 `.call()` 时传入了数组形式的参数，它会把整个数组当作一个参数使用。

不过在 ES6 里你倒是可以试试展开操作符的方法进行传参：

```
char.knows.call(Snow, ...["nothing", "Jon"]);  // You know nothing, Jon Snow 
```

`.bind()` 不会直接触发某个方法，而是根据你传入的参数和上下文返回一个新的方法。当你想要在程序之后的某些上下文环境中调用一个方法时可以使用 `.bind()` 这种方式。

这在我们使用一些异步操作或者事件处理函数时非常有用。`.bind()` 的传参形式则类似于 `.call()` 你需要讲参数以逗号分隔传入：

```
const Snow = {surename: 'Snow'}
const char = {
  surename: 'Stark',
  knows: function(arg, name) {
    console.log(`You know ${arg}, ${name} ${this.surename}`);}
  }
const whoKnowsNothing = char.knows.bind(Snow, 'nothing');
whoKnowsNothing('Jon');  // You know nothing, Jon Snow 
```

# this 关键字

JavaScript 当中的 `this` 理解起来算比较复杂的。

`this` 具体指向什么是由函数的执行上下文决定的（其实上一节已经有很多栗子了）。

关键字 `this` 类似于一个占位符。它会指向具体调用某个方法的对象。

下面这个检查表用来确定 `this` 的顺位规则：

- **new binding** 当使用 `new` 关键字调用某个函数时，`this` 会被绑定到新构造的对象上。

```
function Person(name, age) {
  this.name = name;
  this.age =age;
  console.log(this);
}
const Rachel = new Person('Rachel', 30);   // { age: 30, name: 'Rachel' } 
```

- **显式绑定** 当使用 `call` 或者 `apply` 方法调用函数时，`this` 会指向我们传入的对象。`bind` 我们在前面已经介绍过了，返回的是一个新的函数而不是直接调用。

```
function fn() {
  console.log(this);
}
var agent = {id: '007'};
fn.call(agent);    // { id: '007' }
fn.apply(agent);   // { id: '007' }
var boundFn = fn.bind(agent);
boundFn();         // { id: '007' } 
```

- **隐式绑定** 当我们在某一具体上下文中调用一个函数时，`this` 会指向这个函数所属的对象。

```
var building = {
  floors: 5,
  printThis: function() {
    console.log(this);
  }
}
building.printThis();  // { floors: 5, printThis: function() {…} } 
```

- **默认绑定** 如果函数在调用时不遵从上述所有规则，那么 `this` 会被绑定在全局对象上（在浏览器里，这个全局对象就是 `window`）。

这种情况会在我们定义独立函数时出现，如果一个函数在定义时不属于任何对象，它就会默认成为全局对象的一个属性。

```
function printWindow() {
  console.log(this)
}
printWindow();  // window object 
```

注意： 即使是在不同的作用域下，调用这种独立函数也不会改变其 `this` 的指向：

```
function Dinosaur(name) {
  this.name = name;
  var self = this;
  inner();
  function inner() {
    alert(this);        // window object — the function has overwritten the 'this' context
    console.log(self);  // {name: 'Dino'} — referencing the stored value from the outer context
  }
}
var myDinosaur = new Dinosaur('Dino'); 
```

- **语法绑定** 当你以 `=>` 箭头函数的形式调用某一方法时，相当于为其传入了当前执行上下文的 `this` 值。

```
function Cat(name) {
  this.name = name;
  console.log(this);   // { name: 'Garfield' }
  ( () => console.log(this) )();   // { name: 'Garfield' }
}
var myCat = new Cat('Garfield'); 
```

# 严格模式

我们可以通过使用` "use strict" `指令来启用 JavaScript 的严格模式。它会为你的代码添加更多的限制及错误处理。

使用严格模式的好处有：

- 更方便调试 你能够看到更多的报错，例如在你试图为只读的全局对象或属性赋值时。

- 防止意外产生全局变量 对未声明的变量进行赋值时会报错。

- 禁止无效的删除操作 尝试删除变量、函数、不可删除的属性时会报错。

- 禁止重复的属性名及参数 如果有命名重复的属性名或者参数值就会报错。

- 让 eval() 的调用更加安全 在 eval() 方法内部定义的变量及函数不会污染其他作用域。

- 禁止 this 指向全局对象 当 this 的值为 null 或者 undefined 时不会再默认指向到全局对象。这也就意味着在函数内部的 this 不会再默认指向 window 对象了。

# new 关键字

关键字 `new` 是一种非常特殊的调用函数的方法，被通过 `new` 关键字调用的函数被称为**构造函数**。

所以 `new` 到底进行了哪些操作呢？

- 创建了一个新的对象。

- 新对象的原型继承自构造函数的原型。

- 以新对象的 `this` 执行构造函数。

- 返回新的对象。如果构造函数返回了一个对象，那么这个对象会取代整个 `new` 出来的结果。

```
// In order to better understand what happens under the hood, lets build the new keyword 
function myNew(constructor) {
  var obj = {}
  Object.setPrototypeOf(obj, constructor.prototype);
  return constructor.apply(obj, [...arguments].slice(1)) || obj
} 
```

那么在调用函数时使用 `new` 关键字与否到底有什么区别呢？

```
function Bird() {
  this.wings = 2;
}
/* invoking as a normal function */
let fakeBird = Bird();
console.log(fakeBird);    // undefined
/* invoking as a constructor function */
let realBird= new Bird();
console.log(realBird)     // { wings: 2 } 
```

# 原型与继承

原型是 JavaScript 当中最容易造成困惑的一个概念。原因之一是因为**原型**这个词会在两个语境下使用：

- 原型关系

每个对象都有自己的原型对象，并会继承它原型的所有属性。

你可以通过 `.__proto__` 这种非标准的机制来获取一个对象的原型（在ES6中，在 ES5 标准里还可以通过 `Object.getPrototypeOf() `方法来获取）。

一般的对象还会继承一个叫做 `.constructor` 的属性指向其构造函数。当你使用构造函数生成一个对象时，其 `.__proto__` 属性会指向构造函数的 `.prototype` 属性。

- 原型属性

每个被定义的函数都有一个名为 `.prototype` 的属性。

它是一个继承了原型链上所有属性的对象。这个对象也默认包括一个 `.constructor` 属性，指向原始的构造函数。

所有用构造函数生成的对象也会继承一个指向这个函数的 `.constructor` 属性（用控制台把对象打出来会比较好理解，也可以看下面的示例）。

```
function Dog(breed, name){
  this.breed = breed,
  this.name = name
}
Dog.prototype.describe = function() {
  console.log(`${this.name} is a ${this.breed}`)
}
const rusty = new Dog('Beagle', 'Rusty');

/* .prototype property points to an object which has constructor and attached 
properties to be inherited by objects created by this constructor. */
console.log(Dog.prototype)  // { describe: ƒ , constructor: ƒ }

/* Object created from Dog constructor function */
console.log(rusty)   //  { breed: "Beagle", name: "Rusty" }
/* Object inherited properties from constructor function's prototype */
console.log(rusty.describe())   // "Rusty is a Beagle"
/* .__proto__ property points to the .prototype property of the constructor function */ 
console.log(rusty.__proto__)    // { describe: ƒ , constructor: ƒ }
/* .constructor property points to the constructor of the object */
console.log(rusty.constructor)  // ƒ Dog(breed, name) { ... }
```

## 原型链

原型链描述了对象之间相互引用的关系。

当获取一个对象的属性时，JavaScript 引擎会先从这个对象本身开始查找。如果没有找到，就会转向其原型上的属性，直到第一次找到这个属性为止。原型链上的最后一个对象是内置的 `Object.prototype` 而它的原型则是 `null`（也就是所谓原型链的终点）。JavaScript 引擎在查找属性到这一层还是没有找到时就会返回 `undefined`.

## 自有属性与继承属性

对象的属性分为自有和继承两种。

自有属性也就是在对象内部定义的属性。

继承属性则是通过原型链获得的属性。继承属性是不可枚举的（也就是在 `for/in` 循环里看不到的）。

```
function Car() { }
Car.prototype.wheels = 4;
Car.prototype.airbags = 1;

var myCar = new Car();
myCar.color = 'black';

/*  Check for Property including Prototype Chain:  */
console.log('airbags' in myCar)  // true
console.log(myCar.wheels)        // 4
console.log(myCar.year)          // undefined

/*  Check for Own Property:  */
console.log(myCar.hasOwnProperty('airbags'))  // false — Inherited
console.log(myCar.hasOwnProperty('color'))    // true
```

**Object.create(obj)** 方法可以指定原型来创建对象：

```
var dog = { legs: 4 };
var myDog = Object.create(dog);

console.log(myDog.hasOwnProperty('legs'))  // false
console.log(myDog.legs)                    // 4
console.log(myDog.__proto__ === dog)       // true 
```

## 通过引用继承

继承属性是原型对象上属性的一份引用拷贝。

如果对象在原型上的属性被改变了，继承这一属性的对象也会受到相同的影响。但假如属性被整个替换掉了，则不会影响到继承的对象：

```
var objProt = { text: 'original' };
var objAttachedToProt = Object.create(objProt);
console.log(objAttachedToProt.text)   // original

objProt.text = 'prototype property changed';
console.log(objAttachedToProt.text)   // prototype property changed

objProt = { text: 'replacing property' };
console.log(objAttachedToProt.text)   // prototype property changed 
```

## 类继承与原型继承

在类继承模式当中，对象继承自各式各样的类。就好像为对象设置好了一些模版和描述，并创建出了子类关系。我们可以通过 `new` 关键字从构造函数中创建这样的对象。

但类继承模式有以下这些缺点：

- 死板的层级关系

- 高度耦合

- 基类薄弱

- 代码重复

- 以及传说中的大猩猩/香蕉问题——“What you wanted was a banana, what you got was a gorilla holding the banana, and the entire jungle.”

在原型继承模式中，一个对象可以直接继承另一个对象。我们可以通过 `Object.create()` 或者其他对象方法及工厂函数来创建对象。

下面是三种不同的原型继承的形式：

- 原型代理 我们通常称那些用来给其他对象提供模型的对象为代理原型。当你继承一个代理原型时，新的对象可以获取对代理原型属性的引用。这种操作通常使用 Object.create() 来完成。

- 拼接继承 通过拷贝一个对象的属性来进行继承，不会产生引用关系，这种操作通常使用 Object.assign() 来进行。

- 函数继承 这种方法是通过工厂函数（工厂函数是指不通过 new 关键字创建并返回对象的函数）来创建对象，然后直接将新的属性添加到对象上。这种形式的好处是我们可以使用闭包进行数据封装。

```
const person = function(name) {
  const message = `Hello! My name is ${name}`;
  return { greeting: () => console.log(message) }
}
const will = person("Will");
will.greeting();     // Hello! My name is Will 
```

## 组合与继承

开发者都公认在大多数情况下应该避免使用类继承。因为在这种模式下，你需要很清楚地定义设计你的代码它们是什么。

而另一方面，组合的模式则允许你从你的代码要做什么的角度来定义，从而提高你代码的灵活度和可复用性（React组件就是一个非常好的栗子）。

# JavaScript 异步

JavaScript 是一种单线程编程语言。这意味着 JavaScript 引擎一次只能处理一段代码。其主要影响之一是，当 JavaScript 遇到一段需要很长时间处理的代码时，它会阻塞后续的代码运行。

JavaScript 使用了一种存储运行函数信息的数据结构叫做**调用栈**。你可以吧调用栈想象为一摞书。每本放上去的书都会在之前的一摞书上面。最后放上去的书会最早被拿走，最先放上去的书则到最后才会被拿走（LIFO）。

用来解决大段代码非阻塞执行的方法叫做异步回调函数。这类函数可以推迟执行。

异步过程从一个放入堆或内存中的异步回调函数开始。你可以将堆视为一个**事件管理器（事件队列）**。调用栈可以访问事件管理器只在某个事件发生时才执行特定的函数。当某个事件发生后，事件管理器就会将该函数移至回调用队列。*注意：当事件管理器处理一个函数时，之后的代码不会被阻塞并且能继续执行*。

调用栈会不断检查其是否空闲。当它空闲时，如果有一个等待被调用的函数，则会检查调用队列。当有函数在等待执行时，队列中的第一个函数被压入调用栈并执行。这一过程被称为事件循环当中的一次执行循环.

我们还是用一个栗子来解释吧：

```
const first = function () {
  console.log('First message')
}
const second = function () {
  console.log('Second message')
}
const third = function() {
  console.log('Third message')
}

first();
setTimeout(second, 0);
third();

// Output:
  // First message
  // Third message
  // Second message 
```

1. 初始化浏览器的控制台，清空调用栈及事件管理器。

2. first() 加入调用栈。

3. console.log("First message") 加入调用栈。

4. console.log("First message") 被执行并在浏览器控制台打印出"First message".

5. console.log("First message") 被移出调用栈。

6. first() 被移出调用栈。

7. setTimeout(second, 0) 加入调用栈。

8. setTimeout(second, 0) 被执行并由事件管理器管理。0ms 之后事件管理器将 second() 移入调用队列。

9. setTimeout(second, 0) 完成执行并被移出调用栈。

10. third() 加入调用栈。

11. console.log("Third message") 加入调用栈。

12. console.log("Third message") 被执行并在浏览器控制台打印出"Third message".

13. console.log("Third message") 被移出调用栈。

14. third() 被移出调用栈。

15. 调用栈空闲，second() 在调用队列中等待执行。

16. 事件管理器将 second() 从调用队列中移入调用栈。

17. console.log("Second message") 加入调用栈。

18. console.log("Second message") 被执行并在浏览器控制台打印出"Second message".

19. console.log("Second message") 被移出调用栈。

20. second() 被移出调用栈。

*注意：你为 setTimeout 方法传入的时间参数并不是 second() 延迟执行的时间，而是**事件管理器（事件队列）**将函数移入调用栈的等待时间。*

