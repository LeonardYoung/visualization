<template>
  <div>
    <h2 class="title">{{title}}</h2>
    <label v-for="(item,index) in lineNames" :key="item">
      <input name="factor" type="checkbox" v-model="checkValue[index]" />
      {{item}}
    </label>
    <v-chart ref='chart' class="chart" :option="option" @datazoom="dataZoom" />
  </div>

</template>

<script>
  // import VChart from "vue-echarts";
  // import { use } from "echarts/core";
  // import { CanvasRenderer } from "echarts/renderers";

  // import {
  //   TitleComponent,
  //   TooltipComponent,
  //   LegendComponent
  // } from "echarts/components";
  // import { LineChart } from "echarts/charts";
  // use([
  //   CanvasRenderer,
  //   LineChart,
  //   TitleComponent,
  //   TooltipComponent,
  //   LegendComponent
  // ]);

  import 'echarts'
  import VChart from "vue-echarts";

  export default {
    name: "LineChart",
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
    components: {
      VChart
    },
    watch: {
      checkValue() {
        this.filterSerise(this.checkValue)
        this.$refs.chart.setOption(this.option, true);
      },
      seqx() {
        this.setupSeqx()
        this.$refs.chart.setOption(this.option, true);
      },
      seqy() {
        this.setupSeqy()
        this.$refs.chart.setOption(this.option, true);
      },
      labels() {
        this.setupLabel()
        this.$refs.chart.setOption(this.option, true);
      }
    },
    methods: {
      filterSerise(checkValue) {
        // 设置y轴
        this.option.series = []
        for (let i = 0; i < this.seqy.length; i++) {
          if (checkValue[i] == true) {
            this.option.series.push({
              name: this.lineNames[i],
              type: "line",
              smooth: true,
              data: this.seqy[i]
            });
          }
        }
      },
      dataZoom(data) {
        // console.log("zoom event",data);
        this.start = data.start
        this.end = data.end
        this.option.dataZoom[0].start = this.start;
        this.option.dataZoom[0].end = this.end;

      },
      setupSeqx() {
        // 设置x轴
        this.option.xAxis.data = this.seqx
      },
      setupSeqy() {
        this.checkValue = []
        // 设置y轴
        for (let i = 0; i < this.seqy.length; i++) {
          this.option.series.push({
            name: this.lineNames[i],
            type: "line",
            smooth: true,
            data: this.seqy[i]
          });

          this.checkValue.push(true)
        }
      },
      setupLabel() {
        if (this.labels == undefined) {
          this.lineNames = []
          for (let i = 0; i < this.seqy.length; i++) {
            this.lineNames.push("y" + i)
          }
        } else {
          this.lineNames = this.labels
        }
      },
      setupSeries() {


        this.setupSeqx()

        // 设置标签名
        this.setupLabel()


        this.setupSeqy()

      }
    },
    mounted() {
      this.setupSeries()
    },
    data() {
      return {
        start: 0,
        end: 20,
        checkValue: [],
        lineNames: [],
        option: {
          xAxis: {
            type: 'category',
            // data: [1,2,3,4,5,6]
          },
          // title: {
          //   text: "默认标题",
          // },
          tooltip: {
            trigger: "axis",
          },
          legend: {
            show: true,
          },
          yAxis: {
            type: 'value'
          },
          dataZoom: [{
            type: "slider",
            realtime: true,
            start: this.start,
            end: this.end,
            xAxisIndex: [0],
          }, ],
          series: [
            //   {
            //   data: [10,20,30,40,50,60],
            //   type: 'line'
            // },
          ]
        },
      };
    }
  };
</script>

<style scoped>
  .chart {
    height: 400px;
  }
</style>
