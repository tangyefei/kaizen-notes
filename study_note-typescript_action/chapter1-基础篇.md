# 第一章 基础篇

## 01 重塑“类型思维”

“类型思维”缺失导致可维护性、健壮性、安全性能的问题，在ECMAScript推出静态检查之前，TypeScript会是类型使用的标准。

它的几个特点：语言扩展、类型扩展、工具属性。

## 02 强类型语言和弱类型语言


强类型语言

宽泛的定义，即定义的结构和传入的结构必须一致
详细的定义，不允许改变数据类型，除非强制转化。

弱类型的语言

变量可以被赋予不同的数据类型。

## 03 动态类型和静态类型

动态类型：编译时确定所有变量变量的类型。

静态类型：在使用时候确定所有变量变量的类型。


动态类型在时间和空间上都性能会更差一下，并且隐藏错误，但是动态类型语灵活性更好，需要综合权衡。

## 04 编写你的第一个TypeScript程序


如果你只是写ts文件，然后编译成js文件，直接全局安装typescript，然后执行`tsc filename`即可。

项目开发中如果要基于Webpack开发，如果你有Webpack的基础，基本不用什么新知识，安装下属的配置

```
// package.json

{
  "devDependencies": {
    "clean-webpack-plugin": "^3.0.0",
    "html-webpack-plugin": "^3.2.0",
    "ts-loader": "^6.2.1",
    "typescript": "^3.8.3",
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3",
    "webpack-merge": "^4.2.2"
  }
}
```

在配置文件中增加对ts文件的load而即可

```
// /config/webpack.base.js

module: {
    rules: [{
      test: /\.ts$/, 
      use: 'ts-loader',
      exclude: '/node_modules'
    }]
}
```

作者在配置webpack的比较好的点是，配置文件不管什么环境入口都是 config/webpack.config.js，通过传入的参数来确定加载/合并哪个环境的配置，关键代码：

```
// /config/webpack.config.js

const baseConfig = require('./webpack.base.js');
const devConfig = require('./webpack.dev.js');
const prodConfig = require('./webpack.prod.js');

const merge = require('webpack-merge');
const envConfig = process.NODE_ENV == 'development' ? devConfig : prodConfig;

module.exports = merge(baseConfig, envConfig)
```

```
// package.json

"scripts": {
    "start": "webpack-dev-server --open --mode=development --config ./config/webpack.config.js",
    "build": "webpack --mode=production --config ./config/webpack.config.js"
},
```

## 05 基本类型

TypeScript在ES6的数据类型的基础上新增了：void、any、never、tuple、enum。

具体可参考 [官方文档:basic-types](https://www.typescriptlang.org/docs/handbook/basic-types.html)

## 06 枚举类型
```
enum Color = {
	Red,
	Green,
	Yellow
}
```

## 07 接口定义对象

使用和传入结构不同。如果传入的对象/数组直接量跟定义的interface结构不同，会报错，比如下面 `age` 会提示错误：

```
interface Item {
  id: number,
  name: string,
}
function print(item:Item) {
  console.log(item);
}
print({id:1,name:'yefei', age:18})
```

可以通过下面三种方式解决：

1. 将直接量赋值给变量，然后在print中传入变量；
2. 在直接量后面加`as Item` 或 前面加 `<Item>`；
3. 在interface中增加一行 `[x: string]: any`


可选属性，通过在属性后加问号可以在实际使用时候变为可选属性：

```
interface Item {
  readonly id: number,
  name: string,
  gender?: number,
}
```

只读属性，在属性前加上readonly，会直接在修改的位置提示错误：



## 08 接口定义函数

定义函数的四种方法

```
function add(x: number, y: number) => {
	return x + y;
}
let add: (x: number, y: number) => number;
interface add Add {
 (x: number, y: number) : number
}
type Add =  (x: number, y: number) => number;
```
## 09 函数相关知识点的梳理

预定义的函数参数要匹配，如果希望可选可以加上 `?` 表示参数可选，也可以在函数后面加 `=defaultValue` 表示默认。

预定义多个名字相同、参数不同的函数，来实现函数重载。


## 10 类(1)：继承和成员修饰符

修饰符包含了 public、private、protected、readonly、static，除了属性外构造函数也能增加这些修饰符。

## 11 类(2)：抽象类与多态

`abstract class` 抽象类只能被继承，不能被实现，抽象类中可以定义抽象方法供给子类实现。

多个子类对抽象类的不同实现，称为多态，可以根据不同的对象，实现不同的方法。

## 12 类(3)：类和接口的关系

接口可以继承接口，类可以继承类。

类可以实现接口，并且必须包含接口的所有属性。

## 13 泛型(1)：泛型函数与泛型接口

```
function <T>log(value: T) {
	return value;
}

type log = <T>(value: T) => T;

interface Log {
	<T>(value: T): T
}
```
## 14 泛型(2)：泛型类与泛型约束

泛型中不能明确有什么属性，直接使用不存在定义的属性会报错，通过 `extends` 一个 `interface` 就可以避免这个问题。

```
interface Length  {
	length: number
}
function <T extends Length>log(value: T) {
	console.log(value.length);//
}
```

## 15 类型检查机制(1)：类型推断

给变量直接赋值、参数设置默认值、返回值的时候，都会发生类型推断。

还有一类会根据调用上下文来推动，比如事件处理函数中，参数会推断为Event对象。

在推断不符合预期的时候，可以使用类型断言，会增加灵活性，但是会跳过字段检查不能滥用。

## 16 类型检查机制(2)：类型兼容性

类型兼容性包含了 基础数据类型兼容、函数兼容、类兼容、枚举兼容、泛型兼容。

具体兼容的规则，在不同的数据类型上不一定完全相同，可以在配置文件中修改规则，让兼容跟宽松。

具体使用遇到问题，可以参考本节，或者搜索 “typescript类型兼容”。

## 17 类型检查机制(3)：类型保护

在特定区块中可以保证变量属于某种特定的类型，使用的方法是 `instanceOf`、`in`、`typeof`。

TypeScript另外引入了一种方法：

```
functin isJava(lang: Java | JavaScript): lang is Java {
	return (lang as Java).helloJava != undefined;
}
```

## 18 高级类型(1)：交叉类型和联合类型

简言之就是一个类型可能是多个类型的交集 或 并集，对应 `&` 和 `|` 操作符。


## 19 高级类型(2)：索引类型

`keyof T` 可以表示 `T` 结构中所有属性的集合，可以用于限定参数值取值范围。

## 20 高级类型(3)：映射类型

```
interface Obj = {
	a: number,
	b: number
}
type ReadonlyObj = Readonly<Obj>;
type PartialObj = Partial<Obj>;
type PickObj = Pick<Obj, 'a' | 'b'>;
```

## 21 高级类型(4)：条件类型

```
type TypeName<T> {
	T extends string ? 'string' :
	T extends number ? 'number' :
	'object'
}
type t = TypeName<string>; // t 为 string
```



