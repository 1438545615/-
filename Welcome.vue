<!--  -->
<template>
  <div class="welcome">
    <GetPassengerFlow1 />
    <h2>欢迎XXX进入网站</h2>

    <div id="main" style="width: 1000px; height: 400px"></div>
    <!-- <div id="main2" style="width: 600px; height: 400px"></div>
    <div id="main3" style="width: 600px; height: 400px"></div> -->
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
const echarts = require("echarts");

import { echWatch } from "../../api/users";
import GetPassengerFlow1 from "../../components/getPassengerFlow1.vue";
export default {
  //import引入的组件需要注入到对象中才能使用
  components: { GetPassengerFlow1 },
  data() {
    //这里存放数据
    return { charts: "", opinionData: [] };
  },
  //方法集合
  methods: {
    getEcharts() {
      echWatch().then((res) => {
        console.log(res);
        this.opinionData = res.data.data;
        this.drawLine("main");
      });
    },
    drawLine(id) {
      this.charts = echarts.init(document.getElementById(id));
      // 指定图表的配置项和数据
      this.charts.setOption({
        title: {
          text: "能耗监测", // 标题
          textStyle: {
            fontWeight: "bold",
            fontSize: 14, // 字体大小
          },
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          // data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
          data: this.opinionData.map((v) => v.time),
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            // data: [320, 500, 662, 798, 863, 1330, 1320],
            data: this.opinionData.map((v) => v.value),

            type: "line",
            itemStyle: {
              normal: {
                color: "#597EF7", //折点颜色
                lineStyle: {
                  color: "#597EF7", //折线颜色
                },
              },
              label: { show: true }, //是否在折线点上显示数字
            },
            smooth: true,
            symbolSize: 13, // 空心点大小
            areaStyle: {
              // 折线渐变
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  {
                    offset: 0, // 0%
                    color: "rgba(80,141,255,0.39)",
                  },
                  {
                    offset: 0.5, // 50%
                    color: "rgba(56,155,255,0.25)",
                  },
                  {
                    offset: 1, // 100%
                    color: "rgba(38,197,254,0.00)",
                  },
                ]),
              },
            },
          },
        ],
      });
    },
  },
  mounted() {
    this.getEcharts();
  },
};
</script>
<style>
</style>