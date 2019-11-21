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
            <div style="display: inline-block; padding: 10px;">
              <mt-button size="small"
                         @click="callall()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫全<br>体成员</b></mt-button>
            </div>
            <div style="display: inline-block; padding: 10px;">
              <mt-button size="small"
                         @click="callhall()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>接通指<br>挥中心</b></mt-button>
            </div>
            <!-- <div style="display: inline-block; padding:10px;">
              <mt-button size="small"
                         @click="callcom()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫<br>总指挥</b></mt-button>
            </div>
            <div style="display: inline-block; padding:10px;">
              <mt-button size="small"
                         @click="callasscom()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫现<br>场指挥</b></mt-button>
            </div> -->
            <!-- <div style="display: inline-block; padding:10px;">
              <mt-button size="small" @click="callass()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫<br>现场组</b></mt-button>
            </div>
            <div style="display: inline-block; padding:10px;">
              <mt-button size="small" @click="callcarhos()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫车<br>辆和医院</b></mt-button>
            </div>
            <div style="display: inline-block; padding:10px;">
              <mt-button size="small" @click="callhos()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫<br>医院组</b></mt-button>
            </div>
            <div style="display: inline-block; padding:10px;">
              <mt-button size="small" @click="callexpert()"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>呼叫<br>专家</b></mt-button>
            </div> -->
            <div style="display: inline-block; padding:10px ;">
              <mt-button @click="callsingle()"
                         size="small"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>单独<br>通话</b></mt-button>
            </div>
            <div style="display: inline-block; padding:10px;">
              <mt-button @click="callgroup()"
                         size="small"
                         style="position:relative;height: 120px; width: 120px;font-size: 20px"
                         type="primary"><b>群组<br>通话</b></mt-button>
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
      <mt-tab-container-item id="病人">
        <mt-header fixed
                   style="font-size:25px;height: 50px;"
                   title="病人列表">
          <!-- <mt-button size="small" slot="right"
            @click="SEE()"><small>接收病人</small></mt-button>
          <hr> -->
        </mt-header>
        <br><br>
        <div style="width:80%;height: 60px">
          <!-- <mt-picker :slots="slots" @change="onPatientlistChange" :visible-item-count="3" :itemHeight='30'></mt-picker> -->
        </div>
        <div align="right">
          <mt-button type="primary"
                     style="position:relative;top:-50px"
                     @click="getPatientList()">刷新</mt-button>
        </div>
        <div v-for="(item,index) in patientList"
             align="left"
             style="position:relative;top:-50px">
          <hr><a @click="getpatient(index)">
            <div><span>{{item.PatientId}}</span>
              <span><small style="position:absolute;left:110px">{{item.CreateTime}}</small></span></div>
            <!-- <br> -->
            <div><span style="font-size:23px;">{{item.Name}}</span>
              <span style="position:absolute;left:110px;font-size:20px;"
                    :class="item.Classification">{{item.Classification}}</span>
              <span style="position:absolute;left:220px;font-size:20px;"
                    :class="item.StatusNameHos">{{item.StatusNameHos}}</span></div>
            <!-- <br> -->
            <div>
              <small style="color:grey">
                性别：{{item.Gender}}</small>
              <small style="color:grey;position:absolute;left:80px;">年龄：{{item.Age}}</small>
              <small style="width:9em;text-overflow:ellipsis;overflow:hidden;white-space:nowrap;
            color:grey;position:absolute;left:160px;">诊断：{{item.Diagnose}}</small></div>
            <div>
              <small style="color:grey">医疗点：{{item.LocationName}}</small>
            </div>
            <div>
              <small style="color:grey">后送医院：{{item.OrganizationName}}</small>
            </div>
            <div>
              <small style="color:grey">后送车辆：{{item.CarName}}</small>
              <small style="color:grey;position:absolute;left:110px;">车号：{{item.CarId}}</small>
            </div>
          </a>
        </div>
        <br><br>
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
        <mt-button @click="AssVisable=true"
                   size="small">医疗点
        </mt-button>
        <mt-button @click="CarVisable=true"
                   size="small">车辆组
        </mt-button>
        <mt-button @click="HosVisable=true"
                   size="small">医院组
        </mt-button>
        <mt-button @click="ExpertVisable=true"
                   size="small">专家组
        </mt-button>
        <br><br>
      </mt-tab-container-item>
      <mt-tab-container-item id="统计">
        <mt-header fixed
                   style="font-size:25px;height: 50px;"
                   title="信息统计">
          <hr>
        </mt-header>
        <br><br>
        <div v-show="!datepickerVisiable"
             style="margin-top: 10px;margin-bottom: 10px">
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
          {{item.LocationName}} <i style="font-size: 36px; color:#7eb37e;float:right"
             class="iconfont icon-dianhua"
             @click="sendcall(item.GroupNo)">
          </i></br>
          位置: <span style="color:#d3b29b;">{{item.Description}}</span></br>
          会场负责人: {{item.Manager}} </br> 联系方式: {{item.phone}}
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
          {{item.CarName}} <i style="font-size: 36px; color:#7eb37e;float:right"
             class="iconfont icon-dianhua"
             @click="sendcall(item.GroupNo)">
          </i></br>
          {{item.Description}}</br>
          车牌号: {{item.CarId}}
          &nbsp;&nbsp;
          状态：{{item.CarStatus}}</br>
          车辆负责人:
          {{item.CarManager}}</br>
          联系方式: {{item.phone}}
        </div>
        <hr>
      </div>
    </mt-popup>
    <mt-popup v-model="HosVisable"
              position="bottom"
              style="width:344px;max-height:240px;overflow:auto">
      <div v-for="item in hoslist">
        <div class="text-group"
             @click="choosehos(item.OrganizationCode)">
          {{item.OrganizationName}}<i style="font-size: 36px; color:#7eb37e;float:right"
             class="iconfont icon-dianhua"
             @click="sendcall(item.GroupNo)">
          </i></br>
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
          医院负责人：{{item.realManager}}</br>联系方式：{{item.phone}}
          <hr>
        </div>
      </div>
    </mt-popup>
    <mt-popup v-model="ExpertVisable"
              position="bottom"
              style="width:344px;max-height:240px;overflow:auto">
      <div v-for="item in expertlist">
        <div class="text-group">
          {{item.Name}} <i style="font-size: 36px; color:#7eb37e;float:right"
             class="iconfont icon-dianhua"
             @click="sendcall(item.GroupNo)">
          </i></br>
          {{item.DepartmentCode}} {{item.TitleCode}} &nbsp;&nbsp;
          专长：{{item.Specialty}}
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
        <mt-tab-item id="病人">
          <img slot="icon"
               src="./icon/病人.png">
          <div style="font-size:12px">病人</div>
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
      patientList: []
    };
  },
  mounted() {
    console.log();
    this.getPatientList();
    this.draw1();
    this.draw2();
    this.draw3();
    this.draw4();
    this.draw5();
    this.getCarlist();
    this.getHoslist();
    this.getAsslist();
    this.getExpertlist();
    this.getVideoUserList();
  },
  methods: {
    callsingle() {
      this.$router.push({ name: 'D1' });
    },
    callgroup() {
      this.$router.push({ name: 'D2' });
    },
    callhall() {
      var scheme = 'com.tencent.trtc';
      var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
      appAvailability.check(scheme,
        function () {
          var sApp = startApp.set({
            "application": "com.tencent.trtc"
          }, {
            "roomnumber": 999,
            "videoid": videoid
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
    },
    callcom() {//902 901
      var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
      axios.post('/pushVideo', {
        roomnumber: videoid,
        type: 7,
        tag: ['901']
      }).then((response) => {
        if (response.data.results == "发送成功") {

          var scheme = 'com.tencent.trtc';
          var roomnumber = videoid;
          appAvailability.check(scheme,
            function () {
              var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                "roomnumber": videoid,
                "videoid": videoid
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
    callasscom() {//901 902
      var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
      axios.post('/pushVideo', {
        roomnumber: videoid,
        type: 7,
        tag: ['902']
      }).then((response) => {
        if (response.data.results == "发送成功") {

          var scheme = 'com.tencent.trtc';
          appAvailability.check(scheme,
            function () {
              var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                "roomnumber": videoid,
                "videoid": videoid
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
    draw1() {
      let chart = this.$echarts.init(document.getElementById('myChart1'), 'dark')
      axios.get('/getAllStatusCount', {}).then((response) => {
        var data1 = [];
        var a = response.data.results.chuzhizhong;
        var b = response.data.results.chuzhiwancheng;
        var c = response.data.results.daihousong;
        var d = response.data.results.housongzhong;
        var e = response.data.results.yiruyuan;
        var f = response.data.results.yichuyuan;

        if (a > 0) {
          data1.push({ name: '处置中', value: a })
        }
        if (b > 0) {
          data1.push({ name: '处置完成', value: b })
        }
        if (c > 0) {
          data1.push({ name: '待后送', value: c })
        }
        if (d > 0) {
          data1.push({ name: '后送中', value: d })
        }
        if (e > 0) {
          data1.push({ name: '已入院', value: e })
        }
        if (f > 0) {
          data1.push({ name: '已出院', value: f })
        }
        if (data1.length < 1) {
          data1.push({ name: '处置中', value: 0 }, { name: '处置完成', value: 0 }, { name: '待后送', value: 0 }, { name: '后送中', value: 0 }, { name: '已入院', value: 0 }, { name: '已出院', value: 0 })
        }

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
          console.log(cdata, 'pie2')

          // var f = cdata.find(item => item.Classification == 0).count
          // var a = cdata.find(item => item.Classification == 1).count
          // var b = cdata.find(item => item.Classification == 2).count
          // var c = cdata.find(item => item.Classification == 3).count
          // var d = cdata.find(item => item.Classification == 4).count
          // var e = cdata.find(item => item.Classification == 5).count
          var f = 0
          var a = 0
          var b = 0
          var c = 0
          var d = 0
          var e = 0
          var data1 = [];
          cdata.forEach(item => {
            if (item.Classification == 0) {
              f = cdata.find(item => item.Classification == 0).count

              data1.push({ name: '未分级', value: f })
            }
            else {
              f = 0
            }
            if (item.Classification == 1) {
              a = cdata.find(item => item.Classification == 1).count

              data1.push({ name: 'Ⅰ级', value: a })            }
            else {
              a = 0
            }
            if (item.Classification == 2) {
              b = cdata.find(item => item.Classification == 2).count

              data1.push({ name: 'Ⅱ级', value: b })            }
            else {
              b = 0
            }
            if (item.Classification == 3) {
              c = cdata.find(item => item.Classification == 3).count

              data1.push({ name: 'Ⅲ级', value: c })            }
            else {
              c = 0
            }
            if (item.Classification == 4) {
              d = cdata.find(item => item.Classification == 4).count

              data1.push({ name: 'Ⅳ级', value: d })            }
            else {
              d = 0
            }
            if (item.Classification == 5) {
              e = cdata.find(item => item.Classification == 5).count

              data1.push({ name: 'Ⅴ级', value: e })            }
            else {
              e = 0
            }
          })




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
      var videoid = String(window.localStorage.getItem("VIDEOUSERID"))
      window.JPush.setTags({ sequence: 1, tags: ['R05', videoid] },
        (result) => {
          // alert(tags)
          var sequence = result.sequence
          var tags = result.tags
          // alert(tags)
        }, (error) => {
          // alert(error)
        })
      // axios.get('/getVideoUserList', {}).then((response) => {
      //   for (var i = 0; i < response.data.results.length; i++) {
      //     if (response.data.results[i].VideoId != 901) {
      //       var videoname = response.data.results[i].Name + "(" + response.data.results[i].VideoId + ")"
      //       this.critical1list.push({ label: videoname, value: response.data.results[i].VideoId })
      //     }

      //   }
      // }).catch(function (error) {
      //   console.log("error", error);
      // })
    },
    getPatientList() {
      axios.get('/getPatientListAllCreatetime', {
      }).then((response) => {
        this.patientList = response.data.results
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    getpatient: function (index) {
      console.log(index)
      this.$router.push({ name: 'A1', params: { PATIENTID: this.patientList[index].PatientId } })
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
            var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": videoid
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
      MessageBox.confirm('确定呼叫全体成员?').then(action => {
        axios.post('/pushVideo', {
          type: 14
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            var roomnumber = 990;
            var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": roomnumber,
                  "videoid": videoid
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
      axios.post('/pushVideo', {
        roomnumber: 999,
        type: 7,
        tag: ['501']
      }).then((response) => {
        if (response.data.results == "发送成功") {
          var scheme = 'com.tencent.trtc';
          appAvailability.check(scheme,
            function () {
              var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                "roomnumber": 999,
                "videoid": 902
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
    callcarhos() {
      axios.post('/pushVideo', {
        roomnumber: 999,
        type: 7,
        tag: ['502', '503']
      }).then((response) => {
        if (response.data.results == "发送成功") {
          var scheme = 'com.tencent.trtc';
          appAvailability.check(scheme,
            function () {
              var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                "roomnumber": 999,
                "videoid": 902
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
    callhos() {
      axios.post('/pushVideo', {
        roomnumber: 999,
        type: 7,
        tag: ['503']
      }).then((response) => {
        if (response.data.results == "发送成功") {
          var scheme = 'com.tencent.trtc';
          appAvailability.check(scheme,
            function () {
              var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                "roomnumber": 999,
                "videoid": 902
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
    callexpert() {
      MessageBox.confirm('确定呼叫全体专家组?').then(action => {
        axios.post('/pushVideo', {
          roomnumber: 999,
          type: 7,
          tag: ['401']
        }).then((response) => {
          if (response.data.results == "发送成功") {
            var scheme = 'com.tencent.trtc';
            appAvailability.check(scheme,
              function () {
                var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
                  "roomnumber": 999,
                  "videoid": 902
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
      this.$router.push({ name: 'C1', params: { type: 2, id: Number(window.localStorage.getItem("VIDEOUSERID")) } });
    },
    seereceive() {
      this.$router.push({ name: 'C1', params: { type: 3, id: Number(window.localStorage.getItem("VIDEOUSERID")) } });
    },
    handleConfirm() {
      // console.log(this.dataVal)
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
        var data1 = [];
        var a = response.data.results.chuzhizhong;
        var b = response.data.results.chuzhiwancheng;
        var c = response.data.results.daihousong;
        var d = response.data.results.housongzhong;
        var e = response.data.results.yiruyuan;
        var f = response.data.results.yichuyuan;

        if (a > 0) {
          data1.push({ name: '处置中', value: a })
        }
        if (b > 0) {
          data1.push({ name: '处置完成', value: b })
        }
        if (c > 0) {
          data1.push({ name: '待后送', value: c })
        }
        if (d > 0) {
          data1.push({ name: '后送中', value: d })
        }
        if (e > 0) {
          data1.push({ name: '已入院', value: e })
        }
        if (f > 0) {
          data1.push({ name: '已出院', value: f })
        }

        if (data1.length < 1) {
          data1.push({ name: '处置中', value: 0 }, { name: '处置完成', value: 0 }, { name: '待后送', value: 0 }, { name: '后送中', value: 0 }, { name: '已入院', value: 0 }, { name: '已出院', value: 0 })
        }

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
          var e = 0
          var data1 = [];
          cdata.forEach(item => {
            if (item.Classification == 0) {
              f = cdata.find(item => item.Classification == 0).count

              data1.push({ name: '未分级', value: f })
            }
            else {
              f = 0
            }
            if (item.Classification == 1) {
              a = cdata.find(item => item.Classification == 1).count

              data1.push({ name: 'Ⅰ级', value: a })            }
            else {
              a = 0
            }
            if (item.Classification == 2) {
              b = cdata.find(item => item.Classification == 2).count

              data1.push({ name: 'Ⅱ级', value: b })            }
            else {
              b = 0
            }
            if (item.Classification == 3) {
              c = cdata.find(item => item.Classification == 3).count

              data1.push({ name: 'Ⅲ级', value: c })            }
            else {
              c = 0
            }
            if (item.Classification == 4) {
              d = cdata.find(item => item.Classification == 4).count

              data1.push({ name: 'Ⅳ级', value: d })            }
            else {
              d = 0
            }
            if (item.Classification == 5) {
              e = cdata.find(item => item.Classification == 5).count

              data1.push({ name: 'Ⅴ级', value: e })            }
            else {
              e = 0
            }
          })

          if (data1.length < 1) {
            data1.push({ name: '未分级', value: 0 }, { name: 'Ⅰ级', value: 0 }, { name: 'Ⅱ级', value: 0 }, { name: 'Ⅲ级', value: 0 }, { name: 'Ⅳ级', value: 0 }, { name: 'Ⅴ级', value: 0 })
          }
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
    sendcall(val) {

      axios.post('/pushVideoLeader', {
        "GN": val
      }).then((response) => {
        var scheme = 'com.tencent.trtc';
        var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
        appAvailability.check(scheme,
          function () {
            var sApp = startApp.set({ "application": "com.tencent.trtc" }, {
              "roomnumber": 999,
              "videoid": videoid
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
      }).catch(function (error) {
        console.log("error", error);
      })

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
.block1 {
  width: 120px;
  height: 120px;
  background-color: #26a2ff;
  color: white;

  text-align: center;
  line-height: 120px;
}
</style>
