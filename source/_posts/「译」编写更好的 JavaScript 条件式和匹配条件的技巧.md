---

title: 「译」编写更好的 JavaScript 条件式和匹配条件的技巧
date: 2019/6/26 20:46:29 
tags: JavaScript 代码 风格 编码 技巧

---

10. 原文地址：Tips and Tricks for Better JavaScript Conditionals and Match Criteria
- 原文作者：Milos Protic

# 介绍

如果你像我一样乐于见到整洁的代码，那么你会尽可能地减少代码中的条件语句。通常情况下，面向对象编程让我们得以避免条件式，并代之以继承和多态。我认为我们应当尽可能地遵循这些原则。

正如我在另一篇文章 [JavaScript 整洁代码的最佳实践](https://devinduct.com/blogpost/22/javascript-clean-code-best-practices)里提到的，你写的代码不单单是给机器看的，还是给“<b>未来的自己</b>”以及“<b>其他人</b>”看的。

从另一方面来说，由于各式各样的原因，可能我们的代码最终还是会有条件式。也许是修复 bug 的时间很紧，也许是不使用条件语句会对我们的代码库造成大的改动，等等。本文将会解决这些问题，同时帮助你组织所用的条件语句。

<!-- more -->

# 技巧

以下是关于如何构造 `if...else` 语句以及如何用更少的代码实现更多功能的技巧。阅读愉快！

## 1. 要事第一。小细节，但很重要

不要使用否定条件式（这可能会让人感到疑惑）。同时，使用条件式简写来表示 `boolean` 值。这个无须再强调了，尤其是否定条件式，这不符合正常的思维方式。

不好的：

```js
const isEmailNotVerified = (email) => {
    // 实现
}

if (!isEmailNotVerified(email)) {
    // 做一些事...
}

if (isVerified === true) {
    // 做一些事...
}
```

好的：

```js
const isEmailVerified = (email) => {
    // 实现
}

if (isEmailVerified(email)) {
    // 做一些事...
}

if (isVerified) {
    // 做一些事...
}
```

现在，理清了上面的事情后，我们就可以开始了。

## 2. 对于多个条件，使用 `Array.includes`

假设我们想要在函数中检查汽车模型是 `renault` 还是 `peugeot`。那么代码可能是这样的：

```js
const checkCarModel = (model) => {
    if(model === 'renault' || model === 'peugeot') { 
    console.log('model valid');
    }
}

checkCarModel('renault'); // 输出 'model valid'
```

考虑到我们只有两个模型，这么做似乎也还能接受，但如果我们还想要检查另一个或者是几个模型呢？如果我们增加更多 `or` 语句，那么代码将变得难以维护，且不够整洁。为了让它更加简洁，我们可以像这样重写函数

```js
const checkCarModel = (model) => {
    if(['peugeot', 'renault'].includes(model)) { 
    console.log('model valid');
    }
}

checkCarModel('renault'); // 输出 'model valid'
```

上面的代码看起来已经很漂亮了。为了更进一步改善它，我们可以创建一个变量来存放汽车模型：

```js
const checkCarModel = (model) => {
    const models = ['peugeot', 'renault'];

    if(models.includes(model)) { 
    console.log('model valid');
    }
}

checkCarModel('renault'); // 输出 'model valid'
```

现在，如果我们想要检查更多模型，只需要添加一个新的数组元素即可。此外，如果它很重要的话，我们还可以将 `models` 变量定义在函数作用域外，并在需要的地方重用。这种方式可以让我们集中管理，并使维护变得轻而易举，因为我们只需在代码中更改一个位置。

## 3. 匹配所有条件，使用 `Array.every` 或者 `Array.find`

在本例中，我们想要检查每个汽车模型是否都是传入函数的那一个。为了以更加命令式的方式实现，我们会这么做：

```js
const cars = [
  { model: 'renault', year: 1956 },
  { model: 'peugeot', year: 1968 },
  { model: 'ford', year: 1977 }
];

const checkEveryModel = (model) => {
  let isValid = true;

  for (let car of cars) {
    if (!isValid) {
      break;
    }
    isValid = car.model === model;
  }

  return isValid;
}

console.log(checkEveryModel('renault')); // 输出 false
```

如果你更喜欢以命令式的风格行事，上面的代码或许还不错。另一方面，如果你不关心其背后发生了什么，那么你可以重写上面的函数并使用 `Array.every` 或者 `Array.find` 来达到相同的结果。

```js
const checkEveryModel = (model) => {
  return cars.every(car => car.model === model);
}

console.log(checkEveryModel('renault')); // 输出 false
```

通过使用 `Array.find` 并做轻微的调整，我们可以达到相同的结果。两者的表现是一致的，因为两个函数都为数组中的每一个元素执行了回调，并且在找到一个 `falsy` 项时立即返回 `false`。

```js
const checkEveryModel = (model) => {
  return cars.find(car => car.model !== model) === undefined;
}

console.log(checkEveryModel('renault')); // 输出 false
```

## 4. 匹配部分条件，使用 `Array.some`

`Array.every` 匹配所有条件，这个方法则可以轻松地检查我们的数组是否包含某一个或某几个元素。为此，我们需要提供一个回调并基于条件返回一个布尔值。

我们可以通过编写一个类似的 `for...loop` 语句来实现相同的结果，就像之前写的一样。但幸运的是，有很酷的 JavaScript 函数可以来帮助我们完成这件事。

```js
const cars = [
  { model: 'renault', year: 1956 },
  { model: 'peugeot', year: 1968 },
  { model: 'ford', year: 1977 }
];

const checkForAnyModel = (model) => {
  return cars.some(car => car.model === model);
}

console.log(checkForAnyModel('renault')); // 输出 true
```

## 5. 提前返回而不是使用 `if...else` 分支

当我还是学生的时候，就有人教过我：一个函数应该只有一个返回语句，并且只从一个地方返回。如果细心处理，这个方法倒也还好。我这么说也就意味着，我们应该意识到它在某些情况下可能会引起条件式嵌套地狱。如果不受控制，多个分支和 `if...else` 嵌套将会让我们感到很痛苦。

另一方面，如果代码库很大且包含很多行代码，位于深层的一个返回语句可能会带来问题。现在我们都实行关注点分离和 SOLID 原则，因此，代码行过多这种情况挺罕见的。

举例来解释这个问题。假设我们想要显示所给车辆的模型和生产年份：

```js
const checkModel = (car) => {
  let result; // 首先，定义一个 result 变量
  
  // 检查是否有车
  if(car) {

    // 检查是否有车的模型
    if (car.model) {

      // 检查是否有车的年份
      if(car.year) {
        result = `Car model: ${car.model}; Manufacturing year: ${car.year};`;
      } else {
        result = 'No car year';
      }
    
    } else {
      result = 'No car model'
    }   

  } else {
    result = 'No car';
  }

  return result; // 我们的单独的返回语句
}

console.log(checkModel()); // 输出 'No car'
console.log(checkModel({ year: 1988 })); // 输出 'No car model'
console.log(checkModel({ model: 'ford' })); // 输出 'No car year'
console.log(checkModel({ model: 'ford', year: 1988 })); // 输出 'Car model: ford; Manufacturing year: 1988;'
```

正如你所看到的，即使本例的问题很简单，上面的代码也实在太长了。可以想象一下，如果我们有更加复杂的逻辑会发生什么事。大量的 `if...else` 语句。

我们可以重构上面的函数，分解成多个步骤并稍做改善。例如，使用三元操作符，包括 `&&` 条件式等。不过，这里我直接跳到最后，向你展示借助现代 JavaScript 特性和多个返回语句，代码可以有多简洁。

```js
const checkModel = ({model, year} = {}) => {
  if(!model && !year) return 'No car';
  if(!model) return 'No car model';
  if(!year) return 'No car year';

  // 这里可以任意操作模型或年份
  // 确保它们存在
  // 无需更多检查

  // doSomething(model);
  // doSomethingElse(year);
  
  return `Car model: ${model}; Manufacturing year: ${year};`;
}

console.log(checkModel()); // 输出 'No car'
console.log(checkModel({ year: 1988 })); // 输出 'No car model'
console.log(checkModel({ model: 'ford' })); // 输出 'No car year'
console.log(checkModel({ model: 'ford', year: 1988 })); // 输出 'Car model: ford; Manufacturing year: 1988;'
```

在重构版本中，我们包含了解构和默认参数。默认参数确保我们在传入 `undefined` 时有可用于解构的值。注意，如果传入 `null` ，函数将会抛出错误。这也是之前那个方法的优点所在，因为那个方法在传入 null 的时候会输出 `'No car'`。

对象解构确保函数只取所需。例如，如果我们在给定车辆对象中包含额外属性，则该属性在我们的函数中是无法获取的。

根据偏好，开发者会选择其中一种方式。实践中，编写的代码通常介于两者之间。很多人觉得 `if...else` 语句更容易理解，并且有助于他们更为轻松地遵循程序流程。

## 6. 使用索引或者映射，而不是 `switch` 语句

假设我们想要基于给定的国家获取汽车模型。

```js
const getCarsByState = (state) => {
  switch (state) {
    case 'usa':
      return ['Ford', 'Dodge'];
    case 'france':
      return ['Renault', 'Peugeot'];
    case 'italy':
      return ['Fiat'];
    default:
      return [];
  }
}

console.log(getCarsByState()); // 输出 []
console.log(getCarsByState('usa')); // 输出 ['Ford', 'Dodge']
console.log(getCarsByState('italy')); // 输出 ['Fiat']
```

上诉代码可以重构，完全去除 `switch` 语句。

```js
const cars = new Map()
  .set('usa', ['Ford', 'Dodge'])
  .set('france', ['Renault', 'Peugeot'])
  .set('italy', ['Fiat']);

const getCarsByState = (state) => {
  return cars.get(state) || [];
}

console.log(getCarsByState()); // 输出 []
console.log(getCarsByState('usa')); //输出 ['Ford', 'Dodge']
console.log(getCarsByState('italy')); // 输出 ['Fiat']
```

或者，我们还可以为包含可用汽车列表的每个国家创建一个类，并在需要的时候使用。不过这个就是题外话了，本文的主题是关于条件句的。更恰当的修改是使用对象字面量。

```js
const carState = {
  usa: ['Ford', 'Dodge'],
  france: ['Renault', 'Peugeot'],
  italy: ['Fiat']
};

const getCarsByState = (state) => {
  return carState[state] || [];
}

console.log(getCarsByState()); // 输出 []
console.log(getCarsByState('usa')); // 输出 ['Ford', 'Dodge']
console.log(getCarsByState('france')); // 输出 ['Renault', 'Peugeot']
```

## 7. 使用自判断链接和空合并

到了这一小节，我终于可以说“最后”了。在我看来，这两个功能对于 JavaScript 语言来说是非常有用的。作为一个来自 C# 世界的人，可以说我经常使用它们。

在写这篇文章的时候，这些还没有得到完全的支持。因此，对于以这种方式编写的代码，你需要使用 Babel 进行编译。你可以在自判断链接这里以及在空合并这里查阅。

自判断链接允许我们在没有显式检查中间节点是否存在的时候处理树形结构，空合并可以确保节点不存在时会有一个默认值，配合自判断链接使用会有不错的效果。

让我们用一些例子来支撑上面的结论。一开始，我们还是用以前的老方法：

```js
const car = {
  model: 'Fiesta',
  manufacturer: {
    name: 'Ford',
    address: {
      street: 'Some Street Name',
      number: '5555',
      state: 'USA'
    }
  }
}

// 获取汽车模型
const model = car && car.model || 'default model';
// 获取厂商地址
const street = car && car.manufacturer && car.manufacturer.address && car.manufacturer.address.street || 'default street';
// 请求一个不存在的属性
const phoneNumber = car && car.manufacturer && car.manufacturer.address && car.manufacturer.phoneNumber;

console.log(model) // 输出 'Fiesta'
console.log(street) // 输出 'Some Street Name'
console.log(phoneNumber) // 输出 undefined
```

因此，如果我们想要知道厂商是否来自 USA 并将结果打印，那么代码是这样的：

```js
const checkCarManufacturerState = () => {
  if(car && car.manufacturer && car.manufacturer.address && car.manufacturer.address.state === 'USA') {
    console.log('Is from USA');
  }
}

checkCarManufacturerState() // 输出 'Is from USA'
```

我无需再赘述如果对象结构更加复杂的话，代码会多么混乱了。许多库，例如 lodash，有自己的函数作为替代方案。不过这不是我们想要的，我们想要的是在原生 js 中也能做同样的事。我们来看一下新的方法：

```js
// 获取汽车模型
const model = car?.model ?? 'default model';
// 获取厂商地址
const street = car?.manufacturer?.address?.street ?? 'default street';

// 检查汽车厂商是否来自 USA
const checkCarManufacturerState = () => {
  if(car?.manufacturer?.address?.state === 'USA') {
    console.log('Is from USA');
  }
}
```

这看起来更加漂亮和简洁，对我来说，非常符合逻辑。如果你想知道为什么应该使用 `??` 而不是 `||` ，只需想一想什么值可以当做 `true` 或者 `false`，你将可能有意想不到的输出。

顺便说句题外话。自判断链接同样支持 DOM API，这非常酷，意味着你可以这么做：

```js
const value = document.querySelector('input#user-name')?.value;
```

# 后话

> 对于编码过程中的面条式代码和嵌套式的if/else判断深恶痛绝，但又无可奈何，最近看了一些技术文章，专门提到过javascript复杂判断的优雅写法，深入地学习了一下，现在总算知道一些复杂场景导致复杂的判断如何优雅地编码了，以后尽量不要再写if/else了，都是有更好的解决方案的。以下是自己学习中写的demo，记录一下：

```js
// 一般写法
 var status = 1
 if(status === 1) {
   console.log(status)
 } else if(status === 2) {
   console.log(status)
 } else if(status === 3) {
   console.log(status)
 } else if(status === 4) {
   console.log(status)
 } else {
   console.log(status)
 }

// switch写法
 var status = 1
 console.log(typeof status);
 switch(status - 0) {
   case 1:
     console.log(1);
     break;
   case 2:
   case 3:
     console.log(3);
     break;
   case 4:
     console.log(4);
     break;
   default:
     console.log(5);
     break;
 }

// 用对象实现多分支判断
 var status = 3
 const actions1 = {
   1: () => { console.log(1) },
   2: () => { console.log(2) },
   3: () => { console.log(3) },
   4: () => { console.log(4) },
   'default': () => { console.log(5) },
 }
 actions1[status] ? actions1[status]() : actions1['default']() // 3

//用es6的Map对象封装逻辑
 var status = 2
 const actions = new Map([
   [1, () => console.log(1)],
   [2, () => console.log(2)],
   [3, () => console.log(3)],
   [4, () => console.log(4)],
   ['default', () => console.log('default')],
 ])
 actions.get(status - 0) ? actions.get(status - 0)() : actions.get('default')() // 2


// 二元判断
 var identify = 'guest3'
 var status = 2
 var actions = new Map([
     ['guest1_1', () => console.log('guest_1')],
     ['guest2_2', () => console.log('guest_2')],
     ['guest3_3', () => console.log('guest_3')],
     ['guest4_4', () => console.log('guest_4')],
     ['default', () => console.log('default')]
 ])
 actions.get(`${identify}_${status}`) ? actions.get(`${identify}_${status}`)() : actions.get('default')() // default

// 用对象也可以作二元判断
 var identify = 'guest2'
 var status = 3
 var actions = {
     'guest1_1': () => console.log('guest1_1'),
     'guest2_2': () => console.log('guest2_2'),
     'guest3_3': () => console.log('guest3_3'),
     'guest4_4': () => console.log('guest4_4'),
     'default': () => console.log('default'),
 }
 actions[`${identify}_${status}`] ? actions[`${identify}_${status}`]() : actions['default']() //  default
```

## 关于多元判断的解决方案如下：

```js
const status1 = 3;
const status2 = 2;
const actions = new Map([
  ['11', () => console.log('11')],
  ['12', () => console.log('12')],
  ['13', () => console.log('13')],
  ['21', () => console.log('21')],
  ['22', () => console.log('22')],
  ['23', () => console.log('23')],
  ['31', () => console.log('31')],
  ['32', () => console.log('32')],
  ['33', () => console.log('33')],
  ['default', () => console.log('default')]
])
actions.get(`${status1}${status2}`) ? actions.get(`${status1}${status2}`)() : actions.get('default')() // 23
```