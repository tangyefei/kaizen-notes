# 第二章 工程篇

## 22 ES6与CommonJS模块系统

两个模块系统TS都支持，要解析`.ts`文件为CommonJS模块，要安装`ts-node`，用`ts-node`替代`tsc`执行文件即可。

另外介绍了配置文件中的 `target` 和 `module` 属性，可以用于指定模块系统 和 版本相关的信息。

## 23 使用命名空间

使用 `namespace Module {}` 的方式在内部定义属性，可以方便在其他地方用于通过 `Module` 直接使用其中的定义。

## 24 理解声明合并

同名`interface`包含不同属性，会被合并，定义值的时候必须包含两个接口的所有属性。

特殊地，两个`interface`包含有同名但不同类型的属性，编译器会提示冲突；函数重载则不会提示冲突。

## 25 如何编写声明文件

🤔 为什么要有声明文件？

答：TS的优势就是类型检查，在对第三方类库进行检查时候，因为变量、参数没有声明过，就会无法进行类型检查，导致编译错误。

有的类库声明文件单独存放的，安装类库之后，还需要安装类库的声明文件，通常形式形如 `@types/react`。

可以在 [https://microsoft.github.io/TypeSearch/](https://microsoft.github.io/TypeSearch/) 中搜索所使用的类库是否有声明文件可以安装。

如果没有，可能需要自己写一个，更好地把它贡献发布出去。

本篇介绍了对不同模块类型编写声明文件的方式，不做记录，有需要自己再网搜资料吧。


## 26 tsconfig.json(1):文件选项

TS文件分文 `ts` `d.ts` `tsx` 三类，默认地会编译 src 目录下面的文件。

通过在 `tsconfig.json` 中配置`files ` `include` `exclude` 可以指定编译位置。

可以指定 `extends` 属性指定从另外一个文件中继承配置。

配置 `compileOnSave` 后可以在保存文件的时候自动编译，`VSCode` 不支持，需要用 `iTerm` 并且安装插件。

## 27 tsconfig.json(2):编译选项

编译选项有100多个，本节流水账地介绍了一些，完整的参考[这里](https://www.typescriptlang.org/docs/handbook/compiler-options.html) .

## 28 tsconfig.json(3):工程引用

项目之间有依赖关系的时候，可以通过声明多个tsconfig.json，并通过一些参数建立以来关系。

## 29 编译工具：从ts-loader到Babel



