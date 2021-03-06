
<br/>

# 6. 描述以下变量的区别：`null`、`undefined`或`undeclared`

- `null` 表示"没有对象"，该处初始化了一个空值，转为数值时为0。<br/>
- `undefined`表示"缺少值"，就是定义没有初始化任何值，转为数值时为`NaN`。<br/>
- `undeclared`:`js`语法错误，没有声明明直接使用，`js`无法找到对应的上下文。<br/>

# 7.`==`和`===`有什么区别？

首先，`== equality` 等同，`=== identity` 恒等。`==`，两边值类型不同的时候，要先进行类型转换，再比较。`===`，不做类型转换，类型不同的一定不等。

`===`比较

- 如果类型不同，就不相等
- 如果两个都是数值，并且是同一个值，那么相等；例外的是，如果其中至少一个是`NaN`，那么不相等。（判断一个值是否是`NaN`，只能用`isNaN()`来判断） 
- 如果两个都是字符串，每个位置的字符都一样，那么相等；否则不相等。 
- 如果两个值都是`true`，或者都是`false`，那么相等。 
- 如果两个值都引用同一个对象或函数，那么相等；否则不相等。 
- 如果两个值都是`null`，或者都是`undefined`，那么相等。 

`==`比较

- 如果一个是`null`、一个是`undefined`，那么[相等]。 
- 如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。 
- 如果任一值是 `true`，把它转换成 `1` 再比较；如果任一值是 `false`，把它转换成 `0` 再比较。 
- 如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。

类型转化概述(参考《JavaScript权威指南》3.8节，讲解非常详细）

1. 类型转换发生在：程序期望某个类型数据，而实际格式不符的时候，通常特征是使用了：+、-、*、运算符，或者 if(variable) 判断
2. 类型转换分为（1）原始值到原始值的转换（2）原始值到对象的转换（3）对象到原始值的转换
	1. 	原始值到原始值比较特殊的有：“”转成布尔是false；合规字符串可以转化为数字，两头有非数字/空格组成部分的会转成NaN
	2. 原始值到对象，通过 `String(val)/Number(val)/Boolean(val)` 显式转化成包装对象
	3. 对象到原始值，参考第10条
3. 发生类型转化并不意味着两者相等，比如：`if(undefined) {}` 中，`undefined `被转化为了`false`，并不意味着 `undefined == false`。
4. 运算符管理啊的具体操作规则：`+`（一个是字符串另一个也会转化成字符串），一元运算`++/--`符会将操作转化为数字、一元运算`!`将操作数转化为布尔并取反、`*/-`运算符将两边的转化为数字。
5. 可以使用函数在数字和字符串之间做精准的转化：数字到字符串有 toFixed/toExponential/toPrecisioin，字符串到数字有 Number(str)/parseInt(str)/parseFloat(str)/
6. 对象到原始值：到布尔（都返回true，null为false），到字符串toString->valueOf的处理顺序，到数字遵循valueOf->toString()的处理顺序，获取到原始值时候会停下来用 String/Number显示转化后返回
7. 不同的操作符（`+/-/</==/!=`)，处理对象的时候转化成原始值，有的可能是数字，有的可能是字符串，比如 `[1]+1`//'11'，`[1]-1`//0



对象到数字的转化例子：

```
//[] == 0 的转化过程

[] 
valueOf([]) // []
toString([])  // ''
Number('') // 0

//[2] == 2 的转化过程

[2] 
valueOf([2]) // [2]
toString([2])  // '2'
Number('2') // 2
```

# 8.简述`js`中`this`的指向

第一准则是：`this`永远指向函数运行时所在的对象，而不是函数被创建时所在的对象。
```
var o = {a:function(){console.log(this);}}
o.a();//{a:f}o
var b = o.a;
b();//Window
```

函数作为谁的方法被调用调用，`this`就是谁。<br/>

构造函数的话，如果不用new操作符而直接调用，那即`this`指向`window`。用`new`操作符生成对象实例后，`this`就指向了新生成的对象。<br/>

匿名函数或不处于任何对象中的函数指向`window`。<br/>
如果是`call，apply`等，指定的`this`是谁，就是谁。

# 9. 基本数据类型和引用数据类型

基本数据类型指的是简单的数据段，有`5`种，包括`null、undefined、string、boolean、number、symbol`；<br/>
引用数据类型指的是有多个值构成的对象，包括`object、array、date、regexp、function`等。<br/>

主要区别：<br/>

声明变量时不同的内存分配：前者由于占据的空间大小固定且较小，会被存储在栈当中，也就是变量访问的位置；后者则存储在堆当中，变量访问的其实是一个指针，它指向存储对象的内存地址。<br/>

也正是因为内存分配不同，在复制变量时也不一样。前者复制后2个变量是独立的，因为是把值拷贝了一份；后者则是复制了一个指针，2个变量指向的值是该指针所指向的内容，一旦一方修改，另一方也会受到影响。<br/>

参数传递不同：虽然函数的参数都是按值传递的，但是引用值传递的值是一个内存地址，实参和形参指向的是同一个对象，所以函数内部对这个参数的修改会体现在外部。原始值只是把变量里的值传递给参数，之后参数和这个变量互不影响。

# 10. 深拷贝和浅拷贝

如何区分深拷贝与浅拷贝，简单点来说，就是假设`B`复制了`A`，当修改`A`时，看`B`是否会发生变化，如果`B`也跟着变了，说明这是浅拷贝，拿人手短，如果B没变，那就是深拷贝，自食其力。

```js
  function is(model, value) { return Object.prototype.toString.call(value).slice(8,-1) === model; }
  function isDate(value) { return is('Date', value); }
  function isRegExp(value) { return is('RegExp', value); }
  function isFunction(value) { return is('Function', value); }

  var source = {
    null: null, 
    boolean: 1, 
    undefined: undefined, 
    string: 's',  

    date: new Date(),
    regexp: /\w+/ig,
    func: ()=>{},

    object: {deep:true}, 
    array: [1,2,3],
  };

  function deepCopy(value){
    let type = typeof value;

    // 非复合数据类型，直接返回值
    if(value === null || (type != 'object' && type !=' function')) {
      return value;
    } 
    else if(isDate(value)) {
      const result = new value.constructor(+value)
      return result;
    } 
    else if(isRegExp(value)) {
      const result = new value.constructor(value.source, /\w*$/.exec(value))
      result.lastIndex = value.lastIndex
      return result
    } 
    else if(isFunction(value)) {
      return Object.create(Object.getPrototypeOf(value))
    }
    
    // update: Object.keys会将symbol直接转化为字符串
    const isArr = Array.isArray(value);
    const props = isArr ? value : Object.keys(value);

    let result = isArr ? new Array(value.length) : new Object();

    props.forEach((prop,index) => {
      if(isArr){
        result[index] = deepCopy(prop)
      } else {
        result[prop] = deepCopy(value[prop]);
      }
    });
    return result;
  }

  var target = deepCopy(source);

  console.log('null compare:' + source.null === target.null);
  console.log('boolean compare:' + source.boolean === target.boolean);
  console.log('undefined compare:' + source.undefined === target.undefined);
  console.log('string compare:' + source.string === target.string);
  console.log('date compare:' + source.date === target.date);
  console.log('regexp compare:' + source.regexp === target.regexp);
  console.log('func compare:' + source.func === target.func);
  console.log('object compare:' + source.object === target.object);
  console.log('array compare:' + source.array === target.array);
```
```js
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj))
}
```
数组深拷贝方法：
```js
// ...
var a=[1,2,3]
var [...b]=a;//或b=[...a]
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3

// concat
var a=[1,2,3]
var c=[];
var b=c.concat(a);
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3

// slice
var a=[1,2,3]
var b=a.slice(0);
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3

// JSON
var a=[1,2,3]
var c=JSON.stringify(a);
var b=JSON.parse(c);
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3
```
# 11.`JSON.parse(JSON.stringify())`实现对对象的深拷贝的一些缺陷

1、如果`obj`里面有时间对象，则`JSON.stringify`后再`JSON.parse`的结果，时间将只是字符串的形式;<br>
2、如果`obj`里有`RegExp、Error`对象，则序列化的结果将只得到空对象;<br>
3、如果`obj`里有函数或者`undefined`，则序列化的结果会把函数或`undefined`丢失;<br>
4、如果`obj`里有`NaN、Infinity`和`-Infinity`，则序列化的结果会变成`null`;<br>
5、`JSON.stringify()`只能序列化对象的可枚举的自有属性，例如 如果`obj`中的对象是有构造函数生成的， 则使用`JSON.parse(JSON.stringify(obj))`深拷贝后，会丢弃对象的`constructor`;<br>
6、如果对象中存在循环引用的情况也无法正确实现深拷贝<br>


# 12.原型继承的原理
**原型链**

```js
function BasicType() {
     this.property=true;
     this.getBasicValue = function(){
     return this.property;
      };
}
function NewType() {
     this.subproperty=false;
}
NewType.prototype = new BasicType(); 
var test = new NewType();
alert(test.getBasicValue());   //true
```

由上面可以知道，其本质上是重写了原型对象，代之一个新类型的实例。在通过原型链继承的情况下，要访问一个实例属性时，要经过三个步骤： 

1、搜索实例；<br/>
2、搜索`NewType.prototype`；<br/>
3、搜索`BasicType.prototype`,此时才找到方法。如果找不到属性或者方法，会一直向上回溯到末端才会停止。<br/>

要想确定实例和原型的关系，可以使用`instanceof`和`isPrototypeof()`测试，只要是原型链中出现过的原型，都可以说是该原型链所派生实例的原型。还有一点需要注意，通过原型链实现继承时，不能使用对象字面量创建原型方法，因为这时会重写原型链，原型链会被截断<br/>

**借用构造函数继承**

```js
function BasicType(name) {
     this.name=name;
     this.color=["red","blue","green"];
}
function NewType() {
     BasicType.call(this,"syf");
     this.age=23;
}
var test = new NewType(); 

alert(text.name); //syf
alert(text.age);   //23
```

**组合式继承**

```js
function BasicType(name) {
     this.name=name;
     this.colors=["red","blue","green"];
}
BasicType.prototype.sayName=function(){
     alert(this.name);
}
function NewType(name,age) {
     BasicType.call(this,name);
     this.age=age;
}
NewType.prototype = Object.create(BasicType.prototype);
var test = new NewType("syf","23");
test.colors.push("black");
alert(test.colors);  //"red,blue,green,black"
alert(test.name);   //"syf"
alert(test.age);   //23

// 组合式继承避免了原型链和借用构造函数继承方式的缺陷，融合了他们的优点，成为了js中最常用的继承方式。
```

# 13.`JavaScript`里`arguments`究竟是什么？

`Javascript`中每个函数都会有一个`Arguments`对象实例`arguments`，它引用着函数的实参，可以用数组下标的方式"[]"引用`arguments`的元素。`arguments.length`为函数实参个数，`arguments.callee`引用函数自身。<br/>
`arguments`虽然有一些数组的性质，但其并非真正的数组，只是一个类数组对象。其并没有数组一些方法，真正的数组那样调用`jion()`、`concat()`、`pop()`等方法

# 14.基本类型转换

```js
// 请在问号处填写你的答案,使下方等式成立
let a = ?;
if(a == 1 && a == 2 && a == 3) {
  console.log("Hi, I'm Echi");
}
```
```js
let a = {
  i: 1,
  valueOf() {
    return this.i++;
  }
}
```
对象在转换基本类型时，会调用`valueOf`和`toString`，并且这两个方法你是可以重写的。<br/>
调用哪个方法，主要是要看这个对象倾向于转换为什么。如果倾向于转换为`Number`类型的，就优先调用`valueOf`;如果倾向于转换为`String`类型，就只调用`toString`。

```js
let obj = {
  toString () {
    console.log('toString')
    return 'string'
  },
  valueOf () {
    console.log('valueOf')
    return 'value'
  }
}

alert(obj) // string
console.log(1 + obj) // 1value
```

如果重写了`toString`方法，而没有重写`valueOf`方法，则会调用`toString`方法

```js
var obj = {
  toString () {
    return 'string'
  }
}
console.log(1 + obj) // 1string
```
调用上述两个方法的时候，需要 `return` 原始类型的值 `(primitive value)`，不能是`object、array`等非原始类型的值，不然的话就会去调用另外一个没有返回非原始类型的方法，如果都没有的话就会报错<br/>
如果有`Symbol.toPrimitive`属性的话，会优先调用，它的优先级最高<br/>

```js
let obj = {
  toString () {
    console.log('toString')
    return {}
  },
  valueOf () {
    console.log('valueOf')
    return {}
  },
  [Symbol.toPrimitive] () {
    console.log('primitive')
    return 'primi'
  }
}
console.log(1 + obj) // 1primi
```

# 15.解析 `['1', '2, '3'].map(parseInt)`

```js
let new_array = arr.map(function callback(currentValue,index,array)
```

这个 `callback` 一共可以接收三个参数，其中第一个参数代表当前被处理的元素，而第二个参数代表该元素的索引。<br/>


而 `parseInt` 则是用来解析字符串的，使字符串成为指定基数的整数。 `parseInt(string, radix)`接收两个参数，第一个表示被处理的值（字符串），第二个表示为解析时的基数。<br/>


了解这两个函数后，我们可以模拟一下运行情况 `parseInt(‘1’, 0) //radix` 为 `0` 时，且 `string` 参数不以`“0x”`和`“0”`开头时，按照 `10` 为基数处理。这个时候返回 `1`；<br/>
`parseInt('2', 1)` // 基数为 `1`（`1` 进制）表示的数中，最大值小于 `2`，所以无法解析，返回 `NaN`；<br/>


`parseInt('3', 2)` // 基数为 `2`（`2` 进制）表示的数中，最大值小于 `3`，所以无法解析，返回 `NaN`。<br/>

`map` 函数返回的是一个数组，所以最后结果为 `[1, NaN, NaN]`。


# 16. 什么是防抖和节流？有什么区别？如何实现？
防抖：触发高频事件后 `n` 秒内函数只会执行一次，如果 `n` 秒内高频事件再次被触发，则重新计算时间。<br/>

思路：每次触发事件时都取消之前的延时调用方法

```js
function debounce(fn) {
  let timeout =  null;
  return function() {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      fn.apply(null, arguments)
    },500)
  }
}
```
示例：
 
```js
function sayHi() { console.log('防抖成功'); } 
var inp = document.getElementById('inp'); 
inp.addEventListener('input', debounce(sayHi)); // 防抖
```

节流：高频事件触发，但在 `n` 秒内只会执行一次，所以节流会稀释函数的执行频率。<br/>

思路：每次触发事件时都判断当前是否有等待执行的延时函数<br/>

```js
function throttle(fn) {
  let canRun = true;
  return function() {
    if(!canRun) return;
    canRun = false;
    setTimeout(() => {
      fn.apply(null, arguments);
      canRun = true;
    }, 500)
  }
}
```
示例:

```js
function sayHi(e) {
  console.log(e.target.innerWidth, e.target.innerHeight);
} 
window.addEventListener('resize', throttle(sayHi));
```

# 17. `Set、Map、WeakSet、WeakMap` 的区别？

`Set`<br/>

成员唯一、无序且不重复；`[value, value]`，键值与键名是一致的（或者说只有键值，没有键名）；可以遍历，方法有：`add、delete、has`。<br/>

`WeakSet`<br/>

成员都是对象； 成员都是弱引用，可以被垃圾回收机制回收，可以用来保存 `DOM`节点，不容易造成内存泄漏； 不能遍历，方法有 `add、delete、has`。<br/>

`Map`<br/>

本质上是键值对的集合，类似集合； 可以遍历，方法很多，可以跟各种数据格式转换。<br/>
`WeakMap`<br/>

只接受对象为键名（`null` 除外），不接受其他类型的值作为键名； 键名是弱引用，键值可以是任意的，键名所指向的对象可以被垃圾回收，此时键名是无效的； 不能遍历，方法有 `get、set、has、delete`。<br/>


# 18. 介绍下深度优先遍历和广度优先遍历

**深度优先遍历`(DFS)`**<br/>

`DFS` 就是从图中的一个节点开始追溯，直到最后一个节点，然后回溯，继续追溯下一条路径，直到到达所有的节点，如此往复，直到没有路径为止。<br/>

```js
//深度优先遍历的递归写法
function deepTraversal(node){
  let nodes=[];
  if(node!=null){
      nodes.push[node];
      let childrens=node.children;
      for(let i=0;i<childrens.length;i++) {
        deepTraversal(childrens[i]);
      }
  }
  return nodes;
}
```

**广度优先遍历`(BFS)`**<br/>

`BFS`从一个节点开始，尝试访问尽可能靠近它的目标节点。本质上这种遍历在图上是逐层移动的，首先检查最靠近第一个节点的层，再逐渐向下移动到离起始节点最远的层。

```js
//广度优先遍历的递归写法
function wideTraversal(node){
    let nodes=[],i=0;
    if(node!=null){
        nodes.push(node);
        wideTraversal(node.nextElementSibling);
        node=nodes[i++];
        wideTraversal(node.firstElementChild);
    }
    return nodes;
}
```

# 19. 将数组扁平化

并去除其中重复数据，最终得到一个升序且不重复的数组
```js
Array.from(new Set(arr.flat(Infinity))).sort((a,b)=>{ return a-b})
```
#20. `JS`异步解决方案的发展历程以及优缺点
**1、回调函数`(callback)`**<br/>

缺点：回调地狱，不能用 `try catch` 捕获错误，不能`return`<br/>
回调地狱的根本问题在于：<br/>
缺乏顺序性： 回调地狱导致的调试困难，和大脑的思维方式不符； 嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身，即（控制反转）； 嵌套函数过多的多话，很难处理错误。


**2、`Promise`**<br>


`Promise` 就是为了解决 `callback` 的问题而产生的。<br>
`Promise` 实现了链式调用，也就是说每次`then`后返回的都是一个全新`Promise`，如果我们在`then`中`return`，`return`的结果会被`Promise.resolve()`包装。
优点：解决了回调地狱的问题。<br>
缺点：无法取消 Promise ，错误需要通过回调函数来捕获。<br>


**3、`Generator`**


特点：可以控制函数的执行，可以配合`co`函数库使用。

```js
function * fetch() {
    yield ajax('XXX1', () = >{}) 
    yield ajax('XXX2', () = >{}) 
    yield ajax('XXX3', () = >{})
}
let it = fetch() 
let result1 = it.next() 
let result2 = it.next() 
let result3 = it.next()
```
**4、`Async/await`**


`async、await` 是异步的终极解决方案。<br>

优点是：代码清晰，不用像 `Promise` 写一大堆 `then` 链，处理了回调地狱的问题；<br>
缺点：`await` 将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用 `await` 会导致性能上的降低。

```js
let a = 0;
let b = async () = >{
    a = a + await 10;
    console.log('2', a); // -> '2' 10
}
b();
a++;
console.log('1', a); // -> '1' 1
```

首先函数`b`先执行，在执行到`await 10`之前变量`a`还是`0`，因为`await`内部实现了`generator` ，`generator`会保留堆栈中东西，所以这时候`a = 0`被保存了下来；<br>

因为`await`是异步操作，后来的表达式不返回`Promise`的话，就会包装成`Promise.reslove`(返回值)，然后会去执行函数外的同步代码；<br>

同步代码执行完毕后开始执行异步代码，将保存下来的值拿出来使用，这时候 `a = 0 + 10`。<br>
上述解释中提到了 `await` 内部实现了 `generator`，其实 `await` 就是 `generator` 加上 `Promise`的语法糖，且内部实现了自动执行 `generator`


#21. `js`实现继承

```js
(function(){
  function Person() {
    this.name = 'person name'
  }
  Person.prototype.move = function() {
    console.log('person move')
  }
  function Man() {
    Person.call(this);
    this.name = 'man name'
  }
  Man.prototype = Object.create(Person.prototype)
  // 或者 Man.prototype = new Person()
  let man = new Man();
  man.name // man name
  man.move // person move
})()
```

# 22. bind/call/apply的实现

``` 
Function.prototype.bb = function(){
	var func = this;
	var context = Array.prototype.shift.call(arguments);
	var args = Array.prototype.slice.call(arguments)
	return function(){
	  func.apply(context, args)
	}
}
```

```
Function.prototype.call  = function(context, ...args) {
  var func = this.bind(context);
  func(...args);
}

Function.prototype.apply  = function(context, args) {
  var func = this.bind(context);
  func(...args);
}
```

个人简单版本的实现，也许不周全，但是能实现基本功能。


# 23. filter的实现

```
Array.prototype.ff = function(fn) {
	if(typeof fn != 'function') return;
	
	var result = [];
	for(var i = 0; i < this.length; i++) {
		if(fn.call(null, this[i], i) == true) {
			    result.push(this[i]);
		}
	}
	return result;
}
```

# 24. instanceof的实现

```

function instanceOf(o, Instance) {
  debugger;
  if(o == null) return false;
  var proto = Instance.prototype;
  while(true) {
    if(o.__proto__ == null) return false;
    
    if(o.__proto__ === proto) {
      return true;
    } else {
      o = o.__proto__;
    }
  }
}
console.log(instanceOf({}, Object));//true
console.log(instanceOf(alert, Object));//true
console.log(instanceOf(alert, Function));//true
console.log(instanceOf({}, Number));//false
```

#25. 宏任务/微任务执行顺序

### 例子1 then.then

考察了then.then的执行是有依赖顺序的

```
new Promise((resolve,reject)=>{
  console.log("1")
  resolve()
}).then(()=>{
  console.log("2")
}).then(()=>{
  console.log("3")
})
// 1,2,3
```

```
 new Promise((resolve,reject)=>{
  console.log("1")
  resolve()
}).then(()=>{
  console.log("2")
  return new Promise((resolve,reject)=>{
    console.log("3")
    resolve()
  }).then(()=>{
    console.log("4")
  }).then(()=>{
    console.log("5")
  })

}).then(()=>{
  console.log("6")
})
// 1,2,3,4,5,6
```

### 例子2 then.then + then

主要考察了then.then会被当做the.then，被插入到micro队列的末尾，从而最后执行：


```
Promise.resolve().then(()=>{
	console.log(1);
}).then(()=>{
	console.log(2);
});
Promise.resolve().then(()=>{
	console.log(3);
})
// 1,3,2
```

```
console.log('begin');
setTimeout(() => {
    console.log('setTimeout 1');
    Promise.resolve()
        .then(() => {
            console.log('promise 1');
            setTimeout(() => {
                console.log('setTimeout2');
            });
        })
        .then(() => {
            console.log('promise 2');
        });
    new Promise(resolve => {
        console.log('a');
        resolve();
    }).then(() => {
        console.log('b');
    });
}, 0);
console.log('end');

// begin, end, setTimeout 1, a, promise 1, b, promise 2, setTimeout2,
```

### 例子3 macro.micro > macro


考察了第一个setTimeout执行时，生成了micro（输出7），这时候可以插队到第二个setTimeout（输出3）前。

```
console.log(1);

setTimeout(() => {
  console.log(2);
  new Promise((resolve) => {
    console.log(6);
    resolve();
  }).then(() => {
    console.log(7);
  })
})

setTimeout(() => {
  console.log(3);
})

new Promise((resolve)=>{
  console.log(4);
  resolve();
}).then(()=>{
  console.log(5);
})
// 1,4,5,2,6,7,3
```

同样一个插队的例子。

```
console.log(1);

setTimeout(() => {
  console.log(2);
})

setTimeout(() => {
  console.log(3);
})

new Promise((resolve)=>{
  console.log(4);
  resolve();
}).then(()=>{
  console.log(5);
})

// 1 4 5 2 3
```

各种不讲理插队的例子。

```
console.log(1);

setTimeout(() => {
  console.log(2);
})

setTimeout(() => {
  console.log(3);
})

new Promise((resolve)=>{
  console.log(4);
  resolve();
}).then(()=>{
  console.log(5);
  new Promise((resolve) => {
    console.log(6);
    resolve();
  }).then(() => {
    console.log(7);
  })
})
// 1 4 5 6 7 2 3 
```

变着法插队的例子。

```
console.log(1);
setTimeout(() => {
  console.log(2);
  new Promise((resolve) => {
    console.log(6);
    resolve();
  }).then(() => {
    console.log(7);
  })
})

setTimeout(() => {
  console.log(3);
})

new Promise((resolve)=>{
  console.log(4);
  resolve();
}).then(()=>{
  console.log(5);
})
// 1 4 5 2 6 7 3
```

### 例子4 node.js下的执行

在浏览器环境应该输出1 2 3 4，实际在node.js下是1 2 4 3，可见micro不插队，要等macro都走一遍。

```
setTimeout(function() {
	console.log(1);
	new Promise(function(resolve) {
        console.log(2);
        resolve();
    }).then(function() {
        console.log(3)
    })
});
setTimeout(function() {
	console.log(4);
});
// 1 2 4 4 
```

<<<<<<< HEAD
尽管如此，对于如下为什么5不是在10面前，仍旧是不理解？
	
```
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})

// 7 6 8 2 4 9 11 3 10 5 12 
```

#26. 补全代码

补全createIntervalFunc中的代码，可以增加函数，使log能按照1秒1次，输出4次。

```
function createIntervalFunc(func, times, gap){
}

var log = createIntervalFunc(console.log, 4, 1000)

log('hello');
```

如下为Promise+async/await的一种实现

```
function createIntervalFunc(func, times, gap){
	return async function(...args){
		for (let i = 0; i < times; i++) {
			await wait(gap);
			func.apply(null, args);
		}
	}
}

async function wait(gap) {
	return new Promise((resolve)=>{
		setTimeout(resolve, gap)
	})
}

var log = createIntervalFunc(console.log, 4, 1000)
log('hello');
```

#27. 简述js中this的指向

匿名函数、全局环境下的函数、不处于任何对象中的函数，指向Window。

用new生成对象，它的函数指向新生成的对象；直接定义的对象方法，同样指向该对象。

call、apply、bind方式指定，那么this指向指定的对象。

方法被赋值给对象下面的函数，那么this指向该对象(指向运行时所在对象，而不是创建时所在对象)。 

但箭头函数出现后，this指向定义时所在的环境，而不管执行时。

```
var func = function(){console.log(this);}
var afunc = ()=>{console.log(this);}

var o = {func,afunc};

o.func();//{func:f,afunc:f}
o.afunc();//WIndow

var rfunc = o.func;
rfunc();//Window
```

#28. 补全代码

```
function repeat(func, times, wait) {
	
}

const repeatFunc = repeat(alert, 4, 3000);

repeatFunc('hellworld');

repeatFunc('worldhello')
```	


```
function repeat(func, times, s) {
  return async function(...args) {
    for(let i = 0; i < times; i++) {
      func.apply(null, args);
      await wait(s);
    }
  }
}
async function wait(seconds) {
  return new Promise((resolve,reject) => {
    setTimeout(resolve, seconds);
  })
}
```

#101. 前端的模块化发展历史


模块化主要是用来抽离代码、隔离作用域、避免变量冲突等。

`IIFE`： Immediately Invoked Function Expression，使用立即执行函数来编写模块化。特点：在一个单独的函数作用域中执行代码，避免变量冲突。

`AMD`： 使用`requireJS` 来编写模块化，特点：依赖必须提前声明好。

`CMD`： 使用`seaJS`来编写模块化，特点：支持动态引入依赖文件。

`CommonJS`： `nodejs`中自带的模块化。

`UMD`：兼容`AMD`，`CommonJS` 模块化语法。

`ES Modules`： `ES6`引入的模块化，支持`import`来引入另一个`js` 。

`Webpack`：它的存在更像是一种支持多种使用方式的构建工具，不只是一个方案。


#102. ES6模块与CommonJS规范的区别

## ES6模块

1. 获得的是实时的值，更新能同步到
2. 对引入模块重新赋值会报错


```
// time.js
export var time = (new Date()).getTime();

setTimeout(() => {
  time = (new Date()).getTime();
  console.log('new:' + time)
}, 1000);
```

```
//test.js
import {time} from './time';

console.log('get 1st:' + time);
setTimeout(() => {
  console.log('get 2nd:' + time);
  time = 'try to modify';
}, 2000);

// get 1st:1579860927353
// new:1579860929357
// get 2nd:1579860929357
// Uncaught ReferenceError: time is not defined
```

## CommonJS模块

- 获得的是缓存值，不能同步变化
- 可以对引入模块可以重新赋值

```
//time.js

var time = (new Date()).getTime();

exports.time = time;

setTimeout(() => {
  time = (new Date()).getTime();
  console.log('new:' + time)
}, 2000);

//get 1st:1579861234613
//new:1579861236618
//get 2nd:1579861234613
//try to modify
```


```
//test.js
let {time} = require('./time');

console.log('get 1st:' + time);
setTimeout(() => {
  console.log('get 2nd:' + time);
  time = 'try to modify';
  console.log(time);
}, 2000);
```


#103. export default与export的区别



`export`可以导出多个对象，`export default`只能导出一个对象；<br>

`export` 导出对象需要用`{}`，`export default`不需要`{}`，如：<br>

```js
export { A, B, C};
export default A;
```

在其他文件引用`export default`导出的对象时不一定使用导出时的名字。因为这种方式实际上是将该导出对象设置为默认导出对象，如：

```js
// 文件A
export default deObject;
// 文件B
import deObject from './A'
// 或者
import newDeObject from './A'
```

另：[ Tangyefei的ES6的Module笔记](http://book.tangyefei.cn/es-6/_book/chapter23-module.html) 有很详细的介绍。

#104. ES6代码转成ES5代码的实现思路是什么

解析：解析代码字符串，生成 `AST`<br>
转换： 按一定的规则转换、修改`AST`<br>
生成：将修改后的`AST`转换成普通代码

#105. CommonJS和ES6模块循环引用

该部分的理解不是深，姑且记住一般性的结论，可以参考 [阮一峰的文章](http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html)

## CommonJS

当模块第二次引用时不会去重复加载，而是执行上次缓存的。

一旦出现某个模块被”循环引用”，就只输出已经执行的部分，还未执行的部分不会输出。

## ES6 

ES6中对模块的引用不像CommonJS中那样必须执行`require`进来的模块，而只是保存一个模块的引用而已，因此，即使是循环引用也不会出错。
