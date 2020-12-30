# kaizen-notes

本仓库存放记录的各种学习笔记和文章，仅用于存储文字用于减少仓库大小，图片相关内容通过oss上传后用链接访问。

在文件目录下执行`gitbook serve`即可以在本地查看将`markdown`转化为`html`的网页，网页的结构用文件目录下的`SUMMARY.md`进行描述。

## gitbook使用

[gitbook](https://www.gitbook.com/) 是一个写作工具，我们通过安装它的命令行工具 [gitbook-cli](https://github.com/GitbookIO/gitbook/blob/master/docs/setup.md) 来使用它。

安装后如下几个命令基本可以满足需求：

- `gitbook init`: 在当前文件夹初始book，包含`_book`文件夹用于输出html，`SUMMARY.md`代表了入口
- `gitbook serve`:  会在本地启动一个web服务器，修改了目录下的文件会自动触发刷新
- `gitbook build`: 将markdown文件生成html后打包到`_book`