<template>
  <div id="app">
    <!-- 我们用一个基础的柱+点来完成学习 -->
    <div id="basic-line"> </div>
  </div>
</template>

<script>
//引入滑块
import Slider from '@antv/g2/lib/chart/controller/slider';
//引入滚动条
import Scrollbar from '@antv/g2/lib/chart/controller/scrollbar';

import {Chart,registerComponentController} from '@antv/g2';
import DataSet from '@antv/data-set'
// 引入 slider 组件
registerComponentController('slider', Slider);

registerComponentController('scrollbar', Scrollbar);


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

      //4.提示信息，冒泡提示-------------------------------------------
      //辅助线细节处理
      chart.tooltip({
        //showCrosshairs: true, // 展示 Tooltip 辅助线，中间会多条线
        showMarkers:false,//是否渲染 tooltipMarkers。去不去区别不大
        shared: true,
        //shared属性开启，相当于两个柱在一起的时候合并数据项
        //同时结合 'active-region' 交互行为，就能显示一个选中区域了
        type:"y",

        
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

      //eg:柱状图和柱状图上的点，可以对点和线的颜色进行一些配置
      chart
        .interval()
        .position('year*value')
        .label('value')
        //折线图上显示内容,line对图例和颜色的支持并不良好
        //interval的效果就好很多
        .color("value","blue")
        //有了color，就可以设置图例了，interval的显示良好

        //.adjust('stack')
        /*stack(层叠），将同一个分类的数据值累加起来。
        以层叠的柱状图为例，x 轴方向的同一个分类下面的数据，按照顺序，
        将 y 轴对应的值累加，最终将数据调整的不再重叠。 */

      chart.point().position('year*value');


      //7.图例----------------------------------------------
      //不同类别的色块
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



      //ps1:交互方式-------------------------------------------
      //鼠标选中显示的方式
      chart.interaction("active-region"); 


      //ps2:滑块----------------------------------------------
      //end,可以定义默认显示到什么位置
      /* chart.option('slider', {
        end: 0.5,
      }); */



      //ps3:滚动条-------------------------------------------
      //当宽度不够的时候滚动条就起作用了
      /* chart.option('scrollbar', {
        // 滚动条类型： 'horizontal' / 'vertical'
        type: 'horizontal',
      }); */


      //ps4:几种事件----------------------------------------
      //axis和动画图例什么的可以设置事件监听
      //eg:axis
      //chart.on('axis-label:click', (ev) => {});


      //ps5:动画--------------------------------------------
      //貌似看不出来啥效果
      /* chart.animate({
        //appear、enter、update、leave
        hotUpdate: {
          animation: 'fade-in', // 动画名称
          easing: 'easeQuadIn', // 动画缓动效果
          delay: 100, // 动画延迟执行时间
          duration: 600 // 动画执行时间
        }
      }); */




      //ps6:view图层--------------------------------------------

      //设置view图层，可以设置不同的view
      //一个 View 可以包含有多个子 View，通过这种嵌套关系，可以将一个画布按照不同的布局划分多个不同区域（分面），
      //也可以将不同数据源的多个 View 叠加到一起，形成一个多数据源，多图层的图表。
      //eg:

      /*
        import { Chart } from '@antv/g2';

        // step 1: 需要创建 chart 对象
        const chart = new Chart({
          container: 'container',
          autoFit: false,
          width: 1000,
          height: 500,
        });

        // step 2: 然后创建一个视图
        const view = chart.createView({
          region: {
            start: { x: 0.2, y: 0.2 }, // 指定该视图绘制的起始位置，x y 为 [0 - 1] 范围的数据
            end: { x: 1, y: 1 }, // 指定该视图绘制的结束位置，x y 为 [0 - 1] 范围的数据
          },
          padding: [20, 40], // 指定视图的留白
        });


        
        //-为了让用户更好更快速得指定视图的绘制区域，region 中的 start 和 end 这两个参数只接受 0 至 1 范围的数据。
        //-不指定 region，则默认为父 view 的绘图区域大小。
        //-View 的绘制起始点是画布左上角。
        
        //创建好 view 之后，就可以同 chart 一样载入数据，使用图形语法进行图表的绘制了，语法同 chart。
        
        view.data(data); // 为 View 载入数据
        view.interval().position('x*y').color('x'); // 使用图形语法绘制图表

        chart.render(); // 由 chart 负责统一的渲染

      */

      //ps7:dataView和transform--------------------------------------------
      //dataView数据视图，最终这里的数据会被渲染成视图
      //通过transform可以对view中的数据进行转换处理
      //主要是用于dataView，数据视图，创建dataView，然后通过示例去处理数据
      //另外，这个东西比较鸡肋，有些时候我们可以自己把数据处理好，再传 
      
      const testCSV = `Expt,Run,Speed
        1,1,850
        1,2,740
        1,3,900
        1,4,1070`;

        const dv = new DataSet.DataView().source(testCSV, {
          type: 'csv',
        });
        console.log(dv.rows);
        /*
        * dv.rows:
        * [
        *   {Expt: " 1", Run: "1", Speed: "850"}
        *   {Expt: " 1", Run: "2", Speed: "740"}
        *   {Expt: " 1", Run: "3", Speed: "900"}
        *   {Expt: " 1", Run: "4", Speed: "1070"}
        * ]
        */

        dv.transform({
          type: 'filter',
          callback(row) {
            return row.Run !== '1';
          },
        });
        console.log(dv.rows);
        /*
        * dv.rows:
        * [
        *   {Expt: " 1", Run: "2", Speed: "740"}
        *   {Expt: " 1", Run: "3", Speed: "900"}
        *   {Expt: " 1", Run: "4", Speed: "1070"}
        * ]
        * */

       /*视图的层叠
        // 创建一个dataview 
        const mainMap = ds.createView('back').source(mainData, { type: 'GeoJSON' })
        // 通过chart创建主视图，这是个view
        const mainMapView = chart.createView()
        //然后给主视图设置dataview
        mainMapView.data(mainMap.rows)//通过rows去拿到数据

       */






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
