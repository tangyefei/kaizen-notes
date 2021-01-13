# 2.2 资源的阻塞问题

本篇先介绍一些基础概念，加载和阻塞的一般性结论，最后会用例子去验证这些结论。

## 概念定义

- 加载，资源从服务器到客户端的过程，HTML/CSS/JavaScript/Image都必须有这个过程
- 构建，HTML被构建为DOM，CSS被构建为CSSOM，DOM+CSSOM被构建为Render Tree
- parse，HTML作为富文本语言，需要parse + 构建后能转化为DOM
- render，对应Render Tree被Layout的过程

## 阻塞的一般性结论

一些资源的请求是阻塞性的。这里先列举一般性的结论：

- HTML加载，是最先加载的资源，其他资源依次并行加载，完成时间和加载顺序无关，和网络和文件大小有关。

- JS的加载，会阻塞HTML parse 和 CSS render；JS的执行按照标签出现的顺序执行。

- CSS的加载，会阻塞HTML parse 和 JS 执行；CSS render需要整个文件加载解析完成；CSS render的顺序和标签的出现顺序相同。


## HTML加载

### 例子：资源并行加载

虽然引入资源的代码在HTML文件的不同位置，但浏览器预加载程序会扫描页面以并行的方式加载资源(包括CSS资源）。

结果就是一些比较大的文件先加载但后完成。

```
// html-CRP-block.html
<head>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
  <script src="link-jquery.js"></script>
  <link rel="stylesheet" href="bg-color.css"></link>
</head>
<body>
  <p>Hello, CRP!!!</p>
</body>
```

```
//link-jquery.js
console.log('jquery download and executed ? ' + (!!$ ? 'yes' : 'not ye'));
```

```
//bg-color.css
p {
	background: red;
}
```

我们用Chrome Devtools的Performance记录下来网络情况（如下图），蓝色是HTML，黄色是JS，紫色是CSS。

首当加载完成的是HTML，然后是本地的 `link-jquery.js` 和 `bg-color.css`文件，`jquery-3.4.1.js`先排队了一段时间，然后请求到达，是最晚完成的。

![resource load](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com/resource-load.png)

## JS加载

### 例子：JS的按照加载顺序执行


如下为console中的输出结果，可见虽然本地的js后面的先加载完，但是执行顺序能获取到jquery，说明执行是后执行的。

即JavaScript的执行是严格依赖标签的出现顺序的。

```
> jquery download and executed ? yes
```

### 例子：JS的加载解析阻塞HTML和CSS的解析

#### 阻塞HTML的parse

```
// html-CRP-block.html
<head>
  <link rel="stylesheet" href="bg-color.css"></link>
</head>
<body>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
  <script src="link-jquery.js"></script>
  <p id="hello">Hello, CRP!!!</p>
</body>
```

```
//link-jquery.js
console.log(document.getElementById('hello'));
```

如上代码中，我们可以在console中看到的输出结果为null，说明 **JS的加载执行过程，block了HTML的解析**，因为直接查询DOM Tree是没有该结构的。


#### 阻塞CSS的render

```
//bg-color-red.css
p {
	background: red;
}
```

```
//bg-color-green.css
p {
	background: green;
}
```

```
// html-CRP-block.html
<body>
  <p id="hello">Hello, CRP!!!</p>
  <link rel="stylesheet" href="bg-color-red.css"></link>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
  <link rel="stylesheet" href="bg-color-green.css"></link>
</body>
```


![css-block-render](https://kaizen-notes.oss-cn-beijing.aliyuncs.com/css-block-render.png)

上图可以得到的信息包括：
1. 两个CSS文件都早于JS文件加载完成
2. 在JS文件加载完成之前，第一个CSS的样式已经成功render（此时应该没有所有的CSSOM构建完成才对，为何能红色呢，难道renderTree分批次的吗🤔️）
3. 在JS文件加载即将完成之前（截图没体现），第二个CSS的样式生效
4. DCL（DomContentLoadedEvent）的触发时间在JS加载完成之后，也在第二个CSS生效之后（这和第二描述是一致的）

从第3条可以得知：**JS的加载同样也阻塞CSS的解析和渲染。**

## CSS的加载

关于CSS同样也有一些一般性结论：

1. CSS的加载和解析同步进行的
2. 整个CSS文件都被解析好，才会绘制CSSOM并且完成绘制
3. CSS 的绘制，同样按照标签的出现顺序进行绘制

### 例子：CSS按照出现顺序进行绘制

CSS文件的加载是**加载和解析同步进行的；但只有等到整个CSS文件都被解析好，才会绘制CSSOM并且完成绘制**，这是因为CSS文件中属性是可以相互覆盖的，加载部分就进行渲染无疑会降低性能。

下例子中先引入了bootstrap样式文件，界面长时间处于白色背景（哪怕本地的css已经加载完），然后才出现红色背景，并且body的padding消失，可见 **CSS 的绘制，同样按照标签的出现顺序进行绘制**。

```
// html-CRP-block.html
<body>
  <p id="hello">Hello, CRP!!!</p>
  <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" rel="stylesheet">
  <link rel="stylesheet" href="bg-color-red.css"></link>
</body>
```

### 例子：CSS的加载对HTML/JS的影响

```
//bg-color-red.css
p {
	background: red;
}
```

```
// html-CRP-block.html
<body>
  <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" rel="stylesheet">
  <p id="hello">Hello, CRP!!!</p>
  <script src="link-jquery.js"></script>
  <link rel="stylesheet" href="bg-color-red.css"></link>
</body>
```

网页中打开上述HTML可以看到，首先是页面空白，并且console中没有输出，然后过了数秒，才会出现红色背景的文字区域。

打开分析工具录制，可以看到蓝色的HTML先加载、然后是link-jquery.js、background-red.css先完成（虽然bootstrap.css先加载），在bootstrap.css加载完后，先Parse Stylesheet，然后继续 Parse HTML、Evaluate Script，可见 **CSS的加载和解析也会阻塞HTML的解析 和 JS的解析执行**。


![css-block-parse](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com//css-block-parse.png)

![css-block-parse](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com//css-block-parse-2.png)


## 练习

通过上述的介绍相信你能基本理解网页加载过程中的顺序，尤其是JS/CSS将会如何相互作用，下面是到练习题，你可以试试看：

![try-anwser-page-steps.png](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com/try-anwser-page-steps.png)

答案是`1 3 6 2 4 5`，你也可以参考[视频](https://classroom.udacity.com/courses/ud884/lessons/1464158642/concepts/23732285930923)的讲解。




