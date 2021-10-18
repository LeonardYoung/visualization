# 可视化



#### 起因

这个项目用于长折线图的可视化。在python中用matplotlib绘制曲线时总觉得观感很差。遇到坐标点很多的情况，图像直接糊在一起了，就像这样：

![image-20211018101631359](https://yangsj-first-bucket.oss-cn-guangzhou.aliyuncs.com/markdown/image-20211018101631359.png)



于是想到用echart绘制曲线。最后其实就是写了个`LongChart.vue`组件，对echart进行了封装



#### 组件使用说明

组件名：LineChart

参数：

```js
props: {
      // 标题
      title: {
        type: String,
        default: '默认标题'
      },
      // y轴数据，可以有多个y轴
      seqy: {
        type: Array,
        required: true
      },
      // x轴数据
      seqx: {
        type: Array,
        required: true
      },
      // y轴标签名，应和seqy中的y轴数量一致
      labels: {
        type: Array,
      }
    },
```



效果如下。：

![image-20211018102302230](https://yangsj-first-bucket.oss-cn-guangzhou.aliyuncs.com/markdown/image-20211018102302230.png)

标题下方的选择框可以选择是否展示曲线。



#### 实际使用

。。。



#### todo

封装多图的情况

