## JavaScript：变量提升

**JavaScript权威指南-3.10.1**

类似C语言的编程语言中，花括号内的声明的代码，在代码段之外是看不见的，称为块级作用域；JavaScript中没有块级作用域，取而代之使用了函数作用域：变量在申明它的函数体内，以及嵌套的任意函数体内都有定义。

```
function test(o) {
	var i = 0;
	if(typeof o == 'object') {
		var j = 0;
		for(var k = 0; k < 10; k++) {
			console.log(k);
		}
		console.log(k);//10
	}
	console.log(j); //0;
}
```

有意思的是，变量在声明之前已经可以使用，这个特性被称为声明提前（hoisting）：

```
var scope = 'global';
fucntion f() {
	console.log(scope);//undefined
	var scope = "lobal";
	console.log(scope);//global
}
```

好的编程习惯是让变量声明和使用变量的代码尽可能靠近，甚至特意将变量声明都放在函数体顶部，能反应变量的作用域。

**思考，稍加改动，第一个console会输出什么**

```
fucntion f(scope) {
	console.log(scope);//?
	var scope = "lobal";
	console.log(scope);//global
}
f('global');
```

结果是第一个console会输出'global'，可以理解为：函数变量 或 内部定义的变量，其实都是设定的作用域内的同一个指针
，传入的值会给该变量进行一次初始化，下面的例子可以证明这个结论。

```
var globalScope = {value:1};
function f(scope) {
	console.log(scope);//{value:1}
	var scope;
	console.log(scope==globalScope);//true
}
f(globalScope);
```