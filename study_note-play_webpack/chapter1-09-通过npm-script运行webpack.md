## 09 通过npm script运行webpack

本地安装默会在`node_modules/.bin`下创建一个软链接，而`package.json`是能读取到该目录下的命令的。

因此为了更方便地构建，可在`package.json`中如下配置，然后通过`npm init -y`生成该文件。

然后通过增加配置：

```
"scripts": {
	"webpack": "webpack"
},
```
`npm run webpack`即可执行构建，然后在dist目录上启动自己安装的web服务器即可查看效果（例如 `http-server`)，可以看到 hello world 被输出到了空白页面中。

