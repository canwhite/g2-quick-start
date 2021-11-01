<template>
  <div id="app">
    <!-- 我们用一个基础的线来完成学习 -->
    <div id="basic-line"> </div>
  </div>
</template>

<script>

import {Chart} from '@antv/g2';
export default {
  data(){
    return{

    }
  },
  created() {
    
  },
  mounted() {
    this.setBasicLine();
    
  },
  methods:{

    //画一个基础的折线图，我们把g2的基础知识讲一下
    setBasicLine(){
      const data = [
        { year: '1991', value: 3 },
        { year: '1992', value: 4 },
        { year: '1993', value: 3.5 },
        { year: '1994', value: 5 },
        { year: '1995', value: 4.9 },
        { year: '1996', value: 6 },
        { year: '1997', value: 7 },
        { year: '1998', value: 9 },
        { year: '1999', value: 13 },
      ];
      //1.创建图表实例
      const chart = new Chart({
        container: 'basic-line',
        autoFit: false,
        width:500,
        height: 500,
      });
      
      //2.设置数据
      chart.data(data);


      //3.坐标轴内容和样式
      //(1)scale设置坐标上的具体内容
      chart.scale({
        year: {
          range: [0, 1],//设置坐标值两边留白
        },
        value: {
          min: 0,//min和max确定范围
          nice: true,
          alias:"销售量",
          //刻度线操作
          //tickCount: 10,//自定义刻度线的个数，取得是minCount，可能还多
          ticks: [0,2, 4, 6, 8, 10, 12, 14, 16, 18, 20],//自定义刻度线内容
          formatter: (val) => `￥${val}`,//自定义刻度线显示
        },
      });
      //(2)axis设置坐标轴的样式
      chart.axis("value",{
        title:{
          style:{
            fill: '#1890ff',//alias的显示颜色
          }
        }
      })

      //4.图例
      // 隐藏
      /* ------------------------------------------------
      chart.legend(false); // 隐藏全部图例
      chart.legend('x', false); // 只隐藏 x 维度对应的图例 
      chart.legend('x', {
        position: 'bottom',
      }); // 只更改 x 维度对应的图例的显示位置

      --------------------------------------------------*/
     
     //显示在某个位置
     
      chart.legend("value",{
        position:"right"
      })



      //5.提示信息
      chart.tooltip({
        showCrosshairs: true, // 展示 Tooltip 辅助线，中间会多条线
        shared: true,
      });


      


      //6.如果是饼状图，涉及到坐标系



      


      //7.几何图形,设置不同的样式，需要不同的几何图形
      //常用的有line，point，interval(区间图，柱饼环)

      //eg:折线图和折线图上的点
      //在这个阶段实际上还可以通过链式语法对样式和内容进行更改
      //不过尽量可以做到前边，这里智识一个补充
      chart
        .line()
        .position('year*value')
        .label('value');
      chart.point().position('year*value');


      //ps:交互方式



      //8.配置完的绘制
      chart.render();
    }
  }

}




</script>



<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
