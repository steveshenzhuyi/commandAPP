<template>
  <div>
    <mt-tab-container class="page-tabbar-container"
                      v-model="selected">
      <mt-tab-container-item id="指挥">
        <mt-header fixed
                   style="font-size:25px;height: 50px;"
                   title="视频通话">
          <!--  <mt-button size="small" slot="left"
            @click="$goRoute('/CPR')"><small>今日统计</small></mt-button>
          <mt-button size="small" slot="right"
            @click="$goRoute('/increaseA')"><small>新增病人</small></mt-button> -->
          <hr>
        </mt-header>
        <br><br>
        <mt-navbar v-model="selected1">
          <mt-tab-item id="1">呼叫</mt-tab-item>
          <mt-tab-item id="2">记录</mt-tab-item>
        </mt-navbar>
        <br>
        <mt-tab-container v-model="selected1">
          <mt-tab-container-item id="1">
            <br><br>
            <div @click="callall()"
                 style="margin-top: -50px">
              <mt-cell><span style="font-size: 20px"
                      slot="title"><b>呼叫全体成员</b></span></mt-cell>
            </div>
            <div @click="callass()">
              <mt-cell><span style="font-size: 20px"
                      slot="title"><b>呼叫现场组</b></span></mt-cell>
            </div>
            <div @click="callcar()">
              <mt-cell><span style="font-size: 20px"
                      slot="title"><b>呼叫车辆组</b></span></mt-cell>
            </div>
            <div @click="callhos()">
              <mt-cell><span style="font-size: 20px"
                      slot="title"><b>呼叫医院组</b></span></mt-cell>
            </div>
            <div align="left"
                 style="">
              <mt-checklist @change="checkon"
                            title="成员列表"
                            v-model="critical1"
                            :options="critical1list">
              </mt-checklist>
            </div>
            <div align="left"><b>已选：{{tempcritical}}</b></div>
            <div align="center"
                 style="height: 35px">
              <mt-button size="small"
                         type="danger"
                         style="float: left;margin: 5px 30px 15px"
                         @click="cancel()">取消勾选</mt-button>
              <mt-button size="small"
                         type="primary"
                         style="float: right;margin: 5px 30px 15px"
                         @click="confirmpush()">确定呼叫</mt-button>
            </div>
            <br><br><br><br>
          </mt-tab-container-item>
          <mt-tab-container-item id="2">
            <br><br>
            <div @click="seeall()"
                 style="font-size: 150px;margin-top: -50px">
              <mt-cell>
                <h4 slot="title">全体成员</h4>
              </mt-cell>
            </div>
            <div @click="seeass()">
              <mt-cell>
                <h4 slot="title">所有现场组</h4>
              </mt-cell>
            </div>
            <div @click="seecar()">
              <mt-cell>
                <h4 slot="title">所有车辆组</h4>
              </mt-cell>
            </div>
            <div @click="seehos()">
              <mt-cell>
                <h4 slot="title">所有医院组</h4>
              </mt-cell>
            </div>
            <div @click="seesend()">
              <mt-cell>
                <h4 slot="title">我发出的临时消息</h4>
              </mt-cell>
            </div>
            <div @click="seereceive()">
              <mt-cell>
                <h4 slot="title">我接收的临时消息</h4>
              </mt-cell>
            </div>
          </mt-tab-container-item>
        </mt-tab-container>
      </mt-tab-container-item>
      <mt-tab-container-item id="地图">
        <mt-header fixed
                   style="font-size:25px;height: 50px;"
                   title="地图实况">
          <hr>
        </mt-header>
        <br><br> <br>
        <Gd_map :openmsg="openmsg"> </Gd_map>
        <br>
        <mt-button @click="AssVisable=true">现场组
        </mt-button>
        <mt-button @click="CarVisable=true">车辆组
        </mt-button>
        <mt-button @click="HosVisable=true">医院组
        </mt-button>
        <!-- <mt-button @click="ExpertVisable=true">专家组
        </mt-button> -->
        <br><br>
      </mt-tab-container-item>
      <mt-tab-container-item id="统计">
        <mt-header fixed
                   style="font-size:25px;height: 50px;"
                   title="信息统计">
          <hr>
        </mt-header>
<<<<<<< HEAD
        <div id ="myChart1" style="width:100%;height:300px"></div><hr>
        <div id ="myChart2" style="width:100%;height:300px"></div><hr>
        <div id ="myChart3" style="width:100%;height:300px"></div><hr>
        <div  id="myChart4" style="width:100%;height:300px"></div><hr>
        <div id ="myChart5" style="width:100%;height:300px"></div>
                <br><br>
=======
        <br><br>
        <div v-show="!datepickerVisiable" style="margin-top: 10px;margin-bottom: 10px">
          <mt-button @click.native="open('picker')">请选择日期</mt-button>
        </div>
        <div v-show="datepickerVisiable">
          <mt-button @click.native="open('picker')">{{dataVal | formatDate}}</mt-button>
          <mt-button @click="reset()">重置</mt-button>
        </div>
        <mt-datetime-picker v-model="dataVal"
                            type="date"
                            ref="picker"
                            :endDate="endDate"
                            year-format="{value} 年"
                            month-format="{value} 月"
                            date-format="{value} 日"
                            @confirm="handleConfirm">
        </mt-datetime-picker>
        <div align="center">
          <div id="myChart1"
               style="width:344px;height:300px"></div>
          <hr>
          <div id="myChart2"
               style="width:344px;height:300px"></div>
          <hr>
          <div id="myChart3"
               style="width:344px;height:300px"></div>
          <hr>
          <div id="myChart4"
               style="width:344px;height:300px"></div>
          <hr>
          <div id="myChart5"
               style="width:344px;height:300px"></div>
        </div>
        <br><br>
>>>>>>> a8fa7fb3c838a017c3d9be90ea869c893b7303cd
      </mt-tab-container-item>
    </mt-tab-container>
    <mt-popup v-model="popupVisible1"
              position="right">
    </mt-popup>
    <mt-popup v-model="AssVisable"
              position="bottom"
              style="width:344px;max-height:240px;overflow:auto">
      <div v-for="item in asslist">
        <div @click="chooseass(item.LocationNo)"
             class="text-group">
          {{item.LocationName}} </br>
          位置: <span style="color:#d3b29b;font-size:20px;">{{item.Description}}</span></br>
          会场负责人: {{item.realManager}} &nbsp;&nbsp; 联系方式: {{item.phone}}
          <hr>
        </div>
      </div>
    </mt-popup>
    <mt-popup v-model="CarVisable"
              position="bottom"
              style="width:344px;max-height:240px;overflow:auto">
      <div v-for="item in carlist">
        <div @click="choosecar(item.CarNo)"
             class="text-group">
          {{item.CarName}}</br>
          车牌号: {{item.CarId}}
          &nbsp;&nbsp;
          状态：{{item.CarStatus}}</br>
          车辆负责人:
          {{item.CarManager}}&nbsp;&nbsp;
          联系方式: {{item.phone}}
          <hr>
        </div>
      </div>
    </mt-popup>
    <mt-popup v-model="HosVisable"
              position="bottom"
              style="width:344px;max-height:240px;overflow:auto">
      <div v-for="item in hoslist">
        <div class="text-group"
             @click="choosehos(item.OrganizationCode)">
          {{item.OrganizationName}}</br>
          <mt-badge v-show="item.XiongtongTag">胸痛</mt-badge>
          <mt-badge v-show="item.GanranTag"> 感染</mt-badge>
          <mt-badge v-show="item.ZhongduTag">中毒</mt-badge>
          <mt-badge v-show="item.CuzhongTag">卒中</mt-badge>
          <mt-badge v-show="item.ChuangshangTag">创伤</mt-badge>
          <mt-badge v-show="item.HefusheTag">核辐射</mt-badge>
          <mt-badge v-show="item.YunchanTag">孕产</mt-badge>
          <mt-badge v-show="item.ErtongTag">儿童</mt-badge>
          <mt-badge v-show="item.ErchuangTag">儿创</mt-badge>
          <mt-badge v-show="item.JingshenTag">精神</mt-badge>
          <mt-badge v-show="item.XinliTag">心理</mt-badge>
          <mt-badge v-show="item.ChuanranTag">传染</mt-badge>
          </br>
          位置：{{item.LocationDescription}}</br>
          医院负责人：{{item.realManager}}&nbsp;&nbsp;联系方式：{{item.phone}}
          <hr>
        </div>
      </div>
    </mt-popup>
    <mt-popup v-model="ExpertVisable"
              position="bottom"
              style="width:344px;max-height:240px;overflow:auto">
      <div v-for="item in expertlist">
        <div class="text-group">
          {{item.Name}} </br>
          {{item.DepartmentName}} &nbsp;&nbsp; 专长：{{item.Specialty}}
          <hr>
        </div>
      </div>
    </mt-popup>
    <div>
      <mt-tabbar v-model="selected"
                 fixed>
        <mt-tab-item id="指挥">
          <img slot="icon"
               src="./icon/语音通话.png">
          <div style="font-size:12px">指挥</div>
        </mt-tab-item>
        <mt-tab-item id="地图">
          <img slot="icon"
               src="./icon/本车.png">
          <div style="font-size:12px">地图</div>
        </mt-tab-item>
        <mt-tab-item id="统计">
          <img slot="icon"
               src="./icon/病人.png">
          <div style="font-size:12px">统计</div>
        </mt-tab-item>
      </mt-tabbar>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import { picker } from 'mint-ui';
import axios from 'axios';
import { Toast } from 'mint-ui';
import { MessageBox } from 'mint-ui';
import Gd_map from './Gd_map.vue';
import { formatDate } from '../../static/js/datedd.js'
export default {
  // inject:['reload'],
  components: {
    'Gd_map': Gd_map,

  },
  filters: {                    //时间转换
    formatDate(time) {
      var date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd');
    }
  },
  data() {
    return {
      selected: this.$route.params.SELECTED,
      selected1: this.$route.params.SELECTED1,
      critical1: [],
      tempcritical: [],
      critical1list: [],
      popupVisible1: false,
      datepickerVisiable: false,
      dataVal: '',
      endDate: new Date(),
      AssVisable: false,
      CarVisable: false,
      HosVisable: false,
      ExpertVisable: false,
      carlist: [],
      asslist: [],
      expertlist: [],
      hoslist: [],
      openmsg: '',
    };
  },
  mounted() {
    console.log();

    this.draw1();
    this.draw2();
    this.draw3();
    this.draw4();
    this.draw5();
    this.getCarlist();
    this.getHoslist();
    this.getAsslist();
    this.getExpertlist();
    this.getVideoUserList()
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
    },
    getVideoUserList() {
      window.JPush.setTags({ sequence: 1, tags: ['R05', '901'] },
        (result) => {
          var sequence = result.sequence
          var tags = result.tags
        }, (error) => {
          console.log(error)
        })

      axios.get('/getVideoUserList', {}).then((response) => {
        for (var i = 0; i < response.data.results.length; i++) {
          if (response.data.results[i].VideoId != 901) {
            var videoname = response.data.results[i].Name + "(" + response.data.results[i].VideoId + ")"
            this.critical1list.push({ label: videoname, value: response.data.results[i].VideoId })
          }

        }
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    cancel() {
      this.critical1 = []
      this.tempcritical = []
    },
    confirmpush() {
      console.log(this.critical1)
      var tagset = []
      if (this.critical1.length > 0) {
        for (var i = 0; i < this.critical1.length; i++) {
          tagset.push(String(this.critical1[i]))
        }
        axios.post('/pushVideo', {
          type: 6,
          tag: tagset
        }).then((response) => {
          if (response.data.results == "发送成功") {

            var scheme = 'com.tencent.trtc';
            var roomnumber = 996;
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": 901
                });
                sApp.start(function () {
                }, function (error) {
                  alert(error);
                });
              },
              function () {
                alert('未安装视频通话软件');
              }
            );
          } else {
            alert("无人在线")
          }
        }).catch(function (error) {
          console.log("error", error);
        })
      }
    },
    checkon: function () {
      this.tempcritical = ''
      for (var i = 0; i < this.critical1.length; i++) {
        this.tempcritical += this.critical1[i] + ' '
      }
    },
    callall() {
      // console.log("111")
      MessageBox.confirm('确定呼叫全体成员?').then(action => {
        axios.post('/pushVideo', {
          type: 0
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            var roomnumber = 990;
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": 901
                });
                sApp.start(function () {
                }, function (error) {
                  alert(error);
                });
              },
              function () {
                alert('未安装视频通话软件');
              }
            );
          } else {
            alert("无人在线")
          }
        }).catch(function (error) {
          console.log("error", error);
        })
      })
    },
    callass() {
      MessageBox.confirm('确定呼叫全体现场组?').then(action => {
        axios.post('/pushVideo', {
          type: 1
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            var roomnumber = 991;
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": 901
                });
                sApp.start(function () {
                }, function (error) {
                  alert(error);
                });
              },
              function () {
                alert('未安装视频通话软件');
              }
            );
          } else {
            alert("无人在线")
          }
        }).catch(function (error) {
          console.log("error", error);
        })
      })
    },
    callcar() {
      MessageBox.confirm('确定呼叫全体车辆组?').then(action => {
        axios.post('/pushVideo', {
          type: 2
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            var roomnumber = 992;
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": 901
                });
                sApp.start(function () {
                }, function (error) {
                  alert(error);
                });
              },
              function () {
                alert('未安装视频通话软件');
              }
            );
          } else {
            alert("无人在线")
          }
        }).catch(function (error) {
          console.log("error", error);
        })
      })
    },
    callhos() {
      MessageBox.confirm('确定呼叫全体医院组?').then(action => {
        axios.post('/pushVideo', {
          type: 3
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            var roomnumber = 993;
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": 901
                });
                sApp.start(function () {
                }, function (error) {
                  alert(error);
                });
              },
              function () {
                alert('未安装视频通话软件');
              }
            );
          } else {
            alert("无人在线")
          }
        }).catch(function (error) {
          console.log("error", error);
        })
      })
    },
    callexpert() {
      MessageBox.confirm('确定呼叫全体专家组?').then(action => {
        axios.post('/pushVideo', {
          type: 4
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            var roomnumber = 994;
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": 901
                });
                sApp.start(function () {
                }, function (error) {
                  alert(error);
                });
              },
              function () {
                alert('未安装视频通话软件');
              }
            );
          } else {
            alert("无人在线")
          }
        }).catch(function (error) {
          console.log("error", error);
        })
      })
    },
    calltemp() {
      axios.post('/pushVideo', {
        type: 5,
        tag: this.taggroup
      }).then((response) => {
        if (response.data.results == "发送成功") {
          var scheme = 'com.tencent.trtc';
          var roomnumber = 995;
          appAvailability.check(scheme,
            function () {
              var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                "roomnumber": roomnumber,
                "videoid": 901
              });
              sApp.start(function () {
              }, function (error) {
                alert(error);
              });
            },
            function () {
              alert('未安装视频通话软件');
            }
          );
        } else {
          alert("无人在线")
        }
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    seeall() {
      this.$router.push({ name: 'C1', params: { type: 1, id: 990 } });
    },
    seeass() {
      this.$router.push({ name: 'C1', params: { type: 1, id: 991 } });
    },
    seecar() {
      this.$router.push({ name: 'C1', params: { type: 1, id: 992 } });
    },
    seehos() {
      this.$router.push({ name: 'C1', params: { type: 1, id: 993 } });
    },
    seesend() {
      this.$router.push({ name: 'C1', params: { type: 2, id: 901 } });
    },
    seereceive() {
      this.$router.push({ name: 'C1', params: { type: 3, id: 901 } });
    },
    handleConfirm() {
      console.log(this.dataVal)
      var date = formatDate(this.dataVal, 'yyyy-MM-dd')
      this.datepickerVisiable = true
      this.Redrawpie1(date);
      this.Redrawpie2(date);
      this.Redrawpie3(date);
      this.Redrawbar1(date)
      this.Redrawbar2(date)

    },
    Redrawpie1(val) {
      let chart = this.$echarts.init(document.getElementById('myChart1'), 'dark')
      // console.log(response.data.results)
      axios.post('/getAllStatusCountbyDate', {
        "time": val
      }).then((response) => {
        console.log('根据日期获取一次数据')

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
              center: ['52%', '50%'],
              avoidLabelOverlap: false,
              data: data1
            }
          ]
        })
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    Redrawpie2(val) {
      let chart = this.$echarts.init(document.getElementById('myChart2'), 'dark')
      axios.post('/getAllClassificationCountbyDate', {
        time: val
      })
        .then((response) => {
          console.log('根据日期获取一次数据pie2')
          var cdata = response.data.results
          var f = 0
          var a = 0
          var b = 0
          var c = 0
          var d = 0
          cdata.forEach(item => {
            if (item.Classification == 0) {
              f = cdata.find(item => item.Classification == 0).count            }
            else {
              f = 0
            }
            if (item.Classification == 1) {
              a = cdata.find(item => item.Classification == 1).count            }
            else {
              a = 0
            }
            if (item.Classification == 2) {
              b = cdata.find(item => item.Classification == 2).count            }
            else {
              b = 0
            }
            if (item.Classification == 3) {
              c = cdata.find(item => item.Classification == 3).count            }
            else {
              c = 0
            }
            if (item.Classification == 4) {
              d = cdata.find(item => item.Classification == 4).count            }
            else {
              d = 0
            }
          })

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
                center: ['52%', '50%'],
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
    Redrawpie3(val) {
      let chart = this.$echarts.init(document.getElementById('myChart3'), 'dark')

      axios.post('/getAllOperationCountbyDate', {
        "time": val
      }).then((response) => {

        var cdata = response.data.results
        // console.log(cdata)
        var ddata = []

        cdata.forEach(item => {
          if (['P111', 'P112', 'P113', 'P114', 'P115', 'P116', 'P117'].includes(item.OperationCode)) {
            ddata.push({ name: item.OperationName, value: item.count })
          }

        })
        if (ddata.length < 1) {
          ddata.push(
            { name: '吸氧', value: 0 },
            { name: "心电图", value: 0 },
            { name: "止血包扎", value: 0 },
            { name: "支具固定", value: 0 },
            { name: "口服药物", value: 0 },
            { name: "静脉给药", value: 0 },
            { name: "其它处理", value: 0 }
          )
        }

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
              center: ['52%', '50%'],
              avoidLabelOverlap: false,

              data: ddata
            }
          ]        })

      })
        .catch(function (error) {
          console.log("error", error);
        })
    },
    Redrawbar1(val) {
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
              // console.log(tt[i].GroupNo)
              p1.push(
                this.$axios.post('getEmergencyStatusCountbyDate', {
                  "groupNo": tt[i].GroupNo,
                  "time": val
                }))
            }
          }
          Promise.all(p1).then(res => {
            // console.log(res);
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
                top: '10%',
                data: ['现场处置人数', '后送人数']
              },
              grid: {
                left: '3%',
                right: '7%',
                bottom: '3%',
                containLabel: true
              },
              yAxis: {
                type: 'category',
                data: data1

              },
              xAxis: {

                type: 'value',


              },
              series: [
                {
                  name: '现场处置人数',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
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
                      position: 'top'
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
    Redrawbar2(val) {
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
                this.$axios.post('getHosStatusCountbyDate', {
                  "groupNo": tt[i].GroupNo,
                  "time": val
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
                top: '10%',
                data: ['待后送', '已入院', '已出院']
              },
              grid: {

                left: '3%',
                right: '7%',
                bottom: '3%',

                containLabel: true
              },
              yAxis: {
                type: 'category',
                data: data1

              },
              xAxis: {

                type: 'value',


              },
              series: [
                {
                  name: '待后送',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
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
                      position: 'top'
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
                      position: 'top'
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
    open(picker) {
      this.$refs[picker].open();
      this.dataVal = new Date()   //设置开始
    },
    reset() {
      this.draw1();
      this.draw2();
      this.draw3();
      this.draw4();
      this.draw5();
      this.datepickerVisiable = false
    },
    getCarlist() {
      axios.get('/getCarList', {})
        .then((response) => {

          var tempdata = response.data.results
          this.carlist = tempdata.filter(item => {
            if (item.Longitude != null && item.Latitude != null) {

              if (item.CarStatus == 0) {
                item.CarStatus = '空闲'
              }
              else {
                item.CarStatus = '忙碌'

              }

              return item
            }

          })


          console.log(this.carlist)






        }).catch(function (error) {
          console.log("error", error);
        })


    },

    getHoslist() {
      axios.get('/getHosList', {})
        .then((response) => {
          this.hoslist = response.data.results
        }).catch(function (error) {
          console.log("error", error);
        })
    },
    getAsslist() {
      axios.get('/getAssemblyList', {})
        .then((response) => {
          this.asslist = response.data.results
        }).catch(function (error) {
          console.log("error", error);
        })
    },
    getExpertlist() {
      axios.get('/getExpertList', {})
        .then((response) => {
          this.expertlist = response.data.results
        }).catch(function (error) {
          console.log("error", error);
        })

    },
    chooseass(LocationNo) {

      this.getChange({ 'A': LocationNo })


    },
    choosecar(CarNo) {

      this.getChange({ 'C': CarNo })


    },
    choosehos(OrganizationCode) {

      this.getChange({ 'H': OrganizationCode })


    },
    getChange(val) {
      //this.overallVisiable = false
      console.log(val)
      this.openmsg = val

    },
  },
};
</script>

<style>
.I级 {
  color: red;
}
.II级 {
  color: orange;
}
.III级 {
  color: #e6bd1a;
}
.IV级 {
  color: blue;
}
.V级 {
  color: blue;
}
.处置完成 {
  color: green;
}
.已后送 {
  color: green;
}
.待后送 {
  color: red;
}
.处置中 {
  color: red;
}
.text-group {
  padding: 5px;
  text-align: left;
}
</style>
