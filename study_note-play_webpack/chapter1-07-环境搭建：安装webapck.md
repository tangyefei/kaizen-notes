## 07 环境搭建：安装webapck



### 安装node和npm

本篇教程作者先安装了了nvm，然后基于nvm来安装node和npm。感兴趣可以了解下nvm的使用。

### 安装webpack和webpack-cli

```
$ npm init
$ npm i webpack webpack-cli -D
```

更新：考虑到后续webpack版本升级到5，要沿用本课程学习需要指定版本，可以将下述配置拷贝到pacakge.json中

```
  "webpack": "^4.42.0",
  "webpack-cli": "^3.3.11",
```
