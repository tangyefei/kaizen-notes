# 指标如何计算

第1节内容来自[Google的开发者文档](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp?hl=zh-cn)，第2节内容来自[retcode文档中的指标说明](https://yuque.antfin.com/retcode/arms-retcode/keywords)

![navigation-timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/dom-navtiming.png?hl=zh-cn)

## 关键时间戳

### domLoading

这是整个过程的起始时间戳，浏览器即将开始解析第一批收到的 HTML 文档字节。

### domInteractive

表示浏览器完成对所有 HTML 的解析并且 DOM 构建完成的时间点。

### domContentLoaded

一般表示 DOM 和 CSSOM 均准备就绪的时间点，可以构建渲染树了。

框架可以此时开始执行它的逻辑，通过捕获 EventStart 和 EventEnd 时间戳，能够追踪执行所花费的时间。jQuery的ready就是在此时使用，它的具体用法是:

```
document.addEventListener('DOMContentLoaded', function(){});
```

### domComplete

网页及其所有子资源（图像等）都准备就绪，都已下载完毕。

### loadEvent

作为每个网页加载的最后一步，浏览器会触发 onload 事件，以便触发额外的应用逻辑。

```
window.onload = function(){}
```


## 关键指标

### fpt(first paint time)

首次渲染时间 / 白屏时间，计算公式为：`domLoading - navigationStart`，即键入网址到浏览器开始解析DOM到时间。

### tti (time to interact)

首次可操作时间，计算公式为 `domInteractive - navigationStart`

### ready

html 加载完成时间， 即 dom ready 时间，计算公式为 `domContentLoadEventEnd - navigationStart`，如果页面有同步执行的 js，则同步 `js 执行时间 = ready - tti`。

### load

页面完全加载时间，计算公式为`loadEventStart - navigationStart`，

注：附图中没有loadEventStart，可以理解为 domComplete 时间戳之后。

更细粒度的时间戳可以在 [www.w3.org 规范](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp?hl=zh-cn)中看到。

## 3.思考

对于DOMContentLoaded，Google的文档中有这么一段描述

> 如果JavaScript没有阻塞解析器，则 DOMContentLoaded 将在 domInteractive 后立即触发。



结合第 ready 指标中描述，可以推测出一些结论：

1. DOM 准备就绪后，CSSOM 几乎同步准备就绪，因为 CSS 的引入通常是在head中，并且是阻塞的，DOM 就绪时候，CSSOM 也会是就绪的。

2. 阻塞的 JavaScript 即同步执行的 js 代码。domInteractive 到 domContentLoadEventEnd 的时间，就是同步的 JavaScript 的执行时间；如果没有阻塞的JavaScript，domContentLoadEventEnd 几乎在 domInteractive 后马上执行。
