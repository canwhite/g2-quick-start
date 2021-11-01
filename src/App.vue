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
      //1.创建图表实例-----------------------------------
      const chart = new Chart({
        container: 'basic-line',
        autoFit: false,
        width:500,
        height: 500,
      });
      
      //2.设置数据-------------------------------------
      chart.data(data);


      //3.坐标轴内容和样式-----------------------------
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

      //4.提示信息-------------------------------------------
      chart.tooltip({
        //showCrosshairs: true, // 展示 Tooltip 辅助线，中间会多条线
        showMarkers:false,
        shared: true
        //shared属性开启，相当于两个柱在一起的时候合并数据项
        //同时结合 'active-region' 交互行为，就能显示一个选中区域了
        
      });


    
      //5.如果是饼状图，涉及到坐标系,这种是极坐标系---------------------------
      /* chart.coordinate('theta', {
        radius: 0.75,
        //加内部半径就可以换程环状图，去掉就是饼图
        innerRadius: 0.6,
      });
      */

      


      //6.几何图形--------------------------------------------
      //设置不同的样式，需要不同的几何图形
      //常用的有line，point，interval(区间图，柱饼环)

      //eg:折线图和折线图上的点，可以对点和线的颜色进行一些配置
      chart
        .interval()
        .position('year*value')
        .label('value')
        //折线图上显示内容,line对图例和颜色的支持并不良好
        //interval的效果就好很多
        .color("value","blue"); 
        //有了color，就可以设置图例了，interval的显示良好

        /* .adjust('stack')
        stack(层叠），将同一个分类的数据值累加起来。
        以层叠的柱状图为例，x 轴方向的同一个分类下面的数据，按照顺序，
        将 y 轴对应的值累加，最终将数据调整的不再重叠。 */

      chart.point().position('year*value');


      //7.图例----------------------------------------------
      /* 
      // 隐藏
      chart.legend(false); // 隐藏全部图例
      chart.legend('x', false); // 只隐藏 x 维度对应的图例 
      chart.legend('x', {
        position: 'bottom',
      }); // 只更改 x 维度对应的图例的显示位置

      */


     
     //显示在某个位置
     //注意：如果要使用图例，依赖于对图形颜色的配置
     //要不然不会显示
      chart.legend({
        position: 'right',
      });



      //ps:交互方式-------------------------------------------
      chart.interaction("active-region"); 


      //8.配置完的绘制----------------------------------------
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
