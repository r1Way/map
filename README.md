# map

## 项目简介

用于随机抽取一个省，并显示对应的省/简称/省会等信息。

* 简单模式

  ![easy](img_for_readme\easy_model.png)

* 困难模式

  ![hard](img_for_readme\hard_model.png)

## 使用意义

地理老师可以使用该项目来抽同学回答问题。简单模式显示中国全部地图，困难模式只显示被抽取到的省的形状。

## 开发学习意义

1. 了解vue框架
2. 学习这种svg图片该如何使用
3. 了解前端如何使用浏览器的console进行debug

## 项目结构

* 中国svg地图以及各省的svg信息分别放在了`public\ammap\maps\svg\chinaHigh.svg`与`public\ammap\maps\js\chinaHigh.js`。
* 各省图片放在了`public\ammap\images`里面
* 项目代码主要是放在`src\components\ChinaMap.vue`里面

## 使用（对于非开发者）

只用保留dist目录以及其下的所有文件即可，用浏览器（建议使用edge）打开`dist\index.html`即可。

## 环境

* java 23.0.1
* npm 10.9.2
* vue 5.0.8

## 项目部署（开发者）

```
npm install
```

### 编译及热重载
```
npm run serve
```

### 编译并build成静态文件
```
npm run build
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## 改进（未来）

1. 部分区域由于比较小以及svg精度不是很高，导致显示出来，太棱角分明了
2. 左侧画布添加一个鼠标滚轮放大缩小的功能（虽然我觉得没有什么用）

## 联系作者

如果发现有什么省与图片不对应的情况，请发送邮件到2142341323@qq.com，看到了会回复（虽然平时不怎么看邮件）。
