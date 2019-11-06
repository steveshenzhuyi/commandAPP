<template>
  <div align="center">
    <div id="map-container"
         class="map-root">
      放置地图
    </div>
    <!-- <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
      <circle class="loading-circle-1" cx="100" cy="50" r="3" stroke="red" fill="red" />
    </svg> -->
    <router-view></router-view>
  </div>

</template>

<script>
import axios from 'axios';
/* eslint-disable no-undef */
import AMap from 'AMap'
import AMapUI from 'AMapUI'

export default {
  props: ['openmsg'],
  sockets: {
    news1: function (data) {
      var that = this
      //收到的消息会在这里
      if (data.CarGroup != 'G00') {
        var car;
        var ass;
        var hos;
        var carno;
        var y = data.MessageDetail.indexOf('现场小组')
        var assname = data.MessageDetail.substring(y + 5, y + 8)
        for (var j = 0; j < that.assList.length; j++) {
          if (that.assList[j].LocationName == assname) {
            ass = that.positionAss[j]
            break
          }
        }
        for (var j = 0; j < that.assList.length; j++) {
          if (data.CarGroup == that.carList[j].GroupNo) {
            carno = j;
            car = that.positionCar[j]
            break
          }
        }
        for (var j = 0; j < that.hosList.length; j++) {
          if (data.HosGroup == that.hosList[j].GroupNo) {
            hos = that.positionHos[j]
            break
          }
        }
        that.render(carno, 1, car, ass, hos)
        that.cancelflag1 = 0
        setTimeout(() => {
          this.cancelflag1 = 1
        }, 120000);
      }
    },
    alert: function (data) {
      var that = this;
      var ass;
      var y = data.MessageDetail.indexOf('地点')
      var assname = data.MessageDetail.substring(y + 3, y + 6)
      for (var j = 0; j < that.assList.length; j++) {
        if (that.assList[j].LocationName == assname) {
          ass = j
          break
        }
      }
      console.log(ass)
      that.marker[ass].show();
    }
  },
  data() {
    return {
      state: window.localStorage.getItem('STATE'),
      hospital: this.$route.params.HOSPITAL,
      carNo: this.$route.params.CARNO,
      mdetail: '',
      intervalid1: null,
      carList: [],
      hosList: [],
      assList: [],
      positionHos: [],
      positionAss: [],
      positionCar: [],
      pathSimplifierIns: [],
      marker: [],
      navg: [],
      map: "",
      cancelflag: 1,
      cancelflag1: 1,
    };
  },
  mounted() {
    // this.initMap()
    this.initMap2()
    console.log('打开地图 ')
  },
  //监听返回的openmsg GYX
  watch: {

    openmsg: function (Value, oldValue) {
      // console.log(Value)
      if (Value.A) {
        // console.log(Value.A, '会场')
        var i = this.assList.findIndex(item => {
          return item.LocationNo == Value.A        })
        console.log(i, '找到数组下标')
        // console.log(this.assList, '会场坐标')
        var that = this
        AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
          var infoWindow = new SimpleInfoWindow({
            infoTitle: '<strong>' + that.assList[i].LocationName + '</strong>',
            infoBody: "<div style=\"padding:0px 0px 0px 4px;\">" + that.assList[i].Description + '</div>',
            offset: new AMap.Pixel(0, -20),
            autoMove: true
          })
          infoWindow.open(that.map, that.positionAss[i])
        })

      }
      //测试Hos
      else if (Value.H) {
        // console.log(Value.H, '医院')
        var i = this.hosList.findIndex(item => {
          return item.OrganizationCode == Value.H        })
        console.log(i, '找到医院数组下标')
        var that = this
        var item = that.hosList[i]
        var info = [];
        info.push("<div style=\"padding:5px 0px 5px 4px;\">");
        if (item.XiongtongTag != false) {
          info.push("<b class='rcorners1'>胸痛</b>" + "&nbsp;")
        }
        if (item.GanranTag != false) {
          info.push("<b class='rcorners1'>感染</b>" + "&nbsp;")
        }
        if (item.ZhongduTag != false) {
          info.push("<b class='rcorners1'>中毒</b>" + "&nbsp;")
        }
        if (item.CuzhongTag != false) {
          info.push("<b class='rcorners1'>卒中</b>" + "&nbsp;")
        }
        if (item.ChuangshangTag != false) {
          info.push("<b class='rcorners1'>创伤</b>" + "&nbsp;")
        }
        if (item.HefusheTag != false) {
          info.push("<b class='rcorners2'>核辐射</b>")
        }
        if (item.YunchanTag != false) {
          info.push("<b class='rcorners1'>孕产</b>" + "&nbsp;")
        }
        if (item.ErtongTag != false) {
          info.push("<b class='rcorners1'>儿童</b>" + "&nbsp;")
        }
        if (item.ErchuangTag != false) {
          info.push("<b class='rcorners1'>儿创</b>" + "&nbsp;")
        }
        if (item.JingshenTag != false) {
          info.push("<b class='rcorners1'>精神</b>" + "&nbsp;")
        }
        if (item.XinliTag != false) {
          info.push("<b class='rcorners1'>心理</b>" + "&nbsp;")
        }
        if (item.ChuanranTag != false) {
          info.push("<b class='rcorners1'>传染</b>" + "&nbsp;")
        }
        info.push("</br>" + "</br>" + item.LocationDescription + "</div>")


        AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
          var infoWindow = new SimpleInfoWindow({
            infoTitle: '<strong>' + that.hosList[i].OrganizationName + '</strong>',
            infoBody: info.join(""),
            offset: new AMap.Pixel(0, -20),
            autoMove: true
          })
          infoWindow.open(that.map, that.positionHos[i])
        })

      }
      else {
        // console.log(Value.C, '车辆')

        var i = this.carList.findIndex(item => {
          return item.CarNo == Value.C        })
        console.log(i, '找到车辆数组下标')

        var that = this
        AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
          var infoWindow = new SimpleInfoWindow({
            infoTitle: '<strong>' + that.carList[i].CarName + '</strong>',
            infoBody: "<div style=\"padding:0px 0px 0px 4px;\">" + '编号：' + that.carList[i].CarNo + '<br>车牌号：' + that.carList[i].CarId + '<br>状态：' + that.carList[i].CarStatus + '</div>',
            offset: new AMap.Pixel(0, -20),
            autoMove: true
          })
          infoWindow.open(that.map, that.positionCar[i])
        })



      }

    }
  },
  beforeDestroy() {
    // console.log(this.intervalid1)
    clearInterval(this.intervalid1)
    this.intervalid1 = null
  },

  methods: {
    // Evacuation() {
    //   this.$router.push({ name: 'confirm' })
    // },
    getELID(val) {
      this.$emit('transferID', val)
    },
    //生成推荐方案
    generateRecommend() {
      console.log('生成解决方案')
      //这里加一个2s的loading
      var that = this;
      for (var i = 0; i < that.pathSimplifierIns.length; i++) {
        that.pathSimplifierIns[i].setData([]);
      }
      that.render(0, 1, that.positionCar[0], that.positionAss[0], that.positionHos[0])
      that.getELID({ 'C': 'C01' })
      that.cancelflag = 0
      setTimeout(() => {
        this.cancelflag = 1
      }, 120000);

    },
    //取消闪烁按钮
    cancelFlash() {
      console.log('收到取消短讯')
      var that = this;
      console.log(that)
      for (var j = 0; j < that.marker.length; j++) {
        that.marker[j].hide();
      }
    },
    render(pathNum, status, poscar, posass, poshos) {
      var that = this
      if (status == 1) {
        AMap.plugin(['AMap.Driving'], function () {
          new AMap.Driving({ policy: AMap.DrivingPolicy.LEAST_TIME }).search(poscar, poshos, { waypoints: [posass] }, function (status, result) {
            // console.log(result)
            var searchresult = []
            searchresult.push([poscar.lng, poscar.lat])
            for (var i = 0; i < result.routes[0].steps.length; i++) {
              searchresult.push([result.routes[0].steps[i].start_location.lng, result.routes[0].steps[i].start_location.lat])
            }
            searchresult.push([poshos.lng, poshos.lat])
            that.pathSimplifierIns[pathNum].clearPathNavigators()
            that.pathSimplifierIns[pathNum].setData([
              {
                name: '车辆前往会场',
                path: searchresult
              }
            ]);
            that.navg[pathNum] = that.pathSimplifierIns[pathNum].createPathNavigator(0,
              {
                loop: true,
                speed: 10000
              });
            that.navg[pathNum].start()
          });
        })
      } else if (status == 2) {
        AMap.plugin(['AMap.Driving'], function () {
          new AMap.Driving({ policy: AMap.DrivingPolicy.LEAST_TIME }).search(poscar, poshos, function (status, result) {
            // console.log(result)
            var searchresult = []
            searchresult.push([poscar.lng, poscar.lat])
            for (var i = 0; i < result.routes[0].steps.length; i++) {
              searchresult.push([result.routes[0].steps[i].start_location.lng, result.routes[0].steps[i].start_location.lat])
            }
            searchresult.push([poshos.lng, poshos.lat])
            that.pathSimplifierIns[pathNum].setData([
              {
                name: '车辆前往医院',
                path: searchresult
              }
            ]);
            that.navg[pathNum].start()
          });
        })
      } else {
        that.pathSimplifierIns[pathNum].setData([]);
      }
    },
    creatMarker(mapObj) {
      var that = this
      for (var i = 0; i < that.positionAss.length; i++) {
        (function (j) {
          that.marker[j] = new AMap.Marker({
            content: "<svg xmlns='http://www.w3.org/2000/svg' version='1.1'><circle class='loading-circle-1' cx='100' cy='50' r='3' stroke='red' fill='red' /></svg>",
            position: that.positionAss[j],
            offset: new AMap.Pixel(-100, -50)
          });
          that.marker[j].setMap(mapObj);
          that.marker[j].hide();
        })(i)
      }
    },
    createPath(num, mapObj) {
      var that = this
      for (var i = 0; i < num; i++) {
        (function (j) {
          AMapUI.load(['ui/misc/PathSimplifier'], function (PathSimplifier) {
            if (!PathSimplifier.supportCanvas) {
              alert('当前环境不支持 Canvas！');
              return;
            }
            that.pathSimplifierIns[j] = new PathSimplifier({
              zIndex: 100,
              map: mapObj,
              getPath: function (pathData, pathIndex) {
                return pathData.path;
              },
              getHoverTitle: function (pathData, pathIndex, pointIndex) {
                if (pointIndex >= 0) {
                  return pathData.name + '，点:' + pointIndex + '/' + pathData.path.length;
                }
                return pathData.name + '，点数量' + pathData.path.length;
              },
              autoSetFitView: false,
              renderOptions: {
                pathLineStyle: {
                  strokeStyle: 'red',
                  lineWidth: 6,
                  dirArrowStyle: true
                },
                pathNavigatorStyle: {
                  fillStyle: '#0829c4',
                  pathLinePassedStyle: {
                    strokeStyle: '#116394',
                    lineWidth: 6,
                    dirArrowStyle: true
                  }
                }
              }
            });
          });
        })(i)
      }
      console.log(that.pathSimplifierIns)
    },

    initMap2() {
      var that = this
      var markerCar = [];
      var markerHos = [];
      var markerAss = [];
      var lnglats;
      var marker;


      let mapObj = new AMap.Map('map-container', {
        // center: [120.154539, 30.265048],
        center: [120.581828, 30.621622],
        zoom: 13,
        mapStyle: 'amap://styles/c276159e0bece965039d24472029a3e0',
      })
      that.map = mapObj;
      mapObj.plugin(['AMap.ToolBar', 'AMap.OverView', 'AMap.MapType'], function () {
        mapObj.addControl(new AMap.ToolBar())
        mapObj.addControl(new AMap.OverView({ isOpen: false }))
        mapObj.addControl(new AMap.MapType({ showTraffic: true, showRoad: false }))
      })
      var options = {
        enableHighAccuracy: true,
        maximumAge: 0
      }
      var watchID = navigator.geolocation.getCurrentPosition(onSuccess, onError, options);
      function onSuccess(position) {
        var gps = [position.coords.longitude, position.coords.latitude];
        AMap.convertFrom(gps, 'gps', function (status, result) {
          lnglats = result.locations[0];
          mapObj.setCenter(lnglats)
          marker = new AMap.Marker({
            position: lnglats,
            map: mapObj
          })
          AMap.event.addListener(marker, 'click', (e) => {
            AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
              var infoWindow = new SimpleInfoWindow({
                infoTitle: '<strong>我在这里</strong>',
                infoBody: '',
                offset: new AMap.Pixel(0, -20),
                autoMove: true
              })
              infoWindow.open(mapObj, e.target.getPosition())
            })
          })

        });
      };
      function onError(error) {
        // alert('code: '    + error.code    + '\n' + 'message: ' + error.message + '\n');
      }

      AMapUI.loadUI(['overlay/SvgMarker'], function (SvgMarker) {
        if (!SvgMarker.supportSvg) {
          alert('当前环境不支持SVG');
        }
        axios.get('/getAssemblyList', {}).then((response) => {
          that.assList = response.data.results;
          // console.log(assList)
          for (var i = 0; i < that.assList.length; i++) {
            that.positionAss[i] = new AMap.LngLat(that.assList[i].Longitude, that.assList[i].Latitude)
            // console.log(positionAss[i])
            markerAss[i] = new SvgMarker(
              new SvgMarker.Shape.IconFont({
                symbolJs: null,
                icon: 'icon-yiyuan2',
                size: 30,
                offset: [-15, -25],
                fillColor: 'grey'
              }), {
              map: mapObj,
              position: that.positionAss[i],
              showPositionPoint: {
                color: 'red'
              }
            }
            );
            markerAss[i].assinfo = that.assList[i];
            markerAss[i].on('click', function () {
              var thisMarkerAss = this;
              that.getELID({ 'A': thisMarkerAss.assinfo.LocationNo })
              AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
                var infoWindow = new SimpleInfoWindow({
                  infoTitle: '<strong>' + thisMarkerAss.assinfo.LocationName + '</strong>',
                  infoBody: "<div style=\"padding:0px 0px 0px 4px;\">" + thisMarkerAss.assinfo.Description + '</div>',
                  offset: new AMap.Pixel(0, -20),
                  autoMove: true
                })
                infoWindow.open(mapObj, thisMarkerAss.C.position)

              })
            })
          }
          that.creatMarker(mapObj);
        }).catch(function (error) {
          console.log("error", error);
        })


        axios.get('/getHosList', {}).then((response) => {
          that.hosList = response.data.results;
          // console.log(hosList)
          for (var i = 0; i < that.hosList.length; i++) {
            that.positionHos[i] = new AMap.LngLat(that.hosList[i].Longitude, that.hosList[i].Latitude)
            // console.log(positionHos[i])
            markerHos[i] = new SvgMarker(
              new SvgMarker.Shape.IconFont({
                symbolJs: null,
                icon: 'icon-filled_outline_hospital-cross-building',
                size: 30,
                offset: [-15, -25],
                fillColor: 'red'
              }), {
              map: mapObj,
              position: that.positionHos[i],
              showPositionPoint: {
                color: 'red'
              }
            });
            markerHos[i].hosinfo = that.hosList[i];
            markerHos[i].on('click', function () {

              var thisMarkerHos = this;

              that.getELID({ 'H': thisMarkerHos.hosinfo.OrganizationCode })

              var hosNo = thisMarkerHos.hosinfo.OrganizationCode
              var item = {}
              axios.post('/getHosInfo', {
                'hosNo': hosNo
              }
              )
                .then((response) => {
                  item = response.data['results'][0]

                  var info = [];

                  // info.push("<div class='input-card content-window-card'><div><img style=\"float:left;\" src=\" https://webapi.amap.com/images/autonavi.png \"/></div> ");
                  info.push("<div style=\"padding:5px 0px 5px 4px;\">");
                  // info.push("<p class='input-item'>电话 : 010-84107000   邮编 : 100102</p>");
                  if (item.XiongtongTag != false) {
                    info.push("<b class='rcorners1'>胸痛</b>" + "&nbsp;")
                  }
                  if (item.GanranTag != false) {
                    info.push("<b class='rcorners1'>感染</b>" + "&nbsp;")
                  }
                  if (item.ZhongduTag != false) {
                    info.push("<b class='rcorners1'>中毒</b>" + "&nbsp;")
                  }
                  if (item.CuzhongTag != false) {
                    info.push("<b class='rcorners1'>卒中</b>" + "&nbsp;")
                  }
                  if (item.ChuangshangTag != false) {
                    info.push("<b class='rcorners1'>创伤</b>" + "&nbsp;")
                  }
                  if (item.HefusheTag != false) {
                    info.push("<b class='rcorners2'>核辐射</b>")
                  }
                  if (item.YunchanTag != false) {
                    info.push("<b class='rcorners1'>孕产</b>" + "&nbsp;")
                  }
                  if (item.ErtongTag != false) {
                    info.push("<b class='rcorners1'>儿童</b>" + "&nbsp;")
                  }
                  if (item.ErchuangTag != false) {
                    info.push("<b class='rcorners1'>儿创</b>" + "&nbsp;")
                  }
                  if (item.JingshenTag != false) {
                    info.push("<b class='rcorners1'>精神</b>" + "&nbsp;")
                  }
                  if (item.XinliTag != false) {
                    info.push("<b class='rcorners1'>心理</b>" + "&nbsp;")
                  }
                  if (item.ChuanranTag != false) {
                    info.push("<b class='rcorners1'>传染</b>" + "&nbsp;")
                  }


                  info.push("</br>" + "</br>" + thisMarkerHos.hosinfo.LocationDescription + "</div>")
                  // info.push("<p class='input-item'>地址 :北京市朝阳区望京阜荣街10号首开广场4层</p></div>");


                  AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
                    var infoWindow = new SimpleInfoWindow({
                      infoTitle: '<strong>' + thisMarkerHos.hosinfo.OrganizationName + '</strong>',
                      infoBody: info.join(""),


                    })
                    infoWindow.open(mapObj, thisMarkerHos.C.position)
                  })

                }).catch(function (error) {
                  console.log("error", error);
                })

            })
          }
        }).catch(function (error) {
          console.log("error", error);
        })

        axios.get('/getCarList', {}).then((response) => {
          that.carList = response.data.results;
          that.carList.shift()
          that.createPath(that.carList.length, mapObj)
          // console.log(that.carList)
          for (var i = 0; i < that.carList.length; i++) {
            if (that.carList[i].Longitude != null && that.carList[i].Latitude != null) {
              if (that.carList[i].CarStatus == 0) {
                that.carList[i].CarStatus = '空闲'
              }
              else {
                that.carList[i].CarStatus = '忙碌'
              }
              that.positionCar[i] = new AMap.LngLat(that.carList[i].Longitude, that.carList[i].Latitude)
              // console.log(positionCar[i])
              markerCar[i] = new SvgMarker(
                new SvgMarker.Shape.IconFont({
                  symbolJs: null,
                  icon: 'icon-emergencycar',
                  size: 30,
                  offset: [-15, -25],
                  fillColor: 'blue'
                }), {
                map: mapObj,
                position: that.positionCar[i],
                showPositionPoint: {
                  color: 'red'
                }
              });
              markerCar[i].carinfo = that.carList[i];
              markerCar[i].on('click', function () {
                var thisMarkerCar = this;
                that.getELID({ 'C': thisMarkerCar.carinfo.CarNo })
                AMapUI.loadUI(['overlay/SimpleInfoWindow'], function (SimpleInfoWindow) {
                  var infoWindow = new SimpleInfoWindow({
                    infoTitle: '<strong>' + thisMarkerCar.carinfo.CarName + '</strong>',
                    infoBody: "<div style=\"padding:0px 0px 0px 4px;\">" + '编号：' + thisMarkerCar.carinfo.CarNo + '<br>车牌号：' + thisMarkerCar.carinfo.CarId + '<br>状态：' + thisMarkerCar.carinfo.CarStatus + '</div>',
                    offset: new AMap.Pixel(0, -20),
                    autoMove: true
                  })
                  infoWindow.open(mapObj, thisMarkerCar.C.position)
                })
              })




            }
          }
          that.intervalid1 = setInterval(() => {
            console.log("正在获取新位置")
            axios.get('/getCarList', {}).then((response) => {
              that.carList = [];
              that.carList = response.data.results;
              that.carList.shift();
              // console.log(that.carList)
              for (var i = 0; i < that.carList.length; i++) {
                if (that.carList[i].Longitude != null && that.carList[i].Latitude != null) {
                  that.positionCar[i] = new AMap.LngLat(that.carList[i].Longitude, that.carList[i].Latitude)
                  if (that.carList[i].CarStatus == 1) {
                    var ass;
                    var hos;
                    for (var j = 0; j < that.assList.length; j++) {
                      if (that.carList[j].Assembly == that.assList[j].LocationName) {
                        ass = that.positionAss[j]
                        break
                      }
                    }
                    for (var j = 0; j < that.hosList.length; j++) {
                      if (that.carList[j].Hospital == that.hosList[j].OrganizationName) {
                        hos = that.positionHos[j]
                        break
                      }
                    }
                    that.render(i, 1, that.positionCar[i], ass, hos)
                  } else if (that.carList[i].CarStatus == 2) {
                    var ass = null;
                    var hos;
                    for (var j = 0; j < that.hosList.length; j++) {
                      if (that.carList[j].Hospital == that.hosList[j].OrganizationName) {
                        hos = that.positionHos[j]
                        break
                      }
                    }
                    that.render(i, 2, that.positionCar[i], ass, hos)
                  } else {
                    if (that.cancelflag == 1 && that.cancelflag1 == 1) that.render(i, 0, null, null, null)
                  }
                  // marker操作
                  if (that.carList[i].CarStatus == 0) {
                    that.carList[i].CarStatus = '空闲'
                  }
                  else {
                    that.carList[i].CarStatus = '忙碌'
                  }
                  delete markerCar[i].carinfo;
                  markerCar[i].carinfo = that.carList[i];
                  markerCar[i].setPosition(that.positionCar[i])
                }
              }
            }).catch(function (error) {
              console.log("error", error);
            })
          }, 60000)
        }).catch(function (error) {
          console.log("error", error);
        })
      })

    }

  }
};
</script>

<style>
.map-root {
  width: 100%;
  height: 350px;

  /* padding: 10px; */
  /* border: 5px solid gray; */
  /* margin: 0px; */
}
.rcorners1 {
  border-radius: 25px;
  background: #409eff;
  padding: 5px;
  /* font-size: 14px; */
  width: 40px;
  color: white;
  height: 18px;
  text-align: center;
}
.rcorners2 {
  border-radius: 25px;
  background: #409eff;
  padding: 5px;
  width: 60px;
  color: white;
  height: 18px;
  text-align: center;
}
</style>

<style>
@keyframes morph {
  0% {
    stroke-width: 0;
    stroke-opacity: 1;
  }
  10% {
    stroke-width: 8;
    stroke-opacity: 0.9;
  }
  20% {
    stroke-width: 16;
    stroke-opacity: 0.8;
  }
  30% {
    stroke-width: 24;
    stroke-opacity: 0.7;
  }
  40% {
    stroke-width: 32;
    stroke-opacity: 0.6;
  }
  50% {
    stroke-width: 40;
    stroke-opacity: 0.5;
  }
  60% {
    stroke-width: 48;
    stroke-opacity: 0.4;
  }
  70% {
    stroke-width: 56;
    stroke-opacity: 0.3;
  }
  80% {
    stroke-width: 64;
    stroke-opacity: 0.2;
  }
  90% {
    stroke-width: 72;
    stroke-opacity: 0.1;
  }
  100% {
    stroke-width: 80;
    stroke-opacity: 0.1;
  }
}
.loading-circle-1 {
  -webkit-animation: morph 1s ease infinite;
  animation: morph 1s ease infinite;
}
</style>

