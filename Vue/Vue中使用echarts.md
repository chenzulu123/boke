## Vue中echarts的使用
#### 1、安装echarts
```bash
npm install echarts --save
```
#### 2、在main.js中引入echarts
```js
//引入echarts
import echarts from 'echarts'
//全局注册echarts
Vue.prototype.$echarts = echarts 
```
#### 3、在使用的组件中使用echarts
```js
//在mounted中执行method中定义的方法
  mounted() {
    this.drawLine();
  },
//定义初始化echarts的方法
  methods: {
    drawLine() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("myChart"));
      // 绘制图表
      myChart.setOption({
        title: {
          text: "在Vue中使用echarts",
          show: false
        },
        tooltip: {},
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      });
    }
  }
```
### 总结
至于其他的使用以及参数的配置按照echarts的官方文档进行配置就可以了