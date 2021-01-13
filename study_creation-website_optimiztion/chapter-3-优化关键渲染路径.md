# 3. 优化关键渲染路径

优化关键渲染路径是优化网页性能的核心，这一节先介绍关键渲染路径的概念。

## 关键渲染路径

[CRP（关键渲染路径）](https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path) 简单来说就是浏览器将HTML、CSS和JavaScript转化为可见的像素的过程。

这个过程包含如下几个步骤：

![关键渲染路径](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com/critical-render-path.png)

网页的第一请求个是HTML请求，请求到达服务器后，服务器处理并且返回响应，客户端收到响应后开始进行处理，最后进行展示和布局，对应的过程为：

- request
- loading
- response
- scripting：HTML parsing，DOM Tree，CSSOM Tree，Render Tree
- rendering：layout
- painting: paint

## script

在scripting阶段，HTML parsing过程创建了DOM（document object model）Tree，遇到任意链接的外部资源（比如样式文件、js文件、图片文件），浏览器都会发起一个新的请求。

浏览器会持续不断进行parsing的过程构建DOM，发送获取资源的请求，直到最终结束以后，它开始构建 CSSOM（CSS object model）。在DOM和CSSOM都完成了，浏览器会构建好Render Tree，计算所有可见内容的样式，在浏览器的Performance中通常对应了Recalculate Style这个阶段。

## rendering

在rendering阶段，Render Tree构建好以后，开始进行Layout操作，它决定元素放置的位置和大小。

## painting

等这一切结束以后，在painting阶段，网页通过称为被paint的步骤被绘制出来。
