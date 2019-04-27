---
title: echarts地图自适应中出现重叠错位的两个地图问题排查
date: 2019-04-27 21:10:41
categories:
- 可视化
tags:
- echarts
---

项目的可视化模块中使用了`echarts`作为图表库。当我对散点类地图作自适应开发时，发现在对地图进行偏移或缩放的时候，画布上出现了重叠并且错位的两个地图。

## 问题现象

![offset](/images/echarts/offset.png)

![zoom](/images/echarts/zoom.png)

>图片为演示图，非项目实例图。

为突出效果，图中用蓝、黑两种颜色对地图边框进行了着色。上图是偏移的情况，下图是放大的情况。

设置偏移的代码：
```javascript
option = {
    geo: {
       left: 450 
    },
    series: [
        // ...
    ]
}
```

设置放大的代码：
```javascript
option = {
    geo: {
        zoom: 1.1
    },
    series: [
        // ...
    ]
}
```


## 问题排查

因为项目代码已经很多设置项目，直接从代码里排查问题比较困难，所以就拿来echarts社区的示例来作对比。

首先对[示例代码](https://gallery.echartsjs.com/editor.html?c=xSkUQiHdBz)分别进行偏移和缩放设置，发现显示完全正常，没有重叠或错位现象。

既然示例没有问题，那是不是`option`选项哪里设置不对了？当把项目中的`option`放到示例代码中时，现象出现了，现在可以确定是设置的问题，而不是`echarts`的问题了。
> 代码里有人注释，同时给`geo`和`series`设置相同的偏移可以避免地图重影。但是原因是什么呢？

能确定是选项设置的问题，但从同事给的注释里又看不出本质原因，所以最好的方法就是深入`echarts`项目文档查找答案了。

在`echarts`的[github]()项目issues中查找关于地图重叠的问题时，发现了`map`类型的一个参数：`geoIndex`，只要将其值设置为`0`即可。

虽然也没有给出真正的答案，但`geoIndex`就是线索。

打开`echarts`的配置项手册，查看`series-map`的`geoIndex`选项，发现如下描述：
> 默认情况下，map series 会自己生成内部专用的`geo`组件。但是也可以用这个`geoIndex`指定一个`geo`组件。
> ...
> 当设定了`geoIndex`后，series-map.map 属性，以及 series-map.itemStyle 等样式配置不再起作用，而是采用`geo`中的相应属性。

原来`geo`和`series-map`是两个相互独立的组件，所以最初代码只对`geo`进行偏移或缩放设置时，会出现重叠的地图。

因此当二者同时使用时，要么（一）同时设置缩放或者偏移量，要么（二）给`series-map.map`指定关联的`geo`组件，才能让画布上的地图正常显示。

考虑到项目允许对可视化组件进行自定义配置，所以最后采用了第一种方案。

修改后的代码：
```javascript
option = {
    geo: {
        zoom: 1.1
    },
    series: [
        // ...
        {
            zoom: 1.1
        }
    ]
}
```

![正常缩放的地图](/images/echarts/normal.png)

## 总结

当我们在使用工具库或者框架等成熟方案时，一定要仔细读官方文档，很多问题的答案都在其中。