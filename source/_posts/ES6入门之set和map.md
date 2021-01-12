---

title: ES6入门之set和map
date: 2017/8/13 19:33:31  
tags: Set Map ES6 ECMAScript6 数据结构

---

# Set

ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set函数可以接受一个数组（或类似数组的对象）作为参数，用来初始化。

<!-- more -->

```
// 例一
var set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

var s = new Set();

[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

> 在Set内部，两个NaN是相等。两个对象总是不相等的。可以用length来检测

四个操作方法：

- add(value)：添加某个值，返回Set结构本身。
- delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
- has(value)：返回一个布尔值，表示该值是否为Set的成员。
- clear()：清除所有成员，没有返回值
set内部的元素可以遍历for...of...

## weakset
WeakSet结构与Set类似，也是不重复的值的集合。

## WeakSet和Set的区别：

- WeakSet的成员只能是对象，而不能是其他类型的值
- WeakSet中的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中。这个特点意味着，无法引用WeakSet的成员，因此WeakSet是不可遍历的。

# Map
Map结构提供了“值—值”的对应，是一种更完善的Hash结构实现。如果你需要“键值对”的数据结构，Map比Object更合适。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```
var m = new Map();
var o = {p: "Hello World"};

m.set(o, "content")
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```

`注意，只有对同一个对象的引用，Map结构才将其视为同一个键。这一点要非常小心。`

```
var map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
上面代码的set和get方法，表面是针对同一个键，但实际上这是两个值，内存地址是不一样的，因此get方法无法读取该键，返回undefined。
```

如果Map的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map将其视为一个键，包括0和-0。另外，虽然NaN不严格相等于自身，但Map将其视为同一个键。

实例属性和方法：size、set、get、has、delete、clear

遍历方法：keys（）、values（）、entries（）、forEach（）

## WeakMap可以参考WeakSet