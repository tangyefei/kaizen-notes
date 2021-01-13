# 2.3 关键渲染路径指标

## 网页性能优化的方法

请求得到HTML后，会解析的得到DOM、CSSOM，最终构成Render Tree。因此我们总结下来让网页性能优化的两种途径

### 让网页和所需资源早点到位

- 通过压缩、合并、混淆，我们可以减少请求资源的**大小**。
- 通过浏览器和服务端缓存，我们能减少请求的**次数**。
- 通过媒体查询，可以减少请求资源的**数量**。
- 使用CDN，利用物理距离的缩短，加快请求的**速度**。
- 使用雪碧图，减少请求**数量**。
- 使用裁切得大小合适的图，减少请求资源**大小**。

### 让网页更早满足展示或使用条件

- 通过给不需要马上执行的JS设置async 或 延缓执行，减少阻塞
- 使用内联的CSS，减少阻塞（虽然增加了HTML文件的大小，但是减少了请求的数量）
- 使用骨架屏进行展示

## CRP衡量的三个指标

有一个标准常被用来衡量CRP(关键渲染路径)的性能，他们分别是

- CRP resourcs，代表了请求资源的数量
- CRP KB，代表了所有CRP resources资源的大小
- CRP length，代表了从请求从客户端到服务端来回的次数

内联的CSS，async或defer的JavaScript不参与到上述几个指标的计算中。因此一个网页的上述三个指标适用的文件数量等于 1 + 直接请求的CSS和Javascript文件数量，其中1是HTML文件内容。


在PC端打开该网页

```
<html>
  <head>
    <meta name="viewport" content="width=device-width">
    <link href="style.css" rel="stylesheet">
    <link href="print.css" rel="stylesheet" media="print">
  </head>
  <body>
    <p>
      Hello <span>web performance</span> students!
    </p>
    <div><img src="awesome-photo.jpg"></div>
    <script src="app.js"></script>
    <script src="analytics.js" async></script>
  </body>
</html>
```

- CRP resourcs，数量为2
- CRP KB，大小为`style.css`、`analytics.js`文件大小之和
- CRP length，长度为2
