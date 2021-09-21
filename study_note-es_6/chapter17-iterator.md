# Iterator 和 for...of 循环

> 1.Iterator（遍历器）的概念
2.默认 Iterator 接口
3.调用 Iterator 接口的场合
4.字符串的 Iterator 接口
5.Iterator 接口与 Generator 函数
6.遍历器对象的 return()，throw()
7.for...of 循环

## 1. Iterator（遍历器）的概念

### 为什么要有Iterator

Iterator为数据集合（Array Object Map Set）提供了一种数据结构统一的访问机制，Iterator 接口主要供`for...of`消费。

Iterator 的遍历过程是，创建一个指针对象指向当前数据结构的起始位置，不断调用指针对象的next方法，直到它指向数据结构的结束位置。

下面是模拟next的一个实现：

```
var it = makeIterator(['a', 'b']);

it.next() // { value: "a", done: false }
it.next() // { value: "b", done: false }
it.next() // { value: undefined, done: true }

function makeIterator(array) {
  var nextIndex = 0;
  return {
    next: function() {
      return nextIndex < array.length ?
        {value: array[nextIndex++], done: false} :
        {value: undefined, done: true};
    }
  };
}
```
### Iterator的接口定义

使用TypeScript描述 遍历器接口（Iterable）、指针对象（Iterator）和next方法返回值的规格：

```
interface Iterable {
  [Symbol.iterator]() : Iterator,
}

interface Iterator {
  next(value?: any) : IterationResult,
}

interface IterationResult {
  value: any,
  done: boolean,
}
```

实现了Iterable接口的数据结构，可以通过调用`[Symbol.iterator]()`得到一个便利器对象。

```
var list = [1,2,3]
list[Symbol.iterator]().next() // {value: 1, done: false}
```

## 2. 默认Iterator接口

### Object没有默认Iterator接口

很多数据结构都具有了Iterator接口，可以直接使用`for...of`进行便利，但是Object没有实现Iterator接口，因为遍历器是一种线性处理，对象部署遍历器接口并不是很必要，直接Map就好了。

下面是一个模拟在Object上实现遍历器的例子，done 为 true的值是不被返回的：

```
const obj = {
  [Symbol.iterator] : function () {
    var index = 4;
    return {
      next: function () {
        return {
          value: -- index,
          done: index <= 0
        };
      }
    };
  }
};

for(let v of obj) { console.log('value', v) }
// value 3
// value 2
// value 1
```

### 在Object上使用for of 和 for in

for ... of会报错 is not iterable

for ... in得到对象的属性，**跟数组其实都属于对象上的遍历**

### 在Array上使用for of 和 for in

for ... of 遍历每个元素内容

```

for (let i of arr) {
  console.log(i); //  "3", "5", "7"
}

```

for ... in 得到数组的下标（也是属性的一种），并且会把挂在数组对象下的属性也得到

```
let arr = [3, 5, 7];
arr.foo = 'hello';

for (let i in arr) {
  console.log(i); // "0", "1", "2", "foo"
}

```

### 在类上实现Iterator接口的例子

```
class RangeIterator {
  constructor(start, stop) {
    this.value = start;
    this.stop = stop;
  }

  [Symbol.iterator]() { return this; }

  next() {
    var value = this.value;
    if (value < this.stop) {
      this.value++;
      return {done: false, value: value};
    }
    return {done: true, value: undefined};
  }
}

function range(start, stop) {
  return new RangeIterator(start, stop);
}

for (var value of range(0, 3)) {
  console.log(value); // 0, 1, 2
}
```

### 对象可以实现指针互指的例子

```
function Obj(value) {
  this.value = value;
  this.next = null;
}

Obj.prototype[Symbol.iterator] = function() {
  var iterator = { next: next };

  var current = this;

  function next() {
    if (current) {
      var value = current.value;
      current = current.next;
      return { done: false, value: value };
    }
    return { done: true };
  }
  return iterator;
}

var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);

one.next = two;
two.next = three;

for (var i of one){
  console.log(i); // 1, 2, 3
}
```

## 3. 调用 Iterator 接口的场合

除了下文会介绍的for...of循环，还有几个别的场合。

- 解构赋值 `let [x, y] = set`
- 扩展运算符 `let arr = [ ...oldArr ]`
- yield*
- 其他场合：任何接受数组作为参数的场合

## 4. 字符串的 Iterator 接口

```
var someString = "hi";
typeof someString[Symbol.iterator]
// "function"

var iterator = someString[Symbol.iterator]();

iterator.next()  // { value: "h", done: false }
iterator.next()  // { value: "i", done: false }
iterator.next()  // { value: undefined, done: true }
```

## 5. Iterator 接口与 Generator 函数

```
let myIterable = {
  [Symbol.iterator]: function* () {
    yield 1;
    yield 2;
    yield 3;
  }
};
[...myIterable] // [1, 2, 3]

// 或者采用下面的简洁写法

let obj = {
  * [Symbol.iterator]() {
    yield 'hello';
    yield 'world';
  }
};

for (let x of obj) {
  console.log(x);
}
// "hello"
// "world"
```

## 6. 遍历器对象的 return()，throw()


如果你自己写遍历器对象生成函数，那么next()方法是必须部署的，return()方法和throw()方法是否部署是可选的。

## 7. for...of 循环

### 在各种数据结构上的应用

介绍了将 for...of 应用在：数组、对象、基于entries() keys() valeus()生成的数据结构、类似数组的结构（arguments、字符串、DOM NodeList）。

在对象上不能直接只是用for...for，介绍了几种变通的方法：使用keys，使用yield。

### 遍历的多种语法的比较

`for (var i = 0; i < arr.length; i++) { }`
语法比较麻烦

`myArray.forEach(function (value) {`
无法使用return和break跳出

`for (var index in myArray) {`
遍历的键名是数字
还会遍历手动添加的其他键
某些情况下，会以任意顺序遍历
总之，for...in循环主要是为遍历对象而设计的，不适用于遍历数组

`for (let value of myArray) {`
有着同for...in一样的简洁语法，但是没有for...in那些缺点
不同于forEach方法，它可以与break、continue和return配合使用

























