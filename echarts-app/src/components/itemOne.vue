<template>
  <div>
    <h2>销售总量</h2>
    <div class="chart" id="oneChart">图表的容器</div>
  </div>
</template>

<script setup>
import { inject, onMounted, reactive } from "vue";

let $echarts = inject("echarts");
let $http = inject("axios");

let data = reactive({});
let xdata = reactive([]);
let ydata = reactive([]);
//对数据进行处理
function setData() {
  xdata = data.data.chartOne.chartData.map((v) => v.title);
  ydata = data.data.chartOne.chartData.map((v) => v.num);
}

async function getState() {
  data = await $http({ url: "/one/data" });
  // console.log(oneData.data.chartOne.chartData)
}

onMounted(() => {
  let myChart = $echarts.init(document.getElementById("oneChart"));
  getState().then(() => {
    setData();

    myChart.setOption({
      grid:{
        top:'3%',
        left:'1%',
        right:'6%',
        bottom:'3%',
        containLabel:true
      },
      xAxis: {
        type: "value",
        axisLine:{
          lineStyle:{
            color:'#fff'
          }
        }
      },
      yAxis: {
        type: "category",
        data:xdata,
        axisLine:{
          lineStyle:{
            color:'#fff'
          }
        }
      },
      series: [
        {
          data:ydata,
          type: "bar",
          itemStyle:{
            normal: {
                  barBorderRadius:[0,20,20,0],
                  color: new $echarts.graphic.LinearGradient(0, 0, 1, 0, [
                    {
                      offset: 0,
                      color: "#005eaa",
                    },
                    {
                      offset: 0.5,
                      color: "#339ca8",
                    },
                    {
                      offset: 1,
                      color: "#cda819",
                    },
                  ]),
                },
          }
        },
      ],
    });
  });
});
</script>

<style lang="less" scoped>
.chart {
  height: 4.5rem;
}
h2 {
  height: 0.6rem;
  color: #fff;
  line-height: 0.6rem;
  font-size: 0.25rem;
  text-align: center;
}
</style>