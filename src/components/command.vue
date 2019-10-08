<template>
  <div>
    <mt-tab-container class="page-tabbar-container" v-model="selected">
      <mt-tab-container-item id="指挥">
        <mt-header fixed style="font-size:25px;height: 50px;" title="视频通话">
         <!--  <mt-button size="small" slot="left"
            @click="$goRoute('/CPR')"><small>今日统计</small></mt-button>
          <mt-button size="small" slot="right"
            @click="$goRoute('/increaseA')"><small>新增病人</small></mt-button> -->
          <hr>
        </mt-header>
        <br><br>  
        <br><br>
      </mt-tab-container-item>
      <mt-tab-container-item id="地图">
        <mt-header fixed style="font-size:25px;height: 50px;" title="地图实况">
          <hr>
        </mt-header>
        <br><br>
      </mt-tab-container-item>
      <mt-tab-container-item id="统计">
        <mt-header fixed style="font-size:25px;height: 50px;" title="信息统计">
          <hr>
        </mt-header>
        <div id ="myChart1" style="width:100%;height:300px"></div><hr>
        <div id ="myChart2" style="width:100%;height:300px"></div><hr>
        <div id ="myChart3" style="width:100%;height:300px"></div><hr>
        <div  id="myChart4" style="width:100%;height:300px"></div><hr>
        <div id ="myChart5" style="width:100%;height:300px"></div>
                <br><br>
      </mt-tab-container-item>
    </mt-tab-container>
    <div>
      <mt-tabbar v-model= "selected" fixed>
        <mt-tab-item id="指挥">
          <img slot="icon" src="./icon/语音通话.png"><div style="font-size:12px">指挥</div>
        </mt-tab-item>
        <mt-tab-item id="地图">
         <img slot="icon" src="./icon/本车.png"><div style="font-size:12px">地图</div>
        </mt-tab-item>
        <mt-tab-item id="统计">
          <img slot="icon" src="./icon/病人.png"><div style="font-size:12px">统计</div>
        </mt-tab-item>
      </mt-tabbar>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import {picker} from 'mint-ui';
import axios from 'axios';
import { Toast } from 'mint-ui';
import { MessageBox } from 'mint-ui';
export default {
  // inject:['reload'],
  data() {
    return {
      selected: "统计",

    };
  },
  mounted() {
    console.log();
    this.draw1();
    this.draw2();
    this.draw3();
    this.draw4();
    this.draw5();
  },
  methods: {
    draw1() {
      let chart = this.$echarts.init(document.getElementById('myChart1'), 'dark')
      axios.get('/getAllStatusCount', {}).then((response) => {
        var a = response.data.results.chuzhizhong;
        var b = response.data.results.chuzhiwancheng;
        var c = response.data.results.daihousong;
        var d = response.data.results.housongzhong;
        var e = response.data.results.yiruyuan;
        var f = response.data.results.yichuyuan;
        var data1 = [];
        data1.push({ name: '处置中', value: a }, { name: '处置完成', value: b }, { name: '待后送', value: c }, { name: '后送中', value: d }, { name: '已入院', value: e }, { name: '已出院', value: f })
        chart.setOption({
          title: {
            text: '病人状态统计'
          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
          },
          series: [
            {
              name: '病人状态统计',
              type: 'pie',
              radius: ['30%', '60%'],
              center: ['55%', '50%'],
              avoidLabelOverlap: false,
              data: data1
            }
          ]
        })
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    draw2() {
      let chart = this.$echarts.init(document.getElementById('myChart2'), 'dark')
      axios.get('/getAllClassificationCount', {})
        .then((response) => {
          var cdata = response.data.results
          var f = cdata.find(item => item.Classification == 0).count
          var a = cdata.find(item => item.Classification == 1).count
          var b = cdata.find(item => item.Classification == 2).count
          var c = cdata.find(item => item.Classification == 3).count
          var d = cdata.find(item => item.Classification == 4).count
          // var e = cdata.find(item => item.Classification == 5).count
          var data1 = [];
          data1.push({ name: 'Ⅰ级', value: a }, { name: 'Ⅱ级', value: b }, { name: 'Ⅲ级', value: c }, { name: 'Ⅳ级', value: d }, { name: '未分级', value: f })
          chart.setOption({
            title: {
              text: '病情级别统计'
            },
            tooltip: {
              trigger: 'item',
              formatter: "{a} <br/>{b}: {c} ({d}%)"
            },
            series: [
              {
                name: '病情级别统计',
                type: 'pie',
                radius: ['30%', '60%'],
                center: ['55%', '50%'],
                avoidLabelOverlap: false,
                data: data1
              }
            ]
          })
        })
        .catch(function (error) {
          console.log("error", error);
        })
    },
    draw3() {
      let chart = this.$echarts.init(document.getElementById('myChart3'), 'dark')
      axios.get('/getAllOperationCount', {}).then((response) => {

        var cdata = response.data.results
        // console.log(cdata)
        var ddata = []
        cdata.forEach(item => {
          if (['P111', 'P112', 'P113', 'P114', 'P115', 'P116', 'P117'].includes(item.OperationCode)) {            ddata.push({ name: item.OperationName, value: item.count })
          }
        })
        chart.setOption({
          title: {
            text: '处置情况统计'
          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
          },
          series: [
            {
              name: '处置情况统计',
              type: 'pie',
              radius: ['30%', '60%'],
              center: ['55%', '50%'],
              avoidLabelOverlap: false,
              data: ddata
            }
          ]        })
      })
        .catch(function (error) {
          console.log("error", error);
        })
    },
    draw4() {
      let barchart = this.$echarts.init(document.getElementById('myChart4'), 'dark')
      axios.get('/getAssemblyList', {})
        .then((response) => {
          var tt = response.data.results
          var data1 = []
          var data2 = []
          var data3 = []
          var p1 = []
          for (var i = 0; i < tt.length; i++) {
            if (tt[i].GroupNo != null) {
              data1.push(tt[i].LocationName)
              p1.push(
                this.$axios.post('getEmergencyStatusCount', {
                  "groupNo": tt[i].GroupNo
                }))
            }
          }
          Promise.all(p1).then(res => {
            console.log(res);
            for (var j = 0; j < res.length; j++) {
              data2.push(res[j].data.results.chuzhizhong + res[j].data.results.chuzhiwancheng)
              data3.push(res[j].data.results.daihousong + res[j].data.results.yihousong)
            }
            console.log('会场处置统计')
            barchart.setOption({
              title: {
                text: '会场情况统计'
              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['现场处置人数', '后送人数']
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'value',
              },
              yAxis: {
                type: 'category',
                data: data1
              },
              series: [
                {
                  name: '现场处置人数',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'right'
                    }
                  },
                },
                {
                  name: '后送人数',
                  type: 'bar',
                  data: data3,
                  label: {
                    normal: {
                      show: true,
                      position: 'right'
                    }
                  },
                },
              ]
            })

          }).catch(
            (err) => {
              console.log(err)
            }
          )
        }).catch(function (error) {
          console.log("error", error);
        })
    },
    draw5() {
      let barchart = this.$echarts.init(document.getElementById('myChart5'), 'dark')
      axios.get('/getHosList', {})
        .then((response) => {
          var tt = response.data.results
          var data1 = []
          var data2 = []
          var data3 = []
          var data4 = []

          var p1 = []
          for (var i = 0; i < tt.length; i++) {
            if (tt[i].GroupNo != null) {
              data1.push(tt[i].OrganizationName)
              // console.log(tt[i].GroupNo)
              p1.push(
                this.$axios.post('getHosStatusCount', {
                  "groupNo": tt[i].GroupNo
                }))
            }
          }
          Promise.all(p1).then(res => {
            // console.log(res);
            for (var j = 0; j < res.length; j++) {
              data2.push(res[j].data.results.daisongda)
              data3.push(res[j].data.results.yiruyuan)
              data4.push(res[j].data.results.yichuyuan)
            }
            // console.log('第二步')
            barchart.setOption({
              title: {
                text: '医院情况统计',
              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['待后送', '已入院', '已出院']
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'value',
              },
              yAxis: {
                type: 'category',
                data: data1
              },
              series: [
                {
                  name: '待后送',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'right'
                    }
                  },
                },
                {
                  name: '已入院',
                  type: 'bar',
                  data: data3,
                  label: {
                    normal: {
                      show: true,
                      position: 'right'
                    }
                  },
                },
                {
                  name: '已出院',
                  type: 'bar',
                  data: data4,
                  label: {
                    normal: {
                      show: true,
                      position: 'right'
                    }
                  },
                },
              ]
            })
          }).catch(
            (err) => {
              console.log(err)
            }
          )
        }).catch(function (error) {
          console.log("error", error);
        })
    }
  },
};
</script>

<style>
  .I级{
    color:red;
  }
  .II级{
    color:orange;
  }
  .III级{
    color:#E6BD1A;
  }
  .IV级{
    color:blue;
  }
  .V级{
    color:blue;
  }
  .处置完成{
    color:green;
  }
  .已后送{
    color:green;
  }
  .待后送{
    color:red;
  }
  .处置中{
    color:red;
  }
</style>
