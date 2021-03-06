# 2 HTTP是什么，HTTP又不是什么


## HTTP是什么

HyperText Transfer Protocol



### Protocol 协议

它的两个特点是：

- 两个或者多个参与者才构成“协”
- 对参与者的行为的约定和规范构成“议”

HTTP是一个用在计算机世界里的协议。它使用计算机能够理解的语言确立了一种计算机之间交流通信的规范，以及相关的各种控制和错误处理的方式。

### Transfer 传输


计算机世界里有各种角色，对应五花八门的角色，HTTP是一个传输协议，传输的含义是把东西从A点搬到B点。

它包含了两个重要信息：

- HTTP协议是一个双向协议，数据的流动可以从A到B，也可以从B到A
- 虽然数据在A、B之间传输，但可以有中间的参与者，即“中转”或“接力”


中转过程，只要不打扰基本的数据传输，就可以添加任意的额外功能，比如：安全认证、数据压缩、编码转换，从而优化整个传输过程。

HTTP是一个在计算机世界里，专门用来之间传输数据的约定和规范。

### HyperText 超文本

"文本"即有意义的数据，可以被上层应用程序处理，而非底层协议里的二进制包。

早期只是文字，现在已经包含了图片、音频、视频、甚至压缩包，都能算是“文本”。

超文本就是“超越了普通文本的文本”，它是上述各种资源的混合体，最关键的是含有“超链接”可以从一个文本跳转到另一个“超文本”，形成复杂的非线性的、网状关系。

最熟悉的超文本应该就是HTML了，它只是纯文字文件，但内部定义了各种链接，经过浏览器解释，在我们面前呈现的就是含有多种视听信息的页面。

综上三个关键字，可以回答

> HTTP是一个计算机世界里，专门在两点之间传输文字、图片、音频、视频等超文本数据的约定和规范。


## HTTP不是什么

它不是单独的实体，它不是浏览器、App那样的应用程序，也不是Apache、Nginx那样的应用程序。但它与他们密切相关，并且在通信过程中作为“动态的存在”，是发生在网络连接、传输超文本数据的一个“动态过程”。

HTTP不是互联网，互联网是许多网络连接而形成的巨大的国际网络，存在各种资源，也有各种协议，HTTP只是互联网里的一块重要拼图。

HTTP不是编程语言。反过来我们可以使用编程语言来实现HTTP，告诉计算机如何使用HTTP来通信。

HTTP不是HTML，HTML作为超文本的载体，是一种标记语言，HTTP上传输的是HTML以及它所描述的各种资源。

HTTP不是一个孤立的协议，它跑在TCP/IP协议上。
- 依靠IP来实现寻址和路由
- TCP协议实现可靠数据传输
- DNS协议实现域名查找
- SSL/TLS实现安全通信

此外还有一些协议依赖与HTTP，例如Websocket、HTTPDNS等。

这些洗衣相互交织，构成了一个协议网，而HTTP处于中心地位。

## HTTP的知识地图

![知识地图](https://blog-1258030304.cos.ap-guangzhou.myqcloud.com/books/master-http/http-graphic.jpeg)