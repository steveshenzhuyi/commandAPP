<template>
  <div>
        <mt-header fixed style="font-size:25px;height: 50px;" title="群组通话">
         <mt-button size="small" type="danger" slot="left" icon="back"
        @click="returnA()"><small>返回</small></mt-button>
          <hr>
        </mt-header>
        <br><br> 
        <br>
            <div >
             <mt-button @click="callass()" size="small"  style="position:relative;height: 55px; width: 200px;font-size: 22px"
            type="primary"><b>呼叫全体现场组</b></mt-button></div><br>
            <div>
            <mt-button @click="callcar()" size="small"  style="position:relative;height: 55px; width: 200px;font-size: 22px"
            type="primary"><b>呼叫全体车辆组</b></mt-button></div><br>
            <div>
            <mt-button @click="callhos()" size="small"  style="position:relative;height: 55px; width: 200px;font-size: 22px"
            type="primary"><b>呼叫全体医院组</b></mt-button></div>
            <div align="left" style="">
            <mt-checklist @change="checkon"
            title="成员列表"
            v-model="critical1"
            :options="critical1list">
          </mt-checklist></div>
          <div align="left"><b>已选：{{tempcritical}}</b></div>
          <div align="center" style="height: 35px">
            <mt-button  size="small" type="danger" style="float: left;margin: 5px 30px 15px"
              @click="cancel()">取消勾选</mt-button>
              <mt-button  size="small" type="primary" style="float: right;margin: 5px 30px 15px"
              @click="confirmpush()">确定呼叫</mt-button>
              </div>
              <br><br><br><br>
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
      critical1:[],
      tempcritical:[],
      critical1list:[],
      popupVisible1:false,
      myvideoid:Number(window.localStorage.getItem("VIDEOUSERID"))
    };
  },
  mounted() {
    console.log();
    this.getVideoUserList()
  },
  methods: {
    getVideoUserList(){
      axios.get('/getVideoUserList',{}).then((response) => {
        for(var i=0;i<response.data.results.length;i++){
          if(response.data.results[i].VideoId != this.myvideoid){
            var videoname = response.data.results[i].Name+"("+response.data.results[i].VideoId+")"
            this.critical1list.push({label:videoname,value:response.data.results[i].VideoId})
          }
          
        }
        }).catch(function(error){
        console.log("error",error);
      })
    },
    cancel(){
      this.critical1 = []
      this.tempcritical=[]
    },
    confirmpush(){
      console.log(this.critical1)
      var tagset = []
      if(this.critical1.length>0){
        for (var i = 0; i < this.critical1.length; i++) {
         tagset.push(String(this.critical1[i]))
        }
        axios.post('/pushVideo',{
        roomnumber:996,
        type:6,
        tag:tagset
      }).then((response) => {
        if(response.data.results == "发送成功"){

          var scheme = 'com.tencent.trtc';
          var videoid = Number(window.localStorage.getItem("VIDEOUSERID"));
          console.log(roomnumber)
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":996,
                "videoid":videoid
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
        }else{
          alert("无人在线")
        }
      }).catch(function(error){
        console.log("error",error);
      })
      }
    },
    checkon: function(){
      this.tempcritical = ''
      for (var i = 0; i < this.critical1.length; i++) {
        this.tempcritical+=this.critical1[i]+' '
      }
    },
    callhall(){
      var scheme = 'com.tencent.trtc';
      var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
      appAvailability.check(scheme,
        function() {
          var sApp = startApp.set({"application":"com.tencent.trtc"
        }, { 
                "roomnumber":999,
                "videoid":videoid
              });
        sApp.start(function() {
          }, function(error) {
            alert(error);
          });
        },
        function() {
          alert('未安装视频通话软件');
        }
      );     
    },
    callall(){
      // console.log("111")
      // MessageBox.confirm('确定呼叫全体成员?').then(action => {
      // axios.post('/pushVideo',{
      //   type:0
      // }).then((response) => {
      //   if(response.data.results == "发送成功"){
          var scheme = 'com.tencent.trtc';
          var roomnumber = 990;
          var videoid = Number(window.localStorage.getItem("VIDEOUSERID"));
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":901
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
    //     }else{
    //       alert("无人在线")
    //     }
    //   }).catch(function(error){
    //     console.log("error",error);
    //   })
    // })
    },
    callass(){
       MessageBox.confirm('确定呼叫全体现场组?').then(action => {
      axios.post('/pushVideo',{
        type:1
      }).then((response) => {
        if(response.data.results == "发送成功"){
          var scheme = 'com.tencent.trtc';
          var roomnumber = 991;
          var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":videoid
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
        }else{
          alert("无人在线")
        }
      }).catch(function(error){
        console.log("error",error);
      })
    })
    },
    callcar(){
       MessageBox.confirm('确定呼叫全体车辆组?').then(action => {
      axios.post('/pushVideo',{
        type:2
      }).then((response) => {
        if(response.data.results == "发送成功"){
          var scheme = 'com.tencent.trtc';
          var roomnumber = 992;
          var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":videoid
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
        }else{
          alert("无人在线")
        }
      }).catch(function(error){
        console.log("error",error);
      })
    })
    },
    callhos(){
       MessageBox.confirm('确定呼叫全体医院组?').then(action => {
      axios.post('/pushVideo',{
        type:3
      }).then((response) => {
        if(response.data.results == "发送成功"){
          var scheme = 'com.tencent.trtc';
          var roomnumber = 993;
          var videoid = Number(window.localStorage.getItem("VIDEOUSERID"))
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":videoid
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
        }else{
          alert("无人在线")
        }
      }).catch(function(error){
        console.log("error",error);
      })
    })
    },
    callexpert(){
       MessageBox.confirm('确定呼叫全体专家组?').then(action => {
      axios.post('/pushVideo',{
        type:4
      }).then((response) => {
        if(response.data.results == "发送成功"){
          var scheme = 'com.tencent.trtc';
          var roomnumber = 994;
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":Number(window.localStorage.getItem("VIDEOUSERID"))
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
        }else{
          alert("无人在线")
        }
      }).catch(function(error){
        console.log("error",error);
      })
    })
    },
    calltemp(){
      axios.post('/pushVideo',{
        type:5,
        tag:this.taggroup
      }).then((response) => {
        if(response.data.results == "发送成功"){
          var scheme = 'com.tencent.trtc';
          var roomnumber = 995;
          appAvailability.check(scheme,
            function() {
              var sApp = startApp.set({"application":"com.tencent.trtc"}, { 
                "roomnumber":roomnumber,
                "videoid":901
              });
              sApp.start(function() {
              }, function(error) {
                alert(error);
              });
            },
            function() {
              alert('未安装视频通话软件');
            }
            );
        }else{
          alert("无人在线")
        }
      }).catch(function(error){
        console.log("error",error);
      })
    },
    
    returnA() {
         this.$router.push({name: 'command',params:{SELECTED:'指挥',SELECTED1:'1'}});
  }
}
};
</script>


