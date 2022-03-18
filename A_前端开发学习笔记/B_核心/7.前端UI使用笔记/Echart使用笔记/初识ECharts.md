## 一、 Apache ECharts 简介

 一个基于 JavaScript 的开源可视化图表库 

## 二、安装ECharts

```powershell
npm install echarts --save
```

```powershell
vue add echarts
```

三、引入ECharts

```js
import * as echarts from 'echarts';

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
  title: {
    text: 'ECharts 入门示例'
  },
  tooltip: {},
  xAxis: {
    data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
  },
  yAxis: {},
  series: [
    {
      name: '销量',
      type: 'bar',
      data: [5, 20, 36, 10, 10, 20]
    }
  ]
});
```

## 三、初始化Echarts

通过调用`echarts. init`函数，生成一个ECharts实例对象

参数：

> 1. dom元素   作为实例的容器 如果是隐藏的元素可能因为获取不到宽高导致初始化失败（可以通过设置容器内联样式的宽高来解决，或者容器显示后手动`echartsInstance.resize`调整尺寸）
> 2.  theme  主题    如果不指定主题，也需在传入`opts`前先传入`null` 
> 3.  opts   配置对象
>    -  `devicePixelRatio` 设备像素比 
>    -  `renderer` 渲染器，支持 `'canvas'` 或者 `'svg'` 
>    -  `useDirtyRect` 是否开启脏矩形渲染，默认为`false`。 
>    -  `width` 可显式指定实例宽度，单位为像素。  如果传入值为 `null`/`undefined`/`'auto'`，则表示自动取 `dom`（实例容器）的宽度。 
>    - `height` 可显式指定实例高度，单位为像素。如果传入值为 `null`/`undefined`/`'auto'`，则表示自动取 `dom`（实例容器）的高度。
>    -  `locale` 使用的语言，内置 `'ZH'` 和 `'EN'` 两个语言

