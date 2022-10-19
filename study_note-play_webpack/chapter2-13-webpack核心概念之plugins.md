## 13 webpack核心概念之plugins

### 概念介绍

通常用于loaders输出后的文件内容的优化，包含资源管理，环境变量的注入。

可以理解为用loaders无法做的事情，都交给plugins来做。

### 常用的plugins

- CommonChunsPlugin，把被多个chunks都依赖的模块代码提取成公共的js
- CleanWebpackPlugin，清理构建目录
- ExtractTextWebpackPlugin，将CSS从bundle文件里提取成独立的CSS文件
- CopyWebpackPlugin，将文件或文件夹拷贝到构建的输出目录
- HtmlWebpackPlugin，创建html文件去加载输出的bundle
- UglifyjsWebpackPlugin，压缩js
- ZipWebpackPlugin，将打包出的资源生成一个zip包

### 基础用法

我们现在src目录下新增一个空的html，不包含bundle.js的引用：

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
</body>
</html>

```


同样为了，通过修改配置文件的方式来安装老版本：

```
   "html-webpack-plugin": "^4.5.0",
```

然后在webpack.config.js中增加插件配置：
```
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
	plugins:[
		new HtmlWebpackPlugin({template: "./src/index.html"})
	]
}	
```
