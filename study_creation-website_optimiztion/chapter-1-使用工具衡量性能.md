# 1. 使用工具衡量性能


> You can't optimize what you can't measure

**必须以真实统计的数据为依据，优化过程中必须看妨碍性能的关键指标是否发生了变化**。
## PageSpeed Insights

[PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=zh-cn) 是由 Google 提供的一个检测网站性能的工具，输入网站地址后它会返回网站性能的得分，以及具体改善性能的建议。

![pagespeed-insights-leetcode.png](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com/pagespeed-insights-leetcode.png)


## Chrome DevTools

[Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools)作为最重要的网页开发调试工具，几乎能查看到网页加载使用过程的一切信息，作为开发者有必要熟练运用开发者工具。

![chrome-devtools-leetcode.png](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com/chrome-devtools-leetcode.png)

## 总结

这里只是一个开头，后续就要开始介绍影响性能的两个途径，即：

- 1）优化关键渲染路径
- 2）优化代码书写方式

借助于上述的工具，我们能够很好的衡量网站在具体指标上的表现。


