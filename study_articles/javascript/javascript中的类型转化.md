## JavaScript中的类型转化

1. 类型转换发生在，程序期望某个类型数据，而实际格式不符的时候，通常特征是使用了：==、+、-、*、运算符，或者 if(variable) 判断
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



