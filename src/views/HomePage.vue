<template>
  <div>
    <LongChart title="pH原始值与预测值" :seqy="csvdata.slice(1)" :labels="ynames" :seqx="csvdata[0]" />
  </div>
</template>

<script>
  import LongChart from '../components/LongChart.vue'
  import axios from "axios";
  export default {
    name: 'HomePage',
    components: {
      LongChart
    },
    mounted() {
      this.getCsvfile()
    },
    methods: {
      csvToObject(csvString) {
        var csvarry = csvString.split("\r\n");
        var datas = [];
        for (var i = 0; i < csvarry.length; i++) {
          var data = csvarry[i].split(",");
          datas.push(data);
        }
        datas.pop();
        // console.log(datas);
        return datas;
      },
      getCsvfile() {
        //用axios的方法引入地址
        const that = this;
        axios.get('csv/temp.csv').then((res) => {
          if (res.status == "200") {
            that.csvdata = that.csvToObject(res.data);
            // that.csv2Option(true);
          }
        });
      },
    },
    data() {

      return {
        csvdata:[[],[],[],[]],
        testseq: [

          [10, 20, 30, 40, 50, 60],
          [11, 22, 33, 44, 55, 66],
        ],
        testx: [1, 2, 3, 4, 5, 6],
        ynames: ['原始值', 'y1','y2','y3']
      }
    },

  }
</script>

<style scoped>
</style>
